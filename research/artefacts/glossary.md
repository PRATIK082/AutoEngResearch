# Glossary of Automotive Software Engineering Terminology

**Scope:** Terms used across the research book and artefacts. Definitions are concise and aligned with usage in the research reports.

| Term | Definition |
|---|---|
| ADAS (Advanced Driver Assistance Systems) | Electronic systems that assist the driver with driving and parking functions; software-intensive and safety-relevant. |
| ADAS scenario validation | Validating driver assistance scenarios. |
| ADS (Automated Driving System) | System capable of performing the entire dynamic driving task within an operational design domain; includes perception, planning and control. |
| Agentic AI | AI systems that plan and execute multi-step tasks with tools. |
| AI governance | Policies and controls for responsible AI use in engineering. |
| ALM (Application Lifecycle Management) | Tools and processes managing requirements, design, code, test and release artifacts across the engineering lifecycle. |
| Architecture consistency | Alignment of architecture across teams/suppliers. |
| ARXML (AUTOSAR XML) | XML-based standardized format for describing AUTOSAR systems, ECUs, software components, interfaces and configuration. |
| ASAM (Association for Standardization of Automation and Measuring Systems) | Industry body defining standards for test and measurement (OpenSCENARIO, OpenDRIVE, XCP, etc.). |
| ASIL (Automotive Safety Integrity Level) | Risk classification in ISO 26262 from A (lowest) to D (highest); QM for non-safety-relevant items. |
| ASPICE (Automotive SPICE) | Process assessment model for automotive software development processes (VDA/ISO 15504-derived). |
| Assumption-driven development | Developing based on unstated assumptions. |
| AUTOSAR (AUTomotive Open System ARchitecture) | Standardized automotive software architecture and methodology; Classic Platform for deeply embedded real-time ECUs, Adaptive Platform for HPC/SDV. |
| AUTOSAR Adaptive Platform (AP) | Service-oriented, POSIX/C++-based platform targeting high-performance and zonal/central compute; still evolving. |
| AUTOSAR Classic Platform (CP) | Signal-based, static-config, OSEK/VDX-based platform for deeply embedded, hard real-time ECUs; supports up to ASIL-D. |
| Binding time | Point in the lifecycle at which a variant or configuration decision is fixed (system design, code generation, pre-compile, link, post-build). |
| Board Support Package (BSP) | Software layer providing hardware abstraction and initialization for a specific board/SoC. |
| Bootloader | Software responsible for starting the ECU and (often) performing firmware updates; usually outside AUTOSAR scope. |
| Bootloader validation | Verifying bootloader functionality. |
| Boundary testing | Testing at input boundaries. |
| BSW (Basic Software) | AUTOSAR layered software (services, ECU abstraction, MCAL) below the RTE. |
| Bus factor | Risk that critical knowledge is held by a single individual whose departure would block work. |
| CAN (Controller Area Network) | Multi-master serial bus standard (ISO 11898) widely used in vehicles; CAN FD increases bandwidth. |
| CAN FD | Flexible Data-rate variant of CAN with higher throughput. |
| CAN-TP (ISO 15765-2) | Transport protocol for segmented messages over CAN. |
| Centralised computing | Vehicle architecture consolidating functions onto a few high-performance computers instead of many distributed ECUs. |
| Change Control Board (CCB) | Governance body reviewing and approving engineering changes. |
| Change management | Controlling engineering changes. |
| Change-impact analysis | Determining effects of a change across artifacts. |
| CI/CD (Continuous Integration/Continuous Deployment) | Automated build, test and (in automotive, limited) deployment pipelines. |
| Classic-to-Adaptive migration | Redesign of communication from signals to services and revalidation; not a simple recompile. |
| Climate chamber | Environmental testing facility. |
| Cloud-based HIL | HIL executed in cloud; latency/compliance challenges. |
| Code coverage | Measure of which code elements are exercised by tests (statement, branch, MC/DC). |
| Compliance overhead | Administrative effort for standards/regulation evidence beyond the underlying safety engineering. |
| Component testing | Testing integrated components. |
| Configuration baseline | Approved configuration state for release. |
| Configuration baselines | Approved configuration states. |
| Configuration drift | Divergence of configuration data (e.g., ARXML) between tools/teams over time. |
| Cybersecurity (ISO/SAE 21434) | Standard for automotive cybersecurity engineering; includes TARA and secure development. |
| DDS (Data Distribution Service) | Data-centric publish/subscribe middleware with rich QoS; used in Adaptive and robotics. |
| Defect analysis | Analyzing defects to find root causes. |
| Digital thread | Connected flow of engineering data across lifecycle. |
| Digital thread | End-to-end data connectivity. |
| Digital twin | Virtual representation of a physical system used for simulation, validation and monitoring. |
| DoIP (Diagnostics over IP, ISO 13400) | Diagnostic communication over IP/Ethernet. |
| Domain controller | ECU consolidating functions of a vehicle domain (e.g., ADAS, body, powertrain). |
| Durability | Long-term reliability testing. |
| Durability testing | Long-term wear and stress testing. |
| E/E architecture | Electrical/electronic architecture of the vehicle (ECUs, networks, power). |
| ECU (Electronic Control Unit) | Embedded computer controlling a vehicle subsystem. |
| ECU flashing | Programming ECU software. |
| Edge AI | AI inference executed on vehicle hardware rather than in the cloud. |
| Edge AI | AI inference on local devices. |
| EEPROM/Flash/SRAM/DRAM | Non-volatile and volatile memory technologies used in ECUs. |
| EMC/EMI | Electromagnetic compatibility/interference. |
| End-of-line testing | Manufacturing tests performed on assembled vehicles/ECUs. |
| Engineering data management | Management of configuration artifacts such as ARXML, DBC, system descriptions. |
| Engineering knowledge graph | Graph representation of engineering entities and relationships. |
| Environmental validation | Testing under climate, vibration, EMC conditions. |
| Explainability (XAI) | Ability to understand and justify AI outputs. |
| Fault injection | Deliberately injecting faults to test robustness. |
| Field monitoring | Monitoring deployed vehicles. |
| Fine-tuning | Adapting a pre-trained model to a specific domain. |
| Flaky test | Test that passes/fails non-deterministically. |
| Fleet validation | Validation using vehicle fleets. |
| FMEDA (Failure Modes, Effects and Diagnostic Analysis) | Quantitative hardware safety analysis under ISO 26262. |
| FMI (Functional Mock-up Interface) | Standard for model exchange and co-simulation. |
| FOTA (Firmware Over-The-Air) | Wireless update of ECU firmware. |
| Freedom from Interference (FFI) | ISO 26262 property ensuring faults in one component do not affect safety of another. |
| Functional safety | Absence of unreasonable risk due to hazards from malfunctioning E/E systems (ISO 26262). |
| Gateway | ECU translating and routing messages between networks (e.g., CAN to Ethernet). |
| Glue work | Manual data transformation between tools (spreadsheets/scripts). |
| Hallucination | LLM generating plausible but incorrect content. |
| HARA (Hazard Analysis and Risk Assessment) | ISO 26262 process identifying hazards and deriving safety goals. |
| HIL (Hardware-in-the-Loop) | Testing real hardware against a simulated environment in real time. |
| HIL farm | Cluster of HIL benches for parallel testing. |
| Homologation | Regulatory approval of a vehicle for sale in a market. |
| Homologation/regulatory compliance | Meeting legal requirements for vehicle sale. |
| HPC (High-Performance Computing) | Powerful vehicle computer for ADAS/infotainment/central compute. |
| HSM (Hardware Security Module) | Secure hardware for cryptographic keys and operations. |
| Hypervisor | Software virtualizing hardware to run multiple OSs/ECUs on one SoC. |
| ICD (Interface Control Document) | Document defining interfaces between components/systems. |
| Integration testing | Combining components/ECUs and verifying their interaction. |
| Interface governance | Authority and process for interface definitions. |
| Interoperability | Ability of tools/systems to exchange and use information. |
| IP protection | Safeguarding intellectual property in AI/data workflows. |
| Jitter | Variation in latency or timing. |
| Knowledge silo | Information held by a subset of people, limiting sharing. |
| Latency | Time delay in communication or processing. |
| LLM (Large Language Model) | AI model generating/understanding text; used as engineering assistant. |
| Manufacturing end-of-line | Final tests during production. |
| MBSE (Model-Based Systems Engineering) | Use of models as primary artifacts for system engineering. |
| MC/DC (Modified Condition/Decision Coverage) | Structural coverage criterion required for high-ASIL software. |
| MCAL (Microcontroller Abstraction Layer) | Lowest AUTOSAR software layer abstracting MCU hardware. |
| Middleware | Software layer between OS and applications handling communication/services. |
| MIL (Model-in-the-Loop) | Testing control algorithms against plant models. |
| Mixed-criticality | Coexistence of functions with different safety/criticality levels on shared resources. |
| MLOps | Practices for deploying and maintaining ML models. |
| Model-based development (MBD) | Development using executable models (e.g., Simulink) to generate code. |
| Multicore | Processor with multiple cores; requires partitioning and synchronization. |
| Mutation testing | Introducing faults to evaluate test effectiveness. |
| NDA (Non-Disclosure Agreement) | Contract restricting information sharing. |
| Negative testing | Testing invalid inputs and error conditions. |
| Nondeterminism | Behavior varying between runs under same inputs. |
| ODD (Operational Design Domain) | Conditions under which an automated driving system is designed to operate. |
| Onboarding | Process of bringing new engineers to productivity. |
| Open-source AUTOSAR tooling | Community tools for AUTOSAR (often incomplete/unsupported). |
| OSEK/VDX | Real-time operating system standard underlying AUTOSAR Classic OS. |
| OSS (Open-Source Software) | Software with publicly available source code. |
| OTA (Over-The-Air) | Wireless delivery of software updates to vehicles. |
| OTA deployment | Delivering updates over the air. |
| Performance testing | Testing timing, throughput, resource usage. |
| Physical AI | AI interacting with physical world (robotics/vehicles). |
| PIL (Processor-in-the-Loop) | Testing compiled code on target processor or emulator. |
| Platform debt | Accumulated cost/complexity from platform choices and extensions. |
| PLM (Product Lifecycle Management) | Management of product data across lifecycle (distinct from engineering semantics). |
| Post-production maintenance | Ongoing software maintenance after release. |
| Production programming | Programming ECUs during manufacturing. |
| Proving ground | Physical test track for vehicle validation. |
| QoS (Quality of Service) | Communication performance guarantees (bandwidth, latency, reliability). |
| RAG (Retrieval-Augmented Generation) | LLM technique grounding answers in retrieved documents. |
| Rapid HIL | Quickly deployable HIL solution (IPG). |
| Real-time | System must respond within defined time constraints. |
| Real-world scenario coverage | Coverage of actual driving scenarios. |
| Recall management | Managing safety recalls. |
| Regression testing | Re-running tests after changes to detect regressions. |
| Release management | Process of approving and distributing software releases. |
| Requirements engineering | Elicitation, analysis, specification and management of requirements. |
| Requirements traceability | Links between requirements and downstream artifacts (design, code, tests). |
| Risk-based testing | Testing focused on highest-risk areas. |
| Road testing | On-road validation. |
| Robustness testing | Testing system behavior under stress/faults. |
| RTE (Runtime Environment) | AUTOSAR middleware layer between application SWCs and BSW. |
| Safety case | Structured argument and evidence that a system is acceptably safe. |
| SBOM (Software Bill of Materials) | Inventory of software components and dependencies. |
| Scenario completeness | Degree to which test scenarios cover ODD and hazards. |
| Scenario-based testing | Testing against defined scenarios (OpenSCENARIO) for ADAS/ADS. |
| SDV (Software-Defined Vehicle) | Vehicle whose features are primarily defined and updated by software. |
| SEooC (Safety Element out of Context) | Safety component developed for reuse in multiple systems. |
| Shift-left | Moving testing/validation earlier in the lifecycle. |
| SIL (Software-in-the-Loop) | Testing software (often host-compiled) against simulated environment. |
| Simulation nondeterminism | Variation in simulation results across runs. |
| SoC (System on Chip) | Integrated circuit combining CPU/GPU/NPU and peripherals. |
| Software component (SWC) | AUTOSAR application-level software unit. |
| Software release approval | Approving software for release. |
| Software versioning | Tracking software versions. |
| SOME/IP | Automotive middleware protocol for service-oriented communication over IP. |
| SOME/IP-SD | Service discovery protocol for SOME/IP. |
| SOTIF (Safety of the Intended Functionality, ISO 21448) | Safety of system behavior in absence of faults, including unknown unsafe scenarios. |
| Static analysis | Analyzing code without execution (MISRA/CERT checks). |
| Stress testing | Testing beyond normal operating limits. |
| Supplier tiers (Tier-1/Tier-2) | Supplier hierarchy in automotive supply chain. |
| System engineering | Interdisciplinary engineering of systems over their life cycles. |
| System testing | Testing complete system. |
| Systems visibility | Understanding system-level dependencies. |
| TARA (Threat Analysis and Risk Assessment) | Cybersecurity process under ISO/SAE 21434. |
| Task (AUTOSAR OS) | Schedulable unit in OSEK-based OS. |
| TC (Test case) | Set of inputs, execution conditions and expected results. |
| TDM (Test Data Management) | Versioning and correlation of test data/artifacts across XiL levels. |
| Technical debt | Cost of rework caused by suboptimal design/implementation. |
| Telematics | Vehicle connectivity and data services. |
| Test fleet management | Managing vehicles used for validation campaigns. |
| Test oracle | Mechanism determining expected test outcomes. |
| Test prioritization | Selecting/ordering tests by risk/value. |
| Test track | Controlled environment testing. |
| Timing analysis | Determining whether software meets timing constraints (WCET, schedulability). |
| Toolchain | Set of tools used to develop, build, test and release software. |
| Traceability (vertical/horizontal) | Vertical: requirements to tests; horizontal: across XiL levels. |
| TSN (Time-Sensitive Networking) | Ethernet standards providing deterministic, low-latency communication. |
| Type approval | Regulatory certification that a vehicle meets standards. |
| UDS (Unified Diagnostic Services, ISO 14229) | Diagnostic protocol for ECUs. |
| UNECE R155 | Regulation on cybersecurity management systems (CSMS). |
| UNECE R156 | Regulation on software update management systems (SUMS). |
| Unit testing | Testing individual software units. |
| V-model | Development process with verification/validation paired to design stages. |
| Variant explosion | Combinatorial growth of product variants. |
| Variant management | Handling product variants (hardware, software, region, regulation). |
| vECU (virtual ECU) | Software emulation of an ECU for SIL testing; levels 0-4. |
| Vehicle testing | Testing on full vehicles. |
| Vehicle-in-the-Loop (VIL) | Testing with a real vehicle in a virtual environment. |
| Virtualization | Running multiple isolated environments on shared hardware. |
| Warranty management | Handling warranty claims. |
| Warranty/recall | Post-sale defect remediation. |
| WCET (Worst-Case Execution Time) | Maximum time a task can take to execute. |
| XiL (X-in-the-Loop) | Umbrella for MIL/SIL/PIL/HIL/VIL. |
| Zonal architecture | Vehicle architecture organized by physical zones with zonal controllers. |

*Total terms: 187.*

*End of glossary.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

