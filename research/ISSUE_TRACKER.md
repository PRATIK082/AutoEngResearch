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

---
## Notes

- Duplicate/overlap analysis vs. W2-010, G1-025/026, G3-018, W4-016 performed (Phase 3.3) — see [OVERLAP_ANALYSIS_W6.md](./OVERLAP_ANALYSIS_W6.md).
- Resolution rationale added (Phase 3.4) above.
- Agents: populate rows for W1–W5 and G1–G4 using the same schema. Mark overlaps/duplicates against W6 (see CROSS_CHECK.md and OVERLAP_ANALYSIS_W6.md).

---
See also: [INTEGRATION_PLAN.md](./INTEGRATION_PLAN.md) | [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md)
