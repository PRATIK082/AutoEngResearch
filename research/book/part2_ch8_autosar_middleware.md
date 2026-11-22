# Part II — Chapter 8: AUTOSAR and Middleware Engineering

**Book:** *The State of Automotive Software Engineering*
**Part:** II — Complete Automotive Software Lifecycle
**Chapter:** 8 — AUTOSAR and Middleware Engineering
**Source workstreams:** W1, W2, W4, W5, W6, G5
**Problem IDs in scope:** W2-001, W2-002, W2-003, W2-004, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-011, W2-012, W2-013, W2-014, W2-015, W2-016, W2-017, W2-018, W2-019, W2-020, W1-010, W1-013, W1-018, W4-004, W4-005, W6-001..W6-030, G5-001..G5-032

---

**G6-G11** scope-header range `(G6-G11)` — **G6 — CDI Bootloader & Flash Programming** (G6-001..G6-045) See `research/G6_bootloader_flash.md`, **G7 — SWC partitioning & multicore** (G7-001..G7-010) See `research/G7_swc_partitioning.md`, **G8 — Body & comfort deep-dive** (G8-001..G8-024) See `research/G8_body_comfort.md`, **G9 — Powertrain & EV deep-dive** (G9-001..G9-006) See `research/G9_powertrain_ev.md`, **G10 — AD/ADAS domain deep-dive** (G10-001..G10-015) See `research/G10_adadas.md`, **G11 — Infotainment & Cockpit deep-dive** (G11-001..G11-008) See `research/G11_infotainment.md`

## 8.0 Chapter purpose and problem map

AUTOSAR remains the dominant automotive software standard, but its dual-platform reality (Classic + Adaptive) creates a persistent impedance mismatch. Classic is mature, hard-real-time, signal-based, and ASIL-D capable but rigid; Adaptive is service-oriented, POSIX/C++-based, and intended for HPC/zonal/ADAS, yet still evolving with documented "Known Limitations" across releases. Middleware (SOME/IP, DDS) bridges these worlds but introduces discovery latency, jitter, and real-time determinism problems. Configuration is dominated by ARXML, which is massive, version-fragile, and poorly diffable. Toolchain fragmentation and vendor lock-in persist despite the interoperability standard.

All 20 W2 problem records are in scope for this chapter; selected W1 and W4 records provide cross-cutting context.

---

## 8.1 Introduction

...

(Chapter content continues)