# Part VI — Consolidated Problem Discovery

## Chapter 35 — Complete Automotive Engineering Problem Catalogue

**Book:** *The State of Automotive Software Engineering*  
**Part:** VI — Consolidated Problem Discovery  
**Chapter:** 35 — Complete Automotive Engineering Problem Catalogue  
**Sources:** research/W1..W5, G1..G4; KNOWLEDGE_INDEX.md; Part III Chapters 14–22  
**Total problem records:** ~258 (W1: 18, W2: 20, W3: 38, W4: 30, W5: 25, G1: 35, G2: 32, G3: 28, G4: 32)

---

## 35.0 How to read this catalogue

This chapter is the **master problem catalogue** for the entire research. Every significant problem discovered across the nine workstreams is listed here with a stable **Problem ID**. IDs are of the form `<workstream>-<number>` (e.g. `W3-018`, `G1-007`). They are the canonical keys used throughout the book: Part III chapters group them by theme, Part IV maps them to existing solutions, and Part VI (this part) consolidates them.

**Evidence tiers (see KNOWLEDGE_INDEX §6 and each source report):**
- Problem records follow the structured format: Domain, Lifecycle stage, Problem, Root cause, Evidence, Automation feasibility, AI applicability, Human approval, Confidence, Unanswered questions.
- Numerical claims are sourced estimates, not audited facts.
- Confidence is stated per record (Medium/Medium-High/High); no value is invented.

**Counts by workstream:**

| Workstream | Count | Focus |
|---|---|---|
| W1 Requirements & System Engineering | 18 | Requirements, architecture, timing, variants, compliance, skills |
| W2 AUTOSAR Classic/Adaptive & Middleware | 20 | ARXML, interop, Adaptive maturity, safety, OTA, middleware, skills |
| W3 Testing & Validation | 38 | MIL/SIL/PIL/HIL/vehicle, scenario coverage, traceability, compliance |
| W4 Toolchain & Standards | 30 | Fragmentation, ARXML, compliance overhead, HIL, CI/CD, cost |
| W5 Human Skills & AI/Automation | 25 | Talent gaps, knowledge loss, AI barriers, governance |
| G1 Semiconductor & Hardware | 35 | Silicon, MCAL, BSP, memory, power, sensors, obsolescence |
| G2 Vehicle Domains | 32 | ADAS, powertrain, EV, chassis, cockpit, networking, zonal, HPC |
| G3 Cloud, Backend & Fleet | 28 | OTA orchestration, data volume, latency, privacy, security, fleet |
| G4 Energy & Thermal | 32 | SoC/SoH, charging, thermal runaway, energy management |
| **Total** | **~258** | |

---

## 35.1 Semiconductor and hardware (G1-001 … G1-035)

| ID | Title (short) |
|---|---|
| G1-001 | Semiconductor content growth outpaces design/manufacturing capacity |
| G1-002 | Automotive SoCs compete with AI/datacenter for advanced nodes and memory |
| G1-003 | Mature-node fab underinvestment and capacity reallocation |
| G1-004 | Multi-tier supply-chain opacity beyond Tier-1 |
| G1-005 | Chip shortages caused massive production loss and feature deletion |
| G1-006 | Geopolitical concentration of semiconductor manufacturing |
| G1-007 | Vehicle service life (15+ years) vs semiconductor EOL (<7 years) mismatch |
| G1-008 | Obsolescence-driven ECU redesign and requalification cost |
| G1-009 | Last-time buy inventory risk and capital tie-up |
| G1-010 | AEC-Q100 qualification burden and temperature-grade availability gaps |
| G1-011 | AEC-Q100 temperature-grade mismatch with real mission profiles |
| G1-012 | Zero-defect (ppm) pressure and screening cost |
| G1-013 | Advanced node (<7 nm) reliability and automotive qualification challenges |
| G1-014 | Chiplet standardisation and automotive qualification gaps |
| G1-015 | Hardware/software co-design silos and late integration |
| G1-016 | SoC vs companion MCU split and safety-island complexity |
| G1-017 | Memory wall: compute growth vs memory bandwidth/capacity/safety |
| G1-018 | Embedded flash (eNVM) scalability and cost at advanced nodes |
| G1-019 | External flash bandwidth limitations for execute-in-place (XiP) |
| G1-020 | MCAL development complexity and verification burden |
| G1-021 | MCAL vendor lock-in and toolchain coupling |
| G1-022 | BSP fragility across hardware revisions |
| G1-023 | Hardware bring-up: late discovery and integration surprises |
| G1-024 | ECU hardware architecture: mixed-criticality consolidation, thermal, power, redundancy |
| G1-025 | Power management complexity (PMIC, cold crank, EMI, thermal) |
| G1-026 | Power management standardisation gap at vehicle level |
| G1-027 | Sensor and actuator interface fragmentation and proprietary SerDes |
| G1-028 | Sensor integration pain and lack of standardised hardware interfaces |
| G1-029 | ASIC/FPGA development: long cycles, tape-out risk, mixed-signal verification |
| G1-030 | FPGA-to-ASIC conversion portability pitfalls |
| G1-031 | Hardware revisions and compatibility / BSP patch breakage |
| G1-032 | HIL hardware dependencies: availability, fidelity, sensor/actuator interfaces |
| G1-033 | Hardware/software co-configuration complexity (variant binding) |
| G1-034 | Semiconductor talent shortage (design, process, test) |
| G1-035 | Hardware-in-the-loop dependency on ECU/hardware availability |

**Cross-cutting root cause:** obsolescence is structural (G1-007 … G1-009); MCAL is the hidden lock-in point (G1-020, G1-021, W2-005); hardware/software co-design is late and siloed (G1-015, G1-022, G1-023); memory and power are first-class constraints (G1-017 … G1-019, G1-025, G1-026); sensor/actuator interfaces are not standardised (G1-027, G1-028).

---

## 35.2 Vehicle domains (G2-001 … G2-032)

| ID | Title (short) |
|---|---|
| G2-001 | ADAS perception: sensor fusion, ML model lifecycle, ODD specification |
| G2-002 | ADAS real-time scheduling and priority assignment risks |
| G2-003 | Body electronics: BCM feature creep and central body controller complexity |
| G2-004 | Powertrain/engine management: calibration complexity and emissions compliance |
| G2-005 | Transmission/drivetrain control: shift quality and coordination |
| G2-006 | Chassis/vehicle dynamics: ESC/ABS/TCS integration and sensor fusion |
| G2-007 | Braking and brake-by-wire: safety-critical redundancy and feel |
| G2-008 | Steering and steer-by-wire: fault tolerance and driver feel |
| G2-009 | Suspension and active suspension: control algorithms and road preview |
| G2-010 | EV powertrain: inverter control, e-motor, torque management |
| G2-011 | BMS: SoC/SoH estimation accuracy and cell balancing |
| G2-012 | Charging systems: protocol interoperability and grid interaction |
| G2-013 | Thermal management: coupled multi-domain control |
| G2-014 | Vehicle motion control: integrated chassis control and actuator coordination |
| G2-015 | Cockpit/digital cockpit: multi-domain display and HMI integration |
| G2-016 | IVI: OS fragmentation, app ecosystem, smartphone projection |
| G2-017 | Instrument clusters: safety-critical rendering and tell-tales |
| G2-018 | Telematics/connectivity: TCU software, OTA, data management |
| G2-019 | Gateway and central computing: consolidation and virtualisation |
| G2-020 | Vehicle networking: multi-bus gateways and bandwidth constraints |
| G2-021 | Zonal architecture: software migration and re-architecture |
| G2-022 | HPC: compute resource allocation, scheduling, thermal |
| G2-023 | Vehicle diagnostics: DTC standardisation and proprietary extensions |
| G2-024 | Vehicle security: attack surface expansion and cybersecurity engineering |
| G2-025 | Vehicle energy management: EV range optimisation and energy prediction |
| G2-026 | Vehicle access and comfort systems: keyless entry and comfort features |
| G2-027 | Safety-critical embedded systems: mixed criticality and freedom from interference |
| G2-028 | Aerospace-inspired automotive technologies: certification and reuse gap |
| G2-029 | Commercial vehicles and off-highway: harsh environment and diverse variants |
| G2-030 | Two-/three-wheeler electronics: cost constraints and feature scaling |
| G2-031 | ADAS scenario validation and real-world coverage gap |
| G2-032 | Cross-domain integration: motion, energy, ADAS actuator conflicts |

---

## 35.3 Cloud, backend and fleet (G3-001 … G3-028)

| ID | Title (short) |
|---|---|
| G3-001 | OTA campaign orchestration at fleet scale |
| G3-002 | Bandwidth and connectivity heterogeneity for OTA delivery |
| G3-003 | Connected-vehicle data volume overwhelms cloud pipelines |
| G3-004 | Real-time processing latency: centralised cloud too slow |
| G3-005 | Siloed data systems prevent comprehensive vehicle/customer view |
| G3-006 | Legacy batch ETL and monolithic databases cannot adapt |
| G3-007 | Cloud cost, egress fees and vendor lock-in |
| G3-008 | Data privacy, consent and regulatory compliance (GDPR/CCPA/EU Data Act) |
| G3-009 | Cloud/backend cybersecurity attack surface (APIs, vSOC) |
| G3-010 | Fleet-management platform fragmentation across mixed-OEM fleets |
| G3-011 | Lack of common vehicle-data standards and interoperability |
| G3-012 | Remote diagnostics and predictive maintenance pipeline gaps |
| G3-013 | Digital twin synchronisation and fidelity |
| G3-014 | Cloud HIL latency, security and compliance |
| G3-015 | Test fleet management and real-world data collection inefficiency |
| G3-016 | Vehicle-to-cloud connectivity reliability (dead zones) |
| G3-017 | SUMS evidence, traceability and software identity at fleet scale |
| G3-018 | OTA rollback orchestration and safe-state activation at fleet scale |
| G3-019 | OTA campaign targeting and dependency management |
| G3-020 | Cloud data governance, lineage and quality (veracity) |
| G3-021 | Real-time fleet health monitoring and anomaly detection |
| G3-022 | Data monetisation and customer consent economics |
| G3-023 | Multi-region data residency, sovereignty and hybrid cloud complexity |
| G3-024 | Third-party telematics and API integration fragmentation |
| G3-025 | Cloud observability, logging and audit at fleet scale |
| G3-026 | Cloud/edge skill gaps |
| G3-027 | OEM business-model shift to platform operator |
| G3-028 | Cost and complexity of connected-vehicle cloud infrastructure at scale |

---

## 35.4 Energy and thermal (G4-001 … G4-032)

| ID | Title (short) |
|---|---|
| G4-001 | SoC estimation uncertainty and drift |
| G4-002 | SoH estimation and battery aging model inaccuracy |
| G4-003 | Cell balancing limitations |
| G4-004 | Thermal runaway detection latency and false positives/negatives |
| G4-005 | Battery thermal model fidelity and sensor placement |
| G4-006 | Fast-charging thermal derating and preconditioning control |
| G4-007 | Cold-weather range loss and heating energy management |
| G4-008 | Heat pump software control complexity and efficiency |
| G4-009 | Cabin HVAC energy consumption vs comfort trade-off |
| G4-010 | Regenerative braking blending with friction brake |
| G4-011 | Vehicle energy management optimisation across conflicting objectives |
| G4-012 | DC fast charging curve control and charger-vehicle communication timing |
| G4-013 | ISO 15118 Plug & Charge PKI and certificate management complexity |
| G4-014 | OCPP version fragmentation and interoperability |
| G4-015 | Charging protocol heterogeneity (CCS/CHAdeMO/GB-T/NACS) |
| G4-016 | Charging session reliability (timeouts, retries, drops) |
| G4-017 | V2G bidirectional charging complexity and grid safety |
| G4-018 | High-voltage safety monitoring (isolation, contactor welding, precharge) |
| G4-019 | 12V/48V auxiliary power net and load shedding |
| G4-020 | Range estimation (GOM) uncertainty and driver trust |
| G4-021 | Battery cell voltage measurement accuracy and scaling (modular BMS) |
| G4-022 | Thermal management of power electronics and e-motor |
| G4-023 | Charging software OTA and version management (EVSE + vehicle) |
| G4-024 | Functional safety (ASIL) of charging and BMS software |
| G4-025 | Cybersecurity of charging interfaces |
| G4-026 | Fleet energy management and predictive analytics (cloud) |
| G4-027 | Battery preconditioning and thermal scheduling for fast charge |
| G4-028 | Energy recuperation and one-pedal driving calibration |
| G4-029 | High-voltage battery diagnostics and DTC complexity |
| G4-030 | Thermal runaway propagation containment and software response |
| G4-031 | Energy/thermal HIL and vECU validation gaps |
| G4-032 | Charging interoperability testing coverage |

---

## 35.5 Requirements and system engineering (W1-001 … W1-018)

| ID | Title (short) |
|---|---|
| W1-001 | Ambiguous, underspecified natural-language requirements |
| W1-002 | Unconnected abstraction levels across OEM/Tier-1/Tier-2 |
| W1-003 | Requirements traceability burden and link quality |
| W1-004 | ML/AI-specific requirements and traceability gaps (ADAS/perception) |
| W1-005 | Late and uncontrolled requirement changes |
| W1-006 | Incomplete requirements handed to suppliers (assumption-driven dev) |
| W1-007 | Unstable interfaces and interface governance gaps |
| W1-008 | Inconsistent architecture across suppliers/OEM |
| W1-009 | MBSE adoption barriers (cultural, skill, tool, governance) |
| W1-010 | Timing analysis and predictability unsolved for AUTOSAR |
| W1-011 | Variant explosion and configuration management |
| W1-012 | Requirements-driven rework cost and late defect discovery |
| W1-013 | ARXML tool interoperability and configuration drift |
| W1-014 | Compliance overhead and standards fragmentation |
| W1-015 | System engineering skill and workforce gaps |
| W1-016 | Transition to zonal/centralised architecture: new challenges |
| W1-017 | Architecture consistency and change management without visibility |
| W1-018 | Network architecture and timing of communication (bus/Ethernet) |

---

## 35.6 AUTOSAR Classic/Adaptive and middleware (W2-001 … W2-020)

| ID | Title (short) |
|---|---|
| W2-001 | ARXML Configuration Complexity and Manual Burden |
| W2-002 | Classic vs Adaptive Interoperability & Gateway Translation |
| W2-003 | Adaptive Platform Immaturity & Stabilization Churn |
| W2-004 | Freedom from Interference on Adaptive: Unsolved Real-Time Safety |
| W2-005 | Vendor Lock-in and BSW Toolchain Interoperability |
| W2-006 | Multicore Distribution Complexity in Classic |
| W2-007 | SOME/IP Service Discovery Latency & Jitter |
| W2-008 | DDS Real-Time Determinism & Threading Complexity |
| W2-009 | Hypervisor/Mixed-Criticality Consolidation Challenges |
| W2-010 | OTA/FOTA Background Install, Safe-State Activation, Rollback |
| W2-011 | Variant Management Complexity and Binding-Time Confusion |
| W2-012 | RTE Generation Contract Phase & Consistency Validation Gaps |
| W2-013 | Classic OS (OSEK) Scheduling Limitations |
| W2-014 | Adaptive Dynamic Memory Allocation Non-Determinism |
| W2-015 | ARXML Round-Trip & Version-Control Friction |
| W2-016 | Classic-to-Adaptive Migration Requires Full Communication Redesign |
| W2-017 | Adaptive Platform Diagnostics, Health Management & NM Maturity |
| W2-018 | Safety Integrity Level Gap: Adaptive Targets ASIL-B |
| W2-019 | Skill/Knowledge Gaps in AUTOSAR Methodology and Tooling |
| W2-020 | Adaptive Application Portability vs. Vendor Implementation Divergence |

---

## 35.7 Testing and validation (W3-001 … W3-038)

| ID | Title (short) |
|---|---|
| W3-001 | MIL plant/control models simplified, validated late |
| W3-002 | MIL test cases not reused for code verification |
| W3-003 | MIL/SIL model instability prevents deterministic CI |
| W3-004 | SiL cannot test all HiL functionalities; vECU fidelity |
| W3-005 | Temporal synchronisation in hybrid SiL/HiL |
| W3-006 | SiL simulation inputs limited; realistic stimuli |
| W3-007 | vECU standardisation gap (FMI vs AUTOSAR) |
| W3-008 | PIL limitations (unsupported modelling features) |
| W3-009 | PIL reveals compiler faults but slow/hardware-dependent |
| W3-010 | HIL cost and scalability |
| W3-011 | HIL setup latency, reconfiguration, variant explosion |
| W3-012 | HIL model fidelity (simplified plant models) |
| W3-013 | HIL sensor/actuator physical constraints and latency |
| W3-014 | HIL fixture/test-bench verification difficulty |
| W3-015 | Cloud-based HIL challenges (latency, cyber, compliance) |
| W3-016 | HIL test environment management (idle benches, scheduling) |
| W3-017 | Physical proving-ground testing scope explosion and cost |
| W3-018 | ADAS/ADS scenario coverage and completeness unknown |
| W3-019 | Simulation nondeterminism/flakiness in ADS testing |
| W3-020 | Test scenario reuse unsafe for ADAS/ADS |
| W3-021 | Environmental/EMC/durability validation physical and slow |
| W3-022 | Test fleet management and real-world data collection |
| W3-023 | Lack of traceability across MIL–SIL–HIL (horizontal) |
| W3-024 | Test case specification quality problems |
| W3-025 | Compliance documentation burden for testing |
| W3-026 | Incomplete test coverage and time pressure |
| W3-027 | Invalid test failures and unreliable test infrastructure |
| W3-028 | Regression testing scalability and overhead |
| W3-029 | Non-determinism and flaky tests in ML systems |
| W3-030 | Test data management and artifact versioning across XiL |
| W3-031 | Shift-left adoption incomplete; testing still late |
| W3-032 | Unit testing informal, weak strategies, poor evidence |
| W3-033 | Fault injection manual, expertise-dependent, incomplete models |
| W3-034 | Safety validation statistical evidence and testing effort |
| W3-035 | Mixed discrete-continuous (hybrid) system testing intractability |
| W3-036 | Test tool interoperability and fragmented toolchain |
| W3-037 | Test prioritisation and risk-based testing underdeveloped |
| W3-038 | Knowledge and skill gaps in testing/validation |

---

## 35.8 Toolchain and standards (W4-001 … W4-030)

| ID | Title (short) |
|---|---|
| W4-001 | Fragmented heterogeneous toolchain, poor interoperability |
| W4-002 | Proprietary data formats and ARXML version/dialect fragmentation |
| W4-003 | Manual ARXML authoring/editing error-prone and unscalable |
| W4-004 | AUTOSAR configuration tool lock-in and licensing cost |
| W4-005 | Configuration diff/merge and variant management in ARXML immature |
| W4-006 | Requirements management tools legacy-heavy, hard to onboard |
| W4-007 | End-to-end traceability gaps (req → code → tests → safety) |
| W4-008 | Static analysis false positives and manual deviation effort (MISRA/CERT) |
| W4-009 | MISRA/CERT standard version churn and migration burden |
| W4-010 | Undecidable MISRA rules require manual review |
| W4-011 | ISO 26262 compliance heavy documentation/process overhead |
| W4-012 | Safety case maintenance and change-impact re-assessment cost |
| W4-013 | Automotive SPICE assessment overhead and tailoring burden |
| W4-014 | ISO/SAE 21434 cybersecurity engineering integration cost |
| W4-015 | UNECE R155 type-approval and CSMS supply-chain burden |
| W4-016 | UNECE R156 / ISO 24089 software update engineering complexity |
| W4-017 | SOTIF validation of unknown unsafe scenarios doesn't scale |
| W4-018 | HIL test bench scarcity and scheduling bottlenecks |
| W4-019 | vECU fidelity gaps and integration friction with simulation |
| W4-020 | Generic CI/CD pipelines unsuitable for safety-critical embedded |
| W4-021 | Long build/compilation times for large AUTOSAR codebases |
| W4-022 | Tool onboarding, training time, single-person expertise |
| W4-023 | Supplier/OEM toolchain mismatch and data exchange friction |
| W4-024 | PLM/PDM vs engineering-data (ARXML) integration gaps |
| W4-025 | Open-source AUTOSAR tooling immature and unsupported |
| W4-026 | Standards evolution and regional regulatory fragmentation |
| W4-027 | Tool licensing, infrastructure and cost constraints |
| W4-028 | Manual data transformation via spreadsheets/scripts ("glue work") |
| W4-029 | Test tool interoperability and coverage evidence management |
| W4-030 | AI-assisted engineering tooling trust, certification, IP/data constraints |

---

## 35.9 Human skills and AI/automation (W5-001 … W5-025)

| ID | Title (short) |
|---|---|
| W5-001 | Embedded software engineer shortage (capacity bottleneck) |
| W5-002 | AUTOSAR expertise gap (Classic & Adaptive) |
| W5-003 | Functional safety (ISO 26262) competency scarcity |
| W5-004 | Cybersecurity expertise shortage |
| W5-005 | AI/ML engineering capabilities gap in automotive |
| W5-006 | Requirements engineering skills gaps |
| W5-007 | System/architecture engineering skills gaps |
| W5-008 | Software integration expertise gaps |
| W5-009 | Testing and validation expertise gaps |
| W5-010 | Hardware/software co-design knowledge gaps |
| W5-011 | Toolchain knowledge gaps (vendor-specific) |
| W5-012 | Knowledge silos, single-person expertise, turnover-driven loss |
| W5-013 | Training and onboarding time/cost |
| W5-014 | Communication gaps across disciplines and organisations |
| W5-015 | Compensation disadvantage vs tech-native rivals |
| W5-016 | AI adoption barriers: confidentiality, IP, NDA constraints |
| W5-017 | LLM hallucination in safety-critical engineering |
| W5-018 | Lack of automotive-specific datasets for fine-tuning local LLMs |
| W5-019 | AI-generated code safety and certification gap |
| W5-020 | Explainability and accountability of AI outputs |
| W5-021 | Legacy skills obsolescence vs new digital competencies |
| W5-022 | Education pipeline mismatch |
| W5-023 | Aging workforce / retirement cliff |
| W5-024 | SDV / centralised computing skill gap |
| W5-025 | Absence of AI governance in engineering workflows |

---

## 35.10 Cross-cutting themes (from KNOWLEDGE_INDEX)

1. Requirements & architecture are coupled — incomplete requirements + no system-level architecture + no interface governance = integration as discovery (W1).
2. Tool fragmentation is a root cause, not just symptom — ALM tools store artifacts but don't enforce semantic consistency; ARXML serialization not canonical (W1, W2, W4).
3. Compliance is both genuine safety work and avoidable overhead — boundary organisation-specific and poorly measured (W1, W4).
4. MBSE is the proposed cure but blocked by culture, skills, tooling, governance (W1).
5. Timing analysis is a safety-relevant unsolved problem underpinning zonal/centralised transition (W1, W2).
6. ML/AI introduces new RE categories (data, ODD, monitors) classic RE/traceability don't cover (W1, W4, W5).
7. Classic AUTOSAR is not going away — Adaptive has not gained traction as hoped; hybrid is reality (W2).
8. Testing is the dominant cost/schedule driver — up to 50% of dev cost; invalid failures up to 91% (W3).
9. Shift-left ambition vs reality gap — ~33% implemented; not testing early enough rising (W3).
10. Talent shortage is structural — embedded, AUTOSAR, safety, cybersecurity, AI/ML, systems (W5).
11. AI adoption blocked by confidentiality, IP, hallucination, certification gaps (W5, W4-030).

---

## 35.11 Coverage and completeness notes

- The catalogue is **not** a complete enumeration of every sub-issue; each problem record in the source reports contains contributing factors and unanswered questions that expand the scope.
- Problem IDs are stable; new problems discovered in future research should be appended with new IDs rather than renumbering.
- Cross-references in Part III (Chapters 14–22) group these IDs by theme; Part IV (Chapters 23–27) maps them to existing solutions; Part VII concludes.
- **Evidence limitations:** no OEM-internal data; market/workforce statistics are estimates; see each source report's limitations section and DATA_VERIFY.md.

---

## 35.12 Problem ID index for this chapter

All IDs listed in §§35.1–35.9: G1-001…G1-035, G2-001…G2-032, G3-001…G3-028, G4-001…G4-032, W1-001…W1-018, W2-001…W2-020, W3-001…W3-038, W4-001…W4-030, W5-001…W5-025.

---

*End of Chapter 35.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

