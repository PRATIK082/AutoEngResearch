# Engineering Bottleneck Catalogue

**Artefact:** #9 (Task.md §15)  
**Source basis:** research/W1–W5, G1–G4, KNOWLEDGE_INDEX.md, part6_ch35_problem_catalogue.md, part3–part7 book chapters  
**Date:** 2026-11-05  
**Scope:** Consolidated catalogue of engineering bottlenecks across the automotive software lifecycle, with root-cause problem IDs, impact, and automation feasibility. Bottleneck IDs (BN-*) are artefact-local keys cross-referencing problem IDs.

---

## 1. How to read this catalogue

- **BN-ID:** artefact-local bottleneck identifier.
- **Problem IDs:** canonical problem records from research (W1–W5, G1–G4) that are root causes or primary contributors.
- **Impact:** qualitative impact class (Time / Cost / Quality / Safety / Schedule), with sourced metrics where available.
- **Frequency/Severity:** sourced where available; otherwise stated as "Pervasive / High" (estimate).
- **Automation feasibility:** Low / Medium / High per source problem records (human judgement retained for safety-critical sign-off).

Numerical values are sourced estimates, not audited facts (see DATA_VERIFY.md).

---

## 2. Requirements and system engineering bottlenecks

| BN-ID | Bottleneck | Lifecycle / Domain | Primary problem IDs | Impact | Frequency/Severity | Existing-solution limitation | Automation feasibility |
|---|---|---|---|---|---|---|---|
| BN-01 | Ambiguous, underspecified natural-language requirements | Requirements | W1-001, W1-005, W5-006 | Rework, late defects | Pervasive | Templates enforce structure not semantics | Medium |
| BN-02 | Unconnected abstraction levels (SHRQ→SyRQ→SWRQ→HWRQ) | Requirements/SE | W1-002, W1-006 | Late integration surprises | High (72%+ agreement) | Links present but wrong | Medium–High |
| BN-03 | Requirements traceability burden and stale links | All | W1-003, W4-007 | Compliance evidence gaps, audit findings | High | Manual linking; cross-domain (ML) uncovered | Medium |
| BN-04 | Late and uncontrolled requirement changes | All | W1-005, W1-017 | Rework, schedule slip | High | CCBs slow; no automated impact propagation | Medium |
| BN-05 | Incomplete requirements handed to suppliers (assumption-driven dev) | System engineering | W1-006 | Integration becomes discovery; launch slip | High | ICDs static; MBSE adoption low | Medium |
| BN-06 | Unstable interfaces and no single source of truth | Architecture/integration | W1-007 | Mismatched interfaces; over-engineering | High | AUTOSAR standardises BSW only | Medium |
| BN-07 | Inconsistent architecture across suppliers/OEM | Architecture | W1-008, W1-017 | Technical debt, recalls | Medium–High | Review boards lack enforcement | Low–Medium |
| BN-08 | MBSE adoption barriers (culture, skill, tool, governance) | Architecture | W1-009, W5-007 | Continued document-centric silos | Medium (22% clear definition) | SysML v2 migration cost | Low (human change) |
| BN-09 | Timing analysis unsolved for AUTOSAR | Architecture/validation | W1-010, W1-018, W2-004, W2-007, W2-008 | Unsafe timing estimates; late violations | High (safety-relevant) | Research-stage; not certified | Medium |
| BN-10 | Variant explosion and configuration management | Configuration/release | W1-011, W2-011, W3-011 | Configuration errors; invalid test coverage | High | Tool-specific; no cross-tool consistency | Medium |
| BN-11 | Requirements-driven rework (largest rework category) | Cross-cutting | W1-012 | 35–45% of total rework cost | Pervasive | Agile reduces cost per change, not volume | Medium |
| BN-12 | ARXML tool interoperability and configuration drift | Configuration | W1-013, W4-002, W4-003, W4-005 | Review difficulty; integration failures | High | Serialization rules not enforced | High |
| BN-13 | Compliance overhead and standards fragmentation | All | W1-014, W4-011–W4-017, W4-026 | Administrative overhead; slower release | High | Overlap not eliminated | Medium |
| BN-14 | System engineering skill shortage | SE | W1-015, W5-007 | Dependency on individuals; quality variance | Structural | Training takes years | Low (human) |
| BN-15 | Zonal/centralised architecture transition challenges | Architecture | W1-016, W2-009, W5-024 | New failure modes; validation complexity | Emerging (high) | Virtualisation overhead; SOA portability limited | Medium |
| BN-16 | Network architecture and timing (bus/Ethernet) | Network | W1-018, W2-007, W2-008 | Timing violations; gateway bottlenecks | Medium–High | Timing analysis unsolved | Medium |
| BN-17 | ML/AI-specific requirements and traceability gaps | Requirements (ADAS) | W1-004, W3-018, W3-029 | Safety case gaps | Emerging | Standards maturing; tooling absent | Low–Medium |

---

## 3. AUTOSAR, middleware and integration bottlenecks

| BN-ID | Bottleneck | Lifecycle / Domain | Primary problem IDs | Impact | Frequency/Severity | Existing-solution limitation | Automation feasibility |
|---|---|---|---|---|---|---|---|
| BN-18 | ARXML configuration complexity and manual burden | AUTOSAR config | W2-001, W4-003 | Errors; unscalable | High | Tools generate but drift | High |
| BN-19 | Classic vs Adaptive interoperability and gateway translation | Integration | W2-002, W2-016 | Semantic/timing loss | High (hybrid reality) | Gateways translate bits not semantics | Medium |
| BN-20 | Adaptive Platform immaturity and stabilisation churn | AUTOSAR Adaptive | W2-003, W2-020 | Migration risk; rework | Medium–High | Standard still evolving | Low–Medium |
| BN-21 | Freedom from interference unsolved (Adaptive real-time safety) | Safety | W2-004, W2-014, W2-018 | Safety case risk | High (safety-critical) | Adaptive targets ASIL-B only | Low |
| BN-22 | Vendor lock-in and BSW toolchain interoperability | Toolchain | W2-005, W4-004 | Cost; switching friction | High | Proprietary; licensing | Medium |
| BN-23 | Multicore distribution complexity (Classic) | AUTOSAR Classic | W2-006, W2-013 | Timing/interference risk | Medium–High | Classic designed single-core | Medium |
| BN-24 | SOME/IP service discovery latency & jitter | Middleware | W2-007 | Real-time path degradation | Medium | Multicast scalability limits | Medium |
| BN-25 | DDS real-time determinism & threading complexity | Middleware | W2-008 | Timing violations | Medium | Threading complexity | Medium |
| BN-26 | Hypervisor/mixed-criticality consolidation | Virtualisation | W2-009, W1-016 | Resource contention; certification | Emerging | Safety case for combined control binding | Low–Medium |
| BN-27 | OTA/FOTA background install, safe-state, rollback | Release/OTA | W2-010, W4-016, G3-018 | Fleet update risk | High (connected vehicles) | Rollback orchestration at scale | Medium |
| BN-28 | RTE generation contract phase & consistency gaps | Integration | W2-012, W2-001 | Late communication failures | High | Manual reconciliation | Medium–High |
| BN-29 | ARXML round-trip & version-control friction | Configuration | W2-015, W4-005 | Merge conflicts; review difficulty | High | XML not diff-friendly | High |
| BN-30 | Adaptive diagnostics/health/NM maturity | AUTOSAR Adaptive | W2-017 | Operational reliability gaps | Medium | Immature services | Low–Medium |
| BN-31 | Classic-to-Adaptive migration requires full comms redesign | Migration | W2-016 | Not incremental; high cost | High | No incremental path | Low–Medium |
| BN-32 | Integration expertise gaps (multi-ECU/BSW/middleware) | Human skills | W5-008, W5-011, W5-012 | Dependency on few specialists | Structural | Integration knowledge tacit | Low–Medium (assistants) |

---

## 4. Testing and validation bottlenecks

| BN-ID | Bottleneck | Lifecycle / Domain | Primary problem IDs | Impact | Frequency/Severity | Existing-solution limitation | Automation feasibility |
|---|---|---|---|---|---|---|---|
| BN-33 | MIL plant/control models simplified and validated late | MIL | W3-001 | Late defect discovery | High | Model fidelity vs. cost trade-off | Medium |
| BN-34 | MIL test cases not reused for code verification | MIL/SIL | W3-002 | Duplicate effort | High | Reuse unsafe without traceability | Medium |
| BN-35 | MIL/SIL model instability prevents deterministic CI | XiL | W3-003 | Flaky pipelines | High | Environment not reproducible | Medium |
| BN-36 | vECU fidelity gaps (SiL cannot test all HiL functions) | SIL/vECU | W3-004, W3-007, W4-019 | False confidence | High | FMI vs AUTOSAR gap | Medium |
| BN-37 | Temporal synchronisation in hybrid SiL/HiL | Co-simulation | W3-005 | Timing errors | Medium | No standard sync | Medium |
| BN-38 | HIL cost and scalability | HIL | W3-010, W4-018, W4-027 | Capex; bench scarcity | High | HIL farms expensive | Low–Medium (cost) |
| BN-39 | HIL setup latency, reconfiguration, variant explosion | HIL | W3-011, W1-011 | Weeks of setup | High | Manual environment creation | Medium |
| BN-40 | HIL model fidelity (simplified plant) | HIL | W3-012 | False passes/fails | High | Plant model accuracy vs. cost | Medium |
| BN-41 | HIL sensor/actuator physical constraints and latency | HIL | W3-013 | I/O fidelity gaps | Medium | Physical interfaces hard to simulate | Medium |
| BN-42 | HIL fixture/test-bench verification difficulty | HIL | W3-014 | Unverified test rigs | Medium | Fixture correctness hard to prove | Medium |
| BN-43 | Cloud HIL (latency, cyber, compliance) | Cloud HIL | W3-015, G3-014 | Remote execution barriers | Emerging | Compliance/latency | Medium |
| BN-44 | HIL test environment management (idle benches, scheduling) | HIL ops | W3-016 | Poor utilisation | High | Scheduling immature | Medium |
| BN-45 | Proving-ground testing scope explosion and cost | Vehicle | W3-017 | Cost/time | High | Physical testing doesn't scale | Low (physical) |
| BN-46 | ADAS/ADS scenario coverage and completeness unknown | ADAS validation | W3-018, W4-017 | Unknown safety confidence | High | Scenario generation not complete | Medium |
| BN-47 | Simulation nondeterminism/flakiness (ADS) | ADAS sim | W3-019 | Unreliable evidence | High | Nondeterminism sources uncontrolled | Medium |
| BN-48 | Test scenario reuse unsafe for ADAS/ADS | ADAS testing | W3-020 | Duplicate scenario work | High | Reuse requires safety case | Medium |
| BN-49 | Environmental/EMC/durability validation physical and slow | Vehicle | W3-021 | Long validation cycles | Medium–High | Physical constraints | Low (physical) |
| BN-50 | Test fleet management and real-world data collection | Vehicle/fleet | W3-022, G3-015 | Data collection inefficiency | Medium | Fleet heterogeneity | Medium |
| BN-51 | Lack of horizontal traceability across MIL–SIL–HIL | Traceability | W3-023, W4-007 | Evidence gaps | High | Links not preserved across levels | Medium |
| BN-52 | Test case specification quality problems | Test docs | W3-024 | Ambiguous tests | High | Manual authoring | Medium |
| BN-53 | Compliance documentation burden for testing | Compliance | W3-025, W4-011, W4-013 | Admin overhead | High | Evidence collection manual | Medium |
| BN-54 | Incomplete test coverage and time pressure | Coverage | W3-026, W3-037 | Escaped defects | High | Risk-based testing underdeveloped | Medium |
| BN-55 | Invalid test failures and unreliable test infrastructure | Execution quality | W3-027 | Wasted debug time | High (up to 91% invalid) | Infrastructure not validated | Medium |
| BN-56 | Regression testing scalability and overhead | Regression | W3-028 | Repeated full runs | High | Test prioritisation underdeveloped | Medium–High |
| BN-57 | Non-determinism and flaky tests in ML systems | ML testing | W3-029 | Unreliable ML evidence | Medium–High | ML nondeterminism inherent | Low–Medium |
| BN-58 | Test data management and artefact versioning across XiL | TDM | W3-030 | Non-reproducible results | High | No standard TDM | Medium–High |
| BN-59 | Shift-left adoption incomplete; testing still late | Process | W3-031 | Late defect discovery | High (~33% implemented) | Pipelines unsuitable for safety-critical | Medium |
| BN-60 | Unit testing informal, weak strategies, poor evidence | Unit testing | W3-032, W4-013 | Weak evidence | High | Tooling/training gaps | Medium |
| BN-61 | Fault injection manual, expertise-dependent, incomplete models | Robustness | W3-033 | Incomplete robustness evidence | Medium | Models incomplete | Medium |
| BN-62 | Safety validation statistical evidence and testing effort | Functional safety | W3-034, W4-011 | Huge test volume | High | Statistical methods immature | Low–Medium |
| BN-63 | Mixed discrete-continuous (hybrid) system testing intractability | Model-based testing | W3-035 | Unverifiable hybrid behaviour | Medium | No tractable methods | Low |
| BN-64 | Test tool interoperability and fragmented toolchain | Toolchain | W3-036, W4-029 | Manual data movement | High | Vendor lock-in | Medium |
| BN-65 | Test prioritisation and risk-based testing underdeveloped | Test strategy | W3-037, W3-026 | Inefficient test spend | High | Methods immature | Medium |
| BN-66 | Testing/validation knowledge and skill gaps | Human skills | W3-038, W5-009 | Quality variance | Structural | Training gaps | Low–Medium |

---

## 5. Toolchain, data and compliance bottlenecks

| BN-ID | Bottleneck | Lifecycle / Domain | Primary problem IDs | Impact | Frequency/Severity | Existing-solution limitation | Automation feasibility |
|---|---|---|---|---|---|---|---|
| BN-67 | Fragmented heterogeneous toolchain, poor interoperability | Toolchain | W4-001, W4-023 | Manual glue work | Pervasive | Point-to-point integrations | Medium–High |
| BN-68 | Proprietary data formats and ARXML version/dialect fragmentation | Data | W4-002, W1-013 | Handoff errors | High | No canonical serialization | High |
| BN-69 | Manual ARXML authoring/editing error-prone and unscalable | Data | W4-003, W2-001 | Errors | High | GUI-heavy, no validation | High |
| BN-70 | AUTOSAR configuration tool lock-in and licensing cost | Toolchain | W4-004, W4-027 | Cost; switching friction | High | Vendor ecosystems | Medium |
| BN-71 | Configuration diff/merge and variant management in ARXML immature | Data | W4-005, W2-015 | Merge conflicts | High | No semantic diff | High |
| BN-72 | Requirements management tools legacy-heavy, hard to onboard | Requirements tools | W4-006 | Slow onboarding | Medium | Legacy UX | Medium |
| BN-73 | End-to-end traceability gaps (req→code→tests→safety) | Traceability | W4-007, W1-003, W3-023 | Evidence gaps | High | Links not validated | Medium |
| BN-74 | Static analysis false positives and manual deviation effort | Code quality | W4-008 | Wasted review time | High | Rule sets noisy | Medium |
| BN-75 | MISRA/CERT version churn and migration burden | Standards | W4-009 | Re-verification cost | Medium–High | Version migration manual | Medium |
| BN-76 | Undecidable MISRA rules require manual review | Code quality | W4-010 | Manual effort | Medium | Cannot be automated fully | Low |
| BN-77 | ISO 26262 compliance heavy documentation/process overhead | Functional safety | W4-011, W4-012 | Admin overhead | High | Evidence collection manual | Medium |
| BN-78 | Safety case maintenance and change-impact re-assessment cost | Functional safety | W4-012 | Costly re-assessment | High | Change impact not automated | Medium |
| BN-79 | Automotive SPICE assessment overhead and tailoring burden | Process quality | W4-013 | Assessment cost | Medium–High | Tailoring organisation-specific | Medium |
| BN-80 | ISO/SAE 21434 cybersecurity engineering integration cost | Cybersecurity | W4-014, W5-004 | Cost; skill shortage | High | TARA/manual | Medium |
| BN-81 | UNECE R155 type-approval and CSMS supply-chain burden | Cybersecurity reg | W4-015 | Supply-chain burden | High | CSMS scope broad | Medium |
| BN-82 | UNECE R156 / ISO 24089 software update engineering complexity | OTA | W4-016, W2-010 | Update engineering cost | High | SUMS evidence burden | Medium |
| BN-83 | SOTIF validation of unknown unsafe scenarios doesn't scale | Safety/ADAS | W4-017, W3-018 | Validation scalability | High (ADAS) | Scenario enumeration incomplete | Medium |
| BN-84 | vECU fidelity gaps and integration friction with simulation | Simulation | W4-019, W3-004 | False confidence | Medium–High | No standard vECU format | Medium |
| BN-85 | Generic CI/CD pipelines unsuitable for safety-critical embedded | DevOps | W4-020 | Unsafe automation risk | High | Pipelines lack safety gating | Medium |
| BN-86 | Long build/compilation times for large AUTOSAR codebases | Build | W4-021 | Slow feedback | Medium–High | Incremental builds limited | Medium |
| BN-87 | Tool onboarding, training time, single-person expertise | Human factors | W4-022, W5-011, W5-013 | Key-person dependency | Structural | Vendor-specific knowledge | Low–Medium |
| BN-88 | Supplier/OEM toolchain mismatch and data exchange friction | Supply chain | W4-023, W4-024, W4-028 | Manual re-entry; errors | High | No common data exchange | Medium–High |
| BN-89 | PLM/PDM vs engineering-data (ARXML) integration gaps | Data management | W4-024 | Manual synchronisation | Medium–High | Engineering data not first-class in PLM | Medium |
| BN-90 | Open-source AUTOSAR tooling immature and unsupported | Tooling | W4-025 | Risk; gaps | Medium | Community support limited | Medium |
| BN-91 | Standards evolution and regional regulatory fragmentation | Standards/regulation | W4-026, W1-014 | Re-qualification churn | Medium–High | Regional divergence | Low (external) |
| BN-92 | Tool licensing, infrastructure and cost constraints | Economics | W4-027, W4-004 | Budget pressure | High | Licensing models rigid | Low (commercial) |
| BN-93 | Manual data transformation via spreadsheets/scripts ("glue work") | Process | W4-028, W4-024 | Errors; hidden effort | Pervasive | No end-to-end data flow | High |
| BN-94 | Test tool interoperability and coverage evidence management | Testing tools | W4-029, W3-036 | Evidence gaps | High | Vendor formats | Medium |
| BN-95 | AI-assisted engineering tooling trust, certification, IP/data constraints | AI tooling | W4-030, W5-016–W5-020, W5-025 | Adoption blocked | Emerging | Governance absent | Medium |

---

## 6. Human skills, workforce and AI adoption bottlenecks

| BN-ID | Bottleneck | Lifecycle / Domain | Primary problem IDs | Impact | Frequency/Severity | Existing-solution limitation | Automation feasibility |
|---|---|---|---|---|---|---|---|
| BN-96 | Embedded software engineer shortage | Workforce | W5-001, W5-015 | Capacity bottleneck | Structural | Compensation disadvantage | Low (human) |
| BN-97 | AUTOSAR expertise gap (Classic & Adaptive) | Workforce | W5-002, W2-019 | Integration slips | Structural | Training slow | Low–Medium (assistants) |
| BN-98 | Functional safety competency scarcity | Workforce | W5-003 | Safety case quality | Structural | Certification burden | Low |
| BN-99 | Cybersecurity expertise shortage | Workforce | W5-004, W4-014 | Security gaps | Structural | Training lag | Low–Medium |
| BN-100 | AI/ML engineering capabilities gap in automotive | Workforce | W5-005, W1-004 | ADAS capability gap | Structural | Education mismatch | Medium (assistants) |
| BN-101 | Requirements engineering skills gaps | Workforce | W5-006, W1-001 | Poor requirements | Structural | Training gaps | Medium |
| BN-102 | System/architecture engineering skills gaps | Workforce | W5-007, W1-009 | Architecture quality | Structural | MBSE skill gap | Low–Medium |
| BN-103 | Software integration expertise gaps | Workforce | W5-008 | Integration slips | Structural | Knowledge tacit | Low–Medium |
| BN-104 | Testing and validation expertise gaps | Workforce | W5-009, W3-038 | Test quality | Structural | Training gaps | Low–Medium |
| BN-105 | Hardware/software co-design knowledge gaps | Workforce | W5-010, G1-015 | Late integration | Structural | Siloed disciplines | Low–Medium |
| BN-106 | Toolchain knowledge gaps (vendor-specific) | Workforce | W5-011, W4-022 | Key-person dependency | Structural | Vendor-specific | Low–Medium |
| BN-107 | Knowledge silos, single-person expertise, turnover loss | Knowledge mgmt | W5-012, W4-022, W5-023 | Knowledge loss | Structural | Documentation incomplete | Medium (capture) |
| BN-108 | Training and onboarding time/cost | Workforce | W5-013, W4-022 | Slow ramp-up | High | Training long | Medium |
| BN-109 | Communication gaps across disciplines/organisations | Collaboration | W5-014, W1-007 | Misalignment | High | No shared system model | Low–Medium |
| BN-110 | Compensation disadvantage vs tech-native rivals | Workforce | W5-015 | Talent attrition | Structural | Market-driven | Low (external) |
| BN-111 | AI adoption barriers: confidentiality, IP, NDA constraints | AI adoption | W5-016, W4-030 | Adoption blocked | High | Data governance absent | Medium |
| BN-112 | LLM hallucination in safety-critical engineering | AI reliability | W5-017, W5-020 | Trust barrier | High | No verification | Low–Medium |
| BN-113 | Lack of automotive-specific datasets for fine-tuning local LLMs | AI data | W5-018 | Poor AI quality | Medium | Data silos | Medium |
| BN-114 | AI-generated code safety and certification gap | AI safety | W5-019, W4-030 | Certification blocker | High | No certified AI code | Low |
| BN-115 | Explainability and accountability of AI outputs | AI governance | W5-020, W5-025 | Accountability gap | High | No governance framework | Medium |
| BN-116 | Legacy skills obsolescence vs new digital competencies | Workforce | W5-021 | Reskilling burden | Medium–High | Training lag | Low–Medium |
| BN-117 | Education pipeline mismatch | Workforce | W5-022 | Talent shortage | Structural | Curricula lag | Low (external) |
| BN-118 | Aging workforce / retirement cliff | Workforce | W5-023, W5-012 | Knowledge loss | Structural | Succession planning weak | Low |
| BN-119 | SDV / centralised computing skill gap | Workforce | W5-024, W1-016 | SDV capability gap | Emerging | Training absent | Low–Medium |
| BN-120 | Absence of AI governance in engineering workflows | AI governance | W5-025, W4-030 | Uncontrolled AI use | Emerging | No framework | Medium |

---

## 7. Hardware, semiconductor and vehicle-domain bottlenecks (selected)

| BN-ID | Bottleneck | Lifecycle / Domain | Primary problem IDs | Impact | Frequency/Severity | Existing-solution limitation | Automation feasibility |
|---|---|---|---|---|---|---|---|
| BN-121 | Semiconductor supply-chain opacity and shortages | Supply chain | G1-001–G1-006, G1-004 | Production loss; feature deletion | High (demonstrated) | Visibility stops at Tier-1 | Low (external) |
| BN-122 | Obsolescence mismatch (vehicle 15+ yrs vs silicon <7 yrs) | Lifecycle | G1-007–G1-009 | ECU redesign; requalification | Structural | Last-time buy risk | Low |
| BN-123 | AEC-Q100 qualification burden and temperature-grade gaps | Qualification | G1-010, G1-011, G1-012 | Cost; availability | Medium–High | Qualification slow | Low |
| BN-124 | Hardware/software co-design silos and late integration | HW/SW | G1-015, G1-022, G1-023, G1-031, G1-035 | Late discovery | High | Interfaces agreed late | Medium |
| BN-125 | Memory wall and external flash bandwidth | Architecture | G1-017–G1-019 | Performance constraints | Medium–High | Memory not scaling | Medium |
| BN-126 | MCAL development complexity and vendor lock-in | Drivers/MCAL | G1-020, G1-021, W2-005 | Lock-in; verification burden | High | MCAL toolchain coupled | Medium |
| BN-127 | Sensor/actuator interface fragmentation | Hardware | G1-027, G1-028 | Integration pain | Medium–High | No standard interfaces | Medium |
| BN-128 | HIL hardware dependencies (availability, fidelity) | Validation | G1-032, G1-035, W3-010 | Software gated by hardware | High | HIL depends on ECU HW | Medium |
| BN-129 | ADAS perception and scenario coverage gap | ADAS | G2-001, G2-031, W3-018, W4-017 | Unknown safety confidence | High | Scenario enumeration incomplete | Medium |
| BN-130 | Cross-domain actuator conflicts (motion/energy/ADAS) | Vehicle integration | G2-014, G2-032, G2-006, G2-011, G2-013 | Hidden interaction bugs | High | Combinatorial testing impractical | Medium |
| BN-131 | EV energy/thermal management complexity | EV/thermal | G4-001–G4-011, G4-018, G4-024, G4-031 | Range/safety trade-offs | High | Coupled multi-domain control | Medium |
| BN-132 | OTA at fleet scale (orchestration, rollback, targeting) | Cloud/fleet | G3-001, G3-002, G3-017, G3-018, G3-019 | Fleet update risk | High | Bandwidth/connectivity heterogeneity | Medium |
| BN-133 | Cloud/edge skill gaps and data governance | Workforce/data | G3-026, G3-020, G3-008 | Capability/cost | Medium–High | Governance immature | Low–Medium |

---

## 7. Bootloader & flash programming bottlenecks

| BN-ID | Bottleneck | Lifecycle / Domain | Primary problem IDs | Impact | Frequency/Severity | Existing-solution limitation | Automation feasibility |
|---|---|---|---|---|---|---|---|
| BN-134 | No standardized automotive bootloader | Architecture | W6-001 | Fragmented tooling/skills; OTA reuse blocked | Pervasive | AUTOSAR excludes bootloader | Low-Medium |
| BN-135 | Secure boot implementation complexity | Security | W6-003, W6-018, W6-019 | Exploitable boot path; bricking risk | High consequence | HSM/crypto vendor variation | Low-Medium |
| BN-136 | UDS flash programming protocol friction | Diagnostics/flash | W6-007, W6-016 | Integration failures; service downtime | Frequent | Vendor extensions; timing ambiguity | Medium-High |
| BN-137 | Flash programming under power transients | Power/flash | W6-008, W6-014 | Bricking; field recovery | High consequence | Hardware-dependent; no standard | Medium |
| BN-138 | Bootloader testing and recovery gaps | Validation | W6-009, W6-012, W6-013 | Defect escape; late rework | Pervasive | HIL cost; coverage gaps | Medium-High |
| BN-139 | Manufacturing/EOL flashing throughput and traceability | Manufacturing | W6-015, W6-023 | Line throughput; scrap; audit | Every vehicle | Tool fragmentation; manual glue | High |
| BN-140 | OTA/bootloader orchestration and multi-ECU consistency | Cloud/fleet | W6-025, W6-026, W6-027 | Field inconsistency; safety risk | High (connected) | No campaign-level transaction | Medium |

## 8. Summary statistics

| Category | Count (BN-IDs) | Range |
|---|---|---|
| Requirements & system engineering | 17 | BN-01 … BN-17 |
| AUTOSAR, middleware & integration | 15 | BN-18 … BN-32 |
| Testing & validation | 34 | BN-33 … BN-66 |
| Toolchain, data & compliance | 29 | BN-67 … BN-95 |
| Human skills, workforce & AI adoption | 25 | BN-96 … BN-120 |
| Hardware, semiconductor & vehicle domains | 13 | BN-121 … BN-133 |
| Bootloader & flash programming | 7 | BN-134 � BN-140 |
| **Total** | **140** | BN-01 � BN-140 |

**Note:** BN-IDs are artefact-local and map to problem IDs; they are not a replacement for the canonical problem catalogue (part6_ch35). Testing/validation and toolchain dominate the count, consistent with the finding that testing is the dominant cost/schedule driver (up to 50% of development cost) and tool fragmentation is a root cause (KNOWLEDGE_INDEX cross-cutting themes 8, 2).

---

## 9. Evidence and source references (selected)

- research/KNOWLEDGE_INDEX.md (problem index, cross-cutting themes)
- research/W1–W5 reports (problem records with evidence)
- research/G1–G4 reports (hardware, vehicle domains, cloud/fleet, energy/thermal)
- research/part6_ch35_problem_catalogue.md (master problem catalogue, ~258 records)
- research/part2_ch10_integration.md (integration bottlenecks and quantitative evidence)
- research/DATA_VERIFY.md (verification status of numerical claims)
- research/CROSS_CHECK.md / CROSS_QUESTIONS.md (cross-domain questions)

---

*End of artefact: bottleneck_catalogue.md*


---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

