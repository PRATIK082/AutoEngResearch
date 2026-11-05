# Part III — Chapter 16: Integration Problems

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 16 — Integration Problems  
**Source workstreams:** W1, W2, W3, W4, W5, G1, G2, G3  

---

## 16.0 Chapter purpose and problem map

Integration is where automotive programs fail late and expensively. This chapter collects problems of component, ECU, network, supplier, and fleet integration. It complements Chapter 14 (technical) and Chapter 17 (testing).

| Group | Problem IDs | Count |
|---|---|---|
| Requirements/supplier handoff | W1-006, W1-007 | 2 |
| Integration execution (W5) | W5-008 | 1 |
| RTE/contract | W2-012 | 1 |
| Variant/binding | W1-011, W2-011, G1-033 | 3 |
| ARXML/interop | W1-013, W4-002, W4-003, W4-005 | 4 |
| Supplier/OEM toolchain | W4-023 | 1 |
| Hardware/software integration | G1-015, G1-023, G1-031 | 3 |
| Network integration | W1-018, G2-020, G2-032 | 3 |
| Vehicle-domain integration | G2-006, G2-014, G2-015 | 3 |
| Test-tool integration | W3-036, W4-029 | 2 |
| Cloud/fleet integration | G3-001, G3-018, G3-019, G3-024 | 4 |
| Configuration drift | W4-005 | (included above) |

---

## 16.1 Integration as discovery (W1-006, W1-007, W5-008)

The single most cited root cause of late integration surprises: OEMs deliver incomplete specs; suppliers fill gaps with assumptions; unconsidered interactions surface at integration.

- **W1-006 — Incomplete requirements handed to suppliers (assumption-driven development):** OEM capacity and time pressure; unclear system-requirements ownership.
- **W1-007 — Unstable interfaces and interface governance gaps:** interfaces change without notification; no single owner.
- **W5-008 — Software integration expertise gaps:** integration is a scarce specialism; "integration as discovery" persists because no one owns end-to-end integration until late.

**Root cause:** system-level requirements ownership is unclear between OEM and suppliers; integration is scheduled as a phase rather than being continuously validated.

---

## 16.2 RTE generation contract gaps (W2-012)

- **W2-012 — RTE generation contract phase and consistency validation gaps:** consistency validation between generated RTE and SWC expectations is weak; mismatches surface at integration, not at generation time.

**Root cause:** RTE generation is treated as a build step, not as a verified contract; there is no automated equivalence check between SWC expectations and generated RTE.

---

## 16.3 Variant and binding-time confusion (W1-011, W2-011, G1-033)

- **W1-011 — Variant explosion and configuration management.**
- **W2-011 — Variant management complexity and binding-time confusion:** design-time vs run-time configuration poorly distinguished.
- **G1-033 — Hardware/software co-configuration complexity (variant binding).**

**Root cause:** variant dimensions (hardware, market, software feature, calibration) are managed independently; binding time (compile, link, load, run) is not explicitly modelled, so the same variant is re-derived multiple times with drift.

---

## 16.4 ARXML interoperability and configuration drift (W1-013, W4-002, W4-003, W4-005)

- **W1-013 — ARXML tool interoperability and configuration drift.**
- **W4-002 — Proprietary data formats and ARXML version/dialect fragmentation.**
- **W4-003 — Manual ARXML authoring/editing error-prone and unscalable.**
- **W4-005 — Configuration diff/merge and variant management in ARXML immature.**

**Root cause:** ARXML is verbose, not canonical, and merges badly; round-tripping between tools introduces drift (W2-015). There is no canonical, diffable representation.

---

## 16.5 Supplier/OEM toolchain mismatch (W4-023)

- **W4-023 — Supplier/OEM toolchain mismatch and data exchange friction:** suppliers and OEMs use different tools; data exchange is manual and error-prone.

**Root cause:** toolchains are chosen per organisation for local optimisation; no industry-wide exchange format beyond ARXML, which is itself fragmented (W4-002).

---

## 16.6 Hardware/software integration (G1-015, G1-023, G1-031)

- **G1-015 — Hardware/software co-design silos and late integration.**
- **G1-023 — Hardware bring-up: late discovery and integration surprises.**
- **G1-031 — Hardware revisions and compatibility / BSP patch breakage.**

**Root cause:** hardware and software teams work in parallel with late synchronization; BSP patches break compatibility; bring-up surprises are discovered at integration, not in simulation.

---

## 16.7 Network integration (W1-018, G2-020, G2-032)

- **W1-018 — Network architecture and timing of communication (bus/Ethernet).**
- **G2-020 — Vehicle networking: multi-bus gateways and bandwidth constraints.**
- **G2-032 — Cross-domain integration: motion, energy, ADAS actuator conflicts.**

**Root cause:** network timing budgets are not enforced end-to-end; gateways translate between buses with added latency; cross-domain actuator conflicts emerge when domains are consolidated.

---

## 16.8 Vehicle-domain integration (G2-006, G2-014, G2-015)

- **G2-006 — Chassis/vehicle dynamics: ESC/ABS/TCS integration and sensor fusion.**
- **G2-014 — Vehicle motion control: integrated chassis control and actuator coordination.**
- **G2-015 — Cockpit/digital cockpit: multi-domain display and HMI integration.**

**Root cause:** domain consolidation (motion, cockpit) requires cross-domain actuator and display coordination that the current ECU-per-function architecture does not support natively.

---

## 16.9 Test-tool integration (W3-036, W4-029)

- **W3-036 — Test tool interoperability and fragmented toolchain.**
- **W4-029 — Test tool interoperability and coverage evidence management.**

**Root cause:** test tools (Tessy, VectorCAST, Parasoft, LDRA, GoogleTest) do not share a common result/evidence format; coverage evidence is managed per tool, not per program.

---

## 16.10 Cloud and fleet integration (G3-001, G3-018, G3-019, G3-024)

- **G3-001 — OTA campaign orchestration at fleet scale.**
- **G3-018 — OTA rollback orchestration and safe-state activation at fleet scale.**
- **G3-019 — OTA campaign targeting and dependency management.**
- **G3-024 — Third-party telematics and API integration fragmentation.**

**Root cause:** OTA at fleet scale requires orchestrating dependencies, rollback, and safe-state across heterogeneous vehicle configurations; third-party APIs fragment the integration surface.

---

## 16.11 Cross-cutting integration root causes

1. **Integration is scheduled, not continuous** (W1-006, W5-008).
2. **Interfaces are not governed** (W1-007, W4-023).
3. **Variant binding is implicit** (W1-011, W2-011, G1-033).
4. **ARXML does not compose across tools** (W1-013, W4-002, W4-005).
5. **Hardware and software integrate late** (G1-015, G1-023).

---

## 16.12 Evidence limitations

- Integration-cost quantification is sparse and vendor-sourced; "80% of integration issues surface ~6 months before SOP" (KPIT 2025) is a single-vendor claim.

---

## 16.13 Cross-references

- **Chapter 14 (Technical):** technical limits make integration harder.
- **Chapter 15 (Architecture):** architecture decisions determine integration surface.
- **Chapter 17 (Testing):** integration testing is the validation counterpart.
- **Chapter 18 (Toolchain):** ARXML and toolchain mismatch (W4-002, W4-023) are toolchain problems.
- **Chapter 20 (Organisational/Supplier):** supplier boundaries are organisational.

---

*End of Chapter 16.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

