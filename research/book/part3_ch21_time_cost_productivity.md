# Part III — Chapter 21: Time, Cost, and Productivity Bottlenecks

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 21 — Time, Cost, and Productivity Bottlenecks  
**Source workstreams:** W1, W3, W4, W5, G1, G3, G4  

---

## 21.0 Chapter purpose and problem map

This chapter collects the bottlenecks that consume engineering time and budget. Quantities are cited only where sourced; estimates are flagged.

| Group | Problem IDs | Count |
|---|---|---|
| Requirements rework | W1-012 | 1 |
| HIL cost/scalability | W3-010, W4-018 | 2 |
| Coverage/time pressure | W3-026 | 1 |
| Regression overhead | W3-028 | 1 |
| Build time | W4-021 | 1 |
| Tool licensing/cost | W4-027 | 1 |
| Glue work | W4-028 | 1 |
| AUTOSAR tool lock-in cost | W4-004 | 1 |
| Training/onboarding | W5-013, W4-022 | 2 |
| Semiconductor shortage/obsolescence | G1-005, G1-008, G1-009 | 3 |
| Cloud cost/lock-in | G3-007, G3-028 | 2 |
| Cloud data volume | G3-003 | 1 |
| Charging interoperability testing | G4-032 | 1 |
| Test fleet management | G3-015 | 1 |

---

## 21.1 Requirements-driven rework (W1-012)

- **W1-012 — Requirements-driven rework cost and late defect discovery:** estimated 35–45% of total rework cost (Capers Jones/Namcook Analytics, cross-industry, not automotive-specific).

**Root cause:** compounding effect of W1-001 (ambiguity), W1-005 (late changes), W1-006 (incomplete specs). Rework cost is largely invisible in project accounting.

---

## 21.2 HIL cost and scalability (W3-010, W4-018, W3-016)

- **W3-010 — HIL cost and scalability:** ~$50k+ per bench; ~100–200 tests/day; cannot parallelise.
- **W4-018 — HIL test bench scarcity and scheduling bottlenecks.**
- **W3-016 — HIL test environment management (idle benches, scheduling).**

**Root cause:** HIL benches are capital-intensive and non-virtualisable; scheduling is manual; idle time is wasted.

---

## 21.3 Test coverage and time pressure (W3-026, W3-028)

- **W3-026 — Incomplete test coverage and time pressure.**
- **W3-028 — Regression testing scalability and overhead.**

**Root cause:** test suites grow faster than execution capacity; coverage targets are traded against schedule; regression overhead is not measured.

---

## 21.4 Build and compilation time (W4-021)

- **W4-021 — Long build/compilation times for large AUTOSAR codebases.**

**Root cause:** ARXML-generated code volume and toolchain inefficiency; builds are not incremental across tool boundaries.

---

## 21.5 Tool licensing, infrastructure, and cost (W4-027, W4-004, W4-028)

- **W4-027 — Tool licensing, infrastructure and cost constraints.**
- **W4-004 — AUTOSAR configuration tool lock-in and licensing cost.**
- **W4-028 — Manual data transformation via spreadsheets/scripts ("glue work").**

**Root cause:** expensive per-seat licences, dongles, on-premise infrastructure; a large share of engineering effort is glue work between tools.

---

## 21.6 Training and onboarding (W5-013, W4-022)

- **W5-013 — Training and onboarding time/cost.**
- **W4-022 — Tool onboarding, training time, single-person expertise.**

**Root cause:** tools are vendor-specific and complex; training is informal; new engineers take months to become productive.

---

## 21.7 Semiconductor shortage, obsolescence, and inventory (G1-005, G1-008, G1-009)

- **G1-005 — Chip shortages caused massive production loss and feature deletion.**
- **G1-008 — Obsolescence-driven ECU redesign and requalification cost.**
- **G1-009 — Last-time buy inventory risk and capital tie-up.**

**Root cause:** semiconductor supply volatility and EOL mismatch force last-time buys, inventory capital tie-up, and redesign cost.

---

## 21.8 Cloud cost and data volume (G3-003, G3-007, G3-028, G3-015)

- **G3-003 — Connected-vehicle data volume overwhelms cloud pipelines.**
- **G3-007 — Cloud cost, egress fees and vendor lock-in.**
- **G3-028 — Cost and complexity of connected-vehicle cloud infrastructure at scale.**
- **G3-015 — Test fleet management and real-world data collection inefficiency.**

**Root cause:** vehicle data volumes exceed pipeline capacity; cloud egress and storage costs scale nonlinearly; test fleet management is inefficient.

---

## 21.9 Charging interoperability testing cost (G4-032)

- **G4-032 — Charging interoperability testing coverage.**

**Root cause:** charging interoperability requires testing against many charger variants, a cost that scales with the number of EVSE models and firmware versions.

---

## 21.10 Cross-cutting productivity root causes

1. **Rework is the largest invisible cost** (W1-012).
2. **HIL capacity, not methodology, is the constraint** (W3-010, W4-018).
3. **Glue work is unmeasured** (W4-028).
4. **Onboarding is months, not weeks** (W5-013, W4-022).
5. **Supply-chain volatility converts to program cost** (G1-005, G1-008).

---

## 21.11 Evidence limitations

- Cost figures (HIL bench cost, build time) are indicative vendor/industry figures, not audited.
- Rework percentage is cross-industry (Capers Jones), not automotive-specific.
- Cloud cost claims are vendor/trade-group estimates.

---

## 21.12 Cross-references

- **Chapter 14 (Technical):** build time (W4-021) and HIL (W3-010).
- **Chapter 17 (Testing):** coverage and regression (W3-026, W3-028).
- **Chapter 18 (Toolchain):** licensing (W4-027) and glue work (W4-028).
- **Chapter 19 (Human Skills):** onboarding (W5-013).
- **Chapter 20 (Organisational/Supplier):** supply-chain cost (G1-005).

---

*End of Chapter 21.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

