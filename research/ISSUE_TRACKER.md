# Issue Tracker — Old vs. Current vs. Future

**Purpose:** Single tracker for issues across the research corpus, distinguishing **old** (legacy/long-standing), **current** (active in programs), and **future** (emerging/SDV/AI-era) issues. Supports the multi-agent, multi-mode team workflow.

**Owner:** Issue agent (triage mode)  
**Created:** 2026-11-09  
**Status:** Seed (W6 bootloader/flash populated; other workstreams to be triaged by agents)

---

## Status legend

- **Open** — identified, not yet addressed
- **In progress** — being worked
- **Resolved** — closed with rationale
- **Deferred** — out of current scope
- **Won't fix** — accepted as-is

## Category legend

- **Old** — legacy/long-standing, predates SDV/OTA
- **Current** — active in today's programs
- **Future** — emerging with SDV, zonal, AI, software-defined vehicle

---

## W6 — Bootloader & Flash Programming (seed)

| ID | Category | Title | Status | Owner | Resolution / notes |
|---|---|---|---|---|---|
| W6-001 | Old | No standardized automotive bootloader (AUTOSAR out of scope) | Deferred | Research agent | Root cause of fragmentation; future: industry standard? |
| W6-002 | Current | Bootloader memory layout and partitioning complexity | Open | Book agent | Variant-specific; needs automated checks |
| W6-003 | Current | Secure boot chain-of-trust implementation gaps | Open | Solution agent | HSM/crypto vendor variation |
| W6-004 | Current | Bootloader startup time and fast-boot constraints | Open | Book agent | Boot-time budgets per ECU type |
| W6-005 | Current | Anti-rollback and version monotonicity enforcement gaps | Open | Solution agent | OTP fuse management |
| W6-006 | Current | Dual-bank / A-B update scheme complexity and flash cost | Open | Solution agent | Cost-benefit by ECU class |
| W6-007 | Current | UDS flash programming protocol complexity | Open | Tooling agent | Standardized timing profiles |
| W6-008 | Current | Flash programming reliability under power transients | Open | Solution agent | Minimum voltage margin |
| W6-009 | Current | Bootloader recovery and brick prevention | Open | Solution agent | Standardized recovery paths |
| W6-010 | Current | Application/bootloader interface contract gaps | Open | Interlink agent | Automated contract tests |
| W6-011 | Current | Calibration data and flash interaction | Open | Research agent | Version binding |
| W6-012 | Current | Bootloader testing coverage gaps | Open | Review agent | Test plan template |
| W6-013 | Old | Flash endurance and wear-leveling validation | Open | Solution agent | Write-budget per ECU |
| W6-014 | Current | Power-loss and interrupted programming recovery testing | Open | Tooling agent | Power-fault test plan |
| W6-015 | Current | Manufacturing/EOL flashing and traceability | Open | Artefact agent | End-to-end traceability |
| W6-016 | Current | Diagnostic session transitions to bootloader | Open | Tooling agent | Standardized timing |
| W6-017 | Old | Flash driver (MCAL) portability and abstraction gaps | Deferred | Solution agent | Reusable driver asset |
| W6-018 | Future | Bootloader security vs. upgradability conflict | Deferred | Solution agent | Safe self-update design |
| W6-019 | Current | Key and certificate management lifecycle | Open | Solution agent | Key lifecycle automation |
| W6-020 | Current | Bootloader size constraints and feature creep | Open | Book agent | Size budget governance |
| W6-021 | Current | Bootloader integration with AUTOSAR BSW | Open | Interlink agent | Standardized interface |
| W6-022 | Current | Reprogramming throughput (CAN vs. DoIP/Ethernet) | Open | Solution agent | Transport selection |
| W6-023 | Current | Flash toolchain fragmentation | Open | Tooling agent | Universal flash tool |
| W6-024 | Current | Bootloader V&V evidence and ISO 26262 compliance | Open | Review agent | Safety patterns |
| W6-025 | Future | OTA and bootloader orchestration | Deferred | Solution agent | Standardized safe-state |
| W6-026 | Future | Multi-ECU campaign consistency and partial failure | Deferred | Solution agent | Campaign transaction |
| W6-027 | Future | Bootloader self-update complexity | Deferred | Solution agent | Safe self-update |
| W6-028 | Current | Flash image integrity verification | Open | Solution agent | Verification depth trade-off |
| W6-029 | Current | Human skill gaps in bootloader/flash/security | Open | Review agent | Training curriculum |
| W6-030 | Current | Late bootloader rework cost | Open | Orchestrator | Shift-left |

## W7 — ASIL / Mixed-Criticality / Multicore (added 2026-11-16, Phase D3)

| ID | Category | Title | Status | Owner | Resolution / notes |
|---|---|---|---|---|---|
| W7-001 | Current | ASIL-A implication set: design, process, testing, evidence gaps | Open | Safety Manager | Tailoring rationale + work-product checklist; cross-ref W2-018 |
| W7-002 | Current | ASIL-B implication set: B-vs-A/C boundary and evidence | Open | Safety Manager | SPFM/LFM + AoU register; first-pass assessment survival unknown |
| W7-003 | Current | ASIL-C implication set: systematic faults, independence, HSI | Open | Safety Manager | Independence matrix + HSI coverage; fault-injection coverage open |
| W7-004 | Current | ASIL-D implication set: full rigour and assessment readiness | Open | Safety Manager | Near-D rigour planning; re-verification cost if under-scoped |
| W7-005 | Old | QM component obligations, limits and misuse in ASIL context | Open | Safety Manager | QM is interference source; gap analysis before reuse |
| W7-006 | Current | FFI: memory interference evidence | Open | Safety Manager | MPU/MMU partitioning + interference analysis; cross-ref W2-004 |
| W7-007 | Current | FFI: timing interference evidence | Open | Safety Manager | Budget + measurement on target; cross-ref W1-010, W2-006 |
| W7-008 | Current | FFI: execution and control-flow interference | Open | Safety Manager | Watchdogs, control-flow checks; testing cannot prove FFI alone |
| W7-009 | Current | QM-ASIL coupling via shared libraries and common-cause paths | Open | SW Architect | Shared-code audit + common-cause analysis; E2E does not cover it |
| W7-010 | Current | SWC partitioning by ASIL: granularity, allocation, interfaces | Open | SW Architect | Partitioning discipline + interface ownership; cross-ref W2-012 |
| W7-011 | Current | Runnable-to-task mapping under ASIL constraints | Open | SW Architect | Mapping + timing verification; cross-ref W2-013 scheduling |
| W7-012 | Current | Data consistency across ASIL boundaries | Open | SW Architect | Atomicity + consistency protocol; gateway timing included |
| W7-013 | Old | End-to-end protection (E2E profiles, CRC and sequence) | Open | SW Architect | Standardized profiles; profile selection + coverage still audited |
| W7-014 | Current | Multicore core allocation for mixed-criticality | Open | SW Architect | Static allocation baseline; interference re-analysis on change |
| W7-015 | Current | Multicore scheduling and timing analysis gaps | Open | SW Architect | Analysis unsolved on consolidated SoCs; cross-ref W1-010 |
| W7-016 | Current | Inter-core communication and spinlocks: hazards, ordering, deadlock | Open | SW Architect | Lock ordering + deadlock review; shared-memory protocol |
| W7-017 | Current | Multicore timing verification: contention, cache, measurement vs analysis | Open | SW Architect | Both measurement and analysis required; neither sufficient |
| W7-018 | Current | OS-Applications and memory partitions: trust, restart, containment | Open | Integrator | Trusted vs untrusted + restart strategy; containment tests |
| W7-019 | Future | Hypervisor guests for mixed-criticality (ASIL beside QM Linux/Android) | Deferred | Integrator | SEooC cost uncertain; cross-ref W2-009, W5-024 skill gap |
| W7-020 | Future | Inter-partition communication gateways and FFI boundary | Deferred | Integrator | Gateway as FFI boundary; covert-channel review for zonal/HPC |
| W7-021 | Current | Mode management across partitions: inconsistent assumptions | Open | Integrator | ECU vs partition mode contract; assumption log owned |
| W7-022 | Current | SWC-RTE-OS integration chain: contract vs execution, generation mismatch | Open | Integrator | Generation-mismatch checks; cross-ref W2-012, W6-021 |
| W7-023 | Current | COM and signal-service mapping under ASIL: atomicity, gateway timing | Open | Integrator | Group atomicity + gateway budget; cross-ref G5-001, W2-002 |
| W7-024 | Current | Variant and calibration binding in mixed-criticality builds | Open | Integrator | Post-build variant control; wrong-variant flashing prevention |
| W7-025 | Current | Application integration testing across ASILs: coverage, fault injection | Open | Integrator | SiL/HiL + robustness; cross-ref W3 fault-injection practice |
| W7-026 | Current | BSW module integration under ASIL: stacking, scheduler, ownership | Open | Integrator | Mixed-ASIL stacking review; scheduler ownership |
| W7-027 | Current | BSW configuration consistency: ECUC parameters, multi-tool, loadables | Open | Integrator | Multi-tool consistency checks; post-build loadable control |
| W7-028 | Current | RTE/BSW contract gaps: BswM/EcuM states, NvM results, Dem discipline | Open | Integrator | Contract tests for state/result handling |
| W7-029 | Old | MCAL portability and re-verification burden across MCU changes | Deferred | Integrator | Duplicate-ish W6-017; evidence merged into G1-020/021 canonical; ASIL re-verification context retained |
| W7-030 | Current | MCU driver safety: watchdog, GPT, ADC, DMA, HSI, lockstep/EDC | Open | Integrator | Safety-manual AoU tracking; HSI validation owned |

## G5 — Vehicle Domain Deep-Dive (added 2026-11-16, Phase D3)

| ID | Category | Title | Status | Owner | Resolution / notes |
|---|---|---|---|---|---|
| G5-001 | Current | Middleware signal-to-service gateway correctness and timing | Open | Middleware Architect | Mapping record + end-to-end budget; cross-ref W2-002, W7-023 |
| G5-002 | Current | Middleware time synchronisation and deterministic communication | Open | Middleware Architect | Time-sync verification across legs; deterministic budget |
| G5-003 | Current | Middleware service-interface versioning and mixed-version fleet coexistence | Open | Middleware Architect | Versioning policy + coexistence tests; cross-ref G3 fleet |
| G5-004 | Current | Middleware as integration contract: cross-domain lesson | Open | SW Architect | Gateway ownership + contract tests; integration-point focus |
| G5-005 | Current | Body BCM consolidation, I/O abstraction and load control | Open | Domain Architect | I/O abstraction + load-control validation; QM-ASIL-B mix |
| G5-006 | Current | Body access/security vs safety and power-state timing | Open | Domain Architect | Power-state timing vs security checks; cross-ref W7-021 |
| G5-007 | Old | Body variant explosion across trims and personalization data | Open | Integrator | Long-standing trim sprawl; binding checks; cross-ref W7-024 |
| G5-008 | Current | Body quiet-domain lesson: stalls vehicle integration | Open | Integrator | Body on critical path; early integration slots |
| G5-009 | Current | Powertrain coordination and torque path (EMS/TCM/aftertreatment) | Open | Domain Architect | Torque-path ownership across controllers; cross-ref W7-010 |
| G5-010 | Old | Powertrain ASIL-C/D torque monitoring and OBD interplay | Open | Safety Manager | Established monitoring practice; OBD vs safety arbitration |
| G5-011 | Current | Powertrain emission-calibration protection and field updates under regulation | Open | Integrator | Regulation-protected calibration; cross-ref W4-015, W6-011 |
| G5-012 | Current | Powertrain torque and calibration as vehicle contracts: lesson | Open | SW Architect | Torque/calibration treated as vehicle-level contracts |
| G5-013 | Current | AD/ADAS perception, fusion, planning pipeline and sensor integration | Open | Domain Architect | Pipeline latency ownership; sensor-integration tests |
| G5-014 | Current | AD/ADAS SOTIF plus ASIL-D motion control, ODD exits, latency budgets | Open | Safety Manager | SOTIF + ASIL-D joint argument; cross-ref W4-017, W1-010 |
| G5-015 | Future | AD/ADAS ML model, map and calibration updates vs homologation | Deferred | Safety Manager | Emerging update-vs-homologation tension; cross-ref G3, W5 |
| G5-016 | Current | AD/ADAS shared resources as safety case: lesson | Open | Safety Manager | Every shared resource enters safety case; cross-ref W7-006..009 |
| G5-017 | Current | IVI head-unit SoC with audio and nav stacks beside safety | Open | Domain Architect | SoC sharing review; QM beside ASIL neighbours |
| G5-018 | Current | IVI QM-ASIL coexistence and telltale/display integrity | Open | Safety Manager | Telltale integrity on shared SoC; cross-ref W7-005, G5-022 |
| G5-019 | Future | IVI app-store cadence vs vehicle release discipline | Deferred | Integrator | SDV-era cadence clash; update discipline + rollback |
| G5-020 | Current | IVI QM exuberance vs ASIL neighbours: lesson | Open | SW Architect | QM containment proof required before co-hosting |
| G5-021 | Current | Cockpit cluster, HUD and driver-monitoring consolidation on shared SoC | Open | Domain Architect | Consolidated rendering + monitoring; deadline ownership |
| G5-022 | Current | Cockpit ASIL-B telltales, display-pipeline integrity, rendering deadlines | Open | Safety Manager | ASIL-B telltale evidence; pipeline deadline tests |
| G5-023 | Current | Cockpit HMI personalization and variants vs safety configuration | Open | Integrator | Personalization fenced from safety config; cross-ref W7-024 |
| G5-024 | Current | Cockpit display as safety actuator: lesson | Open | Safety Manager | Display treated as actuator in safety case |
| G5-025 | Future | Zonal I/O consolidation, power distribution, network bridging | Deferred | Domain Architect | Zonal centralization horizon; single-controller risk |
| G5-026 | Future | Zonal mixed-criticality to ASIL-D on one controller and FFI evidence | Deferred | Safety Manager | FFI up to ASIL-D; cross-ref W7-006..009, W7-019/020 |
| G5-027 | Future | Zonal software packaging, partial updates and rollback | Deferred | Integrator | Partial-update + rollback; cross-ref W6-025/026, G3-018 |
| G5-028 | Future | Zonal collision lesson: where domains collide | Deferred | SW Architect | Zone integration as critical path; cross-ref G2 |
| G5-029 | Future | HPC central-compute consolidation, virtualization and SOA deployment | Deferred | SW Architect | Virtualization strategy; cross-ref W2-009, W7-019 |
| G5-030 | Future | HPC mixed-criticality to ASIL-D on shared SoC, hypervisor, interference | Deferred | Safety Manager | Hypervisor qualification + interference; cross-ref W7-030 gap |
| G5-031 | Future | HPC container/VM image updates, atomic rollout, logging pipelines | Deferred | Integrator | Atomic fleet rollout; cross-ref G3-018, W6-025 |
| G5-032 | Future | HPC convergence lesson | Deferred | SW Architect | HPC as binding SDV constraint; cross-ref W2-016, W5-024 |

---

## Cross-workstream triage (to be populated by issue agent)

Agents: populate rows for W1–W5 and G1–G4 using the same schema. Mark overlaps/duplicates against W6 (see CROSS_CHECK.md).

---

## Resolution rationale (Phase 3.4)

Decision per issue: **Keep open** (active, mitigation in progress), **Deferred** (out of current scope / future horizon), or **Cross-referenced** (evidence merged into canonical record in another workstream).

| ID | Decision | Rationale |
|---|---|---|
| W6-001 | Deferred | Long-term industry-standardisation question; mitigation (SC-1) is a 3–5 yr horizon item; tracked as open research question. |
| W6-002 | Keep open | Variant-specific; automated memory-map checks (SC-1/SC-7) targeted for short horizon. |
| W6-003 | Keep open | HSM/crypto vendor variation; reference architecture (SC-2) in medium horizon. |
| W6-004 | Keep open | Boot-time budgets per ECU type to be defined; tied to W1/W2 timing family. |
| W6-005 | Keep open | OTP fuse management policy under SC-2. |
| W6-006 | Keep open | Cost-benefit by ECU class; SC-3 (A/B strategy) medium horizon. |
| W6-007 | Keep open | Standardised UDS timing profiles (SC-5) short horizon. |
| W6-008 | Keep open | Minimum voltage margin study; SC-3 medium horizon. |
| W6-009 | Keep open | Standardised recovery paths; SC-3 medium horizon. |
| W6-010 | Keep open | Automated contract tests (SC-7) short horizon. |
| W6-011 | Keep open | Version binding between calibration and application; research follow-up. |
| W6-012 | Keep open | Bootloader test plan template (SC-4) short horizon. |
| W6-013 | Keep open | Write-budget per ECU; endurance validation framework. |
| W6-014 | Keep open | Power-fault injection test plan (SC-4) short horizon. |
| W6-015 | Keep open | End-to-end EOL traceability (SC-5) short/medium horizon. |
| W6-016 | Keep open | Standardised diagnostic-session timing (SC-5). |
| W6-017 | Deferred | Reusable MCAL flash driver asset; long horizon; cross-ref G1-020/021. |
| W6-018 | Deferred | Future: safe self-update design (SC-6); security vs. upgradability tension. |
| W6-019 | Keep open | Key/certificate lifecycle automation (SC-2) medium horizon; cross-ref G4-013. |
| W6-020 | Keep open | Size budget governance (SC-6/SC-1); process control. |
| W6-021 | Keep open | Standardised bootloader/BSW interface (SC-1/SC-7). |
| W6-022 | Keep open | Transport selection guidance (SC-5). |
| W6-023 | Cross-referenced | Evidence merged into toolchain-fragmentation family (W4-001 canonical); W6 retains flash-tool-specific context. |
| W6-024 | Keep open | Safety patterns and V&V evidence (SC-4) short/medium horizon. |
| W6-025 | Deferred | Future: OTA/bootloader orchestration (SC-6); depends on G3 cloud side. |
| W6-026 | Deferred | Future: multi-ECU campaign transaction (SC-6). |
| W6-027 | Deferred | Future: safe self-update design (SC-6). |
| W6-028 | Keep open | Verification depth trade-off; SC-2/SC-3. |
| W6-029 | Keep open | Training curriculum (SC-8) long horizon; cross-ref W5. |
| W6-030 | Keep open | Shift-left bootloader verification (SC-4) short horizon. |

### W7/G5 (Phase D4)

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
## Notes

- Duplicate/overlap analysis vs. W2-010, G1-025/026, G3-018, W4-016 performed (Phase 3.3) — see [OVERLAP_ANALYSIS_W6.md](./OVERLAP_ANALYSIS_W6.md).
- Resolution rationale added (Phase 3.4) above.
- W7/G5 overlap analysis performed (Phase D4) — see [OVERLAP_ANALYSIS_W7_G5.md](./OVERLAP_ANALYSIS_W7_G5.md); W7/G5 resolution rationale added (Phase D4) above. W7-029 Status updated Open → Deferred (Duplicate-ish W6-017 → G1-020/021 canonical).
- Agents: populate rows for W1–W5 and G1–G4 using the same schema. Mark overlaps/duplicates against W6 (see CROSS_CHECK.md and OVERLAP_ANALYSIS_W6.md).

---
See also: [INTEGRATION_PLAN.md](./INTEGRATION_PLAN.md) | [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md)
