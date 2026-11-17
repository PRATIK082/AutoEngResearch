# G5 — Vehicle Domain Deep-Dive (Middleware, Body, Powertrain, AD/ADAS, IVI, Cockpit, Zonal, HPC)

**Workstream:** G5 (Domain deep-dive)
**Scope:** Per-domain software problem discovery with lesson-learnt + AI-ready guidelines for: Middleware, Body, Powertrain, AD/ADAS, IVI/Infotainment, Cockpit, Zonal, HPC. Complements G2 survey (broad) with deep records, and W7 (ASIL/multicore truth) for safety/timing.
**Date:** 2026-11-16
**Status:** Verify (Tier 2) — records complete (G5-001..G5-032, 4 per domain). Integration into indices/artefacts/book (Phase D) pending.
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

### G5-005 — Body: BCM consolidation, I/O abstraction and load control

- **Domain:** Body
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Comfort, lighting, wiper, seat, and power-distribution functions consolidate into one BCM or a few zonal I/O nodes, but I/O abstraction stays per-pin and load/power control stays implicit. Variant pin remaps and load-dump or thermal-derating behaviour then break integration on every trim. Complements W7-019 (partitions/modes) with the body I/O view.
- **Who:** Body architects, BCM developers, E/E integrators, power-distribution engineers.
- **Root cause:** Consolidation decided as wiring-cost optimisation without a canonical I/O abstraction model or explicit load/power contract per output.
- **Contributing factors:** Trim-driven pin reassignment; smart-FET diagnostics handled per-driver; mixed LIN-direct and zone-routed loads.
- **Technical limitations:** Inrush/thermal/load-dump envelopes per channel; battery-voltage sag coupling across loads; abstraction leaks at diagnostics and short-circuit handling.
- **Tool limitations:** Pin-mapping tables and BCM configurators check single-build consistency, not cross-trim I/O equivalence or load-budget stacking.
- **Process limitations:** No I/O-abstraction owner; load scheduling reviewed only after field fuse or brownout complaints.
- **Frequency/severity:** High on consolidated BCM/zonal programs; severity QM–ASIL-B, up to ASIL-B where exterior lighting or power modes are involved.
- **Time/cost impact:** Late pin-remap rework; harness and BCM-spin churn; HIL re-validation per trim.
- **Downstream effects:** Wrong-load actuation, nuisance fuse trips, voltage-sag resets in neighbouring ECUs.
- **Existing solutions:** AUTOSAR IoHwAb, smart-FET drivers with diagnostics, load-shedding tables, PREEvision wiring mapping.
- **Why insufficient:** Abstraction covers normal drive, not diagnostic, derating, and load-priority behaviour; load budgets live in spreadsheets.
- **Automation feasibility:** High (I/O-abstraction conformance checks, per-trim pin-equivalence diff, load-budget stacking in CI).
- **AI applicability:** Medium (pin-remap impact analysis, load-conflict detection).
- **Human approval:** Required for load-shedding priority and safety-relevant lighting outputs.
- **Evidence/sources:** W7-001/W7-005 (QM-ASIL); W7-019; W4 variants; G2 survey (body frame).
- **Confidence:** High.
- **Unanswered questions:** What load-budget margin actually covers worst-case simultaneous body loads per trim?
- **Lesson learnt:** BCM programs fail at the pins, not in the application logic — a trim pin remap without an abstraction conformance check is a defect, and load control designed as an afterthought always arrives after the first brownout.
- **AI-ready guideline:** IF body output is remapped or load budget changes THEN require I/O-abstraction conformance check + stacked load analysis + derating test WITH pin-equivalence and load-budget report GATE E/E integrator.

### G5-006 — Body: access/security vs safety and power-state timing

- **Domain:** Body
- **Lifecycle stage:** Design, integration, V&V, homologation
- **Problem:** Keyless access, immobiliser, and remote functions (UN-R155/UN-R116 scope) share ECUs, buses, and power states with safety-relevant body behaviour (locking, lighting, power modes). Security countermeasures (authentication delays, bus-off defences, sleep/wake guards) alter timing of lock, crank-enable, and wake paths. Complements W7-005 (QM-ASIL) with the access/power-state view.
- **Who:** Access-system engineers, security engineers, safety engineers, power-management integrators.
- **Root cause:** Access/security treated as a separate feature thread from safety and power-state timing, with no joint requirement on worst-case lock/unlock and wake-to-actuation latency.
- **Contributing factors:** UWB/BLE ranging jitter; relay-attack countermeasures adding challenge-response rounds; partial-network wake matrices tuned per trim.
- **Technical limitations:** Authentication latency vs passive-entry responsiveness; sleep/wake race windows; degraded-mode locking under low voltage or bus fault.
- **Tool limitations:** Security testing (fuzz, penetration) and safety timing analysis run in separate tools with no combined wake-to-actuation deadline check.
- **Process limitations:** No joint sign-off between security, safety, and power-management owners on shared-state transitions.
- **Frequency/severity:** Medium-High; severity up to ASIL-B for inadvertent lock/unlock or immobiliser no-start in hazardous scenes.
- **Time/cost impact:** Late timing rework after combined security+safety test; homologation findings on theft-resistance vs availability trade-offs.
- **Downstream effects:** Delayed or denied entry/start, unintended unlock, battery drain from wake loops.
- **Existing solutions:** ISO 26262 QM-ASIL coexistence (W7-005), ISO/SAE 21434 TARA, UN-R155 CSMS, partial-network management, secure wake filters.
- **Why insufficient:** Each discipline verifies its own leg; nobody verifies worst-case timing of the secured safety path.
- **Automation feasibility:** High (wake-to-actuation timing harness, combined security-timing regression suite).
- **AI applicability:** Medium (wake-trace anomaly detection, attack-vs-fault classification assist).
- **Human approval:** Required for access/safety trade-off decisions and degraded-mode behaviour.
- **Evidence/sources:** W7-001/W7-005; W7-019; ISO 26262; ISO/SAE 21434 and UN-R155/R156 (context); G2 survey.
- **Confidence:** Medium.
- **Unanswered questions:** What wake-to-lock/start deadline applies under combined fault-plus-attack conditions per OEM policy?
- **Lesson learnt:** Access systems pass security tests and pass safety tests and still fail the customer — the only test that counts is authenticated-request to physical-state change under worst-case sleep, voltage, and bus load.
- **AI-ready guideline:** IF access or power-state logic changes THEN require authenticated wake-to-actuation timing test + degraded-mode lock/start check WITH combined security-timing report GATE safety and security managers.

### G5-007 — Body: variant explosion across trims and personalization data

- **Domain:** Body
- **Lifecycle stage:** Configuration management, production, OTA, service
- **Problem:** Body carries the densest trim matrix (doors, lighting, seats, mirrors, trailer, market options) plus user personalization (seat/mirror/light profiles, phone-as-key bindings). Variant coding and personalization restore after update or ECU replacement mismatch, producing dead features or wrong-market behaviour. Links W4 variants and G3 OTA with the body data view.
- **Who:** Variant managers, BCM calibrators, OTA/release engineers, dealers and service.
- **Root cause:** Variant and personalization data modelled as coding bytes and backup blobs rather than as a versioned configuration contract bound to VIN, hardware, and software.
- **Contributing factors:** Hundreds of sales-code combinations; supplier-coded vs factory-coded vs OTA-coded splits; personalization synced via phone/cloud with stale caches.
- **Technical limitations:** Coding space exhaustion; backward restore of profiles onto newer software; end-of-line vs service-tool vs OTA coding divergence.
- **Tool limitations:** Variant-management tooling (W4) checks buildability, not post-OTA or post-replacement configuration correctness per VIN.
- **Process limitations:** No per-VIN configuration owner; personalization restore untested across version skew.
- **Frequency/severity:** High; severity mostly QM (comfort/availability) with ASIL-B edge cases for lighting and locking defaults.
- **Time/cost impact:** Service-bay rework; OTA campaign exclusions per trim; warranty cost on "feature lost after update" complaints.
- **Downstream effects:** Wrong lighting/market coding, lost user profiles, dealer re-flash loops.
- **Existing solutions:** Variant matrices, end-of-line coding, UCM/package attributes, cloud profile backup (G3).
- **Why insufficient:** Coding verified at production, not across OTA version skew or ECU-replacement restore paths.
- **Automation feasibility:** High (per-VIN configuration digest checks, restore-across-versions test matrix).
- **AI applicability:** High as checker (variant-inconsistency finder from coding diffs); low as author.
- **Human approval:** Required for market-relevant coding (lighting, locking) changes.
- **Evidence/sources:** W4 variants; G3-001/019 (campaigns, version skew); W7-030; G2 survey.
- **Confidence:** High.
- **Unanswered questions:** What share of post-OTA body complaints trace to variant/personalization restore rather than software defects?
- **Lesson learnt:** Body variants are a configuration problem wearing a software disguise — programs that bind VIN plus hardware plus software plus coding into one checked digest survive trim growth; programs that trust coding bytes drown in them.
- **AI-ready guideline:** IF body software or coding changes THEN require per-VIN configuration digest + restore-across-versions test WITH configuration verification report GATE variant manager.

### G5-008 — Body: cross-domain lesson — the quiet domain that stalls the vehicle

- **Domain:** Body (cross-domain)
- **Lifecycle stage:** All (integration focus)
- **Problem:** Body is graded QM–ASIL-B and therefore deprioritised, yet it owns wake, power distribution, and access — the three preconditions every other domain needs. A body wake storm, load-shed error, or lock/power-mode fault stalls ADAS, powertrain start, and OTA campaigns alike. Generalises the W7-005 QM-ASIL pattern to vehicle level.
- **Who:** Integrators, architects, all domain teams, energy management.
- **Root cause:** Criticality assessed per-function (comfort = QM) instead of per-dependency (everyone depends on body power and wake).
- **Contributing factors:** Wake-matrix growth with each new feature; load control invisible to HPC/ADAS planners; diagnostics that cannot distinguish body-caused vs domain-caused no-start.
- **Technical limitations:** Wake/power coupling is emergent — no single ECU model predicts vehicle-level quiescent current or wake latency.
- **Tool limitations:** Domain HIL rigs stub body as ideal power and wake; vehicle-level wake/power dashboards arrive late.
- **Process limitations:** Body excluded from early cross-domain timing and energy reviews as "non-safety, non-performance".
- **Frequency/severity:** Pervasive; severity inherits the highest dependent ASIL plus fleet-energy and OTA-campaign impact (G4, G3).
- **Time/cost impact:** Vehicle-level debug dominated by wake/drain hunts; launch-gating energy and start-availability issues.
- **Downstream effects:** Dead batteries, failed OTA preconditions, cross-domain no-fault-found loops.
- **Existing solutions:** Wake-matrix reviews, quiescent-current budgets, load-shedding policies, W7-005 FFI guidance.
- **Why insufficient:** Applied per-ECU, not as a vehicle-level wake/power contract with a named owner.
- **Automation feasibility:** High (vehicle wake/power contract monitor, quiescent-current regression in CI/HIL).
- **AI applicability:** Medium (wake-storm root-cause ranking from vehicle traces).
- **Human approval:** Required (chief architect signs wake/power contract).
- **Evidence/sources:** W7-001/W7-005; W7-019; G3 OTA preconditions; G4 energy; G2 integration-as-discovery.
- **Confidence:** High.
- **Unanswered questions:** Which wake/power contract metric best predicts vehicle-level start-availability and drain complaints?
- **Lesson learnt:** Body is the foundation every domain stands on — treat its wake, power, and access behaviour as vehicle-level contracts with owners and monitors, or every other domain debugs body problems under its own name.
- **AI-ready guideline:** IF program integrates any domain depending on wake or switched power THEN maintain vehicle-level wake/power contract with budgets and monitors WITH wake-power verification report GATE chief architect.

### G5-009 — Powertrain: EMS/TCM/aftertreatment coordination and the torque path

- **Domain:** Powertrain
- **Lifecycle stage:** Architecture, design, integration, calibration
- **Problem:** Engine/motor control (EMS/e-drive), transmission (TCM), and aftertreatment (SCR/DPF/GPF, OBD monitors) coordinate through a torque path with arbitration, intervention (ESP, cruise, limits), and thermal/emission derating. Torque arbitration ownership and signal semantics fragment across suppliers, so interventions stack, cancel, or oscillate. Complements W7-003/004 (ASIL-C/D) with the powertrain integration view.
- **Who:** Powertrain architects, EMS/TCM/aftertreatment function owners, calibrators, integrators.
- **Root cause:** Torque structure defined per-controller (each supplier owns its leg) with no single executable arbitration model or signal-semantics contract.
- **Contributing factors:** Hybrid torque blending (ICE plus e-machine); transmission torque requests vs limits; aftertreatment regeneration and component-protection derating.
- **Technical limitations:** Arbitration priority inversion under stacked interventions; torque-to-actuator latency across controllers; mode-dependent semantics of "requested" vs "allowed" torque.
- **Tool limitations:** Per-controller MIL/SIL rigs validate local torque logic; cross-controller arbitration tested only at late powertrain HIL.
- **Process limitations:** No torque-path owner spanning EMS, TCM, and aftertreatment; calibration changes bypass arbitration review.
- **Frequency/severity:** High on every new powertrain variant; severity ASIL-C/D for unintended acceleration or loss of drive.
- **Time/cost impact:** Calibration-iteration loops; drivability rework; HIL/vehicle test repetition per intervention combination.
- **Downstream effects:** Torque bumps, hesitation, regeneration conflicts, safety-monitor trips.
- **Existing solutions:** Standard torque-structure patterns, AUTOSAR SWC partitioning, powertrain HIL, W7-010/029 partitioning and MCAL guidance.
- **Why insufficient:** Patterns exist as documents per supplier; no machine-checked arbitration contract spans all three controllers.
- **Automation feasibility:** High (executable arbitration model, intervention-combination test generation, torque-semantics checks).
- **AI applicability:** Medium (intervention-conflict prediction from calibration diffs and trace mining).
- **Human approval:** Required for torque-arbitration priority and derating strategy.
- **Evidence/sources:** W7-003/W7-004; W7-010/W7-029; ISO 26262; G2 survey (powertrain frame).
- **Confidence:** High.
- **Unanswered questions:** Which intervention combinations remain untested at SOP on current hybrid programs?
- **Lesson learnt:** The torque path is the powertrain — programs that own arbitration as one executable model with one owner calibrate once; programs that federate it across suppliers calibrate forever.
- **AI-ready guideline:** IF torque request, limit, or arbitration logic changes THEN require executable arbitration check + intervention-combination test WITH torque-path verification report GATE powertrain architect.

### G5-010 — Powertrain: ASIL-C/D torque monitoring and OBD interplay

- **Domain:** Powertrain
- **Lifecycle stage:** Safety design, V&V, homologation, service
- **Problem:** ASIL-C/D torque and speed monitoring (multi-level monitor concepts per ISO 26262) coexists with legislated OBD/emission monitors (misfire, aftertreatment efficiency, rationality) on shared sensors, actuators, and fault memory. Safety reactions (torque cut, limp-home) and OBD reactions (MIL, fault storage, inducement) interact — one monitor's reaction masks or triggers the other's, confusing fault isolation and safety evidence. Complements W7-003/004/010 with the monitor-interplay view.
- **Who:** Functional-safety engineers, OBD/emission engineers, diagnosticians, homologation.
- **Root cause:** Safety monitors and OBD monitors specified against different standards (ISO 26262 vs emission/OBD regulations) with no joint fault-reaction matrix.
- **Contributing factors:** Shared crank, pedal, pressure, NOx/PM sensors; limp-home vs emission-inducement conflicts; debouncing and drive-cycle conditions diverging between the two regimes.
- **Technical limitations:** Detection-time vs fault-tolerant-time-interval tension; monitor intrusiveness (active tests) perturbing torque-monitor plausibility; shared-ADC/sensor fault attribution ambiguity.
- **Tool limitations:** Safety FTA/FMEDA tooling and OBD certification tooling do not cross-check reaction conflicts or shared-sensor attribution.
- **Process limitations:** Safety case and emission certification owned by separate teams with separate fault lists and separate sign-off gates.
- **Frequency/severity:** Medium-High; severity ASIL-C/D for monitoring gaps plus type-approval risk for OBD non-compliance.
- **Time/cost impact:** Certification rework; fault-memory redesign; field campaigns where MIL/safety-lamp strategies confuse dealers.
- **Downstream effects:** Misattributed faults, masked monitors, unreadable field failure signatures.
- **Existing solutions:** Multi-level torque monitoring, ISO 26262 confirmation measures, OBD drive-cycle and readiness procedures, joint DTC strategies (immature).
- **Why insufficient:** Joint safety-plus-OBD reaction matrix rarely written, and almost never tested as one suite.
- **Automation feasibility:** High (joint reaction-matrix checks, monitor-interaction test suite, shared-sensor attribution audit).
- **AI applicability:** Medium (monitor-masking detection from fleet DTC plus freeze-frame mining).
- **Human approval:** Required for any monitor threshold, debounce, or reaction change affecting safety or compliance.
- **Evidence/sources:** W7-003/W7-004; W7-010; ISO 26262; emission/OBD regulations (Euro/EOBD/OBD-II context, CARB/EPA where applicable); G2 survey.
- **Confidence:** High.
- **Unanswered questions:** Which shared-sensor faults remain ambiguously attributed between safety and OBD monitors in production calibrations?
- **Lesson learnt:** Two monitors watching one sensor will disagree in the field — write the joint fault-reaction matrix before certification, or safety and OBD teams will debug each other's reactions for the life of the program.
- **AI-ready guideline:** IF any powertrain monitor or shared-sensor path changes THEN require joint safety-OBD reaction matrix + monitor-interaction test WITH monitor-coexistence report GATE safety manager and homologation.

### G5-011 — Powertrain: emission-relevant calibration protection and field updates under regulation

- **Domain:** Powertrain
- **Lifecycle stage:** Calibration, release, OTA, service, in-use compliance
- **Problem:** Large calibration sets (fuel, ignition/injection, boost, transmission, aftertreatment dosing) are emission-relevant and type-approval-bound, yet field updates (UN-R156 software-update governance, anti-tamper expectations) must flow for drivability, durability, and security fixes. Distinguishing emission-relevant from non-relevant changes and proving post-update compliance is manual and fragile. Links G3 OTA and W4 variants with the emission-compliance view.
- **Who:** Calibrators, release managers, homologation, OTA engineers, cybersecurity.
- **Root cause:** Calibration governance stops at per-release type-approval snapshots; no enforced classification of each parameter and map change by emission relevance across the update chain.
- **Contributing factors:** Thousands of labels per variant; fuel/market/aftertreatment-ageing splits; dealer re-flash and tuning-tool tamper pressure.
- **Technical limitations:** Emissions impact is map-interaction-dependent, not per-label; anti-tamper sealing (secure boot, calibration authentication) vs workshop/authenticated-update needs; rollback to non-compliant prior versions.
- **Tool limitations:** Calibration-diff tooling shows value changes, not emission-relevance rulings; RXSWIN/version reporting per UN-R156 often decoupled from calibration baselines.
- **Process limitations:** No emission-relevance gate in the OTA/release pipeline; tamper-evidence review disconnected from update approval.
- **Frequency/severity:** Medium; severity high (type-approval exposure, recall risk, UN-R156 audit findings).
- **Time/cost impact:** Re-certification testing per update; campaign scope restriction; delayed field fixes awaiting compliance clearance.
- **Downstream effects:** Non-compliant field software, blocked security fixes, undetected tampering attributed to OEM updates.
- **Existing solutions:** Calibration labelling and approval workflows, secure boot and calibration authentication, UN-R156 SUMS/RXSWIN, G3 OTA campaign controls.
- **Why insufficient:** Emission-relevance classification and RXSWIN-to-calibration traceability enforced by procedure, not by pipeline gate.
- **Automation feasibility:** High (emission-relevance classification gate, calibration-to-RXSWIN traceability, tamper-evidence check in update pipeline).
- **AI applicability:** Medium (emission-impact risk ranking of calibration diffs for reviewer triage).
- **Human approval:** Required for any emission-relevant calibration change and every field-update compliance statement.
- **Evidence/sources:** W4 variants; G3-001/019; W7-030; UN-R156 and emission/OBD regulations (context); ISO 26262; ISO/SAE 21434.
- **Confidence:** Medium.
- **Unanswered questions:** What classification rule for emission relevance survives type-approval audit across agencies without per-update retest?
- **Lesson learnt:** Every powertrain update is a compliance event until proven otherwise — programs that classify calibration by emission relevance in the pipeline ship field fixes; programs that classify by paperwork re-certify them.
- **AI-ready guideline:** IF powertrain calibration or software changes THEN require emission-relevance classification + RXSWIN traceability + tamper-evidence check WITH compliance release record GATE homologation and release manager.

### G5-012 — Powertrain: cross-domain lesson — torque and calibration as vehicle contracts

- **Domain:** Powertrain (cross-domain)
- **Lifecycle stage:** All (integration focus)
- **Problem:** Chassis, ADAS, energy, and OTA domains all consume or constrain torque (stability intervention, ACC/cruise requests, thermal and high-voltage limits, update-time drive restrictions) and all assume calibration stays fixed beneath them. Torque-request semantics and calibration baselines drift without vehicle-level ownership, so cross-domain interventions fight the powertrain. Generalises G5-009/G5-011 to vehicle level.
- **Who:** Vehicle architects, powertrain and chassis/ADAS integrators, energy management, OTA planning.
- **Root cause:** Torque and calibration treated as powertrain-internal artefacts rather than as vehicle-level contracts consumed by every motion and energy function.
- **Contributing factors:** ADAS longitudinal requests with implicit timing assumptions; battery/thermal derating communicated as text, not as machine-readable envelopes; OTA campaigns assuming a single calibration baseline across a mixed fleet.
- **Technical limitations:** Cross-domain torque latency budgets unowned; derating-envelope versioning absent; calibration-baseline skew across fleet breaks ADAS/energy assumptions.
- **Tool limitations:** Domain SIL/HIL rigs emulate torque consumers with ideal models; no vehicle-level torque-consumer compatibility check.
- **Process limitations:** No vehicle torque-contract or calibration-baseline owner spanning powertrain, chassis, ADAS, and energy.
- **Frequency/severity:** Pervasive on electrified and ADAS-equipped programs; severity up to ASIL-D for conflicting motion requests.
- **Time/cost impact:** Vehicle-level drivability and intervention-tuning loops; OTA campaign holds awaiting cross-domain clearance.
- **Downstream effects:** Intervention oscillation, unexpected derating, ADAS disengagements blamed on perception instead of torque denial.
- **Existing solutions:** Torque-interface specifications, derating-envelope tables, G3 campaign preconditions, W7-023 integration contracts.
- **Why insufficient:** Specifications exist per bilateral agreement, not as one versioned vehicle contract checked in CI.
- **Automation feasibility:** High (vehicle torque-contract dashboard: requesters, latencies, derating envelopes, calibration baselines).
- **AI applicability:** High as checker (cross-domain torque-conflict finder); low as author.
- **Human approval:** Required (vehicle architect signs torque and calibration-baseline contract).
- **Evidence/sources:** W7-003/W7-004; W7-023; G3-001/019; G4 energy; G2 integration-as-discovery.
- **Confidence:** Medium.
- **Unanswered questions:** What vehicle-level torque-latency and derating-envelope format do chassis, ADAS, and energy consumers actually honour?
- **Lesson learnt:** Torque is the one language every motion domain speaks and calibration is the ground they all stand on — publish both as versioned vehicle contracts, or every domain tunes against a powertrain that no longer exists.
- **AI-ready guideline:** IF any torque consumer or derating source changes THEN require vehicle torque-contract recheck + calibration-baseline compatibility WITH torque-contract report GATE vehicle architect.

### G5-013 — AD/ADAS: perception/fusion/planning pipeline and sensor integration

- **Domain:** AD/ADAS
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Camera/radar/lidar streams converge through synchronisation, fusion, and planning on heterogeneous compute (SoC + accelerator + MCU fallback). Sensor calibration drift, timestamp skew, and middleware jitter break fusion consistency; planning inherits stale or misassociated objects with no single pipeline owner.
- **Who:** Perception engineers, fusion architects, system integrators, sensor suppliers.
- **Root cause:** Pipeline designed per-sensor and per-supplier without a unified time/coordinate-frame/calibration contract owned end-to-end.
- **Contributing factors:** Multi-supplier sensors with proprietary calibration; variant sensor sets per trim; accelerator toolchain separate from MCU toolchain.
- **Technical limitations:** Fusion sensitive to timestamp and extrinsic-calibration error; degraded-sensor fallback modes multiply integration states; compute partitioning constrains pipeline placement.
- **Tool limitations:** Sensor models and fusion simulators validate algorithms, not production calibration drift or loaded-network timing.
- **Process limitations:** No pipeline integration owner; sensor acceptance tested per-sensor rather than fused end-to-end.
- **Frequency/severity:** High on every AD/ADAS program; severity up to ASIL-D where planning commands brake/steer.
- **Time/cost impact:** Late fusion retuning; HIL/vehicle-loop rework dominates ADAS integration phase.
- **Downstream effects:** Ghost/missed objects, planning instability, ODD restriction (feeds G5-014).
- **Existing solutions:** Time-sync (gPTP), extrinsic-calibration rigs, sensor-fusion frameworks, HIL with sensor injection.
- **Why insufficient:** Calibration and timing verified at delivery, not monitored continuously in field; fallback coverage incomplete.
- **Automation feasibility:** High (calibration-drift monitors, timestamp-skew checks, pipeline regression suites).
- **AI applicability:** Medium (fusion-anomaly detection from logs; not as fusion-authority).
- **Human approval:** Required for pipeline architecture and fallback strategy.
- **Evidence/sources:** ISO 26262-4/6/8; ISO 21448 (SOTIF); W7-002/W7-014 (multicore/partitioning); W7-007 (timing); G1 silicon (accelerators).
- **Confidence:** High.
- **Unanswered questions:** What field calibration-drift rate forces fusion retune per sensor class?
- **Lesson learnt:** Fused output is only as good as the worst timestamp and the stalest calibration — programs that monitor both in production ship; programs that calibrate once at EOL chase ghosts.
- **AI-ready guideline:** IF pipeline fuses ≥2 sensor modalities THEN require time/coordinate/calibration contract + drift monitor + degraded-mode tests WITH pipeline verification report GATE fusion architect.

### G5-014 — AD/ADAS: SOTIF + ASIL-D motion control, ODD exits, and latency budgets

- **Domain:** AD/ADAS
- **Lifecycle stage:** Safety case, architecture, V&V, validation
- **Problem:** ASIL-D motion control (brake/steer) must meet end-to-end latency budgets while SOTIF hazards (perception insufficiency, ODD exit, foreseeable misuse) escape fault-based analysis. Latency allocation across sense–fuse–plan–actuate is undocumented, and safe ODD-exit/m minimal-risk-manoeuvre behaviour is under-specified.
- **Who:** Safety managers, SOTIF analysts, motion-control engineers, V&V leads.
- **Root cause:** Functional-safety work products (HARA, FTA) and SOTIF work products (triggering-condition analysis, ODD definition) run as parallel tracks without a joint latency/ODD contract.
- **Contributing factors:** ODD described in marketing terms, not machine-checkable bounds; latency measured per-stage, not end-to-end; driver-takeover assumptions unvalidated.
- **Technical limitations:** End-to-end deadline spans sensors, networks, compute, and actuators; worst-case latency under interference unproven; minimal-risk manoeuvre needs independent actuation path.
- **Tool limitations:** Timing analysis tools cover single ECU/schedule; SOTIF scenario tools lack coverage argument linkage.
- **Process limitations:** No joint ISO 26262 + ISO 21448 acceptance owner; ODD-exit testing deferred to road testing.
- **Frequency/severity:** Pervasive in L2+ programs; severity ASIL-D (loss of motion control).
- **Time/cost impact:** Homologation rework; ODD narrowing late in program; extended validation mileage/campaigns.
- **Downstream effects:** Unsafe ODD exits, SOTIF field findings, safety-case gaps at assessment.
- **Existing solutions:** HARA + SOTIF harm models, latency-budget sheets, MRM/misuse analyses, scenario-based testing (W3).
- **Why insufficient:** Budgets live in spreadsheets, not enforced artefacts; ODD-exit coverage unmeasured.
- **Automation feasibility:** High (end-to-end latency tracking in CI/HIL; ODD-boundary test generation).
- **AI applicability:** Medium (scenario-gap mining; latency-anomaly detection).
- **Human approval:** Required (safety manager signs latency budget and ODD-exit case).
- **Evidence/sources:** ISO 26262-3/4; ISO 21448; W7-004 (latency/interference); W7-006/009 (FFI/cascade); W7-014..W7-018 (multicore timing).
- **Confidence:** High.
- **Unanswered questions:** What end-to-end sense-to-actuate budget is demonstrably met under worst-case interference per ODD?
- **Lesson learnt:** SOTIF findings arrive after the ASIL case is signed — programs that budget latency end-to-end and test ODD exits as first-class requirements pass assessment; the rest renegotiate the ODD.
- **AI-ready guideline:** IF function commands motion at ASIL-D THEN require end-to-end latency budget + ODD-exit/MRM tests + joint 26262/21448 argument WITH safety-case annex GATE safety manager.

### G5-015 — AD/ADAS: ML model, map-data, and calibration updates vs homologation

- **Domain:** AD/ADAS
- **Lifecycle stage:** Release, OTA, homologation, service
- **Problem:** Perception/planning ML models, HD-map data, and calibration parameters evolve faster than type-approval baselines. Silent model or map changes alter behaviour without a re-verification trigger; homologation interplay (what change voids approval) is decided ad hoc per campaign.
- **Who:** ML engineers, map-data providers, homologation leads, release managers.
- **Root cause:** No change-classification scheme linking model/map/calibration deltas to re-verification and re-approval obligations.
- **Contributing factors:** Supplier-trained models with opaque deltas; regional map variants; fleet-learning data pipelines bypassing release gates.
- **Technical limitations:** Model equivalence unprovable by diff; map freshness vs stability trade-off; calibration-per-variant explosion.
- **Tool limitations:** Model registries track weights, not behavioural regression coverage; map-version compatibility checks immature.
- **Process limitations:** OTA campaign process (G3) and homologation process unsynchronised; no model/map change board.
- **Frequency/severity:** Medium-High and rising with L2+ fleets; severity up to ASIL-D for behaviour-altering updates.
- **Time/cost impact:** Re-validation campaigns; approval delays; forced update holds across regions.
- **Downstream effects:** Field behaviour drift, approval non-compliance, rollback complexity.
- **Existing solutions:** Model versioning, map-version pinning, OTA campaigns (G3 OTA), regression scenario suites.
- **Why insufficient:** Versioning without behavioural-change gating; homologation impact assessed manually per release.
- **Automation feasibility:** High (change-classification gate, pinned model+map+calibration bill, regression-suite enforcement).
- **AI applicability:** Medium (behavioural-delta summariser over scenario results; not as approval authority).
- **Human approval:** Required (homologation + safety manager approve behaviour-altering updates).
- **Evidence/sources:** ISO 26262-8 (change management); ISO 21448 (SOTIF change); G3 OTA records; W7-024 (variants); W7-004/005.
- **Confidence:** Medium.
- **Unanswered questions:** Which model/map delta classes do regulators treat as re-approval triggers per market?
- **Lesson learnt:** Every fleet-learning program eventually ships a model that drives differently than the approved one — pinning model, map, and calibration as one approved triple keeps updates shippable and approvals intact.
- **AI-ready guideline:** IF ML model, map data, or ADAS calibration changes THEN require change-class + regression evidence + homologation-impact record WITH update approval pack GATE homologation lead.

### G5-016 — AD/ADAS: cross-domain lesson — ADAS turns every shared resource into a safety case

- **Domain:** AD/ADAS (cross-domain)
- **Lifecycle stage:** All (integration focus)
- **Problem:** AD/ADAS consumes shared actuators (brake/steer), shared networks, and shared compute, so its safety case depends on domains it does not own. Arbitration with driver input, ESP/brake priority, power-state behaviour, and network interference are discovered at vehicle integration.
- **Who:** ADAS integrators, chassis/powertrain owners, E/E architects, safety managers.
- **Root cause:** ADAS specified as a standalone function while its hazards realise only through cross-domain arbitration and shared-resource interference.
- **Contributing factors:** Actuator priority owned by chassis suppliers; network/compute budgets owned elsewhere; W7-006..W7-009 interference classes unmapped to ADAS hazards.
- **Technical limitations:** Arbitration logic spans ECUs and ASILs; interference worst cases untestable per-domain.
- **Tool limitations:** No cross-domain arbitration test harness standard; interference-inclusive HIL rare.
- **Process limitations:** Cross-domain arbitration reviews held late; FFI evidence (W7-006) not demanded of ADAS suppliers.
- **Frequency/severity:** Pervasive; severity inherits ASIL-D motion path.
- **Time/cost impact:** Vehicle-phase arbitration rework; safety-case rewrites pulling in three domains.
- **Downstream effects:** Actuator conflicts, missed deadlines under load, assessment findings on dependent failures.
- **Existing solutions:** Arbitration requirements, FFI analyses (W7-006/009), joint HIL campaigns.
- **Why insufficient:** Analyses exist per-domain; joint evidence assembled at assessment, not in CI.
- **Automation feasibility:** High (arbitration-matrix tests, cross-domain interference suites).
- **AI applicability:** Medium as cross-domain gap finder; low as arbitration author.
- **Human approval:** Required (safety manager + chassis owner sign arbitration).
- **Evidence/sources:** W7-004/006/009; W7-014..W7-018; W2-009 (hypervisor/separation); G1 silicon.
- **Confidence:** High.
- **Unanswered questions:** What arbitration-plus-interference test coverage do assessors accept as dependent-failure evidence?
- **Lesson learnt:** ADAS programs fail at the actuator, not the algorithm — whoever owns arbitration and interference evidence owns the launch date.
- **AI-ready guideline:** IF ADAS commands shared actuators or compute THEN require arbitration matrix + interference-inclusive tests + joint FFI evidence WITH cross-domain safety annex GATE safety manager.

### G5-017 — IVI/Infotainment: head-unit SoC with audio/nav stacks beside safety

- **Domain:** IVI/Infotainment
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** Head-unit SoCs run Android/Linux audio, navigation, connectivity, and app frameworks on multi-core application processors while sharing the board, power, and sometimes the die with safety-relevant display or audio (chimes, alerts). Resource contention and mixed-lifecycle stacks destabilise boot, audio routing, and update behaviour.
- **Who:** IVI architects, BSP/integration engineers, audio/nav suppliers.
- **Root cause:** Consumer-electronics stack cadence bolted onto automotive lifecycle without a separation and resource-ownership model.
- **Contributing factors:** Android version churn; third-party codecs/apps; shared audio DSP paths; navigation map-data coupling.
- **Technical limitations:** Best-effort scheduling vs deterministic audio; memory/bandwidth contention; boot-order dependencies across safety and non-safety partitions.
- **Tool limitations:** Android/Linux profiling tools diagnose lab builds, not production multi-guest contention (cf. W2-009).
- **Process limitations:** No SoC resource owner; app-level defects triaged as vehicle defects and vice versa.
- **Frequency/severity:** High on every IVI program; severity QM mostly, up to ASIL-B where chime/telltale paths shared (see G5-018).
- **Time/cost impact:** Integration churn per Android/security patch; boot/audio defect tail into SOP.
- **Downstream effects:** Boot failures, audio dropouts, nav-data mismatch, OTA coupling (feeds G5-019).
- **Existing solutions:** Hypervisor/containers (W2-009), audio-policy managers, staged boot architectures.
- **Why insufficient:** Separation configured but resource budgets unenforced; patch cadence breaks integration baselines.
- **Automation feasibility:** High (resource-budget monitors, boot/audio regression suites, patch-impact CI).
- **AI applicability:** Medium (log-based audio/boot anomaly triage).
- **Human approval:** Required for SoC partitioning and shared-resource budgets.
- **Evidence/sources:** W2-009 (hypervisor); W7-005/006 (QM-ASIL/FFI); G1 silicon (SoC); G3 OTA.
- **Confidence:** Medium.
- **Unanswered questions:** What memory/bandwidth reservation keeps safety-adjacent audio deterministic under peak IVI load?
- **Lesson learnt:** Head units behave like phones until the vehicle treats them like ECUs — programs that budget SoC resources and gate Android patches through vehicle CI stabilise; the rest debug audio for a year.
- **AI-ready guideline:** IF IVI shares SoC with safety-adjacent functions THEN require partition + resource budgets + boot/audio regression suite WITH SoC integration report GATE E/E architect.

### G5-018 — IVI/Infotainment: QM–ASIL coexistence and telltale/display integrity

- **Domain:** IVI/Infotainment
- **Lifecycle stage:** Safety case, design, V&V
- **Problem:** QM infotainment shares SoC, memory, GPU/display pipeline, or speakers with ASIL-relevant telltales, warnings, and chimes. Freedom-from-interference (memory, timing, execution) is asserted by architecture slide rather than demonstrated, and telltale rendering integrity (correct lamp, in time) lacks end-to-end evidence.
- **Who:** Safety engineers, display/audio integrators, SoC suppliers.
- **Root cause:** Coexistence argued from separation-mechanism datasheets instead of per-interference-class FFI evidence tied to telltale requirements.
- **Contributing factors:** Shared GPU/compositor; shared audio path for chimes; Android-side faults (OOM, watchdog) propagating via shared power/clock.
- **Technical limitations:** GPU/compositor timing unbounded under load; single display failure blanks safety and non-safety alike; chime prioritisation across mixed stacks fragile.
- **Tool limitations:** FFI testing for GPU/display/audio paths immature; fault-injection coverage ad hoc.
- **Process limitations:** Telltale ownership split between cluster/cockpit and IVI teams; FFI work products deferred.
- **Frequency/severity:** Medium; severity up to ASIL-B/C where telltales/chimes are the safety mechanism (cf. W7-005).
- **Time/cost impact:** Safety-case rework; display-pipeline redesign late; assessment findings.
- **Downstream effects:** Missed or delayed telltales/chimes; common-cause display loss.
- **Existing solutions:** Hypervisor separation (W2-009), independent telltale paths, chime-priority schemes, FFI analyses (W7-006).
- **Why insufficient:** Separation present but interference testing absent; telltale timing unmeasured end-to-end.
- **Automation feasibility:** High (FFI fault-injection suites, telltale timing monitors, compositor-load tests).
- **AI applicability:** Low-Medium (timing-anomaly detection only).
- **Human approval:** Required (safety manager signs FFI and telltale evidence).
- **Evidence/sources:** ISO 26262-4/6 (FFI); W7-005/006/009; W2-009; cockpit display linkage (G5 cockpit batch).
- **Confidence:** High.
- **Unanswered questions:** What display-pipeline interference coverage do assessors accept for shared-SoC telltales?
- **Lesson learnt:** Shared displays fail shared — independence must be demonstrated per interference class, and the telltale deadline measured on the glass, not assumed from the GPU datasheet.
- **AI-ready guideline:** IF QM shares SoC/pipeline with telltales or chimes THEN require per-class FFI evidence + telltale end-to-end timing tests WITH FFI report GATE safety manager.

### G5-019 — IVI/Infotainment: app-store cadence vs vehicle release discipline

- **Domain:** IVI/Infotainment
- **Lifecycle stage:** Release, OTA, service
- **Problem:** App-store and Android/security-patch cadence (weeks) collides with vehicle release discipline (months, homologation-bound). Uncoordinated app, OS, map, and codec updates break compatibility, invalidate prior verification, and force emergency full-image flashes.
- **Who:** Release managers, OTA engineers, app-store operators, homologation.
- **Root cause:** Two release trains (consumer and vehicle) share one target with no compatibility and re-verification policy binding them.
- **Contributing factors:** Third-party app churn; partial-update penetration; regional content variants; rollback paths untested per-combination.
- **Technical limitations:** Combinatorial app×OS×map versions; rollback must restore consistent triples; offline/deferred updates fragment the fleet.
- **Tool limitations:** Package managers (UCM-style) install updates but do not prove N-version compatibility or regression coverage.
- **Process limitations:** No update-class policy; app releases bypass vehicle change control.
- **Frequency/severity:** High; severity QM mostly, rising where updates touch shared safety paths.
- **Time/cost impact:** Campaign rework; dealer flashes; app-rollback incidents.
- **Downstream effects:** Broken apps/audio/nav after update; fleet fragmentation; OTA campaign holds.
- **Existing solutions:** Staged rollouts, compatibility matrices, OTA campaigns (G3-001/019), containerised apps.
- **Why insufficient:** Matrices maintained manually; app releases uncoupled from vehicle verification gates.
- **Automation feasibility:** High (compatibility-matrix CI, update-class gating, rollback tests per supported combination).
- **AI applicability:** Medium (compatibility-risk prediction from change metadata).
- **Human approval:** Required for OS/platform updates (release manager); app-only updates under delegated policy.
- **Evidence/sources:** G3 OTA records; W2-016 (update); W7-024 (variants); G5-003 (versioning precedent).
- **Confidence:** Medium.
- **Unanswered questions:** What app×OS×map combination coverage is economical yet sufficient for campaign sign-off?
- **Lesson learnt:** App stores ship weekly and vehicles homologate yearly — programs that classify updates and gate platform changes separately keep both trains moving; the rest flash the fleet to fix the store.
- **AI-ready guideline:** IF IVI update arrives outside vehicle release train THEN require update-class + compatibility matrix + rollback test WITH campaign approval pack GATE release manager.

### G5-020 — IVI/Infotainment: cross-domain lesson — QM exuberance must not sink ASIL neighbours

- **Domain:** IVI/Infotainment (cross-domain)
- **Lifecycle stage:** All (integration focus)
- **Problem:** The most innovative QM domain shares power, network, SoC, and OTA rails with safety neighbours. IVI-driven load spikes, reboot/update behaviour, and bus chatter become other domains' interference, diagnostics, and energy faults — IVI problems surface as cockpit, zonal, or power-management incidents.
- **Who:** E/E architects, integrators, safety and energy engineers, OTA leads.
- **Root cause:** IVI optimised for feature velocity while its shared-footprint obligations (power states, bus load, update atomicity, diagnostic discipline) stay implicit.
- **Contributing factors:** Reboots during drive cycles; streaming-induced bus/power load; OTA windows colliding with safety updates; diagnostic flooding from app faults.
- **Technical limitations:** Power/network budgets span domains; IVI fault containment stops at the app, not the vehicle.
- **Tool limitations:** No shared-footprint dashboard (bus load + power + diagnostic rate) attributed per domain.
- **Process limitations:** IVI milestones decoupled from vehicle integration gates; cross-domain footprint reviews skipped.
- **Frequency/severity:** Pervasive; severity QM locally, up to ASIL-B/C through neighbours.
- **Time/cost impact:** Cross-domain defect reassignment loops; integration-gate failures owned by everyone and fixed by no one.
- **Downstream effects:** Bus overload, power-state faults, OTA contention, neighbour safety-case challenges.
- **Existing solutions:** Domain quotas, power-state coordination, update-window arbitration, integration contracts (G5-004).
- **Why insufficient:** Quotas documented, not monitored or enforced in production builds.
- **Automation feasibility:** High (shared-footprint monitors + quota CI gates).
- **AI applicability:** Medium as footprint-anomaly detector; low as quota setter.
- **Human approval:** Required (E/E architect signs cross-domain footprint budget).
- **Evidence/sources:** W7-005/006/009 (interference classes); G3 OTA; G5-004 (contract precedent).
- **Confidence:** Medium.
- **Unanswered questions:** What per-domain bus/power/diagnostic quota demonstrably contains IVI neighbours' worst case?
- **Lesson learnt:** No safety case is ever failed by IVI features — only by IVI footprints, so the programs that quota and monitor bus, power, and update behaviour per domain stay friends with their neighbours.
- **AI-ready guideline:** IF IVI shares power, network, or OTA rails with safety domains THEN require per-domain footprint quotas + monitoring + joint integration gate WITH footprint report GATE E/E architect.

### G5-021 — Cockpit: cluster/HUD/driver-monitoring consolidation on shared SoC

- **Domain:** Cockpit
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** Instrument cluster, HUD, and driver-monitoring functions consolidate onto one SoC previously split across discrete ECUs. Safety-relevant rendering shares GPU, memory bandwidth, and OS/hypervisor with QM infotainment graphics, so partitioning and resource allocation decide ASIL integrity. Complements W7-002 (ASIL-B implications) with the cockpit integration view.
- **Who:** Cockpit architects, SoC integrators, hypervisor/OS engineers, display suppliers.
- **Root cause:** Consolidation driven by cost and wiring without a matching SoC-level partitioning architecture covering GPU, memory, and guest scheduling.
- **Contributing factors:** Shared GPU/display controller between cluster and IVI; Android/Linux alongside RTOS or hypervisor guests; supplier-split HMI vs platform delivery.
- **Technical limitations:** GPU scheduling contention; shared frame-buffer memory; display-controller single point of failure; hypervisor guest scheduling jitter.
- **Tool limitations:** SoC timing/interference analysis tooling models CPU well but GPU and display-pipeline contention poorly.
- **Process limitations:** No single SoC resource owner; cluster safety requirements allocated late to an IVI-driven platform.
- **Frequency/severity:** High on consolidated-cockpit programs; severity ASIL-B for telltale/display path, higher where driver monitoring feeds ADAS.
- **Time/cost impact:** Late repartitioning; hypervisor retuning; HMI rework when shared-resource faults surface at vehicle integration.
- **Downstream effects:** Display corruption or loss, cross-guest interference findings, safety-case rework (see G5-022).
- **Existing solutions:** Type-1 hypervisors, GPU virtualization or reservation, separate display pipes, ASIL-B certified display drivers.
- **Why insufficient:** Mechanisms exist but are configured per-project without verified partitioning evidence under worst-case graphics load.
- **Automation feasibility:** High (SoC resource-allocation checks, interference test suites, configuration consistency gates).
- **AI applicability:** Low as designer; Medium as checker (allocation-gap and shared-resource finder).
- **Human approval:** Required (system architect signs SoC partitioning concept).
- **Evidence/sources:** W7-002 (ASIL-B); W7-006/W7-007 (FFI memory/timing); W7-019 (partitions); W2-009 (hypervisor, context).
- **Confidence:** High.
- **Unanswered questions:** Which GPU/interconnect partitioning evidence do assessors consistently accept for ASIL-B cluster claims?
- **Lesson learnt:** A consolidated cockpit fails first at the shared GPU and display controller, not the application — reserve and verify the rendering path before styling the HMI.
- **AI-ready guideline:** IF safety-relevant display shares SoC with QM graphics THEN require partitioned GPU/memory/display-pipe allocation + worst-case-load interference evidence WITH SoC partitioning report GATE system architect.

### G5-022 — Cockpit: ASIL-B telltales, display-pipeline integrity, and rendering deadlines

- **Domain:** Cockpit
- **Lifecycle stage:** Design, implementation, V&V
- **Problem:** Mandatory telltales and warnings (e.g. brake, airbag, seat-belt per FMVSS 101 / UN R121 families) must render correctly and within deadline even under graphics or system faults. Display-pipeline faults — corrupt frame buffer, frozen frame, wrong telltale state, missed deadline — are safety failures under ISO 26262. Complements W7-007 (timing interference) with the display-deadline view.
- **Who:** Cluster developers, functional-safety engineers, HMI testers.
- **Root cause:** Telltale requirements treated as HMI styling items rather than safety-related functions with integrity and timing requirements end to end.
- **Contributing factors:** Long pipeline (signal → application → GPU → display controller → panel); overlay/compositing with QM content; fault-detection (CRC, alive-supervision, freeze detection) added late.
- **Technical limitations:** End-to-end rendering deadline must hold under SoC load; single-bit frame-buffer faults can invert telltale meaning; backlight/panel faults outside software scope need system handling.
- **Tool limitations:** HMI test tools verify appearance, not pipeline fault coverage or deadline compliance under fault injection.
- **Process limitations:** Telltale timing/integrity requirements unallocated to pipeline stages; fault-injection testing scoped to application logic only.
- **Frequency/severity:** Medium frequency; severity ASIL-B with regulatory (type-approval) exposure on every program.
- **Time/cost impact:** Late addition of pipeline monitors; re-verification of full telltale set after any graphics-stack change.
- **Downstream effects:** Type-approval risk; field telltale incidents; recall-grade exposure for safety indicators.
- **Existing solutions:** Display-pipeline monitors (frame CRC, telltale read-back, freeze/alive checks), redundant warning paths (chime plus lamp), telltale self-test at start-up.
- **Why insufficient:** Monitors exist as catalogue items but end-to-end deadline and fault-coverage evidence per telltale is rarely completed.
- **Automation feasibility:** High (telltale-to-pipeline trace checks, deadline monitors, automated fault-injection suites).
- **AI applicability:** Medium (telltale-state vs rendered-output mismatch detection from HIL captures).
- **Human approval:** Required for telltale safety verification and type-approval evidence.
- **Evidence/sources:** W7-002 (ASIL-B); W7-007 (timing); ISO 26262-4/-6; FMVSS 101 / UN R121 families (context, no figures cited).
- **Confidence:** High.
- **Unanswered questions:** What rendered-output deadline and fault-detection coverage do assessors expect per telltale class?
- **Lesson learnt:** Telltales pass demo reviews and fail fault injection — test the frozen frame, the corrupt pixel, and the overloaded GPU, because approval and assessment test exactly those.
- **AI-ready guideline:** IF telltale is safety- or regulation-relevant THEN require pipeline-stage allocation + integrity monitor + rendering-deadline evidence + fault-injection results WITH telltale verification report GATE safety manager.

### G5-023 — Cockpit: HMI personalization and variants vs safety-constrained configuration

- **Domain:** Cockpit
- **Lifecycle stage:** Design, calibration, release, OTA
- **Problem:** Cockpit HMI supports personalization (themes, layouts, driver profiles) and market/trim variants while telltales and safety warnings must remain fixed, legible, and correctly prioritized. Unconstrained theming and variant branching can obscure, relocate, or delay safety content. Complements W7-024 (variant/calibration binding) with the HMI-variant view.
- **Who:** HMI designers, variant managers, calibration engineers, safety engineers.
- **Root cause:** Personalization freedom and safety-fixed content governed by the same unconstrained HMI configuration without a safety-constrained variant model.
- **Contributing factors:** Theme/skin engines; driver-profile sync from cloud; market-specific telltale sets; late market additions bypassing HMI review.
- **Technical limitations:** Safe area, contrast, size, and priority rules must hold across every theme and resolution; profile restore must not carry stale safety settings.
- **Tool limitations:** HMI authoring tools preview appearance but do not enforce safety-layout constraints or check all variant combinations.
- **Process limitations:** No safety sign-off gate on theme/variant additions; personalization treated as after-sales content outside release control.
- **Frequency/severity:** Medium; severity ASIL-B where telltale legibility or priority is affected.
- **Time/cost impact:** Variant-explosion testing; late theme rejections; OTA-delivered skins requiring recall-grade review.
- **Downstream effects:** Obscured or delayed warnings; driver confusion; variant-specific type-approval findings.
- **Existing solutions:** HMI style guides with safe areas, telltale priority tables, variant matrices, design-review checklists.
- **Why insufficient:** Guides are documents, not machine-checked constraints; variant coverage is sampled, not exhaustive.
- **Automation feasibility:** High (constraint-checked HMI configuration, variant-combination rendering checks in CI).
- **AI applicability:** Medium (layout-constraint violation and legibility-risk flagging from HIL captures).
- **Human approval:** Required (safety manager approves safety-content layout rules and new themes).
- **Evidence/sources:** W7-002; W4 variant/calibration experience (context); FMVSS 101 / UN R121 telltale requirements (context).
- **Confidence:** Medium.
- **Unanswered questions:** What theme/variant coverage evidence is sufficient to claim telltale legibility across the fleet?
- **Lesson learnt:** Personalization always drifts toward the safety content — fence telltales and warnings with checked layout rules, or the next attractive theme will cover them.
- **AI-ready guideline:** IF HMI theme or variant changes safety-display content THEN require safety-layout constraint check + telltale regression captures WITH HMI variant report GATE safety manager.

### G5-024 — Cockpit: cross-domain lesson — the display is a safety actuator

- **Domain:** Cockpit (cross-domain)
- **Lifecycle stage:** All (integration focus)
- **Problem:** Programs treat the display as output styling while ADAS, body, powertrain, and diagnostics all depend on it to convey safety state to the driver. Display failures therefore convert other domains' correct detections into driver-visible failures. Links W7-002/007 (ASIL-B, timing) to every domain that warns through the cluster or HUD.
- **Who:** System architects, integrators, safety managers across domains.
- **Root cause:** Display owned by HMI/cockpit team while its safety-actuator role for other domains is unallocated and uncontracted.
- **Contributing factors:** Warning requests from multiple domains without priority arbitration; HUD/cluster split ownership; driver-monitoring outputs consumed by ADAS without a shared contract.
- **Technical limitations:** Warning-priority arbitration, rendering deadlines, and degradation behaviour span domains with no single technical owner.
- **Tool limitations:** No cross-domain warning-path dashboard; per-domain tests assume a perfect display.
- **Process limitations:** No end-to-end warning-path owner from detection to driver perception.
- **Frequency/severity:** Pervasive on consolidated E/E; severity inherits the highest warning carried (up to ASIL-D paths using driver alerts).
- **Time/cost impact:** Integration-phase warning-priority disputes; rework when one domain's alert masks another's.
- **Downstream effects:** Masked or delayed driver warnings; SOTIF-relevant misuse where the driver misses degraded-mode cues.
- **Existing solutions:** Warning-priority tables, HMI arbitration concepts, driver-warning E2E tests on selected paths.
- **Why insufficient:** Tables exist but are not enforced as executable arbitration with deadline evidence across all warning sources.
- **Automation feasibility:** High (warning-source registry, arbitration checks, E2E warning-path tests).
- **AI applicability:** Medium (warning-conflict and masking-risk detection across domain inputs).
- **Human approval:** Required (chief safety owner signs warning-arbitration concept).
- **Evidence/sources:** W7-002/W7-007; G5-021/G5-022; AD/ADAS warning-path experience (context).
- **Confidence:** Medium.
- **Unanswered questions:** Which warning-path E2E coverage predicts driver-notification integrity at assessment?
- **Lesson learnt:** Every domain warns through the same glass — arbitrate warnings like actuator commands, with priorities and deadlines, or the display becomes the weakest safety actuator in the vehicle.
- **AI-ready guideline:** IF more than one domain warns through cluster/HUD THEN require warning registry + arbitration rules + E2E deadline evidence WITH warning-path report GATE chief safety owner.

### G5-025 — Zonal: I/O consolidation, power distribution, and network bridging in one controller

- **Domain:** Zonal
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** A zonal controller consolidates body/comfort I/O, smart-power distribution (fuses replaced by semiconductor switches), and network bridging (CAN/LIN/Ethernet) for one vehicle zone. Pin, power, and bandwidth budgets interact: one controller now owns actuation, protection, and gateway timing that used to sit in separate ECUs. Complements W1-016 (zonal challenges) with the software-integration view.
- **Who:** Zonal architects, power-electronics engineers, network designers, BSW integrators.
- **Root cause:** Three previously separate responsibilities (I/O, power, bridging) merged into one software and hardware boundary without a joint budget model.
- **Contributing factors:** High pin counts with mixed signal classes; eFuse current/thermal protection in software; zone bridging as latency contributor; variant-driven pin reassignment.
- **Technical limitations:** Power-stage thermal/current limits bound software actuation duty; bridging latency adds to E2E signal timing; sleep/wake coordination across the zone affects quiescent current.
- **Tool limitations:** Pin/power/network configured in separate tools; cross-budget (current vs thermal vs timing) consistency unchecked.
- **Process limitations:** No single zonal-budget owner; power, I/O, and network teams integrate at HIL instead of at the model.
- **Frequency/severity:** High on every zonal program; severity mixed up to ASIL-D for power-switched safety loads and bridged safety signals.
- **Time/cost impact:** Pin/power rework; harness changes after controller freeze; bridging-timing fixes cascading to domain owners.
- **Downstream effects:** Overload shutdowns, missed bridging deadlines, quiescent-current failures, variant-pin mismatches.
- **Existing solutions:** Zonal reference architectures, eFuse protection stacks, static bridging tables, power-state management concepts.
- **Why insufficient:** Reference concepts assume clean partitioning; per-project pin/power/timing budgets are still reconciled manually.
- **Automation feasibility:** High (joint pin/power/bandwidth budget checks, generated bridging tests, sleep/wake verification).
- **AI applicability:** Medium (budget-conflict and pin-reassignment impact analysis).
- **Human approval:** Required for power-protection and safety-signal bridging configuration.
- **Evidence/sources:** W1-016; W7-006..W7-009 (FFI entry checklist per G5 §1); W7-019..W7-022 (partitions/comms).
- **Confidence:** High.
- **Unanswered questions:** What bridging-latency and protection-coordination budgets hold across temperature and voltage corners?
- **Lesson learnt:** Zonal controllers fail at the budget seams — current, heat, pins, and milliseconds must balance in one model, or integration balances them with harness rework.
- **AI-ready guideline:** IF zonal controller consolidates I/O plus power plus bridging THEN require joint pin/power/timing budget + bridging deadline evidence WITH zonal budget report GATE zonal architect.

### G5-026 — Zonal: mixed-criticality up to ASIL-D on one controller and FFI evidence

- **Domain:** Zonal
- **Lifecycle stage:** Design, implementation, V&V, assessment
- **Problem:** A single zonal controller hosts QM comfort logic beside ASIL-C/D functions (e.g. switched safety loads, gatewayed brake/steer signals, diagnostics that gate safety). Freedom from interference across memory, timing, and execution must be shown per ISO 26262, or the whole controller inherits the highest ASIL. Directly applies W7-006..W7-009 with the zonal-software view.
- **Who:** Safety engineers, OS/scheduler engineers, zonal software integrators.
- **Root cause:** Mixed-criticality placement decided by packaging convenience rather than by an FFI-capable partitioning design with evidence.
- **Contributing factors:** Shared MCU/MPU, shared power-stage drivers, QM diagnostics able to stall safety paths; DMA and interrupt load from comfort I/O.
- **Technical limitations:** Memory partitioning (MPU/MMU), time partitioning (schedule slots, watchdogs), and execution independence (no QM-to-ASIL cascade) must all hold simultaneously.
- **Tool limitations:** FFI analysis tooling covers memory well but timing/execution interference under worst-case I/O storms is measured ad hoc.
- **Process limitations:** FFI claimed by architecture slide instead of by staged evidence (analysis plus test plus review).
- **Frequency/severity:** High; severity up to ASIL-D by construction on safety-carrying zonals.
- **Time/cost impact:** Assessment findings force repartitioning late; re-verification of the full I/O set after any schedule or mapping change.
- **Downstream effects:** ASIL inheritance across the controller; cascade failures from QM to safety; assessment non-conformances.
- **Existing solutions:** AUTOSAR OS-Applications and memory/time partitioning, MPU configuration, alive/deadline supervision, W7-006..009 interference checklists.
- **Why insufficient:** Checklist items are known but per-controller worst-case evidence (I/O storm plus bridging load) is rarely executed and recorded.
- **Automation feasibility:** High (partitioning-configuration checks, interference/stress suites, FFI evidence tracing).
- **AI applicability:** Medium (interference-risk flagging from schedule and mapping artefacts).
- **Human approval:** Required (safety assessor reviews FFI evidence; no AI sign-off).
- **Evidence/sources:** W7-006/W7-007/W7-008/W7-009; W7-019 (partitions); W2-004 (FFI); ISO 26262-6 Ann. D (context).
- **Confidence:** High.
- **Unanswered questions:** What worst-case I/O-plus-bridging load profile do assessors accept as FFI stress evidence per zonal class?
- **Lesson learnt:** Mixed-criticality zonal software passes unit tests and fails the I/O storm — prove independence under the worst combined load, because the assessor will ask for exactly that trace.
- **AI-ready guideline:** IF QM and ASIL-C/D share a zonal controller THEN require MPU/time-partition design + worst-case interference test evidence + cascade analysis WITH FFI report GATE safety assessor.

### G5-027 — Zonal: software packaging, partial updates, and rollback for zonal controllers

- **Domain:** Zonal
- **Lifecycle stage:** Release, OTA, service
- **Problem:** Zonal controllers need independent software packaging: partial updates (one zone or one function without reflashing the vehicle), atomic activation, and rollback when power or bridging software fails. Bootloader, power-latching, and package dependencies make partial update the hardest update class. Applies W6 bootloader truth with the zonal-packaging view.
- **Who:** OTA engineers, bootloader developers, release managers, diagnosticians.
- **Root cause:** Update packaging designed per-ECU while zonal controllers couple power control, I/O, and bridging in one flash and power domain.
- **Contributing factors:** A/B vs single-bank flash constraints; power-latch behaviour during reflash; cross-zone package dependencies; workshop vs OTA update path divergence.
- **Technical limitations:** Atomicity across coupled partitions; fallback when new bridging tables break reachable zones; power-loss-safe resume on a controller that itself switches power.
- **Tool limitations:** Package managers verify single-ECU images, not cross-zone dependency and rollback graphs.
- **Process limitations:** No partial-update compatibility gate; rollback tested for full-vehicle flash, not for single-zone revert under dependency skew.
- **Frequency/severity:** Medium; severity up to ASIL-D where failed bridging or power software strands the vehicle or disables safety loads.
- **Time/cost impact:** Forced full-vehicle campaigns; dealer recovery visits; rollback development under field pressure.
- **Downstream effects:** Bricked zones, dependency-skew faults, OTA campaign stalls, workshop tool divergence.
- **Existing solutions:** UCM-style packaging, A/B partitioning, bootloader fallback images, dependency manifests (cf. W6 bootloader records).
- **Why insufficient:** Fallback images cover boot failure, not semantic incompatibility between a new zone image and old neighbours.
- **Automation feasibility:** High (dependency-graph checks, partial-update/rollback test matrices in CI).
- **AI applicability:** Medium (dependency-skew and rollback-risk prediction from package graphs).
- **Human approval:** Required for zonal update and rollback release decisions.
- **Evidence/sources:** W6 bootloader file (notably W6-017/W6-021 integration, W6-024 V&V); W7-024 (variants); W7-019..W7-022 (partition/mode angle).
- **Confidence:** Medium.
- **Unanswered questions:** What single-zone rollback coverage is sufficient given cross-zone package dependencies?
- **Lesson learnt:** Zonal partial update works until the first cross-zone dependency mismatch — version the dependency graph, not just the image, and test the single-zone revert nobody plans for.
- **AI-ready guideline:** IF zonal software updates independently of neighbours THEN require package dependency graph + partial-update test + single-zone rollback evidence WITH update release report GATE release manager.

### G5-028 — Zonal: cross-domain lesson — the zone is where domains collide

- **Domain:** Zonal (cross-domain)
- **Lifecycle stage:** All (integration focus)
- **Problem:** Body, powertrain, ADAS, and diagnostics all flow through zonal controllers through shared pins, power stages, schedules, and bridges. Every domain's timing, power, and safety assumptions meet — and conflict — inside the zone, yet no domain owns the zone. Links W1-016 with W7-006..009 and G5-025..027.
- **Who:** Vehicle architects, zonal integrators, all domain teams, safety management.
- **Root cause:** Domain teams specify per-function requirements while zonal integration constraints (budgets, interference, update coupling) belong to nobody until HIL or vehicle stage.
- **Contributing factors:** Domain assumption sheets ignoring zone budgets; shared power stages across domains; bridging priorities set per-signal without zone arbitration.
- **Technical limitations:** Zone-level arbitration (power priority, bridge priority, schedule priority) cuts across domain boundaries with no natural single-domain home.
- **Tool limitations:** Domain tools verify domain functions; no zone-level conflict dashboard spans power, timing, safety, and update coupling.
- **Process limitations:** Zonal integrator staffed as wiring coordinator rather than as cross-domain release authority.
- **Frequency/severity:** Pervasive on zonal E/E; severity inherits the highest colliding ASIL (up to D).
- **Time/cost impact:** Zone-level rework charged to domains; program delay concentrates at zonal integration milestones.
- **Downstream effects:** Priority inversions, power-shedding surprises, cross-domain cascade faults, update-campaign coupling.
- **Existing solutions:** Zonal integration roles, zone-level requirement sets, cross-domain timing/power reviews on mature programs.
- **Why insufficient:** Reviews are periodic meetings, not continuously checked zone contracts in CI.
- **Automation feasibility:** High (zone contract dashboard: budgets plus interference plus update coupling in one gate).
- **AI applicability:** High as checker (cross-domain zone-conflict finder); low as arbitrator.
- **Human approval:** Required (vehicle architect signs zone integration contract).
- **Evidence/sources:** W1-016; W7-006..009; G5-025/G5-026/G5-027; G2 integration-as-discovery (context).
- **Confidence:** Medium.
- **Unanswered questions:** What zone-contract check coverage predicts zonal-integration defect escape rate?
- **Lesson learnt:** Domains design functions and zones integrate physics — programs that give the zone a checked contract and a real owner converge; the rest debug power, timing, and updates on the vehicle.
- **AI-ready guideline:** IF multiple domains share a zonal controller THEN maintain machine-checked zone contract (power+timing+safety+update coupling) in CI WITH zone dashboard GATE vehicle architect.

### G5-029 — HPC: central-compute consolidation, virtualization strategy, and SOA deployment

- **Domain:** HPC
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** Consolidating dozens of functions onto central-compute HPC (high-performance SoC + hypervisor + VMs/containers + Adaptive AUTOSAR SOA) creates an allocation problem: which functions share which vCPU/memory/interconnect partition, which hypervisor configuration enforces it, and how SOA services deploy across VMs without breaking timing, safety, or update independence. Consolidation decisions are made per-project without a repeatable virtualization and deployment strategy.
- **Who:** E/E architects, HPC platform engineers, hypervisor integrators, SOA designers.
- **Root cause:** Consolidation treated as hardware sizing plus VM packing rather than as a joint compute/safety/communication/update architecture with explicit allocation contracts.
- **Contributing factors:** Supplier-provided VMs with conflicting OS/hypervisor assumptions; Adaptive applications with undeclared resource demands; shared Ethernet/interconnect with zonal gateways.
- **Technical limitations:** vCPU scheduling jitter; shared cache/memory-bandwidth contention; service-discovery chatter under load; VM image coupling that defeats independent deployment.
- **Tool limitations:** Hypervisor configuration and Adaptive manifest tooling check single-VM consistency, not fleet-wide allocation feasibility or end-to-end SOA timing.
- **Process limitations:** No consolidation owner; VM-to-function allocation decided late after supplier delivery.
- **Frequency/severity:** High on every central-compute program; severity inherits highest consolidated ASIL (up to ASIL-D).
- **Time/cost impact:** Late re-partitioning; VM image rebuild cascades; SOA redeployment rework.
- **Downstream effects:** Integration-phase discovery of resource exhaustion; timing violations; blocked independent updates.
- **Existing solutions:** Type-1 hypervisors, Adaptive AUTOSAR (ara::com), static VM allocation tables, service orchestration.
- **Why insufficient:** Allocation tables are documents, not machine-checked contracts tied to timing, FFI, and update evidence.
- **Automation feasibility:** High (allocation feasibility checks, manifest-to-hypervisor consistency checks, deployment-dependency graphs).
- **AI applicability:** Medium (allocation-anomaly and dependency analysis); low as allocator.
- **Human approval:** Required (E/E architect signs consolidation and deployment strategy).
- **Evidence/sources:** W7-006..W7-009; W7-019..W7-022; W2-009/W2-016/W2-018 (Adaptive); G1 silicon; G5-001/G5-004 (middleware contract).
- **Confidence:** High.
- **Unanswered questions:** What allocation-check coverage predicts consolidation rework on HPC programs?
- **Lesson learnt:** HPC consolidation that is not written down as a checked allocation contract gets renegotiated at every integration build — and the hypervisor always loses to the loudest supplier.
- **AI-ready guideline:** IF functions consolidate onto HPC THEN require VM/vCPU/memory/interconnect allocation contract + SOA deployment map WITH allocation verification report GATE E/E architect.

### G5-030 — HPC: mixed-criticality up to ASIL-D on shared SoC, hypervisor qualification, and interference evidence

- **Domain:** HPC
- **Lifecycle stage:** Design, implementation, V&V, safety assurance
- **Problem:** QM/ASIL-A/B infotainment and data-driven functions share the HPC SoC with ASIL-B/C/D functions (e.g. telltales, motion-relevant planning outputs). Freedom from interference (FFI) across shared cache, memory, interconnect, GPU/accelerator, and time must be evidenced per ISO 26262, and the hypervisor/separation kernel must carry qualification commensurate with the highest hosted ASIL — both are frequently incomplete at assessment.
- **Who:** Safety engineers, hypervisor vendors, SoC integrators, assessors.
- **Root cause:** Mixed-criticality assumed from hypervisor presence rather than demonstrated through partitioning requirements, interference analysis, and shared-resource testing on the production SoC and configuration.
- **Contributing factors:** SoC safety-manual assumptions violated by actual VM load; accelerators without deterministic partitioning; QM guests with unbounded network/I/O behaviour.
- **Technical limitations:** Cache/memory-bandwidth/interconnect interference under worst-case load; accelerator pre-emption gaps; dependent-failure coupling through shared clocks, power, and firmware.
- **Tool limitations:** Lab FFI figures do not transfer to loaded production configurations; interference-measurement tooling is vendor-specific and rarely continuous in CI.
- **Process limitations:** Hypervisor qualification scope mismatched to use (wrong version, configuration, or use-case coverage); FFI evidence owned by no single party.
- **Frequency/severity:** Medium; severity ASIL-D where shared SoC hosts safety-relevant functions.
- **Time/cost impact:** Assessment findings force late re-partitioning or de-scoping of consolidation; re-qualification of hypervisor configuration.
- **Downstream effects:** Safety-case gaps; cascade-failure exposure; launch or SOP risk.
- **Existing solutions:** ISO 26262 FFI measures, hypervisor qualification kits, SoC safety manuals, interference test suites.
- **Why insufficient:** Evidence produced once on reference loads instead of on production VM mix with worst-case QM neighbours.
- **Automation feasibility:** High (interference regression tests on production configuration in CI; qualification-scope traceability).
- **AI applicability:** Medium (interference-anomaly detection from resource traces); low as safety argument.
- **Human approval:** Required for all FFI claims and hypervisor qualification (safety manager, assessor).
- **Evidence/sources:** W7-006..W7-009 (FFI/interference); W7-014..W7-020 (multicore/partition); ISO 26262; SoC safety manuals (context); W2-004.
- **Confidence:** High.
- **Unanswered questions:** Which interference-evidence patterns actually pass independent assessment per hypervisor class?
- **Lesson learnt:** A hypervisor nobody re-qualified on the production VM mix is a hope, not a safety mechanism; test FFI with the noisiest QM neighbour running.
- **AI-ready guideline:** IF QM and ASIL functions share HPC SoC THEN require partition requirements + interference test evidence on production VM mix + hypervisor qualification scoped to use WITH FFI assessment report GATE safety manager.

### G5-031 — HPC: container/VM image updates, atomic fleet rollout, and data-driven logging pipelines

- **Domain:** HPC
- **Lifecycle stage:** Release, OTA, service, operations
- **Problem:** HPC updates ship as VM/container images plus Adaptive application bundles with large size and cross-image dependencies, while data-driven features demand continuous logging and parameter or model refresh. Partial downloads, incompatible image combinations, and unthrottled logging pipelines cause failed campaigns, bricked update states, or network/storage exhaustion across the fleet.
- **Who:** OTA engineers, release managers, data-pipeline engineers, fleet operations.
- **Root cause:** Image-level updating and data-loop needs designed separately — no joint atomicity, compatibility, and bandwidth/storage budget covering images, services, parameters, and logs.
- **Contributing factors:** Mixed-version fleet (cf. G5-003); large image sizes over constrained links; logging left at debug verbosity; supplier image cadence mismatch.
- **Technical limitations:** Atomic A/B slot switching at multi-GB scale; cross-VM version compatibility; log backpressure and on-device retention limits; consent and privacy constraints on collected data.
- **Tool limitations:** UCM/package tooling validates single-vehicle install, not fleet-wide campaign atomicity, rollback coverage, or logging-pipeline load.
- **Process limitations:** No joint release gate covering images + service versions + data artefacts; logging budgets unowned.
- **Frequency/severity:** Medium-High on connected HPC fleets; severity up to ASIL-relevant where updates touch safety functions.
- **Time/cost impact:** Failed or stalled campaigns; forced full-image re-push; storage and backend cost overrun.
- **Downstream effects:** Mixed-version fleet drift; rollback complexity; unusable or non-compliant field data.
- **Existing solutions:** UCM, A/B partitioning, delta updates, consent-gated logging pipelines, fleet campaign dashboards.
- **Why insufficient:** Campaign tooling tracks delivery, not cross-image compatibility plus logging-load feasibility plus rollback proof.
- **Automation feasibility:** High (image-compatibility matrices, atomicity/rollback tests, logging-budget enforcement in CI and campaign gates).
- **AI applicability:** Medium (campaign-risk prediction, log-volume anomaly detection); low as release authority.
- **Human approval:** Required for safety-related image releases and fleet-wide rollout.
- **Evidence/sources:** W2-016/W2-018 (Adaptive/UCM); G3-001/G3-018/G3-019 (OTA/cloud); G5-003 (versioning); W7-030.
- **Confidence:** Medium.
- **Unanswered questions:** What image-size and logging-volume budgets sustain atomic fleet rollout on production networks?
- **Lesson learnt:** Data-driven HPC fleets fail updates twice — once on image compatibility and once on their own logging traffic — so budget the downlink and the uplink together or the campaign stalls on both.
- **AI-ready guideline:** IF HPC ships VM/container image update or data-driven feature THEN require cross-image compatibility matrix + atomicity/rollback test + logging bandwidth/storage budget WITH campaign release report GATE release manager.

### G5-032 — HPC: cross-domain lesson — HPC as the convergence point

- **Domain:** HPC (cross-domain)
- **Lifecycle stage:** All (integration and operations focus)
- **Problem:** Every domain problem converges on HPC: middleware timing and versioning (G5-001/G5-003), zonal consolidation load, AD compute interference, cockpit display deadlines, and fleet update pressure all share the same SoC, hypervisor, network, and release train. Optimising any single domain in isolation moves the failure to a neighbour on the same HPC.
- **Who:** Chief architects, HPC platform owners, safety managers, release managers.
- **Root cause:** Domain teams own functions while nobody owns the converged resource — compute, interference, timing, version, and update budgets fragmented across Middleware, Zonal, AD, Cockpit, and OTA workstreams.
- **Contributing factors:** Supplier VM boundaries; per-domain CI without converged HPC CI; safety evidence per-function instead of per-configuration.
- **Technical limitations:** Converged failure modes (interference + timing + version + update) have no single-domain test that reproduces them.
- **Tool limitations:** No unified HPC dashboard combining allocation, FFI, timing, compatibility, and campaign health.
- **Process limitations:** HPC platform role staffed as infrastructure support rather than as convergence authority with gate power.
- **Frequency/severity:** Pervasive on central-compute architectures; severity inherits highest converged ASIL (up to ASIL-D).
- **Time/cost impact:** Cross-domain rework dominates HPC programs; single-domain fixes recycle as neighbour failures.
- **Downstream effects:** Launch slip; safety-case fragility; fleet-wide update constraints.
- **Existing solutions:** HPC platform teams, converged HIL, cross-domain integration builds.
- **Why insufficient:** Builds integrate software without machine-checked converged budgets and gates.
- **Automation feasibility:** High (converged HPC dashboard: allocation + FFI + timing + version + campaign in one CI/release gate).
- **AI applicability:** High as checker (cross-domain inconsistency and interference-risk finder); low as architect.
- **Human approval:** Required (chief architect and safety manager sign converged HPC release).
- **Evidence/sources:** W7-006..W7-009/W7-014..W7-022; W2-009/W2-016/W2-018; G1 silicon; G3 cloud/OTA; G5-001..G5-004.
- **Confidence:** High.
- **Unanswered questions:** What converged-dashboard coverage best predicts HPC program integration success?
- **Lesson learnt:** HPC is where every domain's margin goes to die — programs that govern the converged SoC, hypervisor, timing, version, and update budgets as one contract ship; programs that govern them per-domain debug each other.
- **AI-ready guideline:** IF program converges ≥2 domains onto HPC THEN maintain converged budget dashboard (compute+FFI+timing+version+update) in CI and release WITH convergence report GATE chief architect.

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
