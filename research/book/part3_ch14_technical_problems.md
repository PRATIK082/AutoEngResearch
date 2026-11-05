# Part III — Chapter 14: Technical Problems

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 14 — Technical Problems  
**Source workstreams:** G1 (Semiconductor & Hardware), G2 (Vehicle Domains), G4 (Energy & Thermal), W1, W2, W3, W4  

---

## 14.0 Chapter purpose and problem map

This chapter collects the **technical** (physics-, silicon-, control-, and code-level) problems of automotive software engineering: problems that originate in hardware limits, real-time constraints, model fidelity, signal processing, and low-level software quality. Organisational, supplier, and process problems are treated in Chapters 16, 20, and 21; testing-process problems in Chapter 17; toolchain problems in Chapter 18.

| Group | Problem IDs | Count |
|---|---|---|
| Semiconductor & hardware | G1-001 … G1-035 | 35 |
| Vehicle domains | G2-001 … G2-032 | 32 |
| Energy & thermal | G4-001 … G4-032 | 32 |
| Timing / real-time / network | W1-010, W1-018, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014, W2-017 | 9 |
| XiL / model fidelity | W3-001 … W3-016, W3-033, W3-035 | 18 |
| Code-level quality | W4-008, W4-009, W4-010, W4-021 | 4 |

---

## 14.1 Semiconductor and hardware technical constraints (G1)

Automotive software is constrained before it is written: by silicon availability, qualification, memory, and power. These constraints propagate into architecture, scheduling, and testability.

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

**Key technical observations (G1):**

1. **Obsolescence is structural, not incidental.** Vehicle service life (15+ years) exceeds semiconductor EOL (<7 years), forcing last-time buys, ECU redesign, and requalification (G1-007, G1-008, G1-009). This is a root cause of long-tail maintenance cost and of "frozen" hardware baselines that constrain software evolution.
2. **MCAL is the hidden lock-in point.** AUTOSAR standardises interfaces, but MCAL implementations are vendor-specific and tightly coupled to toolchains (G1-020, G1-021, W2-005). Changing silicon vendor typically requires re-qualifying the entire MCAL layer.
3. **Hardware/software co-design is late and siloed.** BSP fragility across revisions (G1-022, G1-031) and bring-up surprises (G1-023) push integration discovery to the end of the program.
4. **Memory and power are first-class constraints.** The memory wall (G1-017) and embedded-flash scalability (G1-018, G1-019) limit model complexity and feature growth; power-management standardisation is absent (G1-025, G1-026).
5. **Sensor/actuator interfaces are not standardised**, forcing proprietary SerDes and integration pain (G1-027, G1-028) that scales with ADAS sensor count.

---

## 14.2 Vehicle-domain technical problems (G2)

Domain-specific control software carries its own physics and timing constraints. These are the "what the vehicle actually does" problems.

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

**Key technical observations (G2):**

1. **ADAS is the dominant new technical risk surface.** Perception, sensor fusion, ML lifecycle, ODD specification (G2-001) and real-time scheduling risk (G2-002) combine with scenario-coverage uncertainty (G2-031) to make ADAS validation the largest single cost driver (see Chapter 17).
2. **Brake-by-wire and steer-by-wire** raise fault-tolerance and "feel" (haptics) problems that are safety-critical and hard to validate (G2-007, G2-008).
3. **EV systems add coupled control loops** across inverter, BMS, charging, thermal, and energy management (G2-010, G2-011, G2-012, G2-013, G2-025). Cross-domain actuator conflicts (G2-032) become more frequent as domains consolidate.
4. **Zonal/centralised migration** (G2-021) re-opens every timing and mixed-criticality assumption in the vehicle (G2-019, G2-022, G2-027).

---

## 14.3 Energy and thermal management (G4)

Energy and thermal control is a distinct technical subdomain that cuts across EV powertrain, charging, cabin, and safety.

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

**Key technical observations (G4):** SoC/SoH estimation error (G4-001, G4-002) propagates into range anxiety, charging scheduling, and safety decisions; thermal runaway detection latency (G4-004, G4-030) is safety-critical; charging interoperability (G4-015, G4-016, G4-032) is a field-reliability problem that software alone cannot fully solve because it depends on charger firmware and grid state.

---

## 14.4 Timing, concurrency, and real-time (W1-010, W1-018, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014, W2-017)

Timing analysis is a safety-relevant unsolved problem (W1 cross-cutting theme 5). Classic AUTOSAR assumes static, priority-based scheduling; Adaptive introduces dynamic memory and service discovery jitter; network timing (CAN/Ethernet/SOME/IP/DDS) is analysed with tools that do not compose.

- **W1-010** — Timing analysis and predictability unsolved for AUTOSAR: end-to-end timing budgets are computed manually and rarely validated against runtime.
- **W1-018** — Network architecture and timing of communication (bus/Ethernet): bandwidth, latency, and jitter budgets across heterogeneous buses.
- **W2-004** — Freedom from Interference on Adaptive: unsolved real-time safety; spatial/temporal isolation on Adaptive is not yet certifiable to ASIL-D.
- **W2-006** — Multicore distribution complexity in Classic: manual partitioning of SWCs across cores; timing interference between cores.
- **W2-007** — SOME/IP service discovery latency and jitter: discovery storms and nondeterministic startup.
- **W2-008** — DDS real-time determinism and threading complexity: QoS tuning is expert-dependent and fragile.
- **W2-013** — Classic OS (OSEK) scheduling limitations: static priority, no dynamic load balancing.
- **W2-014** — Adaptive dynamic memory allocation non-determinism: heap fragmentation and WCET uncertainty.
- **W2-017** — Adaptive platform diagnostics, health management, and network management maturity: monitoring gaps for timing violations.

**Root cause:** timing is treated as a property to be checked after design rather than a constraint driving design; tools (chronVAL, SymTA/S, Network Designer) do not integrate with configuration and CI in an automated loop.

---

## 14.5 Model-based and XiL technical fidelity (W3-001 … W3-016, W3-033, W3-035)

XiL loops are technically limited by model simplification, synchronization, and vECU fidelity.

- **W3-001** MIL plant/control models simplified and validated late.
- **W3-002** MIL test cases not reused for code verification.
- **W3-003** MIL/SIL model instability prevents deterministic CI.
- **W3-004** SiL cannot test all HiL functionalities; vECU fidelity gaps.
- **W3-005** Temporal synchronisation in hybrid SiL/HiL.
- **W3-006** SiL simulation inputs limited; realistic stimuli.
- **W3-007** vECU standardisation gap (FMI vs AUTOSAR).
- **W3-008** PIL limitations (unsupported modelling features).
- **W3-009** PIL reveals compiler faults but is slow/hardware-dependent.
- **W3-010** HIL cost and scalability.
- **W3-011** HIL setup latency, reconfiguration, variant explosion.
- **W3-012** HIL model fidelity (simplified plant models).
- **W3-013** HIL sensor/actuator physical constraints and latency.
- **W3-014** HIL fixture/test-bench verification difficulty.
- **W3-015** Cloud-based HIL challenges (latency, cyber, compliance).
- **W3-016** HIL test environment management (idle benches, scheduling).
- **W3-033** Fault injection manual, expertise-dependent, incomplete models.
- **W3-035** Mixed discrete-continuous (hybrid) system testing intractability.

**Technical root cause:** physical fidelity and computational cost are in tension; there is no standard credibility metric for models, and vECU interfaces are not standardised (W3-007), so simulation assets do not compose across vendors.

---

## 14.6 Code-level technical quality (W4-008, W4-009, W4-010, W4-021)

- **W4-008** Static analysis false positives and manual deviation effort (MISRA/CERT).
- **W4-009** MISRA/CERT standard version churn and migration burden.
- **W4-010** Undecidable MISRA rules require manual review.
- **W4-021** Long build/compilation times for large AUTOSAR codebases.

These are technical rather than process problems: undecidable rules (W4-010) cannot be automated by construction, and build times (W4-021) scale with ARXML-generated code volume and toolchain inefficiency.

---

## 14.7 Cross-cutting technical root causes

1. **Physics and silicon constraints are treated as late surprises** rather than first-class design inputs (G1-015, G1-023, G2 cross-cutting).
2. **Timing is analysed, not designed** (W1-010, W2-006, W2-007).
3. **Model fidelity has no standard metric or gate** (W3-001, W3-012).
4. **Interfaces (hardware, software, network) are not standardised** at the level needed for composition (G1-027, W3-007, W4-002).
5. **Obsolescence propagates for decades** (G1-007, G1-008).

---

## 14.8 Evidence limitations

- G1/G2/G4 findings rely on public sources, standards, and vendor publications; no OEM-internal data.
- Market and supply-chain statistics (shortage impact) are estimates, not audited figures.
- vECU fidelity and model-credibility claims lack quantitative benchmarks in public literature.

---

## 14.9 Cross-references

- **Chapter 15 (Architecture):** timing and mixed-criticality (W2-004, W2-009, G2-027) are architecture-level consequences of these technical limits.
- **Chapter 17 (Testing & Validation):** XiL fidelity (W3-001 … W3-016) is both a technical and a process problem.
- **Chapter 18 (Toolchain):** static analysis (W4-008 … W4-010) and build time (W4-021) are toolchain problems too.
- **Chapter 22 (Quality, Safety, Cybersecurity):** safety-relevant timing and fault injection (W3-033) feed safety validation.

---

*End of Chapter 14.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

