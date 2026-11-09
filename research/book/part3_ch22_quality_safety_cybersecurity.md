# Part III — Chapter 22: Quality, Safety, and Cybersecurity Challenges

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 22 — Quality, Safety, and Cybersecurity Challenges  
**Source workstreams:** W2, W3, W4, W5, W6, G2, G3, G4  

---

## 22.0 Chapter purpose and problem map

This chapter collects problems of software quality, functional safety (ISO 26262), SOTIF (ISO 21448), and cybersecurity (ISO/SAE 21434, UNECE R155/R156).

| Group | Problem IDs | Count |
|---|---|---|
| Functional safety (W2/W4/W3/W5) | W2-004, W2-018, W3-034, W4-011, W4-012, W5-003, W5-019 | 7 |
| SOTIF | W4-017 | 1 |
| Cybersecurity (W4/W5/G2/G3/G4) | W4-014, W4-015, W5-004, G2-024, G3-009, G4-025 | 6 |
| Quality/code (W4/W3) | W4-008, W4-009, W4-010, W3-003, W3-027 | 5 |
| Safety of ADAS/vehicle domains | G2-007, G2-008, G2-027, G4-024 | 4 |
| Compliance overhead (safety) | W1-014, W3-025, W4-013, W4-026 | 4 |
| AI safety | W5-017, W5-020 | 2 |
| Cloud/compliance | G3-017 | 1 |

---

## 22.1 Functional safety (ISO 26262) (W2-004, W2-018, W3-034, W4-011, W4-012, W5-003, W5-019)

- **W2-004 — Freedom from Interference on Adaptive unsolved.**
- **W2-018 — Adaptive targets ASIL-B only** (safety integrity gap vs ASIL-D Classic).
- **W3-034 — Safety validation statistical evidence and testing effort.**
- **W4-011 — ISO 26262 compliance heavy documentation/process overhead.**
- **W4-012 — Safety case maintenance and change-impact re-assessment cost.**
- **W5-003 — Functional safety (ISO 26262) competency scarcity.**
- **W5-019 — AI-generated code safety and certification gap.**

**Root cause:** functional safety requires statistical evidence and certified toolchains; Adaptive has not reached ASIL-D; safety cases are maintained manually; safety competencies are scarce; AI-generated code cannot be certified.

---

## 22.2 SOTIF (ISO 21448) (W4-017, W1-014)

- **W4-017 — SOTIF validation of unknown unsafe scenarios doesn't scale.**
- **W1-014 — Compliance overhead and standards fragmentation** (includes SOTIF/AI cross-references in ISO 26262 Ed.3).

**Root cause:** SOTIF requires enumerating unknown unsafe scenarios; the scenario space is unbounded; tooling is immature.

---

## 22.3 Cybersecurity (ISO/SAE 21434, UNECE R155/R156) (W4-014, W4-015, W5-004, G2-024, G3-009, G4-025)

- **W4-014 — ISO/SAE 21434 cybersecurity engineering integration cost.**
- **W4-015 — UNECE R155 type-approval and CSMS supply-chain burden.**
- **W5-004 — Cybersecurity expertise shortage.**
- **G2-024 — Vehicle security: attack surface expansion and cybersecurity engineering.**
- **G3-009 — Cloud/backend cybersecurity attack surface.**
- **G4-025 — Cybersecurity of charging interfaces.**

**Root cause:** cybersecurity is bolted onto existing processes; CSMS spans the supply chain; expertise is scarce; attack surface expands with connectivity, cloud, and charging interfaces.

---

## 22.4 Quality and code-level defects (W4-008, W4-009, W4-010, W3-003, W3-027)

- **W4-008 — Static analysis false positives and manual deviation effort.**
- **W4-009 — MISRA/CERT standard version churn and migration burden.**
- **W4-010 — Undecidable MISRA rules require manual review.**
- **W3-003 — MIL/SIL model instability prevents deterministic CI.**
- **W3-027 — Invalid test failures and unreliable test infrastructure (up to 91% invalid).**

**Root cause:** coding standards evolve faster than tool support; some rules are undecidable; model instability and flaky infrastructure erode quality confidence.

---

## 22.5 Safety-critical vehicle domains (G2-007, G2-008, G2-027, G4-024)

- **G2-007 — Braking and brake-by-wire: safety-critical redundancy and feel.**
- **G2-008 — Steering and steer-by-wire: fault tolerance and driver feel.**
- **G2-027 — Safety-critical embedded systems: mixed criticality and freedom from interference.**
- **G4-024 — Functional safety (ASIL) of charging and BMS software.**

**Root cause:** brake-by-wire and steer-by-wire raise fault-tolerance and haptics problems that are safety-critical and hard to validate; mixed criticality requires proven isolation.

---

## 22.6 Compliance overhead (safety) (W1-014, W3-025, W4-013, W4-026, G3-017)

- **W1-014 — Compliance overhead and standards fragmentation.**
- **W3-025 — Compliance documentation burden for testing.**
- **W4-013 — Automotive SPICE assessment overhead and tailoring burden.**
- **W4-026 — Standards evolution and regional regulatory fragmentation.**
- **G3-017 — SUMS evidence, traceability and software identity at fleet scale.**

**Root cause:** compliance activities are organised per-project; evidence is generated manually; standards overlap and evolve; fleet-scale SUMS evidence is immature.

---

## 22.7 AI safety and governance (W5-017, W5-020, W5-025)

- **W5-017 — LLM hallucination in safety-critical engineering.**
- **W5-020 — Explainability and accountability of AI outputs.**
- **W5-025 — Absence of AI governance in engineering workflows.**

**Root cause:** AI outputs cannot be certified; hallucination is unbounded; accountability is unclear; no AI governance framework exists for engineering.

---

## 22.8 Cross-cutting quality/safety/cyber root causes

1. **Safety is a property to be proven, not assumed** (W2-004, G2-027).
2. **Compliance is both genuine safety work and avoidable overhead** (W1-014, W4-011, W4-013).
3. **Cybersecurity spans the supply chain** (W4-015, G3-009).
4. **AI cannot yet be certified** (W5-019, W5-020).
5. **Scenario coverage is unmeasurable** (W4-017, W3-018).

---

## 22.9 Evidence limitations

- Safety/cybersecurity incident data is not publicly available; claims rely on standards and vendor publications.
- AI-safety claims reflect research-stage assessments, not certified practice.

---

## 22.10 Cross-references

- **Chapter 14 (Technical):** timing and fault injection feed safety validation.
- **Chapter 17 (Testing):** safety validation (W3-034) and SOTIF (W4-017).
- **Chapter 18 (Toolchain):** compliance overhead (W4-011 … W4-013).
- **Chapter 19 (Human Skills):** safety/cybersecurity skills (W5-003, W5-004).
- **Chapter 20 (Organisational/Supplier):** cybersecurity supply chain (W4-015).

---

*End of Chapter 22.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

