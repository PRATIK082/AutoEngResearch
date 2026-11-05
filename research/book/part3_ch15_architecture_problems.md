# Part III — Chapter 15: Architecture Problems

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 15 — Architecture Problems  
**Source workstreams:** W1, W2, G1, G2, G3  

---

## 15.0 Chapter purpose and problem map

Architecture problems are the structural problems of how automotive software is organised: layered stacks, Classic vs Adaptive, zonal/centralised computing, mixed-criticality consolidation, middleware, and cloud/edge architecture. They are distinct from low-level technical problems (Chapter 14) and from integration execution (Chapter 16).

| Group | Problem IDs | Count |
|---|---|---|
| System/architecture (W1) | W1-007, W1-008, W1-009, W1-016, W1-017 | 5 |
| Timing/network (W1) | W1-010, W1-018 | 2 |
| AUTOSAR Classic/Adaptive & middleware (W2) | W2-002, W2-003, W2-004, W2-005, W2-009, W2-016, W2-018, W2-020 | 8 |
| Hardware architecture (G1) | G1-015, G1-016, G1-024, G1-033 | 4 |
| Vehicle-domain architecture (G2) | G2-019, G2-021, G2-022, G2-027, G2-032 | 5 |
| Cloud/edge architecture (G3) | G3-004, G3-005, G3-006, G3-007, G3-013, G3-023 | 6 |

---

## 15.1 AUTOSAR Classic vs Adaptive (W2-002, W2-003, W2-004, W2-016, W2-018, W2-020)

The industry runs two AUTOSAR platforms with incompatible communication models.

- **W2-002 — Classic vs Adaptive interoperability and gateway translation:** signal-based static Classic and service-based dynamic Adaptive must coexist; gateways translate between models, adding latency and complexity.
- **W2-003 — Adaptive Platform immaturity and stabilisation churn:** the Adaptive platform has not gained traction as hoped; R20-11/R21-11/R22-11 churn destabilises long-term bets.
- **W2-004 — Freedom from Interference on Adaptive unsolved:** real-time safety isolation on Adaptive is not yet certifiable to ASIL-D.
- **W2-016 — Classic-to-Adaptive migration requires full communication redesign:** signal-based static model does not map to service-based dynamic model; migration is a rewrite, not a port.
- **W2-018 — Adaptive targets ASIL-B only:** safety integrity gap vs ASIL-D Classic.
- **W2-020 — Adaptive application portability vs vendor implementation divergence:** "portable" applications depend on vendor-specific platform behaviour.

**Root cause:** AUTOSAR standardises interfaces and methodology but not implementations; the two platforms were designed for different compute models (MCU vs HPC) and were never unified. Hybrid (Classic + Adaptive) is the de facto reality (knowledge index cross-cutting theme 7).

---

## 15.2 Mixed-criticality consolidation and hypervisors (W2-009, G2-027, G1-016, G1-024)

Consolidating multiple criticality levels onto one SoC/HPC is the SDV architectural bet, but it is not yet routine.

- **W2-009 — Hypervisor/mixed-criticality consolidation challenges:** virtualisation overhead, certification uncertainty, and tooling gaps.
- **G2-027 — Safety-critical embedded systems: mixed criticality and freedom from interference:** spatial/temporal isolation must be demonstrated, not assumed.
- **G1-016 — SoC vs companion MCU split and safety-island complexity:** safety islands add architectural complexity and verification burden.
- **G1-024 — ECU hardware architecture: mixed-criticality consolidation, thermal, power, redundancy:** hardware consolidation multiplies thermal and power-management problems.

**Root cause:** hypervisor certification evidence and tooling lag the hardware; "freedom from interference" is a property to be proven per platform, not inherited from the hypervisor vendor.

---

## 15.3 Zonal and centralised computing (W1-016, G2-019, G2-021, G2-022)

The vehicle is being re-platformed from distributed ECUs to zone controllers + central HPC + Ethernet backbone.

- **W1-016 — Transition to zonal/centralised architecture: new challenges:** timing, redundancy, single point of failure, thermal.
- **G2-019 — Vehicle gateway and central computing: consolidation and virtualisation.**
- **G2-021 — Zonal architecture: software migration and re-architecture.**
- **G2-022 — HPC: compute resource allocation, scheduling, and thermal.**

**Root cause:** zonal architecture solves wiring cost but concentrates software, timing, and thermal risk into fewer, more complex nodes; migration from distributed ECUs is a re-architecture, not a refactor (G2-021).

---

## 15.4 Middleware: SOME/IP and DDS (W2-007, W2-008)

- **W2-007 — SOME/IP service discovery latency and jitter:** discovery storms, nondeterministic startup, multicast storms.
- **W2-008 — DDS real-time determinism and threading complexity:** QoS tuning is expert-dependent and fragile; DDS real-time behaviour is not guaranteed by configuration defaults.

**Root cause:** service-oriented middleware assumes dynamic discovery, but automotive systems require deterministic startup and bounded latency; the two goals are in tension and resolved per-project by experts.

---

## 15.5 Interface governance and architecture consistency (W1-007, W1-008, W1-017)

- **W1-007 — Unstable interfaces and interface governance gaps:** interfaces change without notification; no single owner.
- **W1-008 — Inconsistent architecture across suppliers/OEM:** each supplier implements its own interpretation.
- **W1-017 — Architecture consistency and change management without visibility:** no global view of architecture; changes ripple unseen.

**Root cause:** interface management is treated as documentation, not as a governed, versioned engineering artifact; tooling does not enforce interface contracts across organisations.

---

## 15.6 MBSE adoption barriers (W1-009, W5-007)

- **W1-009 — MBSE adoption barriers (cultural, skill, tool, governance):** MBSE is the proposed cure for architecture inconsistency but is blocked by culture, skills, tooling, and governance.
- **W5-007 — System/architecture engineering skills gaps:** reinforces W1-009.

**Root cause:** MBSE tools (Rhapsody, Enterprise Architect, PREEvision, SystemDesk) are powerful but require organisational commitment that most OEMs have not made; the skills to operate them are scarce.

---

## 15.7 Vendor lock-in in BSW and MCAL (W2-005, G1-020, G1-021)

- **W2-005 — Vendor lock-in and BSW toolchain interoperability.**
- **G1-020 — MCAL development complexity and verification burden.**
- **G1-021 — MCAL vendor lock-in and toolchain coupling.**

**Root cause:** AUTOSAR standardises the API surface, not the implementation; MCAL and BSW are vendor-specific, so silicon/vendor changes cascade through the whole stack.

---

## 15.8 Cloud and edge architecture (G3-004, G3-005, G3-006, G3-007, G3-013, G3-023)

Cloud-side architecture problems mirror vehicle-side ones.

- **G3-004 — Real-time processing latency: centralised cloud too slow:** edge processing required for safety-relevant functions.
- **G3-005 — Siloed data systems prevent comprehensive vehicle/customer view.**
- **G3-006 — Legacy batch ETL and monolithic databases cannot adapt.**
- **G3-007 — Cloud cost, egress fees and vendor lock-in.**
- **G3-013 — Digital twin synchronisation and fidelity.**
- **G3-023 — Multi-region data residency, sovereignty and hybrid cloud complexity.**

**Root cause:** automotive cloud architectures were built for telematics and OTA, not for real-time fleet intelligence; retrofitting is constrained by data gravity, cost, and sovereignty.

---

## 15.9 Hardware/software co-configuration (G1-033, G1-015)

- **G1-033 — Hardware/software co-configuration complexity (variant binding).**
- **G1-015 — Hardware/software co-design silos and late integration.**

**Root cause:** variant binding between hardware options and software features is managed by spreadsheets and scripts, not by a governed configuration model (see W4-028).

---

## 15.10 Cross-cutting architecture root causes

1. **Two AUTOSAR platforms with incompatible models** force gateway translation and dual skill sets (W2-002, W2-016).
2. **Consolidation concentrates risk** into fewer, more complex nodes (W1-016, G2-022, G2-027).
3. **Interfaces are not governed as engineering artifacts** (W1-007, W1-008, W1-017).
4. **MBSE is blocked by organisation, not by tooling alone** (W1-009, W5-007).
5. **Vendor lock-in is structural** (W2-005, G1-020, G1-021).

---

## 15.11 Evidence limitations

- Adaptive platform maturity assessments rely on AUTOSAR release notes and vendor positioning; independent adoption statistics are scarce.
- Zonal architecture case studies are vendor-led; quantitative migration-cost data is not public.

---

## 15.12 Cross-references

- **Chapter 14 (Technical):** timing limits (W1-010, W2-004) underpin architecture decisions.
- **Chapter 16 (Integration):** architecture problems manifest as integration failures (W1-006, W1-007).
- **Chapter 18 (Toolchain):** MBSE and interface governance depend on tooling (W4-001, W4-007).
- **Chapter 19 (Human Skills):** architecture skills scarcity (W5-007) blocks MBSE.

---

*End of Chapter 15.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

