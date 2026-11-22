# Knowledge Index — Automotive Software Engineering Problem Catalogue

**Source reports:** `research/W1_requirements_system.md`, `W2_autosar_middleware.md`, `W3_testing_validation.md`, `W4_toolchain_standards.md`, `W5_skills_ai.md`, `W6_bootloader_flash.md`, `W7_asil_mixed_criticality_multicore.md`, `G5_domain_deep_dive.md`
**Compiled:** 2026-11-07
**Total problem records:** 289 (W1: 18, W2: 20, W3: 38, W4: 30, W5: 25, W6: 30, W7: 30, G5: 32, G6: 45, G7: 10, G8: 24, G9: 6, G10: 15, G11: 8)

---

## 1. Quick stats

| Workstream | Count | Focus |
|---|---|---|
| W1 Requirements & System Engineering | 18 | Requirements, architecture, timing, variants, compliance, skills |
| W2 AUTOSAR Classic/Adaptive & Middleware | 20 | ARXML, interop, Adaptive maturity, safety, OTA, middleware, skills |
| W3 Testing & Validation | 38 | MIL/SIL/PIL/HIL/vehicle, scenario coverage, traceability, compliance |
| W4 Toolchain & Standards | 30 | Fragmentation, ARXML, compliance overhead, HIL, CI/CD, cost |
| W5 Human Skills & AI/Automation | 25 | Talent gaps, knowledge loss, AI barriers, governance |
| W6 Bootloader & Flash Programming | 30 | Bootloader architecture, flash programming, integration, security, testing, manufacturing |
| G6 CDI Waves | 45 | CDI/CDD problem catalogue, bootloader, flash, OTA, toolchain, standards |
| G7 CDI Waves | 10 | CDI mixed-criticality, FFI, SWC partitioning, multicore |
| G8 CDI Waves | 24 | CDI semiconductor/hardware, memory, chip packaging |
| G9 CDI Waves | 6 | CDI chassis/domain bridging, body electrification |
| G10 CDI Waves | 15 | CDI toolchain, standards, coding, verification |
| G11 CDI Waves | 8 | CDI skills, AI/automation, human approval |
| W7 ASIL & Mixed-Criticality / Multicore | 30 | ASIL-A/B/C/D implications, FFI, SWC partitioning, multicore, multi-partition, SWC-BSW-MCAL integration |
| G5 Vehicle Domain Deep-Dive | 32 | Middleware, Body, Powertrain, AD/ADAS, IVI, Cockpit, Zonal, HPC (4 records each) |

---

## 2. Index by workstream

### W1 — Requirements & System Engineering

| ID | Title | Domain |
|---|---|---|
| W1-001 | Ambiguous, underspecified natural-language requirements | Requirements engineering |
| W1-002 | Unconnected abstraction levels across OEM/Tier-1/Tier-2 | Requirements / system engineering |
| W1-003 | Requirements traceability burden and link quality | Requirements / compliance |
| W1-004 | ML/AI-specific requirements and traceability gaps (ADAS/perception) | Requirements (AI/ML) |
| W1-005 | Late and uncontrolled requirement changes | Requirements / change management |
| W1-006 | Incomplete requirements handed to suppliers (assumption-driven dev) | System engineering / supplier management |
| W1-007 | Unstable interfaces and interface governance gaps | System engineering / interface management |
| W1-008 | Inconsistent architecture across suppliers/OEM | System engineering / architecture |
| W1-009 | MBSE adoption barriers (cultural, skill, tool, governance) | System engineering / MBSE |
| W1-010 | Timing analysis and predictability unsolved for AUTOSAR | System engineering / timing |
| W1-011 | Variant explosion and configuration management | System engineering / configuration |
| W1-012 | Requirements-driven rework cost and late defect discovery | Cross-cutting |
| W1-013 | ARXML tool interoperability and configuration drift | Configuration / engineering data |
| W1-014 | Compliance overhead and standards fragmentation | Standards / regulations |
| W1-015 | System engineering skill and workforce gaps | Human skills |
| W1-016 | Transition to zonal/centralised architecture: new challenges | System engineering / architecture |
| W1-017 | Architecture consistency and change management without visibility | System engineering / change management |
| W1-018 | Network architecture and timing of communication (bus/Ethernet) | System engineering / network |

### W2 — AUTOSAR Classic/Adaptive & Middleware

| ID | Title | Domain |
|---|---|---|
| W2-001 | ARXML Configuration Complexity and Manual Burden | AUTOSAR / configuration |
| W2-002 | Classic vs Adaptive Interoperability & Gateway Translation | Classic/Adaptive interop |
| W2-003 | Adaptive Platform Immaturity & Stabilization Churn | AUTOSAR Adaptive |
| W2-004 | Freedom from Interference on Adaptive: Unsolved Real-Time Safety | AUTOSAR Adaptive / safety |
| W2-005 | Vendor Lock-in and BSW Toolchain Interoperability | Toolchain / BSW |
| W2-006 | Multicore Distribution Complexity in Classic | AUTOSAR Classic OS / RTE |
| W2-007 | SOME/IP Service Discovery Latency & Jitter | Middleware (SOME/IP) |
| W2-008 | DDS Real-Time Determinism & Threading Complexity | Middleware (DDS) |
| W2-009 | Hypervisor/Mixed-Criticality Consolidation Challenges | Virtualization / hypervisors |
| W2-010 | OTA/FOTA Background Install, Safe-State Activation, Rollback | Bootloader / OTA |
| W2-011 | Variant Management Complexity and Binding-Time Confusion | Configuration / variants |
| W2-012 | RTE Generation Contract Phase & Consistency Validation Gaps | RTE |
| W2-013 | Classic OS (OSEK) Scheduling Limitations | AUTOSAR Classic OS |
| W2-014 | Adaptive Dynamic Memory Allocation Non-Determinism | AUTOSAR Adaptive |
| W2-015 | ARXML Round-Trip & Version-Control Friction | Configuration data management |
| W2-016 | Classic-to-Adaptive Migration Requires Full Communication Redesign | Migration / architecture |
| W2-017 | Adaptive Platform Diagnostics, Health Management & NM Maturity | AUTOSAR Adaptive services |
| W2-018 | Safety Integrity Level Gap: Adaptive Targets ASIL-B | Functional safety |
| W2-019 | Skill/Knowledge Gaps in AUTOSAR Methodology and Tooling | Human factors |
| W2-020 | Adaptive Application Portability vs. Vendor Implementation Divergence | AUTOSAR Adaptive |

### W3 — Testing & Validation

| ID | Title | Domain |
|---|---|---|
| W3-001 | MIL plant/control models simplified, validated late | MIL / model-based dev |
| W3-002 | MIL test cases not reused for code verification | MIL/SIL |
| W3-003 | MIL/SIL model instability prevents deterministic CI | MIL/SIL/HIL |
| W3-004 | SiL cannot test all HiL functionalities; vECU fidelity | SIL/vECU |
| W3-005 | Temporal synchronization in hybrid SiL/HiL | SiL/HiL co-simulation |
| W3-006 | SiL simulation inputs limited; realistic stimuli | SIL |
| W3-007 | vECU standardization gap (FMI vs AUTOSAR) | vECU / toolchain |
| W3-008 | PIL limitations (unsupported modeling features) | PIL |
| W3-009 | PIL reveals compiler faults but slow/hardware-dependent | PIL |
| W3-010 | HIL cost and scalability | HIL |
| W3-011 | HIL setup latency, reconfiguration, variant explosion | HIL |
| W3-012 | HIL model fidelity (simplified plant models) | HIL |
| W3-013 | HIL sensor/actuator physical constraints and latency | HIL |
| W3-014 | HIL fixture/test-bench verification difficulty | HIL |
| W3-015 | Cloud-based HIL challenges (latency, cyber, compliance) | Cloud HIL |
| W3-016 | HIL test environment management (idle benches, scheduling) | HIL operations |
| W3-017 | Physical proving-ground testing scope explosion and cost | Vehicle testing |
| W3-018 | ADAS/ADS scenario coverage and completeness unknown | ADAS/ADS validation |
| W3-019 | Simulation nondeterminism/flakiness in ADS testing | ADAS/ADS simulation |
| W3-020 | Test scenario reuse unsafe for ADAS/ADS | ADAS/ADS testing |
| W3-021 | Environmental/EMC/durability validation physical and slow | Environmental validation |
| W3-022 | Test fleet management and real-world data collection | Vehicle/fleet validation |
| W3-023 | Lack of traceability across MIL–SIL–HIL (horizontal) | Test traceability |
| W3-024 | Test case specification quality problems | Test documentation |
| W3-025 | Compliance documentation burden for testing | Compliance |
| W3-026 | Incomplete test coverage and time pressure | Test coverage |
| W3-027 | Invalid test failures and unreliable test infrastructure | Test execution quality |
| W3-028 | Regression testing scalability and overhead | Regression testing |
| W3-029 | Non-determinism and flaky tests in ML systems | ML testing |
| W3-030 | Test data management and artifact versioning across XiL | Test data management |
| W3-031 | Shift-left adoption incomplete; testing still late | Process |
| W3-032 | Unit testing informal, weak strategies, poor evidence | Unit testing |
| W3-033 | Fault injection manual, expertise-dependent, incomplete models | Fault injection / robustness |
| W3-034 | Safety validation statistical evidence and testing effort | Functional safety validation |
| W3-035 | Mixed discrete-continuous (hybrid) system testing intractability | Model-based testing |
| W3-036 | Test tool interoperability and fragmented toolchain | Toolchain |
| W3-037 | Test prioritization and risk-based testing underdeveloped | Test strategy |
| W3-038 | Knowledge and skill gaps in testing/validation | Human skills |

### W4 — Toolchain & Standards

| ID | Title | Domain |
|---|---|---|
| W4-001 | Fragmented heterogeneous toolchain, poor interoperability | Engineering tools |
| W4-002 | Proprietary data formats and ARXML version/dialect fragmentation | Configuration / engineering data |
| W4-003 | Manual ARXML authoring/editing error-prone and unscalable | Configuration / engineering data |
| W4-004 | AUTOSAR configuration tool lock-in and licensing cost | Toolchain / commercial tools |
| W4-005 | Configuration diff/merge and variant management in ARXML immature | Configuration / engineering data |
| W4-006 | Requirements management tools legacy-heavy, hard to onboard | Requirements tools |
| W4-007 | End-to-end traceability gaps (req → code → tests → safety) | Traceability / compliance |
| W4-008 | Static analysis false positives and manual deviation effort (MISRA/CERT) | Code quality / standards |
| W4-009 | MISRA/CERT standard version churn and migration burden | Standards / code quality |
| W4-010 | Undecidable MISRA rules require manual review | Code quality / standards |
| W4-011 | ISO 26262 compliance heavy documentation/process overhead | Functional safety |
| W4-012 | Safety case maintenance and change-impact re-assessment cost | Functional safety |
| W4-013 | Automotive SPICE assessment overhead and tailoring burden | Process quality |
| W4-014 | ISO/SAE 21434 cybersecurity engineering integration cost | Cybersecurity |
| W4-015 | UNECE R155 type-approval and CSMS supply-chain burden | Cybersecurity regulation |
| W4-016 | UNECE R156 / ISO 24089 software update engineering complexity | Software update / OTA |
| W4-017 | SOTIF validation of unknown unsafe scenarios doesn't scale | Safety / ADAS validation |
| W4-018 | HIL test bench scarcity and scheduling bottlenecks | Testing infrastructure |
| W4-019 | vECU fidelity gaps and integration friction with simulation | Simulation / virtual validation |
| W4-020 | Generic CI/CD pipelines unsuitable for safety-critical embedded | DevOps / CI/CD |
| W4-021 | Long build/compilation times for large AUTOSAR codebases | Build / tooling |
| W4-022 | Tool onboarding, training time, single-person expertise | Human factors / tooling |
| W4-023 | Supplier/OEM toolchain mismatch and data exchange friction | Supply chain / tooling |
| W4-024 | PLM/PDM vs engineering-data (ARXML) integration gaps | Data management |
| W4-025 | Open-source AUTOSAR tooling immature and unsupported | Open-source tooling |
| W4-026 | Standards evolution and regional regulatory fragmentation | Standards / regulation |
| W4-027 | Tool licensing, infrastructure and cost constraints | Toolchain economics |
| W4-028 | Manual data transformation via spreadsheets/scripts ("glue work") | Process / tooling |
| W4-029 | Test tool interoperability and coverage evidence management | Testing tools |
| W4-030 | AI-assisted engineering tooling trust, certification, IP/data constraints | AI in engineering tools |

### W5 — Human Skills & AI/Automation

| ID | Title | Domain |
|---|---|---|
| W5-001 | Embedded software engineer shortage (capacity bottleneck) | Human skills / embedded |
| W5-002 | AUTOSAR expertise gap (Classic & Adaptive) | Human skills / AUTOSAR |
| W5-003 | Functional safety (ISO 26262) competency scarcity | Human skills / safety |
| W5-004 | Cybersecurity expertise shortage | Human skills / cybersecurity |
| W5-005 | AI/ML engineering capabilities gap in automotive | Human skills / AI-ML |
| W5-006 | Requirements engineering skills gaps | Human skills / RE |
| W5-007 | System/architecture engineering skills gaps | Human skills / systems |
| W5-008 | Software integration expertise gaps | Human skills / integration |
| W5-009 | Testing and validation expertise gaps | Human skills / testing |
| W5-010 | Hardware/software co-design knowledge gaps | Human skills / HW-SW |
| W5-011 | Toolchain knowledge gaps (vendor-specific) | Human skills / toolchain |
| W5-012 | Knowledge silos, single-person expertise, turnover-driven loss | Human skills / KM |
| W5-013 | Training and onboarding time/cost | Human skills / training |
| W5-014 | Communication gaps across disciplines and organizations | Human skills / collaboration |
| W5-015 | Compensation disadvantage vs tech-native rivals | Human skills / compensation |
| W5-016 | AI adoption barriers: confidentiality, IP, NDA constraints | AI/automation / data governance |
| W5-017 | LLM hallucination in safety-critical engineering | AI/automation / reliability |
| W5-018 | Lack of automotive-specific datasets for fine-tuning local LLMs | AI/automation / data |
| W5-019 | AI-generated code safety and certification gap | AI/automation / safety |
| W5-020 | Explainability and accountability of AI outputs | AI/automation / governance |
| W5-021 | Legacy skills obsolescence vs new digital competencies | Human skills / transformation |
| W5-022 | Education pipeline mismatch | Human skills / education |
| W5-023 | Aging workforce / retirement cliff | Human skills / demographics |
| W5-024 | SDV / centralized computing skill gap | Human skills / SDV |
| W5-025 | Absence of AI governance in engineering workflows | AI/automation / governance |

### W6 — Bootloader & Flash Programming

| ID | Title | Domain |
|---|---|---|
| W6-001 | No standardized automotive bootloader (AUTOSAR out of scope) | Bootloader architecture |
| W6-002 | Bootloader memory layout and partitioning complexity | Bootloader / memory |
| W6-003 | Secure boot chain-of-trust implementation gaps | Bootloader security |
| W6-004 | Bootloader startup time and fast-boot constraints | Bootloader / timing |
| W6-005 | Anti-rollback and version monotonicity enforcement gaps | Bootloader security / update |
| W6-006 | Dual-bank / A-B update scheme complexity and flash cost | Bootloader / update strategy |
| W6-007 | UDS flash programming protocol complexity | Diagnostics / flash programming |
| W6-008 | Flash programming reliability under power transients | Flash programming / power |
| W6-009 | Bootloader recovery and brick prevention | Bootloader reliability |
| W6-010 | Application/bootloader interface contract gaps | Integration |
| W6-011 | Calibration data and flash interaction | Calibration / flash |
| W6-012 | Bootloader testing coverage gaps | Testing / bootloader |
| W6-013 | Flash endurance and wear-leveling validation | Flash memory / reliability |
| W6-014 | Power-loss and interrupted programming recovery testing | Testing / robustness |
| W6-015 | Manufacturing/EOL flashing and production programming traceability | Manufacturing / EOL |
| W6-016 | Diagnostic session transitions to bootloader | Diagnostics / bootloader |
| W6-017 | Flash driver (MCAL) portability and abstraction gaps | Flash driver / MCAL |
| W6-018 | Bootloader security vs. upgradability conflict | Bootloader security |
| W6-019 | Key and certificate management lifecycle | Security / provisioning |
| W6-020 | Bootloader size constraints and feature creep | Bootloader / resources |
| W6-021 | Bootloader integration with AUTOSAR BSW | Integration / AUTOSAR |
| W6-022 | Reprogramming throughput (CAN vs. DoIP/Ethernet) | Flash programming / performance |
| W6-023 | Flash toolchain fragmentation | Toolchain |
| W6-024 | Bootloader V&V evidence and ISO 26262 compliance | Functional safety / bootloader |
| W6-025 | OTA and bootloader orchestration | OTA / bootloader |
| W6-026 | Multi-ECU campaign consistency and partial-failure recovery | OTA / integration |
| W6-027 | Bootloader self-update complexity | Bootloader / update |
| W6-028 | Flash image integrity verification | Security / reliability |
| W6-029 | Human skill gaps in bootloader/flash/security expertise | Human skills |
| W6-030 | Late bootloader rework cost | Process / lifecycle |

### G6 — CDI Waves
### G6-001 | No standardized CDI bootloader (AUTOSAR out of scope) | CDI bootloader architecture |
| G6-002 | CDI memory layout and partitioning complexity | CDI / memory |
| G6-003 | Secure boot chain-of-trust implementation gaps | CDI security |
| G6-004 | CDI startup time and fast-boot constraints | CDI / timing |
| G6-005 | Anti-rollback and version monotonicity enforcement gaps | CDI security / update |
| G6-006 | Dual-bank / A-B update scheme complexity and flash cost | CDI / update strategy |
| G6-007 | UDS flash programming protocol complexity | Diagnostics / flash programming |
| G6-008 | Flash programming reliability under power transients | Flash programming / power |
| G6-009 | Bootloader recovery and brick prevention | CDI reliability |
| G6-010 | Application/bootloader interface contract gaps | Integration |
| G6-011 | Calibration data and flash interaction | Calibration / flash |
| G6-012 | Bootloader testing coverage gaps | Testing / bootloader |
| G6-013 | Flash endurance and wear-leveling validation | Flash memory / reliability |
| G6-014 | Power-loss and interrupted programming recovery testing | Testing / robustness |
| G6-015 | Manufacturing/EOL flashing and production programming traceability | Manufacturing / EOL |
| G6-016 | Diagnostic session transitions to bootloader | Diagnostics / bootloader |
| G6-017 | Flash driver (MCAL) portability and abstraction gaps | Flash driver / MCAL |
| G6-018 | Bootloader security vs. upgradability conflict | CDI security |
| G6-019 | Key and certificate management lifecycle | Security / provisioning |
| G6-020 | Bootloader size constraints and feature creep | CDI / resources |
| G6-021 | Bootloader integration with AUTOSAR BSW | Integration / AUTOSAR |
| G6-022 | Reprogramming throughput (CAN vs. DoIP/Ethernet) | Flash programming / performance |
| G6-023 | Flash toolchain fragmentation | Toolchain |
| G6-024 | Bootloader V&V evidence and ISO 26262 compliance | Functional safety / CDI |
| G6-025 | OTA and bootloader orchestration | OTA / CDI |
| G6-026 | Multi-ECU campaign consistency and partial-failure recovery | OTA / integration |
| G6-027 | Bootloader self-update complexity | CDI / update |
| G6-028 | Flash image integrity verification | Security / reliability |
| G6-029 | Human skill gaps in CDI/flash/security expertise | Human skills |
| G6-030 | Late bootloader rework cost | Process / lifecycle |
| G6-031 | OTA success rate for CDI bootloader updates | OTA / CDI |
| G6-032 | Multi-ECU campaign consistency for CDI | OTA / integration |
| G6-033 | CDI bootloader V&V evidence gaps | Functional safety / CDI |
| G6-034 | CDI toolchain fragmentation | Toolchain |
| G6-035 | CDI OTA orchestration consistency | OTA / CDI |
| G6-036 | CDI flash image integrity verification | Security / reliability |
| G6-037 | CDI human skill gaps in bootloader expertise | Human skills |
| G6-038 | CDI late rework cost | Process / lifecycle |
| G6-039 | CDI secure boot chain-of-trust | CDI security |
| G6-040 | CDI memory partitioning complexity | CDI / memory |
| G6-041 | CDI calibration data interaction | Calibration / flash |
| G6-042 | CDI bootloader testing coverage | Testing / bootloader |
| G6-043 | CDI flash endurance and wear leveling | Flash memory / reliability |
| G6-044 | CDI power-loss recovery testing | Testing / robustness |
| G6-045 | CDI manufacturing/EOL flashing traceability | Manufacturing / EOL |
| G6 Domain | CDI bootloader, flash, OTA, toolchain, standards | — |
### G7 — CDI Waves
### G7-001 | ASIL-A implication set (design, process, testing, evidence) | Functional safety / ASIL-A |
| G7-002 | ASIL-B implication set (design, process, testing, evidence) | Functional safety / ASIL-B |
| G7-003 | ASIL-C implication set (design, process, testing, evidence) | Functional safety / ASIL-C |
| G7-004 | ASIL-D implication set (design, process, testing, evidence) | Functional safety / ASIL-D |
| G7-005 | QM components: obligations, limits, and misuse | Functional safety / QM |
| G7-006 | Freedom from interference: memory interference | Mixed-criticality / FFI |
| G7-007 | Freedom from interference: timing interference | Mixed-criticality / FFI |
| G7-008 | Freedom from interference: execution / control-flow interference | Mixed-criticality |
| G7-009 | QM-ASIL coupling through shared libraries and common-cause paths | Mixed-criticality / dependent failure |
| G7-010 | SWC partitioning by ASIL (granularity, allocation, interface discipline) | SW component design / mixed-criticality |
| G7 Domain | CDI mixed-criticality, FFI, SWC partitioning, multicore | — |
### G8 — CDI Waves
### G8-001 | Semiconductor content growth outpaces design and manufacturing capacity | Semiconductor architecture / chip design |
| G8-002 | Automotive SoCs compete with AI/datacenter for advanced nodes and memory | Semiconductor supply chain / advanced nodes |
| G8-003 | Memory hierarchy and bandwidth constraints for zonal E/E architectures | Semiconductor memory / bandwidth |
| G8-004 | Chiplet integration and interconnect standardization gaps | Chiplet integration / interconnect |
| G8-005 | Advanced-node fabrication capacity limits for automotive-grade silicon | Foundry capacity / automotive-grade |
| G8-006 | ESD and Latch-up protection in mixed-signal chip domains | ESD / latch-up protection |
| G8-007 | Clock distribution and timing skew in high-speed chip domains | Clock distribution / timing skew |
| G8-008 | Power distribution network (PDN) integrity for ASIL-D domains | PDN integrity / ASIL-D |
| G8-009 | Thermal management and hot-spot mitigation in dense chip domains | Thermal management / hot-spot |
| G8-010 | Verification and validation coverage for chip-level safety | Chip-level V&V / coverage |
| G8-011 | Qualification pipeline latency and regulatory compliance gaps | Qualification / regulatory |
| G8-012 | Silicon bring-up and bring-up debug methodology gaps | Silicon bring-up / debug |
| G8-013 | DAC/ADC calibration and validation accuracy gaps | DAC/ADC / calibration |
| G8-014 | Memory controller and ECC configuration errors in automotive MCUs | Memory controller / ECC |
| G8-015 | Power sequencing and reset timing marginality issues | Power sequencing / reset timing |
| G8-016 | JTAG/SWD debug port security and exploitation risks | Debug port / security |
| G8-017 | OTP/One-Time-Programmability fuse programming yield issues | OTP / yield |
| G8-018 | Package-on-Package (PoP) and 3D IC integration risks | 3D IC / integration |
| G8-019 | IP block integration and interface compliance gaps | IP block / integration |
| G8-020 | Silicon lifecycle management and end-of-life tracking | Silicon lifecycle / EOL |
| G8-021 | Package thermal resistance and board-level co-design | Package thermal / co-design |
| G8-022 | Silicon test vector generation and exhaustiveness | Silicon test / vector generation |
| G8-023 | Analog/mixed-signal test coverage gaps | Analog test / coverage |
| G8-024 | Silicon manufacturing defect classification | Manufacturing defect / classification |
| G8 Domain | CDI semiconductor/hardware, memory, chip packaging | — |
### G9 — CDI Waves
### G9-001 | Body controller BCM consolidation, I/O abstraction and load control | Body |
| G9-002 | Body: access/security vs safety and power-state timing | Body |
| G9-003 | Body: variant explosion across trims and personalization data | Body (cross-domain) |
| G9-004 | Body: cross-domain lesson - the quiet domain that stalls the vehicle | Body (cross-domain) |
| G9-005 | Powertrain: EMS/TCM/aftertreatment coordination and the torque path | Powertrain |
| G9-006 | Powertrain: ASIL-C/D torque monitoring and OBD interplay | Powertrain |
| G9 Domain | CDI chassis/domain deep-dive (6 records) | — |
### G10 — CDI Waves
### G10-001 | Middleware: signal-service gateway correctness and timing | Middleware |
| G10-002 | Middleware: time synchronisation and deterministic communication | Middleware |
| G10-003 | Middleware: service versioning and mixed-version fleet coexistence | Middleware |
| G10-004 | Middleware: cross-domain lesson - middleware as the integration contract | Middleware (cross-domain) |
| G10-005 | Body: BCM consolidation, I/O abstraction and load control | Body |
| G10-006 | Body: access/security vs safety and power-state timing | Body |
| G10-007 | Body: variant explosion across trims and personalization data | Body (cross-domain) |
| G10-008 | Body: cross-domain lesson - the quiet domain that stalls the vehicle | Body (cross-domain) |
| G10-009 | Powertrain: EMS/TCM/aftertreatment coordination and the torque path | Powertrain |
| G10-010 | Powertrain: ASIL-C/D torque monitoring and OBD interplay | Powertrain |
| G10-011 | Powertrain: emission-relevant calibration protection and field updates under regulation | Powertrain |
| G10-012 | Powertrain: cross-domain lesson - torque and calibration as vehicle contracts | Powertrain (cross-domain) |
| G10-013 | AD/ADAS: perception/fusion/planning pipeline and sensor integration | AD/ADAS |
| G10-014 | AD/ADAS: SOTIF + ASIL-D motion control, ODD exits, and latency budgets | AD/ADAS |
| G10-015 | AD/ADAS: ML model, map-data, and calibration updates vs homologation | AD/ADAS |
| G10 Domain | CDI middleware, body, powertrain, AD/ADAS (15 records) | — |
### G11 — CDI Waves
### G11-001 | Middleware: signal↔service gateway correctness and timing | Middleware |
| G11-002 | Middleware: time synchronisation and deterministic communication | Middleware |
| G11-003 | Middleware: service versioning and mixed-version fleet coexistence | Middleware |
| G11-004 | Body: BCM consolidation, I/O abstraction and load control | Body |
| G11-005 | Body: access/security vs safety and power-state timing | Body |
| G11-006 | Body: variant explosion across trims and personalization data | Body (cross-domain) |
| G11-007 | Powertrain: EMS/TCM/aftertreatment coordination and the torque path | Powertrain |
| G11-008 | Powertrain: ASIL-C/D torque monitoring and OBD interplay | Powertrain |
| G11 Domain | CDI middleware, body, powertrain (8 records) | — |

### W7 — ASIL & Mixed-Criticality / Multicore

| ID | Title | Domain |
|---|---|---|
| W7-001 | ASIL-A implication set (design, process, testing, evidence) | Functional safety / ASIL-A |
| W7-002 | ASIL-B implication set (design, process, testing, evidence) | Functional safety / ASIL-B |
| W7-003 | ASIL-C implication set (design, process, testing, evidence) | Functional safety / ASIL-C |
| W7-004 | ASIL-D implication set (design, process, testing, evidence) | Functional safety / ASIL-D |
| W7-005 | QM components: obligations, limits, and misuse | Functional safety / QM |
| W7-006 | Freedom from interference: memory interference | Mixed-criticality / FFI |
| W7-007 | Freedom from interference: timing interference | Mixed-criticality / FFI |
| W7-008 | Freedom from interference: execution / control-flow interference | Mixed-criticality / FFI |
| W7-009 | QM-ASIL coupling through shared libraries and common-cause paths | Mixed-criticality / dependent failure |
| W7-010 | SWC partitioning by ASIL (granularity, allocation, interface discipline) | SW component design / mixed-criticality |
| W7-011 | Runnable-to-task mapping under ASIL constraints | SW component design / AUTOSAR OS |
| W7-012 | Data consistency across ASIL boundaries | Mixed-criticality / RTE communication |
| W7-013 | End-to-end protection (E2E profiles, CRC/sequence) | Safety communication / E2E |
| W7-014 | Multicore core allocation for mixed-criticality | Multicore architecture / mixed-criticality |
| W7-015 | Multicore scheduling and timing analysis gaps | Multicore timing / AUTOSAR OS |
| W7-016 | Inter-core communication and spinlocks (shared-memory hazards, lock ordering, deadlock) | Multicore / mixed-criticality |
| W7-017 | Multicore timing verification (contention, cache effects, measurement vs analysis) | Multicore timing / V&V |
| W7-018 | OS-Applications and memory partitions (trusted vs untrusted, restart, containment) | Multi-partition / OS separation |
| W7-019 | Hypervisor guests for mixed-criticality (ASIL guest beside QM Linux/Android) | Virtualization / mixed-criticality |
| W7-020 | Inter-partition communication gateways (I/O routing, FFI boundary, covert channels) | Multi-partition communication / mixed-criticality |
| W7-021 | Mode management across partitions (ECU modes, partition requests, inconsistent assumptions) | Multi-partition / mode management |
| W7-022 | SWC to RTE to OS integration chain (contract vs execution phase, generation mismatch) | Application/BSW integration / RTE contract |
| W7-023 | COM and signal-service mapping under ASIL (grouping, atomicity, gateway timing) | Communication / mixed-criticality integration |
| W7-024 | Variant and calibration binding in mixed-criticality builds (post-build variants, sprawl, wrong-variant flashing) | Variants / calibration / production |
| W7-025 | Application integration testing across ASILs (SiL/HiL coverage, fault injection, robustness) | Integration testing / mixed-criticality V&V |
| W7-026 | BSW module integration under ASIL (mixed-ASIL stacking, scheduler, ownership) | BSW stack integration / mixed-criticality |
| W7-027 | BSW configuration consistency (ECUC parameters, multi-tool, post-build loadables) | BSW configuration / integration |
| W7-028 | RTE/BSW contract gaps (BswM/EcuM states, NvM job results, Dem discipline) | RTE/BSW contracts / state and fault handling |
| W7-029 | MCAL portability and verification burden (MCU change, re-verification, safety-manual assumptions) | MCAL / hardware-software interface |
| W7-030 | MCU driver safety challenges (watchdog, GPT, ADC, DMA, HSI and lockstep/EDC interaction) | MCAL drivers / safety mechanisms |

### G5 — Vehicle Domain Deep-Dive

| ID | Title | Domain |
|---|---|---|
| G5-001 | Middleware: signal-service gateway correctness and timing | Middleware |
| G5-002 | Middleware: time synchronisation and deterministic communication | Middleware |
| G5-003 | Middleware: service versioning and mixed-version fleet coexistence | Middleware |
| G5-004 | Middleware: cross-domain lesson - middleware as the integration contract | Middleware (cross-domain) |
| G5-005 | Body: BCM consolidation, I/O abstraction and load control | Body |
| G5-006 | Body: access/security vs safety and power-state timing | Body |
| G5-007 | Body: variant explosion across trims and personalization data | Body |
| G5-008 | Body: cross-domain lesson - the quiet domain that stalls the vehicle | Body (cross-domain) |
| G5-009 | Powertrain: EMS/TCM/aftertreatment coordination and the torque path | Powertrain |
| G5-010 | Powertrain: ASIL-C/D torque monitoring and OBD interplay | Powertrain |
| G5-011 | Powertrain: emission-relevant calibration protection and field updates under regulation | Powertrain |
| G5-012 | Powertrain: cross-domain lesson - torque and calibration as vehicle contracts | Powertrain (cross-domain) |
| G5-013 | AD/ADAS: perception/fusion/planning pipeline and sensor integration | AD/ADAS |
| G5-014 | AD/ADAS: SOTIF + ASIL-D motion control, ODD exits, and latency budgets | AD/ADAS |
| G5-015 | AD/ADAS: ML model, map-data, and calibration updates vs homologation | AD/ADAS |
| G5-016 | AD/ADAS: cross-domain lesson - ADAS turns every shared resource into a safety case | AD/ADAS (cross-domain) |
| G5-017 | IVI/Infotainment: head-unit SoC with audio/nav stacks beside safety | IVI/Infotainment |
| G5-018 | IVI/Infotainment: QM-ASIL coexistence and telltale/display integrity | IVI/Infotainment |
| G5-019 | IVI/Infotainment: app-store cadence vs vehicle release discipline | IVI/Infotainment |
| G5-020 | IVI/Infotainment: cross-domain lesson - QM exuberance must not sink ASIL neighbours | IVI/Infotainment (cross-domain) |
| G5-021 | Cockpit: cluster/HUD/driver-monitoring consolidation on shared SoC | Cockpit |
| G5-022 | Cockpit: ASIL-B telltales, display-pipeline integrity, and rendering deadlines | Cockpit |
| G5-023 | Cockpit: HMI personalization and variants vs safety-constrained configuration | Cockpit |
| G5-024 | Cockpit: cross-domain lesson - the display is a safety actuator | Cockpit (cross-domain) |
| G5-025 | Zonal: I/O consolidation, power distribution, and network bridging in one controller | Zonal |
| G5-026 | Zonal: mixed-criticality up to ASIL-D on one controller and FFI evidence | Zonal |
| G5-027 | Zonal: software packaging, partial updates, and rollback for zonal controllers | Zonal |
| G5-028 | Zonal: cross-domain lesson - the zone is where domains collide | Zonal (cross-domain) |
| G5-029 | HPC: central-compute consolidation, virtualization strategy, and SOA deployment | HPC |
| G5-030 | HPC: mixed-criticality up to ASIL-D on shared SoC, hypervisor qualification, and interference evidence | HPC |
| G5-031 | HPC: container/VM image updates, atomic fleet rollout, and data-driven logging pipelines | HPC |
| G5-032 | HPC: cross-domain lesson - HPC as the convergence point | HPC (cross-domain) |

---

## 3. Domain index

### Requirements engineering
W1-001, W1-002, W1-003, W1-004, W1-005, W1-006, W5-006

### System engineering / architecture
W1-006, W1-007, W1-008, W1-009, W1-016, W1-017, W1-018, W5-007

### Timing (AUTOSAR / network)
W1-010, W1-018, W2-007, W2-008, W2-013

### Variant / configuration management
W1-011, W1-013, W2-001, W2-011, W2-015, W4-002, W4-003, W4-005

### AUTOSAR Classic
W2-001, W2-005, W2-006, W2-012, W2-013

### AUTOSAR Adaptive
W2-002, W2-003, W2-004, W2-014, W2-016, W2-017, W2-018, W2-020

### Middleware (SOME/IP, DDS)
W2-007, W2-008, G5-001..G5-004

### Virtualization / hypervisors
W2-009, W1-016

### OTA / bootloader / software update
W2-010, W4-016, W6-001..W6-030, W7-024, G5-027, G5-031, G6-025..G6-036, G7-007..G7-010

### Bootloader / flash programming
W6-001..W6-030, G6-001..G6-030

### CDI / chip-domain interface
G6-001..G6-045, G7-001..G7-010, G8-001..G8-024, G9-001..G9-006, G10-001..G10-015, G11-001..G11-008

### ASIL / mixed-criticality
W7-001..W7-030

### ASIL / mixed-criticality
W7-001..W7-030

### Multicore / multi-partition
W7-014..W7-021

### Vehicle domains (deep-dive)
G5-001..G5-032, G6-001..G6-045, G7-001..G7-010

### Testing & validation (general)
W3-001..W3-038 (all), W4-018, W4-019, W4-029

### MIL / SIL / PIL
W3-001, W3-002, W3-003, W3-004, W3-005, W3-006, W3-008, W3-009

### HIL
W3-010, W3-011, W3-012, W3-013, W3-014, W3-016, W4-018

### vECU
W3-004, W3-007, W4-019

### ADAS / ADS scenario validation
W3-017, W3-018, W3-019, W3-020, W4-017

### Environmental / durability / EMC
W3-021

### Compliance (ISO 26262, ASPICE, etc.)
W1-014, W3-025, W4-011, W4-012, W4-013, W4-017, W4-026

### Standards / regulations / cybersecurity
W4-014, W4-015, W4-016, W4-026, W1-014

### Traceability
W1-003, W3-023, W4-007

### Static analysis / code quality (MISRA/CERT)
W4-008, W4-009, W4-010

### Toolchain / tooling
W2-005, W4-001, W4-004, W4-006, W4-020, W4-021, W4-022, W4-023, W4-024, W4-025, W4-027, W4-028, W4-030

### Build / CI/CD
W4-020, W4-021

### Data management / PLM / glue work
W4-024, W4-028

### Human skills / workforce
W1-015, W2-019, W3-038, W4-022, W5-001..W5-025

### AI / automation
W4-030, W5-016, W5-017, W5-018, W5-019, W5-020, W5-025

### Knowledge management / training / communication
W5-012, W5-013, W5-014, W5-022, W5-023

---

## 4. Key terms index (term → problem IDs)

- **ADAS/ADS** — W1-004, W3-017, W3-018, W3-019, W3-020, W4-017, W5-005, G5-013..G5-016
- **Ambiguity (requirements)** — W1-001, W1-005, W5-006
- **ARXML** — W1-013, W2-001, W2-015, W4-002, W4-003, W4-005
- **ASPICE** — W1-014, W3-025, W3-032, W4-013
- **AUTOSAR Classic** — W2-006, W2-013, W4-004
- **AUTOSAR Adaptive** — W2-003, W2-004, W2-014, W2-017, W2-018, W2-020
- **Bus/Ethernet/network timing** — W1-018, W2-007, W2-008
- **Change management / late changes** — W1-005, W1-017, W4-012
- **Cloud HIL** — W3-015
- **Compliance overhead** — W1-014, W3-025, W4-011, W4-013, W4-026
- **Cybersecurity** — W4-014, W4-015, W5-004
- **DDS** — W2-008
- **Fault injection** — W3-033
- **Flakiness / nondeterminism** — W3-003, W3-019, W3-029
- **Functional safety (ISO 26262)** — W2-004, W2-018, W3-034, W4-011, W4-012, W5-003, W5-019, W7-001..W7-030
- **Hallucination (LLM)** — W5-017, W5-020
- **HIL** — W3-010..W3-016, W4-018
- **Hypervisor / virtualization** — W2-009, W1-016, W7-019, G5-030
- **Integration (multi-ECU)** — W5-008, W1-006, W1-007
- **Interface governance** — W1-007, W1-008
- **Knowledge silos / turnover** — W5-012, W5-023, W4-022
- **MBSE** — W1-009, W5-007
- **MIL/SIL/PIL** — W3-001..W3-009
- **MISRA/CERT** — W4-008, W4-009, W4-010
- **ML/AI requirements** — W1-004
- **Multicore** — W2-006, W7-014..W7-017
- **Mixed-criticality / FFI** — W7-006..W7-009, W7-018..W7-021, G5-026, G5-030, G7-006..G7-008
- **CDD / chip-domain interface** — G6-001..G6-045, G7-001..G7-010
- **ASIL-D** — W7-004, G7-004, G5-026, G5-030
- **ODD / scenario coverage** — W3-018, W4-017
- **Onboarding / training cost** — W5-013, W4-022
- **OTA / FOTA** — W2-010, W4-016, W7-024, G5-027, G5-031
- **Bootloader / flash programming** — W6-001..W6-030, W7-024
- **Proving ground** — W3-017
- **Regression testing** — W3-028
- **Requirements traceability** — W1-003, W4-007
- **Rework cost** — W1-012, W1-001
- **Scenario reuse** — W3-020
- **Shift-left** — W3-031
- **Skill gaps** — W1-015, W2-019, W5-001..W5-011, W5-021..W5-024
- **SOME/IP** — W2-007
- **SOTIF (ISO 21448)** — W1-014, W4-017
- **Talent shortage (embedded)** — W5-001
- **Test case spec quality** — W3-024
- **Test data management (TDM)** — W3-030
- **Test tool interoperability** — W3-036, W4-029
- **Timing analysis unsolved** — W1-010, W2-004
- **Tool lock-in / licensing** — W4-004, W4-027
- **Traceability (horizontal XiL)** — W3-023
- **Unit testing** — W3-032
- **vECU** — W3-004, W3-007, W4-019
- **Variant explosion** — W1-011, W2-011, W3-011
- **Vendor lock-in** — W2-005, W4-004
- **Zonal / centralised architecture** — W1-016, W2-009, W5-024, G5-025..G5-032

---

## 5. Cross-cutting themes (from reports)

1. **Requirements & architecture are coupled** — incomplete requirements + no system-level architecture + no interface governance = integration as discovery (W1).
2. **Tool fragmentation is a root cause, not just symptom** — ALM tools store artifacts but don't enforce semantic consistency; ARXML serialization not canonical (W1, W2, W4).
3. **Compliance is both genuine safety work and avoidable overhead** — boundary organisation-specific and poorly measured (W1, W4).
4. **MBSE is the proposed cure but blocked** by culture, skills, tooling, governance (W1).
5. **Timing analysis is a safety-relevant unsolved problem** underpinning zonal/centralised transition (W1, W2).
6. **ML/AI introduces new RE categories** (data, ODD, monitors) classic RE/traceability don't cover (W1, W4, W5).
7. **Classic AUTOSAR is not going away** — Adaptive has not gained traction as hoped; hybrid is reality (W2).
8. **Testing is the dominant cost/schedule driver** — up to 50% of dev cost; invalid failures up to 91% (W3).
9. **Shift-left ambition vs reality gap** — 33% implemented; not testing early enough rising (W3).
10. **Talent shortage is structural** — embedded, AUTOSAR, safety, cybersecurity, AI/ML, systems (W5).
11. **AI adoption blocked by confidentiality, IP, hallucination, certification gaps** (W5, W4-030).

---

## 6. Notes

- Problem records follow the structured format in each W*.md (Domain, Lifecycle stage, Problem, Root cause, Evidence, Automation feasibility, AI applicability, Human approval, Confidence, Unanswered questions).
- Numerical claims are sourced; treat market/workforce stats as estimates (see each report's limitations).
- This index is a navigation aid; consult the source reports for full evidence and citations.

---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

