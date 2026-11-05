# Part I — Automotive Industry Fundamentals

## Chapter 3 — Vehicle Domains and Engineering Disciplines

### Summary

Vehicle software is not one discipline but many. Each functional domain—powertrain, chassis, braking, steering, EV/BMS, ADAS, body, cockpit/IVI, connectivity, diagnostics, security, energy—has its own physics, safety criticality, control theory, calibration practice, and supplier ecosystem. Problems recur across domains (mixed criticality, variant explosion, supplier boundaries, validation scalability) but manifest differently: powertrain calibration is an empirical, months-long manual effort (G2-004); ADAS validation faces an unbounded scenario space (G2-001, G2-031); BMS estimation accuracy affects range and safety over years (G2-011); brake/steer-by-wire remove mechanical fallbacks and demand redundancy (G2-007, G2-008). Cross-domain integration—motion, energy, and ADAS sharing actuators—is where hidden interaction bugs surface (G2-032). This chapter maps the domains as the application layer atop the electronics platform of Chapter 2 and the silicon layer of Chapter 4.

---

### 3.1 Introduction

A vehicle is a collection of interacting control systems. Historically, each domain had its own ECU(s) and was developed by a specialised team or supplier. Modern consolidation (domain controllers, zonal, centralised HPC) forces these domains onto shared compute and networks, exposing interaction bugs that previously stayed isolated. Understanding domain-specific challenges is necessary to understand why integration is hard and why "software-defined vehicle" is not just a software problem but a domain-engineering problem.

This chapter groups domains into: (A) propulsion and energy, (B) chassis and motion, (C) ADAS and automated driving, (D) body, comfort, and access, (E) cockpit, IVI, and displays, (F) connectivity, gateway, and compute, (G) diagnostics, security, and safety, (H) commercial and special vehicles.

### 3.2 Domain × ASIL × ECU Reference

| Domain | Typical ECU | Typical ASIL | Network | Notes |
|---|---|---|---|---|
| Powertrain / Engine | MCU ECU (Classic) | ASIL C/D | CAN/CAN FD | Real-time control, emissions, calibration |
| Transmission / Drivetrain | MCU ECU (Classic) | ASIL C/D | CAN/CAN FD | Shift quality, torque handover |
| EV Traction & Inverter | MCU ECU (Classic) | ASIL C/D | CAN/CAN FD | High-voltage safety, torque control |
| BMS | MCU ECU (Classic) | ASIL C/D | CAN/CAN FD | SoC/SoH, thermal runaway prevention |
| Charging / Power Electronics | MCU ECU (Classic) | ASIL B/C | CAN/Ethernet | Protocol interoperability |
| Thermal Management | MCU ECU / Zonal | ASIL B/C | CAN/Ethernet | Coupled multi-domain control |
| Chassis / VDC | MCU ECU (Classic) | ASIL D | CAN/FlexRay/Ethernet | Stability, torque vectoring |
| Braking (ESC/BBW) | MCU ECU (Classic) | ASIL D | CAN/FlexRay/Ethernet | Redundancy, fail-safe |
| Steering (EPS/SBW) | MCU ECU (Classic) | ASIL D | CAN/FlexRay/Ethernet | Redundancy, fail-operational |
| Suspension (Active) | MCU ECU (Classic) | ASIL B/C | CAN/Ethernet | Comfort vs. handling |
| Vehicle Motion Control | Domain ctrl / HPC | ASIL D | Ethernet | Multi-actuator coordination |
| ADAS Perception | Domain ctrl / HPC partition | ASIL B–D | Automotive Ethernet, LVDS/GMSL | ML lifecycle, ODD, monitors |
| Autonomous Driving | HPC (partition) | ASIL D | Automotive Ethernet, DDS | Safety case (SOTIF), ML certification |
| Body / BCM | MCU ECU / Zonal | ASIL A/B (mostly QM) | LIN, CAN, CAN FD | I/O, diagnostics, low cost |
| Vehicle Access & Comfort | MCU ECU / Zonal | Partly | CAN/LIN | Security, UX |
| Vehicle Energy Management | MCU ECU / Zonal | Partly | CAN/Ethernet | Power states, wake-up |
| Lighting | MCU ECU / Zonal | Partly | LIN/CAN | ADB, diagnostics |
| Cockpit / IVI | HPC / Head unit | QM (ASIL A/B tell-tales) | Automotive Ethernet | UX, app ecosystem, cybersecurity, OTA |
| Instrument Cluster | MCU/SoC ECU | ASIL A/B (tell-tales) | CAN/Ethernet | Graphics, timing |
| Telematics / Connectivity | Application processor | QM (security) | Cellular, Ethernet | Connectivity, security, data privacy |
| Gateway | MCU/SoC ECU | Cybersecurity (QM/ASIL B) | All (multi-bus) | Routing, firewall, diagnostics |
| HPC / Central Compute | HPC (SoC) | Mixed (ASIL B–D + QM) | Ethernet backbone | Mixed-criticality, virtualisation, thermal |
| Zonal Controller | Zonal ECU | ASIL B–D | Ethernet backbone + CAN/LIN edge | Mixed-criticality, wiring reduction |
| Diagnostics (UDS) | All ECUs | QM | CAN (ISO-TP), Ethernet (DoIP) | UDS, OBD, flashing |
| Vehicle Security | All ECUs | Cybersecurity | All | ISO/SAE 21434, UNECE R155 |
| OTA / Software Update | All ECUs + TCU | Safety + security | Cellular, Ethernet | Safe-state activation, rollback |
| Commercial Vehicles / Off-Highway | Various | Varies | CAN (J1939), Ethernet | Harsh environment, long lifecycle |
| Two/Three-Wheeler | Low-cost MCU | Varies | CAN/LIN | Cost constraints, ABS mandates |

### 3.3 Propulsion and Energy Domains

**Powertrain / Engine Management (G2-004):** Engine control software (fuel injection, ignition, idle, VVT, boost, aftertreatment) requires massive calibration datasets (maps) tuned across engine variants, fuels, climates, and emissions regulations (Euro 7, EPA Tier 3, China 6). Calibration is largely manual, iterative, and vehicle-dependent. Hybridisation adds coordination between ICE and electric drive. Timing analysis (W1-010) and emissions compliance dominate. Root cause: physical complexity of combustion; lack of first-principles models accurate enough to replace empirical calibration.

**Transmission / Drivetrain (G2-005):** Shift-quality tuning is subjective and empirical; hybrid/e-drive transitions add coordination failure modes (torque handover, clutch thermal limits, e-drive torque ripple). Metrics for subjective quality are incomplete.

**EV Traction & Inverter (G2-010):** High-speed motor control (PWM, field-oriented control) demands deterministic real-time; ASIL-C/D torque; integration with regen braking blending and vehicle stability. Real-time compute, sensor resolution, and motor thermal model accuracy are limiting.

**BMS (G2-011):** SoC/SoH/SoP estimation across cell chemistries, temperatures, ageing; active/passive balancing across hundreds/thousands of cells; safety protection (over/under voltage, temp, current, ground fault). Cell voltage is a poor SoC indicator for some chemistries (e.g., LFP); ageing models inaccurate; thermal gradients; max 255 nodes in modular architecture; internal communication voltage-shift challenges. Estimation errors affect range, performance, and safety (thermal runaway). AI applicability medium–high (data-driven SoH, anomaly detection) but safety sign-off remains human.

**Charging Systems (G2-012):** Multiple standards (CCS, CHAdeMO, GB/T, NACS, wireless), charger negotiation, payment/roaming, grid interaction (V2G/V2H), thermal/power limits. Interoperability across charger vendors and vehicles is poor; field charging failures common. ISO 15118 (Plug & Charge), OCPP, conformance testing exist but adoption slow.

**Thermal Management (G2-013):** Coordinates battery, motor, inverter, cabin HVAC, and cooling/heating across conflicting demands (range vs. comfort vs. performance vs. battery life). Coupled and nonlinear; largely heuristic; optimisation not global.

**Vehicle Energy Management (G2-025):** EV range optimisation and energy prediction across driving conditions, climate, terrain, user behaviour; integration with navigation, thermal, BMS.

### 3.4 Chassis and Motion Domains

**Chassis & Vehicle Dynamics (G2-006):** ESC/ABS/TCS fuse wheel-speed, yaw, lateral acceleration, steering angle, brake pressure sensors to control individual wheel brakes and torque. Integration of multiple chassis functions on shared sensors/actuators creates interaction conflicts; calibration across surfaces/conditions extensive; validation requires diverse road surfaces and weather. Tire model accuracy and low-mu conditions are limiting.

**Braking / Brake-by-Wire (G2-007):** Removing the mechanical/hydraulic fallback requires redundant sensing, actuation, and power; software must guarantee braking under all fault conditions while preserving pedal feel (ASIL-D). Validation of all degraded modes is impractical; feel tuning empirical.

**Steering / Steer-by-Wire (G2-008):** Removes mechanical steering column; redundant motors/sensors/power; synthesized steering feel; safety case for loss of steering assistance severe. Cost, driver acceptance, validation burden.

**Suspension / Active Suspension (G2-009):** Balance comfort and handling in real time; road-preview sensors (camera/radar); subjective comfort metrics; tuning empirical; interaction with ADAS (camera for road preview) couples domains.

**Vehicle Motion Control (G2-014):** Coordinates brakes, steering, powertrain, suspension for stability, agility, comfort (torque vectoring, chassis control). Multiple controllers compete for shared actuators; integration creates conflicts; supplier boundaries (brake vs. steering vs. powertrain suppliers) impede unified control.

### 3.5 ADAS and Automated Driving Domains

**ADAS Perception (G2-001):** Fuses heterogeneous sensors (camera, radar, lidar, ultrasonic) into a consistent world model; manages ML model training/data/ODD; validates against an effectively unbounded scenario space. ML components are data-driven and opaque; requirements on data, ODD, and monitors are not handled by classic RE (W1-004). ALM tools do not trace models/datasets/ODDs; simulation platforms (CARLA, dSPACE, IPG) have fidelity limits.

**ADAS Real-Time Scheduling (G2-002):** ADAS are real-time systems reacting to multiple inputs; incorrect assignment of preemptive priorities can cause more harm than good (Wikipedia ADAS). Timing predictability on consolidated compute unsolved (W1-010).

**ADAS Scenario Validation (G2-031):** Validation requires scenario coverage across ODD, weather, lighting, road conditions; real-world testing expensive and slow; simulation fidelity limited; edge cases rare but critical; regulatory approval (UNECE R157 for ALKS) demands evidence. Coverage metrics immature; simulation not accepted as sole evidence.

**Autonomous Driving:** L3+ automated driving, planning, control; ODD, safety case (SOTIF), ML certification; ISO/PAS 8800 for AI safety. Dominant on Linux/ROS2 (Table 2.3). Validation scalability crisis.

### 3.6 Body, Comfort, and Access Domains

**Body Electronics / BCM (G2-003):** Generic ECU controlling power windows, mirrors, HVAC, immobiliser, central locking, lighting, etc., over CAN. Feature creep turns BCM into a large, poorly modularised feature set with many I/O and timing constraints; central body controllers consolidate more functions, increasing single-point-of-failure risk. Feature changes late in cycle; no body-domain architecture authority. AUTOSAR overhead heavy for simple body features.

**Vehicle Access & Comfort (G2-026):** Keyless entry, passive entry, seat/steering memory, ambient lighting, comfort features involve RF, UWB, BLE, actuator control. Relay attacks on keyless entry are a known security issue; UWB adoption cost; legacy systems vulnerable.

**Vehicle Energy Management (G2-025, part of 3.3):** power states, wake-up, diagnostics.

**Lighting Systems:** Headlamps, ambient, matrix lighting; ADB control, diagnostics.

### 3.7 Cockpit, IVI, and Displays

**Cockpit / Digital Cockpit (G2-015):** Integrates instrument cluster, IVI, HUD, multiple displays with safety-critical tell-tales and non-safety entertainment on shared SoC/GPU. Mixed criticality on shared graphics; driver distraction; HMI consistency across brands; boot time and responsiveness. Hypervisor overhead; safety certification of graphics stacks.

**IVI (G2-016):** Spans multiple OSes (Linux, QNX, Android Automotive, AGL) and app ecosystems (Android Auto, CarPlay, OEM apps). Fragmentation across OEMs and tiers; app certification; OTA updates; cybersecurity (remote attack surface); driver distraction. Long vehicle lifecycle vs. fast consumer tech.

**Instrument Clusters / Displays (G2-017):** Must render safety-critical tell-tales (MIL, ABS, airbag) reliably and within regulatory timing; boot fast; never fail to display critical warnings. Validation of all tell-tale states and failure modes. Safety-certified graphics stacks; certification cost.

### 3.8 Connectivity, Gateway, and Compute Domains

**Telematics & Connectivity (G2-018):** TCU manages cellular connectivity, GPS, OTA updates, remote diagnostics, eCall, connected services. Intermittent connectivity; OTA reliability; data privacy; cybersecurity. Regional cellular standards; carrier fragmentation; eCall mandate (EU). OTA platforms exist but failures in field occur.

**Vehicle Gateway (G2-019):** Consolidation of domain functions onto central compute via hypervisors/virtualisation creates mixed-criticality integration, resource contention, thermal/power challenges, supplier ecosystem disruption. "Thinking vs. acting" separation; single point of failure risk.

**HPC / Central Compute (G2-022):** Hosts ADAS, IVI, cockpit, connectivity with mixed criticality on shared SoC/GPU/NPU. Resource allocation, scheduling, thermal management unsolved; real-time guarantees on non-RT accelerators; software portability across SoC generations.

**Zonal Architecture (G2-021):** Migration from domain-based to zonal requires re-partitioning software across zonal ECUs and central compute, redefining interfaces, re-validating. Legacy software not designed for zonal deployment; migration cost and risk high. SOA portability limited; standards evolving.

**Vehicle Networking (G2-020):** Integrates CAN, CAN FD, LIN, FlexRay, Automotive Ethernet through gateways. Bandwidth, latency, timing constraints across heterogeneous networks; gateway design is a bottleneck. Timing analysis unsolved; gateway bottlenecks; cost of Ethernet migration.

### 3.9 Diagnostics, Security, and Safety Domains

**Vehicle Diagnostics (G2-023):** OBD-II/EOBD standardise connector and basic PIDs, but manufacturers add proprietary DTCs and PIDs; diagnostic coverage for non-emissions systems (chassis, body, ADAS) inconsistent; aftermarket tools cannot access all systems; security (UDS security access) gates diagnostics. ODX management; proprietary extensions.

**Vehicle Security (G2-024):** Connected vehicles expose large attack surface (TCU, IVI, OBD, V2X, OTA). Cybersecurity must be engineered across lifecycle (ISO/SAE 21434, UNECE R155); threat analysis, secure coding, penetration testing, incident response immature; legacy ECUs lack security. Compliance burden; skills gap; evolving threats.

**Safety-Critical Embedded Systems (G2-027):** Safety-critical software (ASIL-B/C/D) must demonstrate freedom from interference when integrated with non-safety software on shared compute; mixed-criticality scheduling; safety OS and hypervisor certification; tool qualification. Certification cost; interference analysis immaturity.

**OTA / Software Update (W2-010, W4-016, G2-018):** Campaign management, background install, rollback, safe-state activation, UNECE R156 compliance.

### 3.10 Commercial and Special Vehicle Domains

**Commercial Vehicles & Off-Highway (G2-029):** Trucks, buses, construction, agriculture operate in harsh environments, long lifecycles (10–20 years), diverse duty cycles, many variants (PTO, body-builder interfaces). Software must support body-builder integration, telematics for fleet, durability; aftermarket modifications common. Long-term maintenance cost over 20-year lifecycle poorly quantified.

**Aerospace-Inspired Automotive (G2-028):** Aerospace practices (DO-178C, ARINC 653, IMA) inspire centralised computing, but certification regimes differ (ISO 26262 vs DO-178C); reuse limited by cost, certification, operating environment. Cost/benefit unclear.

**Two/Three-Wheeler Electronics (G2-030):** Severe cost constraints, limited compute, growing feature demand (ABS, connectivity, EV conversion). ABS mandatory in many markets; EV two-wheelers need BMS and charging at minimal cost. Cost of ABS compliance for low-end two-wheelers; EV two-wheeler BMS field issues.

### 3.11 Cross-Domain Integration: Where Defects Surface

**Cross-Domain Integration (G2-032):** Motion control, energy management, and ADAS share actuators (brakes, steering, powertrain); integration reveals conflicts (e.g., ADAS emergency braking vs. energy-regen blending vs. ESC). Integration is where hidden interaction bugs surface; testing all combinations is impractical. Root cause: domain silos, shared actuators, incomplete interface specs, combinatorial explosion of scenarios. This is arguably the single most important unsolved integration problem in modern vehicle engineering.

### 3.12 Cross-Cutting Observations (Vehicle Domains)

1. ADAS/AV validation is the dominant cost driver in modern vehicle programs; scenario coverage is unbounded and simulation fidelity limited (G2).
2. EV/BMS software is safety-critical and long-lived: estimation accuracy affects range and safety; field ageing data is slow to accumulate.
3. Mixed criticality on shared compute (cockpit, HPC, zonal) creates new failure modes and certification challenges across all domains.
4. Cross-domain integration is where defects surface: motion, energy, and ADAS share actuators; integration testing is combinatorially expensive.
5. Variant explosion affects every domain (powertrain calibrations, regional features, trim levels); configuration management is weak (W1-011, W2-011).
6. Supplier/OEM boundaries split domain software ownership; interface specifications are incomplete and drift (W1-006, W1-007, W5-014).
7. Legacy-to-SDV transition forces re-architecture of proven domain software; migration cost and risk are high (W2-016, G2-021).
8. Field feedback loops are slow: BMS ageing, ADAS edge cases, and connectivity issues take months/years to surface.

### 3.13 Summary and Bridge to Chapter 4

Vehicle domains are the application layer of automotive software. Each domain carries its own physics, safety criticality, calibration burden, and supplier ecosystem. The problems are not merely software problems; they are domain-engineering problems that software must accommodate. Consolidation onto shared compute (zonal/centralised) forces these domains to interact in ways they never did before, and the industry's tools, standards, and organisations are only partially prepared for that (W1-016, W2-009, G2-019, G2-021, G2-032).

Chapter 4 examines the **semiconductor-to-vehicle software ecosystem**—how silicon, hardware, and the long supply chain beneath the ECUs propagate constraints and problems upward into software architecture, development, integration, and testing (G1).

### Problem IDs referenced in this chapter

G2-001, G2-002, G2-003, G2-004, G2-005, G2-006, G2-007, G2-008, G2-009, G2-010, G2-011, G2-012, G2-013, G2-014, G2-015, G2-016, G2-017, G2-018, G2-019, G2-020, G2-021, G2-022, G2-023, G2-024, G2-025, G2-026, G2-027, G2-028, G2-029, G2-030, G2-031, G2-032; W1-004, W1-006, W1-007, W1-010, W1-011, W1-016; W2-009, W2-010, W2-011, W2-016; W4-016, W4-017; W5-014.
---

*End of Chapter 3.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

