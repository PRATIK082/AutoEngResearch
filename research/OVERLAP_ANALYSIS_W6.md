# Overlap Analysis — W6 (Bootloader & Flash) vs. existing workstreams

**Purpose:** Phase 3.3 deliverable. Maps each W6 problem record to existing G1/G3/G4/W2 (and W3/W5) records to identify duplicates, partial overlaps and related problems, with a recommended action for each.

**Owner:** Review agent (review mode)  
**Created:** 2026-11-09  
**Status:** Complete

---

## Legend

- **Duplicate** — same underlying problem recorded under a different ID; should be merged or one designated canonical.
- **Partial** — W6 covers a facet of a broader existing problem (or vice versa).
- **Related** — adjacent problem; cross-reference only.
- **New** — no meaningful overlap; W6 adds genuine new coverage.

## Mapping

| W6 ID | Overlaps with | Type | Recommended action |
|---|---|---|---|
| W6-001 | W2-010 (OTA/FOTA), W4-016 (R156/ISO 24089) | Partial | Keep W6-001 canonical for bootloader standardisation; cross-ref W2-010/W4-016 (vehicle/cloud side). |
| W6-002 | G1-018 (eNVM scalability), G1-019 (external flash XiP), G1-017 (memory wall) | Related | Cross-reference; memory-layout problem is W6-specific. |
| W6-003 | G4-025 (charging cybersecurity), W4-014 (ISO/SAE 21434), G3 (cloud security) | Partial | Keep W6-003 (vehicle-side chain of trust); cross-ref security workstreams. |
| W6-004 | W1-010, W1-018 (timing), W2-006, W2-013 (scheduling), W3-005 (temporal sync) | Partial | Cross-ref timing family; fast-boot is a distinct constraint. |
| W6-005 | W6-003, W2-010, G4-023 (version management) | Related | Keep; anti-rollback policy is W6-specific. |
| W6-006 | W2-010, G3-018 (rollback orchestration) | Partial | Cross-ref; dual-bank cost is a W6 concern. |
| W6-007 | G4-029 (HV diagnostics/DTC), G4-013 (PKI/certs), W4-016 | Related | Cross-ref; UDS flash protocol complexity is W6-specific. |
| W6-008 | G1-025, G1-026 (power management), G4-018 (HV safety) | Partial | Cross-ref; power-transient programming is W6-specific. |
| W6-009 | G3-018, W2-010, W6-006 | Related | Cross-ref; recovery/brick prevention is W6-specific. |
| W6-010 | W2-002 (Classic/Adaptive interop), W2-012 (RTE contract), W1-007 (unstable interfaces) | Partial | Cross-ref interface-governance family. |
| W6-011 | G4 (calibration burden), W1-011 (variant management), W4-005 | Partial | Cross-ref; calibration/flash binding is W6-specific. |
| W6-012 | W3-032/033/034 (test coverage), W3-010/012 (HIL), G4-031/032 | Partial | Cross-ref; bootloader test-plan template is W6 deliverable. |
| W6-013 | G1-018 (eNVM), G1-007 (service life vs. EOL), G4-001/002 (battery aging) | Related | Cross-ref; flash endurance is W6-specific. |
| W6-014 | W3-033 (power-fault testing), W6-008/009 | Related | Cross-ref; W6-owned test concern. |
| W6-015 | W4-023 (supplier/OEM toolchain mismatch), W4-007 (traceability) | Partial | Cross-ref; EOL flashing traceability is W6-specific. |
| W6-016 | W4-016 (R156), G4-029 (diagnostics), W4-014 | Related | Cross-ref; session-transition timing is W6-specific. |
| W6-017 | G1-020, G1-021 (MCAL complexity/lock-in), W2-005 (toolchain) | Duplicate-ish | Consider merging evidence with G1-020/021; keep W6-017 for bootloader-specific MCAL flash driver. |
| W6-018 | W2-010, W6-003/005/027 | Related | Keep; security-vs-upgradability tension is W6-specific. |
| W6-019 | G4-013 (charging PKI/certs), W6-003 | Partial | Cross-ref; key lifecycle is shared concern (vehicle + charging). |
| W6-020 | G1-017 (memory wall), W6-001/002 | Related | Cross-ref; size-budget governance is W6-specific. |
| W6-021 | W2 (AUTOSAR BSW), W6-010/017 | Partial | Cross-ref; AUTOSAR bootloader integration is W6-specific. |
| W6-022 | W4-001 (toolchain), G3-002 (connectivity bandwidth) | Related | Cross-ref; throughput/transport selection is W6-specific. |
| W6-023 | W4-001, W4-023, W4-028, W2-005, W3-036 | Duplicate-ish | Cross-ref; consider merging into toolchain-fragmentation family (W4-001 canonical). |
| W6-024 | W3-025 (compliance docs), W4-011, W4-013, G4-024 | Partial | Cross-ref; bootloader V&V evidence is W6-specific. |
| W6-025 | W2-010, G3-001/018/019, W4-016, G4-023 | Partial | Cross-ref; W6 owns vehicle-side, G3 owns cloud-side. |
| W6-026 | G3-001/019, W2-010 | Partial | Cross-ref; multi-ECU campaign consistency shared with G3. |
| W6-027 | W6-018, W2-010 | Related | Keep; self-update design is W6-specific. |
| W6-028 | W6-003, G4-025, ISO/SAE 21434 | Related | Cross-ref; image-integrity verification is W6-specific. |
| W6-029 | W5 (all skill problems), W1-015, W2-019, W3-038, W4-022 | Partial | Cross-ref; W6 adds bootloader/security specialist subset. |
| W6-030 | W3-031 (late testing), W6-012, W4-019 (vECU) | Partial | Cross-ref; late bootloader rework is W6-specific. |

## Summary

- **No full duplicates** found: every W6 record adds vehicle-side bootloader/flash coverage not present elsewhere.
- **Partial overlaps (14):** W6-001, W6-003, W6-004, W6-006, W6-010, W6-011, W6-012, W6-015, W6-019, W6-021, W6-024, W6-025, W6-026, W6-029 — cross-reference only.
- **Duplicate-ish (2):** W6-017 (MCAL) and W6-023 (toolchain fragmentation) — consider merging evidence into G1-020/021 and W4-001 canonical records respectively, while retaining W6-specific context.
- **Related (14):** all others — cross-reference only.

## Cross-reference actions taken

- W6 report §3 already cites W2-010, G1-025/026, G3-018, W4-016 in its evidence bases.
- ISSUE_TRACKER.md notes updated (Phase 3.3 complete).
- No record IDs were changed or removed; canonical ownership remains with the originating workstream.

---
See also: [ISSUE_TRACKER.md](./ISSUE_TRACKER.md) | [CROSS_CHECK.md](./CROSS_CHECK.md) | [W6_bootloader_flash.md](./W6_bootloader_flash.md)
