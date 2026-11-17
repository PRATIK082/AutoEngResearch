# Overlap Analysis — W7 (ASIL/Mixed-Criticality/Multicore) & G5 (Domain Deep-Dive) vs. existing workstreams

**Purpose:** Phase D4 deliverable. Maps each W7 (W7-001..W7-030) and G5 (G5-001..G5-032) problem record to existing W1–W6 and G1–G4 records to identify duplicates, partial overlaps and related problems, with a recommended action for each.

**Owner:** Review agent (review mode)
**Created:** 2026-11-16
**Status:** Complete

---

## Legend

- **Duplicate-ish** — same underlying problem recorded under a different ID; consider merging evidence or designating one canonical record.
- **Partial** — W7/G5 covers a facet of a broader existing problem (or vice versa).
- **Related** — adjacent problem; cross-reference only.
- **New** — no meaningful overlap; W7/G5 adds genuine new coverage.

## Mapping — W7 (ASIL / Mixed-Criticality / Multicore)

| W7 ID | Overlaps with | Type | Recommended action |
|---|---|---|---|
| W7-001 | W2-018 (ASIL-B gap), W1-014 (compliance overhead), W4-011 (ISO 26262 documentation overhead) | Partial | Keep W7-001 canonical for the ASIL-A implication delta; cross-ref W2-018/W1-014/W4-011 (compliance family). |
| W7-002 | W2-018 (Adaptive targets ASIL-B — canonical ASIL-B gap), W1-016 (zonal challenges) | Partial | Keep W7-002 for the B-vs-A/C boundary evidence; cross-ref W2-018 (nearest canonical). |
| W7-003 | W2-018, G4-024 (ASIL of charging/BMS software) | Related | Keep; ASIL-C systematic-fault/independence/HSI facet is W7-specific. |
| W7-004 | W6-024 (bootloader V&V/ISO 26262 evidence), G2-027 (mixed criticality/FFI), G4-024 | Partial | Keep W7-004 (ASIL-D rigour planning); cross-ref W6-024 (shared V&V/assessment evidence practice). |
| W7-005 | G2-027, W2-018; W6-009/W6-024 (recovery, QM evidence practice) | Related | Keep; QM-obligation framing is new. Cross-ref G2-027/W6-009/W6-024. |
| W7-006 | W2-004 (FFI on Adaptive — canonical FFI record), G2-027, G1-016 (safety island), G1-024 (mixed-criticality consolidation HW) | Partial | Keep W7-006 for memory-interference evidence specifics; cross-ref W2-004 (nearest canonical). |
| W7-007 | W1-010 (timing unsolved), W2-006 (multicore distribution), W2-013 (scheduling), W3-005 (temporal sync) | Partial | Keep W7-007 (timing-interference evidence); cross-ref timing family W1-010/W2-006/W2-013. |
| W7-008 | W2-004 (FFI), W3-033 (fault injection) | Partial | Keep; execution/control-flow interference facet is W7-specific. Cross-ref W2-004. |
| W7-009 | G2-027, W2-004 (common-cause coupling as FFI facet); W5-024 (SDV/central-compute skill gap) | Partial | Keep W7-009 (shared-library/common-cause audit); cross-ref G2-027/W2-004 and W5-024 (consolidation skill link). |
| W7-010 | W2-012 (RTE contract), G2-027, G2-032 (cross-domain actuator conflicts) | Partial | Keep W7-010 (partitioning discipline + interface ownership); cross-ref W2-012. |
| W7-011 | W2-013 (Classic OS scheduling), W2-006, W1-010 | Partial | Keep; ASIL-constrained runnable-to-task mapping is W7-specific. Cross-ref scheduling family. |
| W7-012 | W2-012 (RTE contract), W1-007 (interface governance); W6-024 (V&V evidence practice) | Partial | Keep W7-012 (atomicity/consistency protocol + gateway timing); cross-ref W2-012/W1-007. |
| W7-013 | G2-027 (partitioning/FFI), W2-012, W2-002 (interop) | Partial | Keep; E2E profile selection + coverage argument is W7-specific. Cross-ref G2-027. |
| W7-014 | W2-006 (canonical multicore distribution), G1-024, G2-022 (HPC resource allocation) | Partial | Keep W7-014 for ASIL static-allocation + re-analysis specifics; cross-ref W2-006. |
| W7-015 | W1-010, W2-006/W2-013, G2-002 (ADAS scheduling), G2-027 | Partial | Keep; consolidated-SoC scheduling-analysis gap is W7-specific. Cross-ref timing/scheduling family. |
| W7-016 | W2-006, W2-013, G1-017 (memory wall) | Related | Keep; spinlock ordering/deadlock/shared-memory protocol is W7-specific. |
| W7-017 | W1-010, W3-004 (SiL/vECU fidelity), G1-032 (HIL HW dependencies) | Partial | Keep W7-017 (measurement-vs-analysis contention/cache argument); cross-ref W1-010/W3-004. |
| W7-018 | W2-009 (hypervisor/consolidation), G2-027, G1-016 | Partial | Keep; OS-Application trust/restart/containment tests are W7-specific. Cross-ref W2-009. |
| W7-019 | W2-009 (canonical hypervisor record), G2-019 (gateway/central computing), W5-024 | Partial | Keep W7-019 for ASIL-guest qualification/SEooC specifics; cross-ref W2-009 (nearest canonical) and W5-024. |
| W7-020 | W2-002 (interop/gateway), G2-020 (multi-bus gateways), W2-009 | Partial | Keep; gateway-as-FFI-boundary + covert-channel review is W7-specific. |
| W7-021 | G2-027, G2-021 (zonal migration); W6-016 (diagnostic session transitions) | Partial | Keep; partition mode-contract + assumption log is W7-specific. Cross-ref G2-027. |
| W7-022 | W2-012 (canonical RTE contract), W6-021 (AUTOSAR/BSW integration); W6-011 (calibration binding surfaces in chain) | Partial | Keep W7-022 (generation-mismatch chain view); cross-ref W2-012/W6-021. |
| W7-023 | W2-002 (canonical interop/gateway), G2-020; intra-wave G5-001 (gateway integration view) | Partial | Keep W7-023 (ASIL atomicity + gateway timing budget); cross-ref W2-002 and G5-001. |
| W7-024 | W1-011 (variant explosion — canonical), W4-005 (config diff/variants), W6-011 (calibration/flash binding) | Partial | Keep W7-024 for post-build variant control in mixed-criticality builds; cross-ref W1-011/W4-005/W6-011. |
| W7-025 | W3-031 (shift-left), W3-033 (fault injection), W3-004; W5-009 (testing expertise) | Partial | Keep; cross-ASIL coverage + fault-injection argument is W7-specific. Cross-ref W3 family. |
| W7-026 | W2-005 (BSW toolchain), W6-021 | Partial | Keep; mixed-ASIL BSW stacking/scheduler ownership is W7-specific. |
| W7-027 | W4-002 (ARXML fragmentation), W4-005, W2-005; W6-021 | Partial | Keep; ASIL ECUC/multi-tool/loadable consistency is W7-specific. Cross-ref W4-002/W4-005. |
| W7-028 | W2-012, G2-023 (DTC standardisation); W6-011 (calibration/state-result handling) | Partial | Keep; BswM/EcuM/NvM/Dem contract-test facet is W7-specific. Cross-ref W2-012/G2-023. |
| W7-029 | W6-017 (MCAL portability — already Duplicate-ish → G1-020/021), G1-020/G1-021 (MCAL canonical) | Duplicate-ish | Consider merging evidence into G1-020/021 canonical (via W6-017); keep W7-029 for ASIL re-verification-burden specifics. |
| W7-030 | G1-016, G1-020 (safety-manual AoU), G2-027; W6-017 | Partial | Keep; MCU-driver safety (watchdog/GPT/ADC/DMA/HSI/lockstep) specifics are new. Cross-ref G1-016/G1-020. |

## Mapping — G5 (Vehicle Domain Deep-Dive)

| G5 ID | Overlaps with | Type | Recommended action |
|---|---|---|---|
| G5-001 | W2-002 (canonical interop/gateway), G2-020; intra-wave W7-023 | Partial | Keep G5-001 (gateway integration/test view); cross-ref W2-002 and W7-023. |
| G5-002 | W2-007 (SOME/IP), W2-008 (DDS determinism), W1-018 (network timing), W3-005 | Partial | Keep; domain time-sync verification + failover facet is G5-specific. |
| G5-003 | W2-010 (OTA), G3-019 (campaign targeting), W1-011, W4-002 | Partial | Keep; service-versioning + mixed-fleet coexistence is G5-specific. Cross-ref G3-019/W2-010. |
| G5-004 | W2-002, W1-007/W1-008 (interfaces/architecture) | Related | Keep; middleware-as-contract lesson-learnt format is G5-specific. |
| G5-005 | G2-003 (BCM — canonical body record), G1-033 (HW/SW co-configuration) | Partial | Keep G5-005 for BCM-consolidation depth; cross-ref G2-003 (nearest canonical). |
| G5-006 | G2-026 (access/comfort), G2-024 (vehicle security), W7-021 (mode management) | Partial | Keep; power-state-timing vs security facet is G5-specific. |
| G5-007 | G2-029 (variants), W1-011, W4-005, W7-024 | Partial | Keep; trim/personalization bindingchecks are G5-specific. Cross-ref variant family. |
| G5-008 | G2-003, W1-012 (rework cost) | Related | Keep; quiet-domain lesson-learnt format is G5-specific. |
| G5-009 | G2-004 (powertrain calibration), G2-005 (transmission), G2-010 (EV torque); G1-016/G1-024 (consolidation HW) | Partial | Keep G5-009 (torque-path ownership); cross-ref G2-004/G2-005 and G1-016/G1-024. |
| G5-010 | G2-004, W7-003/W7-004 (ASIL-C/D), G1-016 | Partial | Keep; torque-monitoring vs OBD arbitration is G5-specific. |
| G5-011 | G2-004, W6-011 (calibration/flash), W4-015/W4-016 (R155/R156 regulation) | Partial | Keep; regulation-protected field-update facet is G5-specific. Cross-ref W6-011/W4-015. |
| G5-012 | G2-004/G2-005, W7-010 | Related | Keep; torque/calibration-as-contract lesson-learnt is G5-specific. |
| G5-013 | G2-001 (ADAS perception — canonical), G1-027/G1-028 (sensor interfaces); G3-003 (data volume), W1-004 (ML requirements) | Partial | Keep G5-013 (pipeline latency/sensor-integration ownership); cross-ref G2-001 and G1-027/028. |
| G5-014 | G2-031 (scenario validation), W4-017 (SOTIF — canonical), W1-010, W7-004; G4-024 (ASIL software precedent) | Partial | Keep; SOTIF-plus-ASIL-D joint argument is G5-specific. Cross-ref W4-017/G2-031. |
| G5-015 | G3-017 (SUMS evidence), G3-001/G3-019 (OTA), W4-016, W6-025; G4-023 (charging OTA) | Partial | Keep; ML/map-update-vs-homologation tension is G5-specific (future horizon). Cross-ref G3-017/W4-016. |
| G5-016 | G2-032 (cross-domain conflicts), W7-006..W7-009, G4-022 (shared thermal/power) | Related | Keep; shared-resource safety-case lesson-learnt is G5-specific. |
| G5-017 | G2-016 (IVI — canonical); W5-024 (SDV skill), W5-002 (AUTOSAR gap) | Partial | Keep G5-017 (SoC-sharing review); cross-ref G2-016 and W5-024. |
| G5-018 | G2-017 (clusters/telltales), W7-005 (QM), W2-009; W5-003 (safety competency) | Partial | Keep; telltale-integrity on shared SoC is G5-specific. Cross-ref G2-017/W7-005. |
| G5-019 | G3-001 (OTA campaigns), W6-025, W4-020 (CI/CD unsuitable); W5-024 | Partial | Keep; app-store-vs-release discipline is G5-specific (future horizon). Cross-ref G3-001/W6-025. |
| G5-020 | G2-016, W7-005 | Related | Keep; QM-exuberance lesson-learnt is G5-specific. |
| G5-021 | G2-015 (cockpit — canonical), G2-017; G1-024 | Partial | Keep G5-021 (consolidated rendering + monitoring); cross-ref G2-015. |
| G5-022 | G2-017 (canonical telltales), W7-002 (ASIL-B) | Partial | Keep G5-022 for display-pipeline/deadline depth; cross-ref G2-017 (nearest canonical). |
| G5-023 | W1-011, W7-024, G2-015 | Partial | Keep; personalization-fenced-from-safety-config is G5-specific. |
| G5-024 | G2-017, W7-002 | Related | Keep; display-as-actuator lesson-learnt is G5-specific. |
| G5-025 | G2-021 (zonal — canonical), W1-016, G1-024 | Partial | Keep; zonal I/O/power/network-bridging depth is G5-specific (future horizon). Cross-ref G2-021. |
| G5-026 | G2-027, W7-006..W7-009, W7-019/W7-020, G1-024 | Partial | Keep; zonal ASIL-D FFI-evidence facet is G5-specific (future horizon). Cross-ref G2-027/W7. |
| G5-027 | G3-018 (rollback), G3-019, W6-025/W6-026, W2-010 | Partial | Keep; zonal packaging/partial-update facet is G5-specific (future horizon). Cross-ref G3-018/W6-025. |
| G5-028 | G2-032 (canonical cross-domain), G2-021 | Related | Keep; zone-collision lesson-learnt is G5-specific (future horizon). |
| G5-029 | G2-022 (HPC — canonical), G2-019, W2-009, W1-016; W3-004/W4-018 (vECU/HIL) | Partial | Keep; virtualization/SOA-deployment strategy is G5-specific (future horizon). Cross-ref G2-022/W2-009. |
| G5-030 | G2-027, G2-022, W7-019/W7-030, W2-009; W3-033, W5-024 | Partial | Keep; shared-SoC hypervisor/interference evidence is G5-specific (future horizon). Cross-ref W7-019/W2-009. |
| G5-031 | G3-001/G3-018/G3-019, W6-025, W2-010; W4-020 | Partial | Keep; container/VM atomic-rollout facet is G5-specific (future horizon). Cross-ref G3/W6-025. |
| G5-032 | G2-019/G2-022, W2-016 (migration), W5-024 | Related | Keep; HPC-convergence lesson-learnt is G5-specific (future horizon). |

## Summary

- **No full duplicates** found: every W7 record adds ASIL/multicore-specific coverage and every G5 record adds per-domain depth not present elsewhere.
- **Partial overlaps (50):** W7-001, W7-002, W7-004, W7-006..W7-015, W7-017..W7-028, W7-030; G5-001..G5-003, G5-005..G5-007, G5-009..G5-011, G5-013..G5-015, G5-017..G5-019, G5-021..G5-023, G5-025..G5-027, G5-029..G5-031 — cross-reference only.
- **Duplicate-ish (1):** W7-029 (MCAL portability/re-verification) — same underlying problem as W6-017 (already Duplicate-ish → G1-020/021 canonical); consider merging evidence into G1-020/021 while retaining ASIL re-verification context.
- **Related (11):** W7-003, W7-005, W7-016; G5-004, G5-008, G5-012, G5-016, G5-020, G5-024, G5-028, G5-032 — cross-reference only (mostly G5 lesson-learnt records, whose format is itself new).
- **New (0, standalone):** no W7/G5 record is free of prior-wave neighbours — expected for an integration wave cutting across safety, middleware, domains and silicon; novelty lies in the ASIL implication sets (W7-001..W7-004), FFI evidence specifics (W7-006..W7-008), and per-domain lesson-learnt records (G5-004/008/012/016/020/024/028/032).

## Cross-reference actions taken

- W7 report §2 and G5 report §2 already cite W2-002/004/006/009/012/013/018, W1-010/011, W6-011/017/021/024/025, G1-016/020/021/024, G2-027, G3-017/018, W4-015/016/017 in their evidence bases.
- CROSS_REFERENCE_INDEX.md gained a Wave cross-references (W7/G5) section (Phase D1).
- ISSUE_TRACKER.md populated with 62 W7/G5 issues (Phase D3); resolution rationale in ISSUE_TRACKER.md (Phase D4 section below).
- No record IDs were changed or removed; canonical ownership remains with the originating workstream.

---

## Resolution rationale — W7/G5 (Phase D4)

Decision per issue: **Keep open** (active, mitigation in progress), **Deferred** (out of current scope / future horizon), or **Cross-referenced** (evidence merged into canonical record in another workstream). Mirrors the Phase 3.4 precedent: only mark Deferred/Cross-referenced where a genuine duplicate-ish or future-horizon item exists; default Keep open.

| ID | Decision | Rationale |
|---|---|---|
| W7-001 | Keep open | ASIL-A tailoring checklist (short horizon); cross-ref W2-018/W1-014/W4-011. |
| W7-002 | Keep open | B-vs-A/C boundary evidence; SPFM/LFM + AoU register (short/medium horizon). |
| W7-003 | Keep open | Independence matrix + HSI coverage; fault-injection coverage open. |
| W7-004 | Keep open | Near-D rigour planning; re-verification cost if under-scoped; cross-ref W6-024. |
| W7-005 | Keep open | QM gap analysis before reuse; interference-source discipline. |
| W7-006 | Keep open | MPU/MMU partitioning + interference analysis; cross-ref W2-004. |
| W7-007 | Keep open | Budget + measurement on target; cross-ref W1-010/W2-006. |
| W7-008 | Keep open | Watchdog/control-flow checks; testing-alone-insufficient argument. |
| W7-009 | Keep open | Shared-code audit + common-cause analysis; cross-ref G2-027/W2-004. |
| W7-010 | Keep open | Partitioning discipline + interface ownership; cross-ref W2-012. |
| W7-011 | Keep open | Mapping + timing verification; cross-ref W2-013 scheduling. |
| W7-012 | Keep open | Atomicity + consistency protocol; gateway timing included. |
| W7-013 | Keep open | Standardized profiles; profile selection + coverage still audited. |
| W7-014 | Keep open | Static allocation baseline; interference re-analysis on change. |
| W7-015 | Keep open | Analysis unsolved on consolidated SoCs; cross-ref W1-010. |
| W7-016 | Keep open | Lock ordering + deadlock review; shared-memory protocol. |
| W7-017 | Keep open | Both measurement and analysis required; neither sufficient. |
| W7-018 | Keep open | Trusted vs untrusted + restart strategy; containment tests. |
| W7-019 | Deferred | Future: hypervisor-guest SEooC cost uncertain; zonal/HPC horizon; cross-ref W2-009, W5-024. |
| W7-020 | Deferred | Future: gateway as FFI boundary; covert-channel review for zonal/HPC horizon. |
| W7-021 | Keep open | ECU vs partition mode contract; assumption log owned. |
| W7-022 | Keep open | Generation-mismatch checks; cross-ref W2-012, W6-021. |
| W7-023 | Keep open | Group atomicity + gateway budget; cross-ref G5-001, W2-002. |
| W7-024 | Keep open | Post-build variant control; wrong-variant flashing prevention. |
| W7-025 | Keep open | SiL/HiL + robustness; cross-ref W3 fault-injection practice. |
| W7-026 | Keep open | Mixed-ASIL stacking review; scheduler ownership. |
| W7-027 | Keep open | Multi-tool consistency checks; post-build loadable control. |
| W7-028 | Keep open | Contract tests for state/result handling. |
| W7-029 | Deferred | Long horizon: reusable MCAL driver asset; evidence merged into G1-020/021 canonical (via W6-017 precedent). |
| W7-030 | Keep open | Safety-manual AoU tracking; HSI validation owned. |
| G5-001 | Keep open | Mapping record + end-to-end budget; cross-ref W2-002, W7-023. |
| G5-002 | Keep open | Time-sync verification across legs; deterministic budget. |
| G5-003 | Keep open | Versioning policy + coexistence tests; cross-ref G3 fleet. |
| G5-004 | Keep open | Gateway ownership + contract tests; integration-point focus. |
| G5-005 | Keep open | I/O abstraction + load-control validation; QM-ASIL-B mix. |
| G5-006 | Keep open | Power-state timing vs security checks; cross-ref W7-021. |
| G5-007 | Keep open | Long-standing trim sprawl; binding checks; cross-ref W7-024. |
| G5-008 | Keep open | Body on critical path; early integration slots. |
| G5-009 | Keep open | Torque-path ownership across controllers; cross-ref W7-010. |
| G5-010 | Keep open | Established monitoring practice; OBD vs safety arbitration. |
| G5-011 | Keep open | Regulation-protected calibration; cross-ref W4-015, W6-011. |
| G5-012 | Keep open | Torque/calibration treated as vehicle-level contracts. |
| G5-013 | Keep open | Pipeline latency ownership; sensor-integration tests. |
| G5-014 | Keep open | SOTIF + ASIL-D joint argument; cross-ref W4-017, W1-010. |
| G5-015 | Deferred | Future/emerging: update-vs-homologation tension; cross-ref G3, W5. |
| G5-016 | Keep open | Every shared resource enters safety case; cross-ref W7-006..009. |
| G5-017 | Keep open | SoC sharing review; QM beside ASIL neighbours. |
| G5-018 | Keep open | Telltale integrity on shared SoC; cross-ref W7-005, G5-022. |
| G5-019 | Deferred | Future/SDV-era: cadence clash; update discipline + rollback. |
| G5-020 | Keep open | QM containment proof required before co-hosting. |
| G5-021 | Keep open | Consolidated rendering + monitoring; deadline ownership. |
| G5-022 | Keep open | ASIL-B telltale evidence; pipeline deadline tests. |
| G5-023 | Keep open | Personalization fenced from safety config; cross-ref W7-024. |
| G5-024 | Keep open | Display treated as actuator in safety case. |
| G5-025 | Deferred | Future: zonal centralization horizon; single-controller risk. |
| G5-026 | Deferred | Future: FFI up to ASIL-D; cross-ref W7-006..009, W7-019/020. |
| G5-027 | Deferred | Future: partial-update + rollback; cross-ref W6-025/026, G3-018. |
| G5-028 | Deferred | Future: zone integration as critical path; cross-ref G2. |
| G5-029 | Deferred | Future: virtualization strategy; cross-ref W2-009, W7-019. |
| G5-030 | Deferred | Future: hypervisor qualification + interference; cross-ref W7-030 gap. |
| G5-031 | Deferred | Future: atomic fleet rollout; cross-ref G3-018, W6-025. |
| G5-032 | Deferred | Future: HPC as binding SDV constraint; cross-ref W2-016, W5-024. |

---

See also: [ISSUE_TRACKER.md](./ISSUE_TRACKER.md) | [CROSS_CHECK.md](./CROSS_CHECK.md) | [W7_asil_mixed_criticality_multicore.md](./W7_asil_mixed_criticality_multicore.md) | [G5_domain_deep_dive.md](./G5_domain_deep_dive.md) | [OVERLAP_ANALYSIS_W6.md](./OVERLAP_ANALYSIS_W6.md)
