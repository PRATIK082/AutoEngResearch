# G5 — Vehicle Domain Deep-Dive (Middleware, Body, Powertrain, AD/ADAS, IVI, Cockpit, Zonal, HPC)

**Workstream:** G5 (Domain deep-dive)
**Scope:** Per-domain software problem discovery with lesson-learnt + AI-ready guidelines for: Middleware, Body, Powertrain, AD/ADAS, IVI/Infotainment, Cockpit, Zonal, HPC. Complements G2 survey (broad) with deep records, and W7 (ASIL/multicore truth) for safety/timing.
**Date:** 2026-11-16
**Status:** Verify (Tier 2) — in progress (Phase A seed: §1 + domain frame + G5-001..G5-004 Middleware). Full target: 32 records (G5-001..G5-032, 4 per domain).
**Method:** Same schema and tiers as W7 (see PROMPT_W7_G5_ASIL_DOMAINS §3–§4). Cross-refs: W7 (safety/multicore), W2 (AUTOSAR/middleware), W3 (testing), G1 (silicon), G3 (cloud), G4 (energy).

---

## 1. Workstream Summary

Each domain gets four records: (a) architecture/integration, (b) safety/timing, (c) update/calibration/data, (d) cross-domain lesson-learnt. Domains interact through shared actuators, shared compute, and shared networks — integration is where domain problems become vehicle problems (G2 §1).

Domain frame (ASIL context per DOMAIN_MAP + G2):

| Domain | Representative ASILs | Shared-resource hotspots | W7 linkage |
|---|---|---|---|
| Middleware | Inherits highest user | Buses, gateways, time-sync | W7-006/007/008, W7-023/026 |
| Body | QM–ASIL-B | I/O, power states, diagnostics | W7-001/005, W7-019 |
| Powertrain | ASIL-C/D | Torque path, calibration, aftertreatment | W7-003/004, W7-010/029 |
| AD/ADAS | ASIL-B/D | Perception compute, actuators, ODD data | W7-002/004, W7-014..018 |
| IVI/Infotainment | QM (mostly) | SoC shared with cluster/HUD, OTA | W7-005, W7-006 |
| Cockpit | ASIL-B (telltales) | Display pipeline, driver monitoring | W7-002/007 |
| Zonal | Mixed (up to D) | Consolidated I/O, power, network | W7-006..009, W7-019..022 |
| HPC | Mixed (up to D) | vCPU/memory/interconnect, hypervisor | W7-006..009, W7-014..022 |

---

## 2. Problem Catalogue

### G5-001 — Middleware: signal↔service gateway correctness and timing

- **Domain:** Middleware
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Zonal/centralised E/E moves signals (CAN/LIN) to services (SOME/IP/DDS) through gateways. Signal-to-service mapping is manual; atomicity of signal groups, end-to-end timing, and conversion correctness break at the gateway. Complements W2-002 (interop) with the integration/test view.
- **Who:** Gateway developers, system integrators, timing analysts.
- **Root cause:** Two communication paradigms bridged per-project without a canonical mapping model or timing contract.
- **Contributing factors:** Variant-specific mappings; gateway as bottleneck/SPOF; mixed toolchains (PREEvision/SystemDesk/gateway configurators).
- **Technical limitations:** Gateway latency/jitter; payload adaptation; service-discovery dynamics vs. static signal timing.
- **Tool limitations:** Gateway config generation exists but end-to-end timing verification across signal+service legs is manual.
- **Process limitations:** No gateway mapping owner; testing per-leg instead of end-to-end.
- **Frequency/severity:** High on every mixed-architecture vehicle; severity up to ASIL-D for brake/steer signals.
- **Time/cost impact:** Integration rework; timing-violation fixes cascade across domains.
- **Downstream effects:** Signal staleness, mode confusion, safety-case timing gaps.
- **Existing solutions:** SOME/IP gateways, static mapping tables, CANoe CO testing.
- **Why insufficient:** Per-leg verification misses end-to-end semantics and timing.
- **Automation feasibility:** High (mapping consistency checks, generated gateway tests, timing-budget tracking).
- **AI applicability:** Medium (mapping-anomaly detection, impact analysis).
- **Human approval:** Required for safety-related signal routing.
- **Evidence/sources:** W2-002; W1-018; W7-007/W7-023; Vector/PREEvision docs (context).
- **Confidence:** High.
- **Unanswered questions:** What gateway-induced latency budget is acceptable per signal class?
- **Lesson learnt:** Gateways pass functional tests and fail timing tests — end-to-end deadline measurement across both legs is the test that matters.
- **AI-ready guideline:** IF signal crosses signal↔service gateway THEN require mapping record + end-to-end timing budget + gateway negative tests WITH gateway verification report GATE integrator.

### G5-002 — Middleware: time synchronisation and deterministic communication

- **Domain:** Middleware
- **Lifecycle stage:** Architecture, integration, V&V
- **Problem:** Fusion, control, and logging depend on a common time base (gPTP/TSN, SOME/IP time-sync, DDS time). Sync loss, grandmaster failover gaps, and unbounded middleware jitter break determinism and make logs unusable for root cause.
- **Who:** Network architects, middleware integrators, safety engineers.
- **Root cause:** Time-sync treated as network configuration detail rather than a safety-relevant function with its own requirements, monitoring, and fallback.
- **Contributing factors:** Mixed TSN/non-TSN segments; Docker/VM environments dropping multicast; QoS left at defaults.
- **Technical limitations:** gPTP failover transients; DDS/SOME/IP jitter under load; timestamping point ambiguity.
- **Tool limitations:** Lab jitter figures (e.g. TSN 17.12 µs in lab, W2-007) do not transfer to loaded vehicle networks; monitoring tooling sparse.
- **Process limitations:** No time-sync health owner; no sync-loss fallback procedure validated.
- **Frequency/severity:** Medium-High; severity up to ASIL-D for motion/perception timestamps.
- **Time/cost impact:** Late network rework; unusable field logs prolong every subsequent investigation.
- **Downstream effects:** Stale fusion, control instability, forensic blind spots.
- **Existing solutions:** gPTP, TSN (Qbv/Qav), time-sync monitoring, holdover clocks.
- **Why insufficient:** Configured but not monitored or failure-tested; fallback unproven.
- **Automation feasibility:** High (sync-health monitors, jitter logging, failover tests).
- **AI applicability:** Medium (jitter/sync anomaly detection from traces).
- **Human approval:** Required for safety-relevant time base.
- **Evidence/sources:** W2-007/W2-008; W1-018; W7-007.
- **Confidence:** High.
- **Unanswered questions:** What sync accuracy is actually achieved in loaded production networks vs. lab figures?
- **Lesson learnt:** Time-sync is a safety function — monitor it, alarm on it, and test grandmaster loss, or every timestamp in the vehicle is suspect.
- **AI-ready guideline:** IF safety function depends on network time THEN require sync-health monitor + failover test + timestamp-accuracy budget WITH sync verification report GATE network architect.

### G5-003 — Middleware: service接口 versioning and mixed-version fleet coexistence

- **Domain:** Middleware
- **Lifecycle stage:** Design, release, OTA, service
- **Problem:** Service interfaces evolve (fields added, semantics changed) while the fleet runs mixed software versions. Incompatible SOME/IP/DDS messages across versions cause silent misbehaviour; no enforced compatibility policy or version-negotiation strategy.
- **Who:** SOA designers, release managers, OTA engineers.
- **Root cause:** Interface governance stops at single-release consistency; fleet mixed-version reality unowned.
- **Contributing factors:** Partial OTA penetration; supplier cadence mismatch; no central service registry with compatibility rules.
- **Technical limitations:** Backward/forward compatibility must be designed (optional fields, version negotiation, safe defaults); runtime negotiation adds complexity.
- **Tool limitations:** ARXML/service-registry tooling checks single-release consistency, not N-version compatibility.
- **Process limitations:** No compatibility policy gate in release process.
- **Frequency/severity:** Medium; severity up to ASIL-C/D where services carry control data.
- **Time/cost impact:** Field incidents; forced full-fleet updates; rollback complexity (G3-018).
- **Downstream effects:** Silent data misinterpretation; OTA campaign constraints.
- **Existing solutions:** Versioned services, UCM/package management, compatibility matrices (immature).
- **Why insufficient:** Policy and enforcement tooling absent; compatibility tested ad hoc.
- **Automation feasibility:** High (N-version compatibility matrix checks in CI).
- **AI applicability:** Medium (breaking-change detection from IDL diffs).
- **Human approval:** Required for safety-related service changes.
- **Evidence/sources:** W2-002/W2-016; W4-005 (variants); G3-001/019 (campaigns); W7-030.
- **Confidence:** Medium-High.
- **Unanswered questions:** What is the field incident rate from service-version mismatch?
- **Lesson learnt:** Every SOA program eventually learns mixed-version operation the hard way — compatibility policy belongs in the release gate from SOP, not added after the first incident.
- **AI-ready guideline:** IF service interface changes THEN require N-version compatibility matrix + negotiation/fallback test WITH compatibility report GATE release manager.

### G5-004 — Middleware: cross-domain lesson — middleware as the integration contract

- **Domain:** Middleware (cross-domain)
- **Lifecycle stage:** All (integration focus)
- **Problem:** Domains integrate through middleware, but no single artefact owns the end-to-end contract (signals, services, timing, versions, diagnostics). Integration becomes discovery at HIL/vehicle stage.
- **Who:** Integrators, architects, all domain teams.
- **Root cause:** Contract ownership fragmented across signal DB, service registry, timing sheets, and diagnostic specs.
- **Contributing factors:** Supplier boundaries; tool silos; middleware treated as plumbing, not as architecture.
- **Technical limitations:** Contract spans four dimensions (data, timing, version, safety) with no unified model.
- **Tool limitations:** No single contract dashboard; consistency checks per-tool only.
- **Process limitations:** Integration role staffed late; contract reviews skipped.
- **Frequency/severity:** Pervasive; severity inherits highest integrated ASIL.
- **Time/cost impact:** Integration-phase rework dominates program delay (cf. W1-012, W3 KPIT anchor).
- **Downstream effects:** Late defect discovery; launch slip.
- **Existing solutions:** Interface management plans, contract-phase RTE (W2-012), integration workflows.
- **Why insufficient:** Plans exist as documents, not as machine-checked contracts in CI.
- **Automation feasibility:** High (contract dashboard: data+timing+version+safety in one CI gate).
- **AI applicability:** High as checker (contract inconsistency finder); low as author.
- **Human approval:** Required (chief architect signs integration contract).
- **Evidence/sources:** W1-007/W1-008; W2-002/W2-012; W7-023; G2 integration-as-discovery.
- **Confidence:** High.
- **Unanswered questions:** What contract-check automation coverage predicts integration-phase defect density?
- **Lesson learnt:** Middleware is the architecture — programs that treat the integration contract as a CI-checked artefact integrate; programs that treat it as documentation discover.
- **AI-ready guideline:** IF program integrates ≥2 domains through middleware THEN maintain machine-checked integration contract (data+timing+version+safety) in CI WITH contract dashboard GATE chief architect.

<!-- Domain batches 2–8 (Body, Powertrain, AD/ADAS, IVI, Cockpit, Zonal, HPC): G5-005..G5-032 follow the same 4-record pattern in Phase C. -->

---

## 3. Cross-Cutting Observations (seed)

1. Middleware carries every domain's timing and versioning risk; it is the highest-leverage deep-dive starting point.
2. Body/QM domains become safety-relevant through shared resources — W7-005 pattern repeats per domain.
3. Powertrain/AD carry the ASIL-C/D + multicore burden — W7-003/004/014..018 apply directly.
4. Zonal/HPC concentrate all interference classes — W7-006..009 is the entry checklist for those domains.

---

## 4. Evidence and Source Index (seed)

- ISO 26262 series; ISO 21448 (SOTIF, AD/ADAS); AUTOSAR SWS (COM, SOME/IP, DDS, OS, RTE, MCAL); gPTP/TSN (IEEE 802.1AS/Qbv/Qav).
- Repo-internal: W7 (safety/multicore), W2 (AUTOSAR/middleware), W1-010/018 (timing/network), W3 (testing), G1 (silicon), G3 (cloud/OTA), G4 (energy), G2 (domain survey), DOMAIN_MAP taxonomy.

---

## 5. Unanswered Research Questions (seed)

1. Per-domain timing budgets across signal+service legs (all domains)?
2. Mixed-version coexistence policy enforcement per domain?
3. Domain-specific FFI evidence: what passes assessment per domain?
4. Domain OTA success rates and rollback effectiveness (link G3)?

---

## 6. Limitations of This Seed

- Phase A seed: Middleware batch only; Body/Powertrain/AD-ADAS/IVI/Cockpit/Zonal/HPC batches follow in Phase C with the same schema.
- No numerical values invented; domain figures to be sourced in Phase C or marked unquantified.

---
See also: [PROMPT_W7_G5_ASIL_DOMAINS.md](./PROMPT_W7_G5_ASIL_DOMAINS.md) | [W7_asil_mixed_criticality_multicore.md](./W7_asil_mixed_criticality_multicore.md) | [DOMAIN_MAP.md](./DOMAIN_MAP.md)
