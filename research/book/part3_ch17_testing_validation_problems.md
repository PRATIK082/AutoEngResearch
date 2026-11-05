# Part III — Chapter 17: Testing and Validation Problems

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 17 — Testing and Validation Problems  
**Source workstreams:** W3, W4, G1, G3, G4  

---

## 17.0 Chapter purpose and problem map

Testing and validation is the dominant cost and schedule driver: up to ~50% of software development cost; V&V is the most time-consuming activity (47% of ~600 professionals, Perforce 2024); up to 91% of failed test executions invalid (ACM ICSE-SEIP 2022). This chapter catalogues the V&V problems.

| Group | Problem IDs | Count |
|---|---|---|
| MIL/SIL/PIL/vECU | W3-001 … W3-009 | 9 |
| HIL | W3-010 … W3-016 | 7 |
| Vehicle/ADAS/environmental | W3-017 … W3-022 | 6 |
| Traceability/spec/coverage | W3-023 … W3-027 | 5 |
| Regression/flakiness/TDM | W3-028 … W3-031 | 4 |
| Unit/fault injection/safety | W3-032 … W3-034 | 3 |
| Hybrid system testing | W3-035 | 1 |
| Test toolchain | W3-036, W4-029 | 2 |
| Test prioritisation | W3-037 | 1 |
| Testing skills | W3-038 | 1 |
| Hardware HIL deps | G1-032, G1-035 | 2 |
| Cloud HIL | G3-014, G3-015 | 2 |
| Energy/thermal V&V | G4-031, G4-032 | 2 |
| ADAS scenario validation | G2-031 | 1 |
| SOTIF validation | W4-017 | 1 |

---

## 17.1 MIL/SIL/PIL/vECU problems (W3-001 … W3-009)

- **W3-001** MIL plant/control models simplified and validated late.
- **W3-002** MIL test cases not reused for code verification.
- **W3-003** MIL/SIL model instability prevents deterministic CI.
- **W3-004** SiL cannot test all HiL functionalities; vECU fidelity.
- **W3-005** Temporal synchronisation in hybrid SiL/HiL.
- **W3-006** SiL simulation inputs limited; realistic stimuli.
- **W3-007** vECU standardisation gap (FMI vs AUTOSAR).
- **W3-008** PIL limitations (unsupported modelling features).
- **W3-009** PIL reveals compiler faults but is slow/hardware-dependent.

**Root cause:** model fidelity and computational cost are in tension; vECU interfaces are not standardised (W3-007), so simulation assets do not compose across vendors; MIL tests are not reused (W3-002), duplicating effort.

---

## 17.2 HIL problems (W3-010 … W3-016, G1-032, G1-035)

- **W3-010** HIL cost and scalability (~$50k+ per bench; ~100–200 tests/day).
- **W3-011** HIL setup latency, reconfiguration, variant explosion.
- **W3-012** HIL model fidelity (simplified plant models).
- **W3-013** HIL sensor/actuator physical constraints and latency.
- **W3-014** HIL fixture/test-bench verification difficulty.
- **W3-015** Cloud-based HIL challenges (latency, cyber, compliance).
- **W3-016** HIL test environment management (idle benches, scheduling).
- **G1-032** HIL hardware dependencies: availability, fidelity, sensor/actuator interfaces.
- **G1-035** Hardware-in-the-loop dependency on ECU/hardware availability.

**Root cause:** HIL benches are scarce, expensive, and non-parallelisable; setup and reconfiguration dominate; hardware availability gates testing.

---

## 17.3 Vehicle-level and ADAS validation (W3-017 … W3-022, G2-031, W4-017)

- **W3-017** Physical proving-ground testing scope explosion and cost.
- **W3-018** ADAS/ADS scenario coverage and completeness unknown.
- **W3-019** Simulation nondeterminism/flakiness in ADS testing (43.6% flaky scenarios in CARLA).
- **W3-020** Test scenario reuse unsafe for ADAS/ADS.
- **W3-021** Environmental/EMC/durability validation physical and slow.
- **W3-022** Test fleet management and real-world data collection.
- **G2-031** ADAS scenario validation and real-world coverage gap.
- **W4-017** SOTIF validation of unknown unsafe scenarios doesn't scale.

**Root cause:** the scenario space is effectively unbounded; simulation is nondeterministic; SOTIF requires enumerating unknown unsafe scenarios, which does not scale.

---

## 17.4 Traceability, specification, and coverage (W3-023 … W3-027, W4-007)

- **W3-023** Lack of traceability across MIL–SIL–HIL (horizontal).
- **W3-024** Test case specification quality problems.
- **W3-025** Compliance documentation burden for testing.
- **W3-026** Incomplete test coverage and time pressure.
- **W3-027** Invalid test failures and unreliable test infrastructure (up to 91% invalid failures).
- **W4-007** End-to-end traceability gaps (req → code → tests → safety).

**Root cause:** vertical (requirement-to-test) traceability exists but horizontal (XiL-level) traceability does not (W3-023); test specs are weak (W3-024); infrastructure unreliability dominates failure triage (W3-027).

---

## 17.5 Regression, flakiness, and test data (W3-028 … W3-031)

- **W3-028** Regression testing scalability and overhead.
- **W3-029** Non-determinism and flaky tests in ML systems.
- **W3-030** Test data management and artifact versioning across XiL.
- **W3-031** Shift-left adoption incomplete; testing still late (only ~33% implemented).

**Root cause:** regression suites grow faster than execution capacity; flakiness erodes trust; test data is not versioned alongside code; shift-left is aspirational.

---

## 17.6 Unit testing, fault injection, and safety validation (W3-032 … W3-034)

- **W3-032** Unit testing informal, weak strategies, poor evidence.
- **W3-033** Fault injection manual, expertise-dependent, incomplete models.
- **W3-034** Safety validation statistical evidence and testing effort (ISO 26262).

**Root cause:** unit testing is not standardised; fault injection requires expert modelling; safety validation requires statistical evidence that is expensive to generate.

---

## 17.7 Hybrid system testing (W3-035)

- **W3-035** Mixed discrete-continuous (hybrid) system testing intractability.

**Root cause:** hybrid systems (discrete control + continuous physics) have no tractable general testing method; coverage criteria are undefined.

---

## 17.8 Test toolchain (W3-036, W4-029, W4-018, W4-019)

- **W3-036** Test tool interoperability and fragmented toolchain.
- **W4-029** Test tool interoperability and coverage evidence management.
- **W4-018** HIL test bench scarcity and scheduling bottlenecks.
- **W4-019** vECU fidelity gaps and integration friction with simulation.

**Root cause:** test tools do not share evidence formats; HIL bench scarcity is an infrastructure problem; vECU fidelity is a standardisation problem (W3-007).

---

## 17.9 Cloud HIL and fleet validation (G3-014, G3-015, W3-015)

- **G3-014** Cloud HIL latency, security and compliance.
- **G3-015** Test fleet management and real-world data collection inefficiency.
- **W3-015** Cloud-based HIL challenges (latency, cyber, compliance).

**Root cause:** cloud HIL promises scale but introduces latency, cybersecurity, and compliance (data residency) barriers.

---

## 17.10 Energy/thermal validation (G4-031, G4-032)

- **G4-031** Energy/thermal HIL and vECU validation gaps.
- **G4-032** Charging interoperability testing coverage.

**Root cause:** energy/thermal models are hard to validate in HIL; charging interoperability requires testing against many charger variants.

---

## 17.11 Testing skills (W3-038, W5-009)

- **W3-038** Knowledge and skill gaps in testing/validation.
- **W5-009** Testing and validation expertise gaps.

**Root cause:** testing is undervalued as a career path; expertise is concentrated in few individuals (W5-012).

---

## 17.12 Cross-cutting testing root causes

1. **Testing is the last bottleneck** (W3-031): shift-left is incomplete.
2. **Invalid failures dominate triage** (W3-027): infrastructure reliability is a first-order problem.
3. **Horizontal traceability is missing** (W3-023): XiL levels are not linked.
4. **Scenario coverage is unmeasurable** (W3-018, W4-017): no completeness metric.
5. **HIL is scarce and slow** (W3-010, W4-018): capacity, not methodology, is the constraint.

---

## 17.13 Evidence limitations

- Perforce 2024 survey figures reflect respondent self-report; sample not independently audited.
- 91% invalid-failure figure derives from four automotive system-testing projects (ACM ICSE-SEIP 2022); generalisability unverified.
- ADAS flakiness figure (43.6%) from a CARLA study; may not transfer to other simulators.

---

## 17.14 Cross-references

- **Chapter 14 (Technical):** model fidelity (W3-001, W3-012) is a technical problem.
- **Chapter 16 (Integration):** integration testing is the validation counterpart.
- **Chapter 18 (Toolchain):** test tool interoperability (W3-036, W4-029).
- **Chapter 19 (Human Skills):** testing skills (W3-038, W5-009).
- **Chapter 22 (Quality/Safety):** safety validation (W3-034) and SOTIF (W4-017).

---

*End of Chapter 17.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

