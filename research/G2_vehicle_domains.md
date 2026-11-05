# G2 — Vehicle Engineering Domains: Software Challenges

**Workstream:** G2 (Vehicle Domains)  
**Scope:** Problem discovery, root-cause analysis, evidence, unanswered questions across vehicle engineering/product domains (ADAS, EV, body, powertrain, chassis, etc.)  
**Date:** 2026-11-05  
**Status:** Verify (Tier 2) — findings cross-checked against multiple sources; numerical claims only where sourced.

---

## 1. Workstream Summary

Vehicle-domain software in the automotive industry is characterised by:

- **Domain-specific control problems** (ADAS perception, powertrain calibration, chassis control, EV energy management) that are tightly coupled to physics, sensors, and actuators, and cannot be solved by generic software engineering alone.
- **Explosive growth in software complexity**: features are increasingly software-defined (OTA-updatable), while hardware remains fixed at SOP; the gap between hardware capability and software ambition is widening.
- **Safety-criticality gradient**: from non-safety body/infotainment to ASIL-D braking/steering, with mixed-criticality integration on shared compute (zonal/HPC) creating new failure modes.
- **Validation scalability crisis**: real-world scenario coverage for ADAS/AV is unbounded; HIL availability and vehicle test capacity are bottlenecks; simulation fidelity is limited.
- **Cross-domain integration as discovery**: motion control, energy management, and ADAS share actuators (brakes, steering, powertrain); integration is where hidden interaction bugs surface.
- **Variant explosion**: hardware, software, region, regulation, and powertrain variants multiply combinatorially; calibration and configuration drift across variants is poorly managed.
- **Legacy-to-SDV transition**: distributed ECUs → domain controllers → zonal/centralised HPC forces re-architecture of proven (but fragmented) domain software.
- **Supplier/OEM boundary blur**: domain software ownership is split across OEM, Tier-1 (Bosch, Continental, ZF, Aptiv), and Tier-2 (silicon/sensor vendors), with incomplete interface specifications.

---

## 2. Problem Catalogue

### G2-001 — ADAS perception software: sensor fusion, ML model lifecycle, and ODD specification

- **Domain:** ADAS
- **Lifecycle stage:** Software design/development, validation, release
- **Problem:** ADAS perception software must fuse heterogeneous sensors (camera, radar, lidar, ultrasonic) into a consistent world model, manage ML model training/data/ODD, and validate against an effectively unbounded scenario space. ML components are data-driven and opaque; requirements on data, ODD, and monitors are not handled by classic RE.
- **Who experiences it:** ADAS software engineers, perception ML engineers, system validation engineers, safety engineers.
- **Root cause:** RE methods designed for deterministic software; ML is data-driven; perception is open-world; no universal automotive requirement ontology for ML.
- **Contributing factors:** Multi-sensor fusion complexity; ODD boundaries poorly specified; training data quality/curation; model drift; sensor degradation over vehicle life.
- **Technical limitations:** Sensor fusion algorithms (early/late/track-level) trade-offs; ML opaqueness; limited real-world scenario coverage; simulation fidelity gaps.
- **Human skill limitations:** ML engineering + functional safety hybrid skills scarce; scenario authoring expertise.
- **Tool limitations:** ALM tools do not trace models/datasets/ODDs; simulation platforms (CARLA, dSPACE, IPG) have fidelity limits.
- **Process limitations:** No mandatory ODD specification at intake; change control for models is immature.
- **Frequency/severity:** Pervasive for L2+ ADAS; safety-critical.
- **Time/cost impact:** ADAS validation is a dominant cost driver in modern vehicle programs (industry estimates often cite ADAS as largest software R&D share; specific figures not verified here).
- **Downstream effects:** Late defect discovery, recall risk, safety case gaps, regulatory uncertainty (UNECE R157 for ALKS, ISO 21448 SOTIF, ISO/PAS 8800 AI safety).
- **Existing solutions:** SOTIF (ISO 21448), ISO/PAS 8800, scenario-based testing frameworks, sensor fusion stacks, MLOps.
- **Why insufficient:** Standards still maturing; tooling for ML traceability absent; scenario coverage unbounded.
- **Automation feasibility:** Medium (scenario generation, data curation, model monitoring).
- **AI applicability:** High as tool (generation, analysis); low as authority for safety sign-off.
- **Human approval:** Mandatory for safety-relevant ADAS.
- **Evidence/sources:**
  - Wikipedia "Advanced driver-assistance system" — real-time preemptive priority scheduling; ODD defines SAE automation level. https://en.wikipedia.org/wiki/Advanced_driver-assistance_system
  - Habibullah et al. 2024, Requirements Eng 29:25–48 — ML-specific RE gaps (ODDs, scenarios, training data). https://link.springer.com/article/10.1007/s00766-023-00410-1
  - W1-004 (requirements for ML/AI) in W1_requirements_system.md.
- **Confidence:** Medium–High.
- **Unanswered questions:** How do OEMs currently specify training-data requirements? What is the state of practice for ODD specification? What is the achievable scenario coverage?

### G2-002 — ADAS real-time scheduling and priority assignment risks

- **Domain:** ADAS
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** ADAS are real-time systems reacting to multiple inputs; incorrect assignment of preemptive priorities can cause more harm than good; timing predictability on consolidated compute is unsolved (see W1-010).
- **Who experiences it:** ADAS platform engineers, RTOS/safety-OS engineers, timing analysts.
- **Root cause:** Complexity of multi-sensor, multi-function real-time scheduling; AUTOSAR standard lacks timing aspects; cache effects.
- **Contributing factors:** Consolidation onto fewer cores; mixed-criticality; lack of timing models bridging system and implementation.
- **Technical limitations:** Timing analysis largely unsolved for AUTOSAR; probabilistic timing analysis research-stage.
- **Existing solutions:** AUTOSAR timing extensions; formal verification research; WCET analysis tools.
- **Why insufficient:** Not integrated into standard toolchains; not certified.
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Evidence/sources:** Wikipedia ADAS (preemptive priority scheduling; incorrect priority assignment can cause harm); TU Munich/TU Braunschweig timing papers (W1-010).
- **Confidence:** High.
- **Unanswered questions:** How often do priority/timing errors surface in integration vs. design?

### G2-003 — Body electronics: BCM feature creep and central body controller complexity

- **Domain:** Body Electronics / BCM
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** The BCM is a generic ECU controlling power windows, mirrors, HVAC, immobiliser, central locking, lighting, etc., communicating over CAN. As comfort/convenience features multiply (keyless entry, ambient lighting, gesture, seat memory, soft-close doors), BCM software grows into a large, poorly modularised feature set with many I/O and timing constraints; central body controllers consolidate more functions, increasing single-point-of-failure risk.
- **Who experiences it:** Body electronics engineers, BCM suppliers, OEM body-domain architects.
- **Root cause:** Feature-driven growth without architectural governance; BCM treated as "dumping ground" for body features; lack of domain-level software platform.
- **Contributing factors:** OEM-specific feature differentiation; late feature additions; variant explosion (trim levels); wiring harness cost pressure driving consolidation.
- **Technical limitations:** I/O pin count and PWM/ADC constraints; real-time lighting/comfort control; EMC; low-power sleep/wake management.
- **Human skill limitations:** Body-domain knowledge; low-level driver expertise.
- **Tool limitations:** Limited body-domain simulation; configuration via AUTOSAR is heavy for simple body features.
- **Process limitations:** Feature changes late in cycle; no body-domain architecture authority.
- **Frequency/severity:** Pervasive; moderate safety (immobiliser, lighting) but high customer-visible quality impact.
- **Time/cost impact:** BCM software is a frequent source of late integration defects and warranty claims (estimate; not quantified here).
- **Downstream effects:** Warranty, customer dissatisfaction, sleep-current (battery drain) issues, immobiliser/security defects.
- **Existing solutions:** Central body controllers, zonal ECUs, AUTOSAR BCM stacks, feature configuration tools.
- **Why insufficient:** AUTOSAR overhead for simple features; variant management weak; centralisation increases blast radius.
- **Automation feasibility:** Medium (feature configuration, I/O mapping).
- **AI applicability:** Medium.
- **Human approval:** Required for safety/security features.
- **Evidence/sources:**
  - Wikipedia "Body control module" — generic ECU for body accessories; communicates over CAN; controls load drivers/relays. https://en.wikipedia.org/wiki/Body_control_module
  - TI automotive central body controller solution reference.
- **Confidence:** Medium.
- **Unanswered questions:** What fraction of warranty claims trace to body electronics software? How much BCM software is reused across OEM programs?

### G2-004 — Powertrain/engine management: calibration complexity and emissions compliance

- **Domain:** Powertrain / Engine Management
- **Lifecycle stage:** Design, calibration, validation, release
- **Problem:** Engine control software (fuel injection, ignition, idle, VVT, boost, aftertreatment) requires massive calibration datasets (maps) tuned across engine variants, fuels, climates, and emissions regulations. Calibration is largely manual, iterative, and vehicle-dependent; emissions compliance (Euro 7, EPA Tier 3, China 6) pushes control complexity; hybridisation adds coordination between ICE and electric drive.
- **Who experiences it:** Powertrain calibration engineers, ECU developers, emissions test engineers, OEM powertrain teams.
- **Root cause:** Physical complexity of combustion and aftertreatment; regulatory pressure; lack of first-principles models accurate enough to replace empirical calibration.
- **Contributing factors:** Variant explosion (engine × transmission × region × fuel); test cell and vehicle test capacity; late hardware changes invalidating calibrations.
- **Technical limitations:** Model-based calibration (MBC) accuracy; real-time ECU compute limits; sensor accuracy (lambda, NOx, PM); aftertreatment thermal management.
- **Human skill limitations:** Calibration expertise is scarce and tacit; long training time.
- **Tool limitations:** Calibration tools (INCA, CANape) are powerful but manual; automation of DOE-based calibration is partial.
- **Process limitations:** Calibration late in cycle; change propagation across variants manual.
- **Frequency/severity:** Pervasive; high cost.
- **Time/cost impact:** Powertrain calibration is a multi-month, multi-vehicle effort; emissions failures late in program cause major delays (industry knowledge; not quantified here).
- **Downstream effects:** Emissions non-compliance, fuel consumption, drivability complaints, recall.
- **Existing solutions:** Model-based calibration, automated DOE, virtual calibration, HIL for powertrain.
- **Why insufficient:** Models not accurate enough for full virtual calibration; vehicle-in-the-loop still required; regulatory acceptance of virtual methods limited.
- **Automation feasibility:** Medium (DOE automation, virtual calibration).
- **AI applicability:** Medium (surrogate models, optimisation).
- **Human approval:** Required for emissions compliance.
- **Evidence/sources:**
  - Wikipedia "Engine control unit" — fuel injection, ignition, idle, VVT; extensive sensor set; ECU introduced by Bosch Motronic 1979. https://en.wikipedia.org/wiki/Engine_control_unit
  - W1-010 timing analysis challenges.
- **Confidence:** Medium–High.
- **Unanswered questions:** What fraction of calibration can be moved virtual? Cost of late emissions failure?

### G2-005 — Transmission and drivetrain control software: shift quality and coordination

- **Domain:** Transmission / Drivetrain
- **Lifecycle stage:** Design, calibration, integration
- **Problem:** Transmission control software (AT, DCT, CVT, e-drive) must deliver smooth shift quality, launch, and coordination with engine/e-motor torque, across variants and driving styles. Shift-quality tuning is subjective and empirical; hybrid/e-drive transitions add new coordination failure modes.
- **Who experiences it:** Transmission control engineers, drivability engineers, integration teams.
- **Root cause:** Coupled torque handover between engine and transmission; lack of predictive models for subjective shift quality; variant-specific tuning.
- **Contributing factors:** DCT clutch thermal limits; e-drive torque ripple; driver expectation variance; regional driving style differences.
- **Technical limitations:** Real-time torque coordination; sensor resolution; actuator response time.
- **Existing solutions:** Model-based control, HIL for transmission, automated shift-quality metrics.
- **Why insufficient:** Subjective quality still requires human judgement; metrics incomplete.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (drivability prediction).
- **Human approval:** Required.
- **Evidence/sources:** Industry knowledge; OBD-II DTCs include transmission (P07xx/P08xx) — https://en.wikipedia.org/wiki/On-board_diagnostics.
- **Confidence:** Medium.
- **Unanswered questions:** How much shift-quality tuning can be automated? Prevalence of drivability complaints?

### G2-006 — Chassis and vehicle dynamics: ESC/ABS/TCS integration and sensor fusion

- **Domain:** Chassis / Vehicle Dynamics
- **Lifecycle stage:** Design, integration, validation
- **Problem:** Chassis control (ESC, ABS, TCS, AWD) fuses wheel-speed, yaw, lateral acceleration, steering angle, and brake pressure sensors to control individual wheel brakes and torque. Integration of multiple chassis functions on shared sensors/actuators creates interaction conflicts; calibration across surfaces/conditions is extensive; validation requires diverse road surfaces and weather.
- **Who experiences it:** Chassis control engineers, vehicle dynamics engineers, test drivers.
- **Root cause:** Nonlinear tire dynamics; coupled control objectives (stability vs. agility vs. comfort); sensor/actuator limits.
- **Contributing factors:** Mixed surfaces, low-mu conditions, sensor faults, actuator saturation.
- **Technical limitations:** Tire model accuracy; real-time compute; sensor fault detection.
- **Existing solutions:** Integrated chassis control, HIL for ESC, vehicle dynamics simulation (CarMaker, VI-CarRealTime).
- **Why insufficient:** Simulation of low-mu/tire behavior limited; validation still requires physical testing.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required for safety.
- **Evidence/sources:**
  - Wikipedia ADAS — ESC reduces speed and activates individual brakes to prevent understeer/oversteer; uses wheel-speed sensors shared with ABS/TCS. https://en.wikipedia.org/wiki/Advanced_driver-assistance_system
  - W1-016 zonal/centralised architecture challenges.
- **Confidence:** Medium–High.
- **Unanswered questions:** Prevalence of chassis interaction defects in integration? How much chassis tuning can be virtualised?

### G2-007 — Braking systems and brake-by-wire: safety-critical redundancy and feel

- **Domain:** Braking / Brake-by-Wire
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** Brake-by-wire (and electro-hydraulic braking) removes the mechanical/hydraulic fallback, requiring redundant sensing, actuation, and power; software must guarantee braking under all fault conditions while preserving pedal feel. ASIL-D. Validation must cover fault injection and degraded modes.
- **Who experiences it:** Brake system engineers, functional safety engineers, validation engineers.
- **Root cause:** Removal of mechanical fallback increases software/hardware redundancy burden; pedal-feel is subjective; fault-tolerant control complexity.
- **Contributing factors:** Redundant power supplies, ECU failover, sensor disagreement handling, actuator fail-safe states.
- **Technical limitations:** Redundancy cost/weight; real-time fault detection latency; simulation of degraded modes.
- **Existing solutions:** Redundant ECUs, diverse software, fault injection testing, HIL.
- **Why insufficient:** Cost pressure; validation of all degraded modes impractical; feel tuning empirical.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Confidence:** Medium–High.
- **Unanswered questions:** Failure rates of brake-by-wire systems in field? Cost of redundancy?

### G2-008 — Steering systems and steer-by-wire: fault tolerance and driver feel

- **Domain:** Steering / Steer-by-Wire
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** Steer-by-wire removes mechanical steering column, requiring redundant motors/sensors/power and software-controlled steering feel; torque feedback must be synthesized; safety case for loss of steering assistance is severe.
- **Who experiences it:** Steering system engineers, safety engineers.
- **Root cause:** Removal of mechanical linkage; need for continuous steering torque without mechanical backup; ASIL-D.
- **Contributing factors:** Redundant actuators, sensor fusion, power redundancy, thermal management of motors.
- **Technical limitations:** Motor torque ripple, latency, fail-operational design.
- **Existing solutions:** Redundant steer-by-wire systems (Infiniti Q50 early), EPS with fallback.
- **Why insufficient:** Cost; driver acceptance of synthesized feel; validation burden.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Confidence:** Medium.
- **Unanswered questions:** Adoption rate of steer-by-wire? Field reliability?

### G2-009 — Suspension and active suspension: control algorithms and road preview

- **Domain:** Suspension / Active Suspension
- **Lifecycle stage:** Design, calibration, validation
- **Problem:** Active/semi-active suspension control must balance comfort and handling in real time using damper/actuator control, often with road-preview sensors (camera/radar) and body/chassis state estimation. Tuning is empirical and subjective; interaction with ADAS (e.g., camera for road preview) couples domains.
- **Who experiences it:** Suspension control engineers, vehicle dynamics engineers.
- **Root cause:** Coupled multi-objective control; subjective comfort metrics; sensor fusion across domains.
- **Contributing factors:** Road preview sensor availability; actuator bandwidth; cost.
- **Existing solutions:** Skyhook/groundhook control, model-predictive control, preview control.
- **Why insufficient:** Tuning remains empirical; preview sensors not always available.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (comfort prediction).
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** How much suspension tuning can be automated?

### G2-010 — EV powertrain software: inverter control, e-motor, and torque management

- **Domain:** EV / Electric Powertrain
- **Lifecycle stage:** Design, integration, validation
- **Problem:** EV powertrain software controls inverter/motor torque, regenerative braking, torque vectoring, and coordination with BMS/thermal/vehicle dynamics. High-speed motor control (PWM, field-oriented control) demands deterministic real-time; functional safety (ASIL-C/D for torque) and integration with regen braking blending create complex failure modes.
- **Who experiences it:** EV powertrain software engineers, inverter engineers, vehicle dynamics teams.
- **Root cause:** High-bandwidth motor control; safety-critical torque; coupling with regen blending and vehicle stability.
- **Contributing factors:** Motor/inverter thermal limits; battery power limits from BMS; regen blending with friction brakes; torque vectoring interaction.
- **Technical limitations:** Real-time compute; sensor resolution; model accuracy for motor thermal.
- **Existing solutions:** Field-oriented control, torque blending algorithms, HIL for EV.
- **Why insufficient:** Integration complexity; validation of all blending scenarios.
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Confidence:** Medium–High.
- **Unanswered questions:** Prevalence of EV torque-related defects? Cost of EV powertrain validation?

### G2-011 — BMS software: SoC/SoH estimation accuracy and cell balancing

- **Domain:** BMS
- **Lifecycle stage:** Design, validation, field monitoring
- **Problem:** BMS software must estimate SoC, SoH, SoP accurately across cell chemistries, temperatures, ageing, and usage patterns; active/passive balancing across hundreds/thousands of cells; safety protection (over/under voltage, temp, current, ground fault). Estimation errors affect range, performance, and safety (thermal runaway prevention).
- **Who experiences it:** BMS software engineers, battery engineers, safety engineers, field engineers.
- **Root cause:** Cell voltage is a poor indicator of SoC for some chemistries (e.g., LFP); ageing models inaccurate; thermal gradients; internal communication voltage-shift challenges.
- **Contributing factors:** Cell-to-cell variation; temperature gradients; fast charging; cold-weather performance; calibration across cell suppliers.
- **Technical limitations:** Coulomb counting drift; ECM (equivalent circuit model) accuracy; internal communication (optical/wireless isolation) constraints; max 255 nodes in modular architecture.
- **Human skill limitations:** Electrochemistry + embedded software hybrid skills.
- **Tool limitations:** BMS HIL/simulation; cell ageing test time (months/years).
- **Process limitations:** Long validation cycles; field data feedback slow.
- **Frequency/severity:** Pervasive; safety-critical (thermal runaway).
- **Time/cost impact:** BMS development and validation is a major EV program cost; field battery issues drive warranty/recall.
- **Downstream effects:** Range anxiety, performance derating, safety incidents, warranty, recall.
- **Existing solutions:** ECM/Kalman filtering, active balancing, thermal management integration, BMS HIL.
- **Why insufficient:** Models not accurate enough across life; field ageing data limited; safety margins conservative (reducing usable capacity).
- **Automation feasibility:** Medium (model identification, parameter estimation).
- **AI applicability:** Medium–High (data-driven SoH, anomaly detection).
- **Human approval:** Required for safety.
- **Evidence/sources:**
  - Wikipedia "Battery management system" — SoC/SoH/SoP computation; protection (over/under voltage, temp, current, ground fault); balancing (passive/active); internal communication voltage-shift challenges; modular max 255 nodes; BMS tightly integrated with charger/thermal/emergency shutdown. https://en.wikipedia.org/wiki/Battery_management_system
  - Pradhan & Chakraborty 2022, J. Energy Storage 51:104427 (BMS strategies review).
- **Confidence:** Medium–High.
- **Unanswered questions:** Achievable SoC/SoH accuracy over battery life? Cost of conservative safety margins on usable capacity?

### G2-012 — Charging systems and power electronics: protocol interoperability and grid interaction

- **Domain:** Charging / Power Electronics
- **Lifecycle stage:** Design, integration, validation
- **Problem:** Charging software must support multiple standards (CCS, CHAdeMO, GB/T, NACS, wireless), handle charger negotiation, payment/roaming, grid interaction (V2G/V2H), and thermal/power limits. Interoperability across charger vendors and vehicles is poor; field charging failures are common.
- **Who experiences it:** Charging software engineers, OEM connectivity teams, EV owners.
- **Root cause:** Fragmented charging standards and protocols; OEM-specific implementations; lack of conformance testing infrastructure.
- **Contributing factors:** Regional standard divergence; charger firmware variability; payment/roaming fragmentation.
- **Technical limitations:** PLC communication (HomePlug Green PHY) reliability; high-voltage safety; thermal management during fast charge.
- **Existing solutions:** ISO 15118 (Plug & Charge), OCPP for chargers, conformance testing.
- **Why insufficient:** Adoption slow; interoperability issues persist; V2G standards maturing.
- **Automation feasibility:** Medium (conformance testing, protocol validation).
- **AI applicability:** Low–Medium.
- **Human approval:** Required for safety.
- **Confidence:** Medium.
- **Unanswered questions:** Field charging failure rates? Cost of interoperability testing?

### G2-013 — Thermal management: coupled multi-domain control

- **Domain:** Thermal Management
- **Lifecycle stage:** Design, integration, validation
- **Problem:** Vehicle thermal management coordinates battery, motor, inverter, cabin HVAC, and power electronics cooling/heating across conflicting demands (range vs. comfort vs. performance vs. battery life). Control is coupled and nonlinear; heat pump/PTC heater coordination; cold-weather range loss.
- **Who experiences it:** Thermal system engineers, BMS engineers, HVAC engineers.
- **Root cause:** Multiple heat sources/sinks with conflicting priorities; lack of integrated thermal models; real-time optimisation under constraints.
- **Contributing factors:** Cold climate range loss; fast charging heat; cabin comfort expectations.
- **Technical limitations:** Coolant network complexity; pump/valve actuation; model accuracy.
- **Existing solutions:** Integrated thermal management, predictive thermal control, heat pumps.
- **Why insufficient:** Still largely heuristic; optimisation not global.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (predictive control).
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Achievable range improvement from optimised thermal management?

### G2-014 — Vehicle motion control: integrated chassis control and actuator coordination

- **Domain:** Vehicle Motion Control
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** Vehicle motion control coordinates brakes, steering, powertrain, and suspension to achieve stability, agility, and comfort objectives (torque vectoring, chassis control). Multiple controllers compete for shared actuators; integration creates conflicts; safety case for combined control.
- **Who experiences it:** Motion control engineers, vehicle dynamics engineers, integration teams.
- **Root cause:** Coupled control objectives; lack of unified motion control architecture; actuator saturation.
- **Contributing factors:** Domain silos (brake supplier vs. steering supplier vs. powertrain); timing mismatch.
- **Existing solutions:** Integrated vehicle dynamics control, centralised motion control in SDV.
- **Why insufficient:** Supplier boundaries; standards evolving; validation burden.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Effectiveness of centralised motion control in production?

### G2-015 — Cockpit and digital cockpit: multi-domain display and HMI integration

- **Domain:** Cockpit / Digital Cockpit
- **Lifecycle stage:** Design, integration, validation
- **Problem:** Digital cockpits integrate instrument cluster, IVI, HUD, and multiple displays with safety-critical tell-tales and non-safety entertainment on shared SoC/GPU. Mixed criticality on shared graphics; driver distraction; HMI consistency across brands; boot time and responsiveness.
- **Who experiences it:** Cockpit software engineers, HMI designers, safety engineers.
- **Root cause:** Consolidation of displays onto central compute; mixed-criticality graphics; HMI complexity.
- **Contributing factors:** Regulatory tell-tale requirements; driver distraction regulations; brand differentiation.
- **Technical limitations:** GPU resource contention; boot time; graphics safety (ASIL for tell-tales).
- **Existing solutions:** Hypervisors, mixed-criticality graphics frameworks, digital cockpit platforms.
- **Why insufficient:** Hypervisor overhead; safety certification of graphics stacks; HMI testing burden.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required for safety tell-tales.
- **Confidence:** Medium.
- **Unanswered questions:** Prevalence of cockpit-related defects? Boot-time requirements?

### G2-016 — IVI: OS fragmentation, app ecosystem, and smartphone projection

- **Domain:** IVI
- **Lifecycle stage:** Design, development, integration
- **Problem:** IVI software spans multiple OSes (Linux, QNX, Android Automotive, AGL) and app ecosystems (Android Auto, CarPlay, OEM apps). Fragmentation across OEMs and tiers; app certification; OTA updates; cybersecurity (remote attack surface); driver distraction.
- **Who experiences it:** IVI software engineers, OEM digital teams, app developers.
- **Root cause:** Competing platforms and OEM differentiation; long vehicle lifecycle vs. fast consumer tech.
- **Contributing factors:** Smartphone projection (CarPlay/Android Auto) reduces OEM control but improves UX; app store governance; security patching.
- **Technical limitations:** Real-time constraints on non-RT OS; graphics performance; storage; connectivity.
- **Existing solutions:** Android Automotive OS, AGL, QNX, hypervisors, smartphone projection.
- **Why insufficient:** Fragmentation persists; OTA for IVI is complex; app quality variable.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (voice assistants, personalisation).
- **Human approval:** Required for safety-relevant HMI.
- **Evidence/sources:**
  - Wikipedia "In-car entertainment" — IVI originated with radios; now navigation, USB/BT, carputers, Wi-Fi; tech companies (Google, Apple) competing; safety (distracted driving) and security (CAN bus access, remote hacking) concerns. https://en.wikipedia.org/wiki/In-car_entertainment
- **Confidence:** Medium.
- **Unanswered questions:** IVI defect rates? Cost of maintaining multiple IVI platforms?

### G2-017 — Instrument clusters and displays: safety-critical rendering and tell-tales

- **Domain:** Instrument Clusters / Displays
- **Lifecycle stage:** Design, validation
- **Problem:** Digital instrument clusters must render safety-critical tell-tales (MIL, ABS, airbag, etc.) reliably and within regulatory timing; cluster software must boot fast and never fail to display critical warnings. Validation of all tell-tale states and failure modes.
- **Who experiences it:** Cluster software engineers, safety engineers, regulatory compliance teams.
- **Root cause:** Safety-critical rendering on consumer-grade graphics hardware; mixed criticality; boot-time requirements.
- **Contributing factors:** Regulatory tell-tale specifications; display failure modes; dimming/visibility.
- **Existing solutions:** Safety-certified graphics stacks, redundant tell-tale hardware, hypervisors.
- **Why insufficient:** Certification cost; graphics performance vs. safety trade-off.
- **Automation feasibility:** Medium (automated tell-tale verification).
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Confidence:** Medium.
- **Unanswered questions:** Cluster failure rates in field? Cost of safety-certified graphics?

### G2-018 — Telematics and connectivity: TCU software, OTA, and data management

- **Domain:** Telematics / Connectivity
- **Lifecycle stage:** Design, development, release, field monitoring
- **Problem:** Telematic control units (TCUs) manage cellular connectivity, GPS, OTA updates, remote diagnostics, eCall, and consumer connected services. Software must handle intermittent connectivity, OTA reliability, data privacy, and cybersecurity; field connectivity failures are common.
- **Who experiences it:** Telematics software engineers, connectivity teams, OEM digital teams.
- **Root cause:** Intermittent/unreliable networks; OTA complexity; data privacy regulations; security attack surface.
- **Contributing factors:** Regional cellular standards; carrier fragmentation; cost of connectivity; eCall mandate (EU).
- **Technical limitations:** OTA rollback, partial update, A/B partitioning; bandwidth; latency.
- **Existing solutions:** OTA platforms (Airbiquity, Harman, Sibros), eCall modules, V2X.
- **Why insufficient:** OTA failures in field; connectivity gaps; privacy compliance burden.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (anomaly detection, predictive maintenance).
- **Human approval:** Required for safety-critical OTA.
- **Evidence/sources:**
  - Wikipedia "Telematics" — TCU, fleet management, eCall, V2X, connected services, standards (AEMP/ISO 15143-3). https://en.wikipedia.org/wiki/Telematics
  - ISO 24089 (OTA) referenced in W1.
- **Confidence:** Medium.
- **Unanswered questions:** OTA failure rates? Cost of connectivity over vehicle life?

### G2-019 — Vehicle gateway and central computing: consolidation and virtualisation

- **Domain:** Vehicle Gateway / Central Computing
- **Lifecycle stage:** Architecture, integration
- **Problem:** Consolidation of domain functions onto central compute (HPC) via hypervisors/virtualisation creates mixed-criticality integration, resource contention, thermal/power challenges, and supplier ecosystem disruption. "Thinking vs. acting" separation; single point of failure risk.
- **Who experiences it:** Platform architects, OEM E/E architects, Tier-1s.
- **Root cause:** Consolidation trend; virtualisation overhead; standards evolving; supplier re-negotiation.
- **Contributing factors:** Cost pressure; software complexity; thermal limits; real-time predictability.
- **Technical limitations:** Hypervisor determinism; resource allocation; safety certification of virtualised stacks; thermal management.
- **Existing solutions:** Hypervisors (QNX, Linux/KVM, PikeOS), service-oriented architecture, zonal ECUs.
- **Why insufficient:** SOA portability limited (apps still platform-specific); virtualisation overhead; standards immature.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - W1-016 (zonal/centralised architecture) — Promwad 2025, Accelerat 2025, Frontiers 2025, ArXiv 2412.01840, Bosch thinking/acting separation, McKinsey 2026 outlook.
- **Confidence:** Medium–High.
- **Unanswered questions:** Failure rates of consolidated compute? Cost of redundancy?

### G2-020 — Vehicle networking: multi-bus gateways and bandwidth constraints

- **Domain:** Vehicle Networking
- **Lifecycle stage:** Architecture, integration
- **Problem:** Modern vehicles integrate CAN, CAN FD, LIN, FlexRay, and Automotive Ethernet through gateways. Bandwidth, latency, and timing constraints across heterogeneous networks; gateway design is a bottleneck; function-based vs. ECU-based timing constraints; network management complexity.
- **Who experiences it:** Network architects, gateway developers, integration engineers.
- **Root cause:** Legacy multi-bus architectures; distributed development by many teams; timing constraints not captured in AUTOSAR.
- **Contributing factors:** ADAS/infotainment bandwidth demands; Ethernet adoption; gateway processing limits.
- **Technical limitations:** CAN bandwidth (1 Mbps / FD 8 Mbps) vs. Ethernet (100 Mbps/1 Gbps); gateway latency; network management state machines.
- **Existing solutions:** Automotive Ethernet, TSN, AUTOSAR communication stacks, network simulation.
- **Why insufficient:** Timing analysis unsolved; gateway bottlenecks; cost of Ethernet migration.
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - W1-018 (network architecture and timing); AUTOSAR FO Timing Analysis TR (R24-11).
- **Confidence:** Medium.
- **Unanswered questions:** Prevalence of network timing violations in production?

### G2-021 — Zonal architecture: software migration and re-architecture

- **Domain:** Zonal Architecture
- **Lifecycle stage:** Architecture, migration, integration
- **Problem:** Migration from domain-based to zonal architecture requires re-partitioning software across zonal ECUs and central compute, redefining interfaces, and re-validating. Legacy software not designed for zonal deployment; migration cost and risk high.
- **Who experiences it:** OEM E/E architects, software platform teams, Tier-1s.
- **Root cause:** Legacy software tightly coupled to domain ECUs; lack of portable software components; SOA maturity.
- **Contributing factors:** Variant management; supplier IP; safety certification reuse.
- **Existing solutions:** Virtualisation, SOA, AUTOSAR Adaptive, zonal ECUs.
- **Why insufficient:** SOA portability limited; migration cost; standards evolving.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Migration cost per program? Success rate of zonal transitions?

### G2-022 — HPC: compute resource allocation, scheduling, and thermal

- **Domain:** HPC / High-Performance Computing
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** Central HPC must host ADAS, IVI, cockpit, and connectivity workloads with mixed criticality on shared SoC/GPU/NPU. Resource allocation, scheduling, and thermal management are unsolved; real-time guarantees on non-RT accelerators; software portability across SoC generations.
- **Who experiences it:** Platform architects, ADAS platform teams, SoC vendors.
- **Root cause:** Mixed-criticality on shared compute; lack of automotive-grade virtualisation; SoC vendor lock-in; thermal limits.
- **Contributing factors:** AI/ML workload growth; NPU heterogeneity; software portability.
- **Existing solutions:** Hypervisors, ROS 2, AUTOSAR Adaptive, containerisation.
- **Why insufficient:** Real-time guarantees on GPU/NPU; portability across SoC; thermal.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Real-time performance of shared GPU/NPU? Cost of SoC migration?

### G2-023 — Vehicle diagnostics: DTC standardisation and proprietary extensions

- **Domain:** Vehicle Diagnostics
- **Lifecycle stage:** Design, validation, service
- **Problem:** OBD-II/EOBD standardise connector and basic PIDs, but manufacturers add proprietary DTCs and PIDs; diagnostic coverage for non-emissions systems (chassis, body, ADAS) is inconsistent; aftermarket tools cannot access all systems; security (UDS security access) gates diagnostics.
- **Who experiences it:** Diagnostic engineers, service technicians, aftermarket tool vendors, fleet operators.
- **Root cause:** Regulatory scope limited to emissions; OEMs add proprietary diagnostics; security requirements.
- **Contributing factors:** UDS security access (seed/key); diagnostic session management; ECU variant differences.
- **Technical limitations:** OBD-II PID coverage; UDS service availability; diagnostic database (ODX) management.
- **Existing solutions:** UDS (ISO 14229), ODX (ISO 22901), diagnostic tools (CANoe, CANdelaStudio).
- **Why insufficient:** Proprietary extensions; ODX consistency; security access friction.
- **Automation feasibility:** Medium (ODX validation, diagnostic coverage checks).
- **AI applicability:** Medium.
- **Human approval:** Required for security access.
- **Evidence/sources:**
  - Wikipedia "On-board diagnostics" — OBD-II standardised connector/PIDs but manufacturers add proprietary DTCs; UDS security; ODX. https://en.wikipedia.org/wiki/On-board_diagnostics
- **Confidence:** Medium.
- **Unanswered questions:** Diagnostic coverage gaps in production? Cost of diagnostic database maintenance?

### G2-024 — Vehicle security: attack surface expansion and cybersecurity engineering

- **Domain:** Vehicle Security
- **Lifecycle stage:** Architecture, design, validation, field monitoring
- **Problem:** Connected vehicles expose large attack surface (TCU, IVI, OBD, V2X, OTA). Cybersecurity must be engineered across the lifecycle (ISO/SAE 21434, UNECE R155); threat analysis, secure coding, penetration testing, and incident response are immature; legacy ECUs lack security.
- **Who experiences it:** Cybersecurity engineers, OEM security teams, suppliers.
- **Root cause:** Connectivity growth outpacing security engineering; legacy architecture not designed for security; skills gap.
- **Contributing factors:** Supply-chain security; third-party software; OTA attack surface; resource constraints on MCUs.
- **Technical limitations:** Crypto on constrained MCUs; secure boot; key management; intrusion detection.
- **Existing solutions:** ISO/SAE 21434, UNECE R155, secure boot, HSMs, IDS/IPS.
- **Why insufficient:** Compliance burden; legacy ECU retrofit; skills gap; evolving threats.
- **Automation feasibility:** Medium (threat modelling, SAST/DAST).
- **AI applicability:** Medium (anomaly detection).
- **Human approval:** Required.
- **Evidence/sources:**
  - Wikipedia "In-car entertainment" — IVI can access CAN bus; remote car hacking concerns. https://en.wikipedia.org/wiki/In-car_entertainment
  - W1-014 compliance overhead.
- **Confidence:** Medium–High.
- **Unanswered questions:** Field cyber incident rates? Cost of cybersecurity compliance per program?

### G2-025 — Vehicle energy management: EV range optimisation and energy prediction

- **Domain:** Vehicle Energy Management
- **Lifecycle stage:** Design, validation, field monitoring
- **Problem:** EV energy management optimises range across driving conditions, climate, terrain, and user behavior; prediction accuracy affects user trust and planning; integration with navigation, thermal, and BMS; energy consumption modelling.
- **Who experiences it:** Energy management engineers, BMS teams, navigation teams.
- **Root cause:** Variability of real-world conditions; model accuracy; user behavior.
- **Contributing factors:** Climate, terrain, driving style, HVAC usage, battery ageing.
- **Technical limitations:** Energy consumption models; real-time optimisation; data availability.
- **Existing solutions:** Predictive energy management, eco-routing, regen optimisation.
- **Why insufficient:** Prediction errors; user trust; integration complexity.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium–High (prediction, personalisation).
- **Human approval:** Required for safety.
- **Confidence:** Medium.
- **Unanswered questions:** Achievable prediction accuracy? User trust impact?

### G2-026 — Vehicle access and comfort systems: keyless entry and comfort features

- **Domain:** Vehicle Access / Comfort
- **Lifecycle stage:** Design, validation
- **Problem:** Keyless entry, passive entry, seat/steering memory, ambient lighting, and comfort features involve RF, UWB, BLE, and actuator control; relay attacks on keyless entry are a known security issue; comfort feature integration with body/seat ECUs.
- **Who experiences it:** Body/access engineers, security engineers.
- **Root cause:** RF security weaknesses (relay attacks); feature proliferation; integration with body domain.
- **Contributing factors:** Cost of secure RF (UWB); legacy keyless systems; variant complexity.
- **Existing solutions:** UWB (ultra-wideband) for secure ranging, challenge-response, motion sensing.
- **Why insufficient:** Legacy systems vulnerable; UWB adoption cost; migration.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Required for security.
- **Confidence:** Medium.
- **Unanswered questions:** Keyless theft rates? Cost of UWB migration?

### G2-027 — Safety-critical embedded systems: mixed criticality and freedom from interference

- **Domain:** Safety-Critical Embedded Systems
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** Safety-critical embedded software (ASIL-B/C/D) must demonstrate freedom from interference when integrated with non-safety software on shared compute; mixed-criticality scheduling; safety OS and hypervisor certification; tool qualification.
- **Who experiences it:** Safety engineers, OS/platform engineers.
- **Root cause:** Consolidation trend; certification cost; complex interference analysis.
- **Contributing factors:** Shared memory, cache, bus contention; timing interference.
- **Existing solutions:** Safety OS (QNX, PikeOS, AUTOSAR OS), hypervisors, partitioning, WCET analysis.
- **Why insufficient:** Certification cost; interference analysis immaturity; tool qualification.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Confidence:** Medium–High.
- **Unanswered questions:** Cost of safety certification for virtualised stacks?

### G2-028 — Aerospace-inspired automotive technologies: certification and reuse gap

- **Domain:** Aerospace-inspired Automotive
- **Lifecycle stage:** Architecture, certification
- **Problem:** Aerospace practices (DO-178C, ARINC 653, IMA) inspire automotive centralised computing, but certification regimes differ (ISO 26262 vs DO-178C); reuse of aerospace components in automotive is limited by cost, certification, and operating environment; aerospace-grade determinism vs. automotive cost.
- **Who experiences it:** System architects, safety engineers, certification teams.
- **Root cause:** Different certification ecosystems; cost/weight constraints; operating environment differences.
- **Contributing factors:** Automotive cost pressure; aerospace development pace; toolchains.
- **Existing solutions:** IMA-inspired architectures, ARINC 653-like partitioning, DO-178C-inspired processes.
- **Why insufficient:** Cost; certification gap; maturity.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Confidence:** Medium.
- **Unanswered questions:** Actual reuse of aerospace components in automotive? Cost/benefit?

### G2-029 — Commercial vehicles and off-highway: harsh environment and diverse variants

- **Domain:** Commercial Vehicles / Off-Highway
- **Lifecycle stage:** Design, validation, field
- **Problem:** Commercial/off-highway vehicles (trucks, buses, construction, agriculture) operate in harsh environments, long lifecycles (10-20 years), diverse duty cycles, and many variants (PTO, body builder interfaces). Software must support body-builder integration, telematics for fleet, and durability; aftermarket modifications are common.
- **Who experiences it:** CV software engineers, body builders, fleet operators.
- **Root cause:** Long lifecycle; harsh environment; body-builder interface variability; aftermarket modifications.
- **Contributing factors:** Regulatory (Euro VI, EPA HD), telematics mandates, connectivity.
- **Technical limitations:** Durability of electronics; vibration/temperature; power supply quality.
- **Existing solutions:** J1939 (CAN for HD), OBD for HD, telematics standards (AEMP).
- **Why insufficient:** Body-builder interface complexity; aftermarket support; long-term maintenance.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (predictive maintenance).
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** CV software maintenance cost over 20-year lifecycle?

### G2-030 — Two-wheeler and three-wheeler electronics: cost constraints and feature scaling

- **Domain:** Two/Three-Wheeler Electronics
- **Lifecycle stage:** Design, cost engineering
- **Problem:** Two/three-wheelers (motorcycles, scooters, e-rickshaws) have severe cost constraints, limited compute, and growing feature demand (ABS, connectivity, EV conversion). Software must deliver safety (ABS mandatory in many markets) and connectivity at low cost; EV two-wheelers need BMS and charging at minimal cost.
- **Who experiences it:** Two-wheeler OEMs, Tier-1s, suppliers.
- **Root cause:** Cost pressure; limited compute; feature demand growth; EV transition.
- **Contributing factors:** Emerging market price sensitivity; regulatory (ABS mandates); charging infrastructure.
- **Technical limitations:** Low-cost MCUs; limited memory; cost of sensors.
- **Existing solutions:** Low-cost ABS, basic BMS, simple connectivity.
- **Why insufficient:** Feature gap vs. cars; cost of safety features; EV range/cost.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Required for safety.
- **Confidence:** Medium.
- **Unanswered questions:** Cost of ABS compliance for low-end two-wheelers? EV two-wheeler BMS field issues?

### G2-031 — ADAS scenario validation and real-world coverage gap

- **Domain:** ADAS / Validation
- **Lifecycle stage:** Testing and validation
- **Problem:** ADAS validation requires scenario coverage across ODD, weather, lighting, and road conditions; real-world testing is expensive and slow; simulation fidelity is limited; edge cases are rare but critical; regulatory approval (UNECE R157 for ALKS) demands evidence.
- **Who experiences it:** Validation engineers, safety engineers, regulators.
- **Root cause:** Unbounded scenario space; limited test capacity; simulation fidelity gaps.
- **Contributing factors:** Weather variability; rare edge cases; sensor degradation; HIL availability.
- **Technical limitations:** Simulation fidelity (sensor models, tire/road, weather); scenario database management; coverage metrics.
- **Existing solutions:** Scenario-based testing, HIL, SIL, virtual validation, safety case frameworks.
- **Why insufficient:** Coverage metrics immature; simulation not accepted as sole evidence; cost.
- **Automation feasibility:** Medium (scenario generation, coverage analysis).
- **AI applicability:** Medium (scenario generation, anomaly detection).
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Achievable scenario coverage? Regulatory acceptance of virtual validation?

### G2-032 — Cross-domain integration: motion, energy, and ADAS actuator conflicts

- **Domain:** Cross-domain (Vehicle-level)
- **Lifecycle stage:** Integration, validation
- **Problem:** Motion control, energy management, and ADAS share actuators (brakes, steering, powertrain); integration reveals conflicts (e.g., ADAS emergency braking vs. energy-regen blending vs. ESC). Integration is where hidden interaction bugs surface; testing all combinations is impractical.
- **Who experiences it:** Integration engineers, vehicle dynamics engineers, ADAS teams.
- **Root cause:** Domain silos; shared actuators; incomplete interface specs; combinatorial explosion of scenarios.
- **Contributing factors:** Supplier boundaries; timing mismatch; lack of integrated simulation.
- **Existing solutions:** Integrated vehicle simulation, HIL, vehicle-level testing.
- **Why insufficient:** Simulation fidelity; test capacity; combinatorial explosion.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Prevalence of cross-domain interaction defects? Cost of integration testing?

---

## 3. Cross-Cutting Observations

1. **ADAS/AV validation is the dominant cost driver** in modern vehicle programs; scenario coverage is unbounded and simulation fidelity is limited.
2. **EV/BMS software is safety-critical and long-lived**: estimation accuracy affects range and safety; field ageing data is slow to accumulate.
3. **Mixed criticality on shared compute** (cockpit, HPC, zonal) creates new failure modes and certification challenges across all domains.
4. **Cross-domain integration is where defects surface**: motion, energy, and ADAS share actuators; integration testing is combinatorially expensive.
5. **Variant explosion** affects every domain (powertrain calibrations, regional features, trim levels); configuration management is weak.
6. **Supplier/OEM boundaries** split domain software ownership; interface specifications are incomplete and drift.
7. **Legacy-to-SDV transition** forces re-architecture of proven domain software; migration cost and risk are high.
8. **Field feedback loops are slow**: BMS ageing, ADAS edge cases, and connectivity issues take months/years to surface.

---

## 4. Evidence and Source Index (selected)

- Wikipedia "Advanced driver-assistance system" — ADAS real-time scheduling, ODD, sensor fusion. https://en.wikipedia.org/wiki/Advanced_driver-assistance_system
- Wikipedia "Battery management system" — BMS functions, protection, balancing, internal communication. https://en.wikipedia.org/wiki/Battery_management_system
- Wikipedia "Body control module" — BCM functions, CAN communication. https://en.wikipedia.org/wiki/Body_control_module
- Wikipedia "Engine control unit" — ECU functions, sensors, history. https://en.wikipedia.org/wiki/Engine_control_unit
- Wikipedia "In-car entertainment" — IVI, safety, security. https://en.wikipedia.org/wiki/In-car_entertainment
- Wikipedia "Telematics" — TCU, connectivity, standards. https://en.wikipedia.org/wiki/Telematics
- Wikipedia "On-board diagnostics" — OBD-II, diagnostics, security. https://en.wikipedia.org/wiki/On-board_diagnostics
- Habibullah et al. 2024, Requirements Eng 29:25–48 — ML-specific RE gaps.
- Pradhan & Chakraborty 2022, J. Energy Storage 51:104427 — BMS strategies.
- W1_requirements_system.md — requirements/architecture/timing/zonal/compliance problems.
- W2_autosar_middleware.md — AUTOSAR Classic/Adaptive, middleware (referenced).
- W3_testing_validation.md — testing/validation (referenced).
- ISO 26262, ISO/SAE 21434, ISO 21448 (SOTIF), ISO/PAS 8800, UNECE R155/R156/R157, ISO 14229 (UDS), ISO 15118, ISO 24089.
- AUTOSAR FO Timing Analysis TR (R24-11).

---

## 5. Unanswered Research Questions (consolidated)

1. What is the actual cost share of ADAS validation in modern vehicle programs?
2. How much powertrain calibration can be moved to virtual (model-based) methods?
3. What is the achievable SoC/SoH estimation accuracy over battery life?
4. What is the field failure rate of brake-by-wire and steer-by-wire systems?
5. How much scenario coverage is achievable for ADAS, and what are the regulatory acceptance criteria?
6. What is the cost and success rate of zonal architecture migration?
7. What is the real-time performance of shared GPU/NPU for mixed-criticality workloads?
8. What fraction of IVI/cockpit defects are safety-relevant vs. quality?
9. What is the OTA failure rate in the field?
10. What is the cost of cybersecurity compliance per vehicle program?
11. How much cross-domain integration testing is needed, and what is its cost?
12. What is the long-term maintenance cost of CV/off-highway software over 20-year lifecycles?

---

## 6. Limitations of This Workstream

- Numerical figures are not invented; where industry estimates are referenced, they are flagged as unverified.
- No direct access to OEM proprietary data; findings rely on published studies, Wikipedia, vendor blogs, and open papers.
- Tool-specific claims reflect vendor positioning; treat as indicative.
- Some sources are vendor marketing; contradictory evidence not fully resolved.
- Tier 2 (Verify) depth; Tier 3 (Auditor) would require full pagination of all sources and additional independent verification.
- Web search tool was unavailable during this research pass; sources gathered via direct fetch of known authoritative pages; breadth may be uneven across domains.
- Coverage of two/three-wheeler and off-highway domains is thinner; deeper research recommended.

---

*End of G2 report.*

---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

