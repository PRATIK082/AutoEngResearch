# Automotive Industry Domain Map

**Artefact:** Complete Automotive Industry Domain Map (Research Artefact #1)  
**Parent project:** THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING  
**Date compiled:** 2026-11-05  
**Scope:** Global automotive software industry — ICE, hybrid, EV, ADAS/ADS; Classic + Adaptive AUTOSAR; OEM/Tier-1/Tier-2 perspectives.  
**Evidence basis:** Task.md (research master prompt) + workstreams W1–W5 problem reports.

---

## 0. How to read this map

This document is the **top-level domain map** of the automotive software industry. It enumerates:

1. **Vehicle domains / product domains** — the functional areas of a vehicle that carry software.
2. **ECU types** — the compute nodes that host that software.
3. **Network types** — the communication fabrics connecting ECUs.
4. **Standards & regulations** — the normative framework governing development.
5. **Cross-cutting dependency map** — how domains, ECUs, networks and standards interact (including the distributed → zonal → centralised evolution).

It is intended as a shared reference (common terminology) for the rest of the research book and for downstream artefacts (lifecycle map, semiconductor-to-software map, toolchain map, etc.).

---

## 1. Vehicle Domains (Product / Functional Domains)

Vehicle software is organised by **functional domain**. Historically each domain had its own ECUs; modern architectures consolidate these.

### 1.1 Core vehicle domains

| # | Domain | Abbreviation | Primary function | Safety-critical? | Key software concerns |
|---|--------|--------------|------------------|------------------|-----------------------|
| 1 | **Powertrain / Engine Management** | PT / EMS | Engine control, fuel/air, ignition, emissions | Yes (ASIL C/D) | Real-time control, emissions compliance, calibration |
| 2 | **Transmission / Drivetrain** | TCM | Gear shifting, torque converter, clutch | Yes (ASIL C/D) | Shift quality, timing, thermal |
| 3 | **Electric Vehicle / Traction** | EV / EM | Motor control, inverter, DC/DC | Yes (ASIL C/D) | High voltage safety, torque control, efficiency |
| 4 | **Battery Management System** | BMS | Cell monitoring, SOC/SOH, thermal, balancing | Yes (ASIL C/D) | Safety, longevity, thermal runaway prevention |
| 5 | **Charging Systems & Power Electronics** | OBC / PE | On-board charger, DC fast charge, power conversion | Yes (ASIL B/C) | Protocol (CCS/CHAdeMO), grid interaction, safety |
| 6 | **Thermal Management** | TM | Coolant loops, HVAC, battery/ motor cooling | Yes (ASIL B/C) | Energy efficiency, comfort, safety |
| 7 | **Chassis & Vehicle Dynamics** | VDC | Stability control, traction, torque vectoring | Yes (ASIL D) | Determinism, sensor fusion, actuator control |
| 8 | **Braking Systems / Brake-by-Wire** | ESC / BBW | Service brake, regenerative blending, AEB | Yes (ASIL D) | Redundancy, fail-safe, timing |
| 9 | **Steering Systems / Steer-by-Wire** | EPS / SBW | Power steering, steering feel, lane keeping | Yes (ASIL D) | Redundancy, fail-operational, haptics |
| 10 | **Suspension & Active Suspension** | CDC / AS | Damping control, ride height, roll control | Yes (ASIL B/C) | Sensor fusion, real-time control |
| 11 | **Vehicle Motion Control** | VMC | Coordinated control of propulsion/braking/steering | Yes (ASIL D) | Multi-actuator coordination, timing, safety |

### 1.2 ADAS / Automated Driving domains

| # | Domain | Abbreviation | Primary function | Safety-critical? | Key software concerns |
|---|--------|--------------|------------------|------------------|-----------------------|
| 12 | **ADAS — Advanced Driver Assistance** | ADAS | ACC, LKA, BSD, TSR, PA, AEB, DMS | Yes (ASIL B–D) | Sensor fusion, perception, scenario coverage |
| 13 | **Autonomous Driving** | AD / ADS | L3+ automated driving, planning, control | Yes (ASIL D) | ODD, safety case (SOTIF), ML certification |
| 14 | **Perception (vision/radar/lidar)** | PER | Object detection, classification, tracking | Yes (ASIL B–D) | ML lifecycle, data, ODD, monitors |
| 15 | **Sensor Fusion & Localisation** | SF/LOC | Multi-sensor fusion, HD maps, positioning | Yes | Timing, accuracy, redundancy |

### 1.3 Body, comfort and access domains

| # | Domain | Abbreviation | Primary function | Safety-critical? | Key software concerns |
|---|--------|--------------|------------------|------------------|-----------------------|
| 16 | **Body Electronics / BCM** | BCM | Lighting, locks, windows, wipers, seats | Partly (ASIL A/B) | I/O, diagnostics, low cost |
| 17 | **Vehicle Access & Comfort** | VAC | Keyless entry, immobiliser, comfort features | Partly | Security, UX |
| 18 | **Vehicle Energy Management** | VEM | Power distribution, load shedding, sleep modes | Partly | Power states, wake-up, diagnostics |
| 19 | **Lighting Systems** | — | Headlamps, ambient, matrix lighting | Partly | ADB control, diagnostics |

### 1.4 Cockpit, IVI and displays

| # | Domain | Abbreviation | Primary function | Safety-critical? | Key software concerns |
|---|--------|--------------|------------------|------------------|-----------------------|
| 20 | **IVI — In-Vehicle Infotainment** | IVI / HMI | Media, navigation, apps, voice | QM (mostly) | UX, app ecosystem, cybersecurity, OTA |
| 21 | **Digital Cockpit / Instrument Cluster** | ICS / CLU | Driver displays, HUD, tell-tales | Partly (ASIL A/B for safety tell-tales) | Graphics, timing, mixed-criticality |
| 22 | **Rear-Seat Entertainment** | RSE | Passenger displays | QM | Content, connectivity |

### 1.5 Connectivity, gateway and compute

| # | Domain | Abbreviation | Primary function | Safety-critical? | Key software concerns |
|---|--------|--------------|------------------|------------------|-----------------------|
| 23 | **Telematics & Connectivity** | TCU | Cellular, V2X, cloud, OTA delivery | Partly (cybersecurity) | Connectivity, security, data privacy |
| 24 | **Vehicle Gateway** | GW | Network routing, protocol translation, security | Yes (cybersecurity) | Routing, firewall, diagnostics, timing |
| 25 | **Central / High-Performance Computing** | HPC / CDC | Central compute, AI inference, virtualisation | Mixed (ASIL B–D + QM) | Mixed-criticality, virtualisation, thermal, power |
| 26 | **Zonal Controllers** | ZCU | Zone-level I/O aggregation, local compute, networking | Yes (ASIL B–D) | Mixed-criticality, wiring reduction, diagnostics |

### 1.6 Diagnostics, security and safety

| # | Domain | Abbreviation | Primary function | Safety-critical? | Key software concerns |
|---|--------|--------------|------------------|------------------|-----------------------|
| 27 | **Vehicle Diagnostics** | UDS / OBD | Fault detection, DTCs, reprogramming, EOL | Partly | UDS (ISO 14229), OBD-II, flashing |
| 28 | **Vehicle Security** | SEC | Secure boot, crypto, HSM, intrusion detection | Yes (cybersecurity) | ISO/SAE 21434, UNECE R155 |
| 29 | **Functional Safety** | FS | Hazard analysis, safety case, monitoring | Yes (ISO 26262) | ASIL, FFI, safety case |
| 30 | **OTA / Software Update** | FOTA / OTA | Campaign management, background install, rollback | Yes (safety + security) | Safe-state activation, rollback, UNECE R156 |

### 1.7 Commercial / special vehicle domains

| # | Domain | Notes |
|---|--------|-------|
| 31 | **Commercial Vehicles & Off-Highway** | Trucks, buses, agricultural, construction — often higher durability, longer lifecycles, different regulatory (Euro VI, EPA) |
| 32 | **Two-Wheeler / Three-Wheeler Electronics** | Cost-sensitive, smaller ECUs, growing electrification |
| 33 | **Aerospace-inspired automotive** | Redundancy, fail-operational concepts borrowed from aerospace (e.g., steer/brake-by-wire) |

### 1.8 Domain → lifecycle / problem mapping (cross-reference)

- **ADAS/AD** ↔ perception/ML requirements (W1-004), scenario coverage (W3-018), SOTIF (W4-017), AI safety (W5-019).
- **Powertrain/EV/BMS** ↔ real-time control (W1-010), safety ASIL-D on Classic (W2-018), HIL fidelity (W3-012).
- **Body/Comfort** ↔ BCM cost pressure, Classic AUTOSAR (W2), variant explosion (W1-011).
- **Cockpit/IVI** ↔ Adaptive/QNX/Linux, OTA infotainment, cybersecurity.
- **Gateway/Connectivity** ↔ SOME/IP/DDS (W2-007/008), security (W4-014/015).
- **HPC/Zonal** ↔ mixed-criticality (W1-016), hypervisors (W2-009), FFI (W2-004).

---

## 2. ECU Types (Compute Nodes)

ECUs evolved from many small single-function controllers to fewer, more powerful domain/zonal/central computers.

### 2.1 ECU taxonomy

| ECU type | Typical compute | OS / platform | ASIL capability | Typical domains | Trend |
|----------|-----------------|---------------|-----------------|-----------------|-------|
| **Microcontroller ECU (classic small ECU)** | 8/16/32-bit MCU, single-core | OSEK/AUTOSAR Classic OS | Up to ASIL-D | Body, powertrain, braking, steering, sensors | Retained for safety-critical & cost-sensitive |
| **Domain Controller** | 32-bit MCU / SoC, multi-core | AUTOSAR Classic or Adaptive | ASIL B–D | ADAS, powertrain, chassis, body domain | Consolidation target |
| **Zonal ECU / Zonal Controller** | MCU/SoC, multi-core | AUTOSAR Classic/Adaptive + RTOS | ASIL B–D | Zone I/O aggregation, local compute, networking | Emerging (zonal architecture) |
| **High-Performance Computer (HPC) / Central Compute** | Multi-core SoC (CPU/GPU/NPU), GB RAM | QNX / Linux / Adaptive AUTOSAR + hypervisor | Mixed (ASIL B–D + QM partitions) | ADAS/AD, cockpit, central services, AI inference | Emerging (SDV/centralised) |
| **Gateway ECU** | MCU/SoC | AUTOSAR Classic/Adaptive + security stack | Cybersecurity (QM/ASIL) | Network routing, firewall, diagnostics | Retained/evolved |
| **Telematics Control Unit (TCU)** | Application processor + modem | Linux/Android/QNX | QM (security) | Connectivity, OTA, cloud | Evolving |
| **Infotainment Head Unit** | Application processor | Android Automotive / QNX / Linux | QM | IVI, apps, media | Evolving |
| **Instrument Cluster ECU** | MCU or SoC | AUTOSAR / RTOS | ASIL A/B (safety tell-tales) | Displays, tell-tales | Evolving |
| **Sensor ECU (smart sensor)** | MCU/DSP/NPU | RTOS / bare-metal | ASIL B–D | Camera, radar, lidar, ultrasonic | Growing (edge AI) |
| **Actuator ECU** | MCU | AUTOSAR Classic | ASIL C/D | Brake, steering, throttle actuators | Retained |

### 2.2 ECU evolution: distributed → domain → zonal → centralised

```
Generation 1: Distributed (many small ECUs, one function each)
   80-100+ ECUs in high-end cars (Broy/TUM)
        │
        ▼
Generation 2: Domain controllers (consolidate by function domain)
   ADAS domain ctrl, powertrain domain ctrl, body domain ctrl, ...
        │
        ▼
Generation 3: Zonal (consolidate by physical zone; reduce wiring)
   Front zone, rear zone, left/right zone controllers + central HPC
        │
        ▼
Generation 4: Centralised (few HPCs + zonal I/O; SDV)
   1-3 HPCs running virtualised mixed-criticality workloads
```

**Drivers:** wiring reduction, cost, SDV (software-defined vehicle), OTA, centralised AI.  
**Challenges introduced:** mixed-criticality integration, timing predictability, redundancy/fail-operational, thermal/power, hypervisor certification, single point of failure (W1-016, W2-009).

### 2.3 ECU × domain × ASIL matrix (representative)

| Domain | Typical ECU | Typical ASIL |
|--------|-------------|--------------|
| Braking (service + AEB) | MCU ECU / domain ctrl | ASIL D |
| Steering (EPS/SBW) | MCU ECU / domain ctrl | ASIL D |
| Powertrain (engine/transmission) | MCU ECU / domain ctrl | ASIL C/D |
| EV traction & BMS | MCU ECU / domain ctrl | ASIL C/D |
| Chassis / VDC | MCU ECU / domain ctrl | ASIL D |
| ADAS perception | Domain ctrl / HPC (partition) | ASIL B–D |
| Autonomous driving | HPC (partition) | ASIL D |
| Body / comfort | MCU ECU / zonal | ASIL A/B (mostly QM) |
| Cockpit / IVI | HPC / head unit | QM (ASIL A/B for safety tell-tales) |
| Gateway | MCU/SoC ECU | Cybersecurity (QM/ASIL B) |
| Telematics | Application processor | QM (security) |
| Diagnostics | All ECUs | QM |

---

## 3. Network Types (Communication Fabrics)

Automotive networks are heterogeneous and layered: legacy fieldbuses coexist with Ethernet backbones.

### 3.1 In-vehicle network technologies

| Network | Speed | Topology | Typical use | Status |
|---------|-------|----------|-------------|--------|
| **LIN (Local Interconnect Network)** | 1–20 kbps | Single-wire master/slave | Body, sensors, actuators, mirrors, seats | Legacy, still widely used (cost) |
| **CAN (Controller Area Network)** | 125 kbps – 1 Mbps (classic) | Multi-master bus | Powertrain, chassis, body, diagnostics | Legacy, universal |
| **CAN FD (Flexible Data-rate)** | Up to 8 Mbps (data phase) | Multi-master bus | Higher bandwidth needs, zonal, diagnostics | Growing, transitional |
| **FlexRay** | 10 Mbps | Time-triggered dual channel | Chassis (x-by-wire), safety-critical, deterministic | Niche, declining (replaced by Ethernet/TSN) |
| **Automotive Ethernet** | 100 Mbps – 10 Gbps | Switched (point-to-point/star) | Backbone, ADAS sensors, cameras, zonal, HPC | Growing rapidly |
| **TSN (Time-Sensitive Networking)** | Over Ethernet | Switched with time sync | Deterministic Ethernet, safety-critical over Ethernet | Emerging |
| **MOST (Media Oriented Systems Transport)** | 25–150 Mbps | Ring | Infotainment (audio/video) | Legacy, declining (replaced by Ethernet) |
| **LVDS / GMSL / FPD-Link** | Gbps | Point-to-point | Camera/serializer links to ADAS | Growing |
| **PCIe / CXL (in-SoC/inter-chip)** | Gbps | Interconnect | HPC internal, accelerator interconnect | Emerging |

### 3.2 Middleware / service layers over networks

| Middleware | Transport | Paradigm | Typical use | Concerns |
|------------|-----------|----------|-------------|----------|
| **SOME/IP** | Automotive Ethernet (UDP/TCP) | Service-oriented (RPC + pub/sub) | Adaptive AUTOSAR, ADAS, zonal, cockpit | Service discovery latency, jitter, QoS (W2-007) |
| **DDS (Data Distribution Service)** | Ethernet / various | Data-centric pub/sub with rich QoS | ADAS/AD perception pipelines, ROS2, autonomous | Real-time determinism, threading, memory footprint (W2-008) |
| **AUTOSAR COM / PDU Router** | CAN/LIN/FlexRay/Ethernet | Signal-based PDU routing | Classic AUTOSAR ECU communication | Static config, gateway translation (W2-002) |
| **SOME/IP-SD** | Ethernet | Service discovery | Adaptive service discovery | Subscription latency, multicast drops (W2-007) |
| **DoIP (Diagnostics over IP)** | Ethernet | Diagnostics transport | UDS over Ethernet | Security, firewall |
| **XCP (Universal Measurement & Calibration Protocol)** | CAN/Ethernet | Measurement & calibration | ECU calibration, CANape | Security, access |

### 3.3 Network evolution

```
Legacy (bus-based):
   LIN (body) + CAN (powertrain/chassis/body) + FlexRay (chassis x-by-wire) + MOST (infotainment)
        │
        ▼
Transitional (mixed):
   CAN/CAN FD + Automotive Ethernet (ADAS backbone) + gateways translating signal↔service
        │
        ▼
Modern (zonal/centralised):
   Automotive Ethernet backbone (100 Mbps–10 Gbps) + TSN for determinism
   + SOME/IP/DDS middleware
   + CAN FD/LIN retained for edge sensors/actuators
   + Gateways at zone boundaries
```

**Key tension:** signal-based static (Classic) vs service-based dynamic (Adaptive) communication models must coexist via gateways, with timing/consistency preserved (W2-002).

---

## 4. Standards, Regulations & Frameworks

### 4.1 Functional safety

| Standard | Scope | Impact |
|----------|-------|--------|
| **ISO 26262** | Functional safety for road vehicles (ASIL A–D) | Process, evidence, safety case, FFI; heavy documentation (W4-011) |
| **ISO 21448 (SOTIF)** | Safety of the intended functionality (unknown unsafe scenarios) | ADAS/AD scenario validation; completeness problem (W4-017) |
| **ISO/PAS 8800** | Safety for AI/ML in road vehicles (emerging) | ML safety lifecycle (W5-019) |
| **IEC 61508** | Functional safety (generic) | Base for ISO 26262; industrial |

### 4.2 Cybersecurity

| Standard / Regulation | Scope | Impact |
|----------------------|-------|--------|
| **ISO/SAE 21434** | Cybersecurity engineering for road vehicles | TARA, security requirements, verification, monitoring (W4-014) |
| **UNECE R155** | CSMS type approval (vehicle cybersecurity) | Supply-chain cascade, audits (W4-015) |
| **UNECE R156** | Software update management (SUMS) | OTA engineering, versioning, rollback (W4-016, W2-010) |
| **UNECE R10** | EMC | Physical validation |

### 4.3 Software update & OTA

| Standard | Scope |
|----------|-------|
| **ISO 24089** | Road vehicles — software update engineering |
| **UNECE R156** | SUMS type approval |

### 4.4 Automotive software architecture / middleware

| Standard | Scope |
|----------|-------|
| **AUTOSAR Classic Platform (CP)** | MCU/embedded, hard real-time, signal-based, ASIL-D capable (W2) |
| **AUTOSAR Adaptive Platform (AP)** | HPC/zonal, service-oriented, POSIX/C++, soft real-time (W2) |
| **AUTOSAR Foundation** | Common concepts across CP/AP |
| **AUTOSAR Methodology & Templates** | Engineering process, ARXML, configuration |

### 4.5 Process quality

| Standard | Scope |
|----------|-------|
| **Automotive SPICE (ASPICE)** | Process assessment (VDA scope, PAM) — levels 0–5 (W4-013) |
| **ISO 9001** | General QMS |
| **IATF 16949** | Automotive QMS (manufacturing) |

### 4.6 Coding standards

| Standard | Scope |
|----------|-------|
| **MISRA C:2012 / 2023** | C coding guidelines for safety (W4-008/009/010) |
| **MISRA C++:2008 / 2023** | C++ coding guidelines (merged with AUTOSAR C++14 in 2023) |
| **CERT C / CERT C++** | Secure coding standards |
| **AUTOSAR C++14** | C++ coding guidelines for AUTOSAR (merged into MISRA C++:2023) |

### 4.7 Communication & diagnostics standards

| Standard | Scope |
|----------|-------|
| **ISO 11898** | CAN (physical/data link) |
| **ISO 15765** | Diagnostics on CAN (ISO-TP) |
| **ISO 14229 (UDS)** | Unified Diagnostic Services |
| **ISO 13400** | DoIP (Diagnostics over IP) |
| **ISO 17987** | LIN |
| **ISO 17458** | FlexRay |
| **IEEE 802.3 / 802.1Q / 802.1AS** | Ethernet, VLAN, time sync |
| **ASAM standards** | MCD-2 MC (measurement & calibration), XIL (HIL interfaces), OpenSCENARIO, OpenDRIVE, OSI |
| **SAE J1939** | Heavy-duty/commercial vehicle networks |

### 4.8 Tool qualification & certification

| Framework | Scope |
|-----------|-------|
| **ISO 26262 Part 8 (tool qualification)** | TCL1–3, tool confidence levels |
| **IEC 61508 / ISO 26262 tool qualification** | Qualification of software tools used in development |
| **MISRA Compliance:2020** | Process for claiming MISRA compliance |

### 4.9 Emerging AI governance

| Framework | Scope |
|-----------|-------|
| **EU AI Act** | AI system regulation (risk-based) |
| **ISO/PAS 8800** | AI safety for road vehicles |
| **UNECE R155/R156** | (cybersecurity/update, includes software) |

### 4.10 Standards interaction map (simplified)

```
Safety:        ISO 26262 ──► SOTIF (ISO 21448) ──► AI safety (ISO/PAS 8800)
Cybersecurity: ISO/SAE 21434 ──► UNECE R155 (CSMS) ──► UNECE R156 (SUMS)
Architecture:  AUTOSAR CP / AP ──► Foundation ──► SOME/IP / DDS / ARXML
Process:       ASPICE ──► ISO 26262 (combined assessments)
Coding:        MISRA C/C++ ──► CERT ──► AUTOSAR C++
Comms:         ISO 11898 (CAN) ──► 15765 (ISO-TP) ──► 14229 (UDS) ──► 13400 (DoIP)
Update:        ISO 24089 ──► UNECE R156
```

---

## 5. Domain × ECU × Network × Standard Cross-Reference

### 5.1 Representative mapping (simplified)

| Domain | Typical ECU | Network | Key standards |
|--------|-------------|---------|---------------|
| Powertrain / Engine | MCU ECU (Classic) | CAN / CAN FD | ISO 26262 (ASIL C/D), MISRA, AUTOSAR CP |
| Transmission | MCU ECU (Classic) | CAN / CAN FD | ISO 26262, MISRA, AUTOSAR CP |
| EV Traction & Inverter | MCU ECU (Classic) | CAN / CAN FD | ISO 26262, MISRA, AUTOSAR CP |
| BMS | MCU ECU (Classic) | CAN / CAN FD | ISO 26262 (ASIL C/D), MISRA, AUTOSAR CP |
| Braking (ESC/BBW) | MCU ECU (Classic) | CAN / FlexRay / Ethernet | ISO 26262 (ASIL D), MISRA, AUTOSAR CP |
| Steering (EPS/SBW) | MCU ECU (Classic) | CAN / FlexRay / Ethernet | ISO 26262 (ASIL D), MISRA, AUTOSAR CP |
| Chassis / VDC | MCU ECU (Classic) | CAN / FlexRay | ISO 26262 (ASIL D), MISRA, AUTOSAR CP |
| ADAS Perception | Domain ctrl / HPC partition | Automotive Ethernet (100M–10G), LVDS/GMSL | ISO 26262, SOTIF, ISO/SAE 21434, AUTOSAR AP/CP, MISRA |
| Autonomous Driving | HPC (partition) | Automotive Ethernet, DDS | ISO 26262, SOTIF, ISO/PAS 8800, ISO/SAE 21434, UNECE |
| Body / BCM | MCU ECU / Zonal | LIN, CAN, CAN FD | ISO 26262 (ASIL A/B), MISRA, AUTOSAR CP |
| Cockpit / IVI | HPC / Head unit | Automotive Ethernet, MOST (legacy) | Cybersecurity, MISRA, ASPICE |
| Instrument Cluster | MCU/SoC ECU | CAN, Ethernet | ISO 26262 (ASIL A/B for tell-tales) |
| Gateway | MCU/SoC ECU | All (multi-bus) | ISO/SAE 21434, UNECE R155, AUTOSAR CP/AP |
| Telematics | Application processor | Cellular, Ethernet | UNECE R155/R156, ISO/SAE 21434 |
| Zonal Controller | Zonal ECU | Ethernet backbone + CAN/LIN edge | ISO 26262, AUTOSAR CP/AP, Cybersecurity |
| HPC / Central Compute | HPC (SoC) | Ethernet backbone | Mixed: ISO 26262, AUTOSAR AP, hypervisor, Cybersecurity |
| Diagnostics (UDS) | All ECUs | CAN (ISO-TP), Ethernet (DoIP) | ISO 14229, ISO 15765, ISO 13400 |
| OTA / Update | All ECUs + TCU | Cellular, Ethernet | ISO 24089, UNECE R156, ISO/SAE 21434 |

### 5.2 ASIL distribution across the vehicle

- **ASIL D:** braking, steering, chassis/VMC, AD (L3+), airbag, some powertrain.
- **ASIL C:** powertrain, EV traction, BMS (some), ADAS (some).
- **ASIL B:** ADAS (some), suspension, thermal, body (some), ADAS perception (some).
- **ASIL A:** body, comfort, displays (tell-tales).
- **QM:** IVI, telematics apps, RSE, most comfort features.

### 5.3 Classic vs Adaptive usage by domain

| Domain | Classic (CP) | Adaptive (AP) | Non-AUTOSAR (Linux/QNX/Android) |
|--------|--------------|---------------|----------------------------------|
| Powertrain, chassis, braking, steering | Dominant | Rare | Rare |
| BMS, EV traction | Dominant | Rare | Rare |
| Body / BCM | Dominant | Rare | Rare |
| ADAS perception & fusion | Mixed | Growing | Growing (ROS2) |
| Autonomous driving | Mixed | Growing | Dominant (Linux/ROS2) |
| Cockpit / IVI | Rare | Some | Dominant (Android/QNX) |
| Gateway | Dominant | Some | Some |
| HPC / central compute | Partitions | Growing | Dominant (Linux/QNX) |
| Telematics | Rare | Some | Dominant (Linux) |

---

## 6. Stakeholder Map (Supply Chain)

| Stakeholder | Role in software | Key concerns |
|-------------|------------------|--------------|
| **OEM (Original Equipment Manufacturer)** | Vehicle owner, system integrator, architecture authority | Time-to-market, cost, compliance, platform strategy |
| **Tier-1 supplier** | ECU/system supplier, often full software stack | Requirements completeness, tooling, integration, margins |
| **Tier-2 supplier** | Component/subsystem supplier (sensors, actuators, semiconductors) | Interface specs, volume, qualification |
| **Semiconductor manufacturer** | SoC/MCU/sensor silicon | Qualification, software enablement, supply |
| **ECU manufacturer** | Hardware manufacturing | Cost, quality, test coverage |
| **Software vendor / tool vendor** | AUTOSAR stacks, tools, middleware | Licensing, standards evolution, lock-in |
| **Engineering service provider** | Contract engineering, integration, testing | Capacity, skill availability, tool access |
| **Testing service provider** | HIL, vehicle, homologation testing | Capacity, equipment, scenarios |
| **Automotive startup / EV manufacturer** | New entrants, SDV-first | Cost (tools), talent, speed |
| **Autonomous driving company** | AD software, sensor stacks | Safety case, data, compute, talent |

---

## 7. Architecture Evolution Summary (Distributed → Zonal → Centralised)

```
┌─────────────────────────────────────────────────────────────────────┐
│  DISTRIBUTED (legacy)                                               │
│  80-100+ ECUs · CAN/LIN/FlexRay · signal-based · one function/ECU  │
└──────────────────────────────┬──────────────────────────────────────┘
                               │  consolidation pressure
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│  DOMAIN-CONTROLLER (current mainstream)                             │
│  Per-domain ECUs (ADAS, PT, chassis, body, cockpit, GW)             │
│  CAN/CAN FD + Ethernet backbone · mixed signal/service              │
└──────────────────────────────┬──────────────────────────────────────┘
                               │  wiring cost + SDV pressure
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│  ZONAL (emerging)                                                   │
│  Zone controllers (front/rear/left/right) + central HPC             │
│  Ethernet backbone (TSN) · SOME/IP/DDS · mixed-criticality         │
└──────────────────────────────┬──────────────────────────────────────┘
                               │  SDV / AI pressure
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│  CENTRALISED (SDV target)                                           │
│  1-3 HPCs (hypervisor, virtualised) + zonal I/O gateways          │
│  Service-oriented · OTA-native · AI inference · mixed ASIL          │
└─────────────────────────────────────────────────────────────────────┘
```

**Each transition introduces:** mixed-criticality integration, timing predictability challenges, redundancy/fail-operational requirements, thermal/power management, hypervisor certification uncertainty, and supplier ecosystem disruption (W1-016, W2-009, W2-016).

---

## 8. Glossary (selected automotive engineering terms)

| Term | Definition |
|------|------------|
| **ADAS** | Advanced Driver Assistance Systems |
| **ADS / AD** | Automated Driving System / Autonomous Driving |
| **ARXML** | AUTOSAR XML — the central engineering data format |
| **ASIL** | Automotive Safety Integrity Level (A–D, QM) |
| **ASPICE** | Automotive SPICE process assessment model |
| **AUTOSAR CP** | AUTOSAR Classic Platform |
| **AUTOSAR AP** | AUTOSAR Adaptive Platform |
| **BCM** | Body Control Module |
| **BMS** | Battery Management System |
| **BSW** | Basic Software (AUTOSAR) |
| **CAN / CAN FD** | Controller Area Network / Flexible Data-rate |
| **DDS** | Data Distribution Service (middleware) |
| **DoIP** | Diagnostics over IP |
| **ECU** | Electronic Control Unit |
| **FFI** | Freedom From Interference (ISO 26262) |
| **FOTA / OTA** | Firmware/Software Over-The-Air update |
| **GW** | Gateway |
| **HIL / SIL / MIL / PIL** | Hardware/Software/Model/Processor-in-the-Loop |
| **HPC** | High-Performance Computer (central compute) |
| **HSM** | Hardware Security Module |
| **IVI** | In-Vehicle Infotainment |
| **LIN** | Local Interconnect Network |
| **MCAL** | Microcontroller Abstraction Layer |
| **MBSE** | Model-Based Systems Engineering |
| **MISRA** | Motor Industry Software Reliability Association coding guidelines |
| **ODD** | Operational Design Domain |
| **OEM** | Original Equipment Manufacturer |
| **RTE** | Runtime Environment (AUTOSAR) |
| **SDV** | Software-Defined Vehicle |
| **SOME/IP** | Scalable service-Oriented MiddlewarE over IP |
| **SOTIF** | Safety Of The Intended Functionality (ISO 21448) |
| **SWC** | Software Component (AUTOSAR) |
| **TCU** | Telematics Control Unit |
| **TSN** | Time-Sensitive Networking |
| **UDS** | Unified Diagnostic Services (ISO 14229) |
| **vECU** | Virtual ECU |
| **VMC** | Vehicle Motion Control |
| **ZCU** | Zonal Control Unit |

---

## 9. Sources

- Task.md — DEEP RESEARCH MASTER PROMPT (research master prompt; domain lists §3A, §9, §10).
- W1 — Requirements Engineering and System Engineering/Architecture (vehicle domains, architecture transition, timing, variants).
- W2 — AUTOSAR Classic/Adaptive and Middleware (CP/AP, ECU types, networks, middleware, hypervisors, OTA).
- W3 — Testing and Validation (MIL/SIL/PIL/HIL/vehicle, vECU, scenario coverage).
- W4 — Engineering Tools and Standards/Compliance (toolchain, standards, regulations, HIL).
- W5 — Human Skills/Knowledge Gaps and AI/Automation (talent, AI adoption, governance).

---

*End of DOMAIN_MAP.md — Automotive Industry Domain Map (Artefact #1).*