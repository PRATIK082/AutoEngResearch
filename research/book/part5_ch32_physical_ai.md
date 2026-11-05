# Part V — Future Automotive Engineering

## Chapter 32 — Physical AI and Intelligent Vehicles

### Summary

Physical AI is AI that acts in the physical world: perception, prediction, planning, and control running on-vehicle, in real time, under safety constraints. In automotive, this is the ADAS/AD perception and planning stack, plus emerging intelligent control (energy management, thermal, motion control). The research record shows Physical AI is where automotive AI is most advanced in capability and most constrained in certification: ML is probabilistic, ISO 26262 is deterministic, SOTIF and ISO/PAS 8800 are still maturing, scenario coverage is unbounded, and hardware (SoC/GPU/NPU) real-time guarantees are not established (G2-001, G2-031, W1-004, W5-019, W4-017, G2-022). Physical AI is therefore the highest-value, highest-risk frontier of the SDV.

---

### 32.1 What Physical AI requires in a vehicle

Physical AI in vehicles has four layers:

1. **Perception:** sensor fusion (camera, radar, lidar, ultrasonic) into a world model (G2-001).
2. **Prediction:** behaviour prediction of other road users, environment.
3. **Planning and control:** trajectory planning, motion control, actuator commands (G2-014, G2-006).
4. **Learning and adaptation:** data collection, model updates, fleet learning (G3-003, G3-012, G3-013).

Each layer has unsolved problems, and the layers interact: a perception error becomes a planning error becomes an actuator command.

---

### 32.2 Physical AI problem map

| Problem ID | Title | Physical-AI relevance |
|---|---|---|
| G2-001 | ADAS perception: sensor fusion, ML lifecycle, ODD | Core of physical AI |
| G2-002 | ADAS real-time scheduling and priority risks | Timing of perception/control |
| G2-006 | Chassis/vehicle dynamics: ESC/ABS/TCS | AI-assisted control integration |
| G2-010 | EV powertrain: inverter, torque management | AI in torque control |
| G2-014 | Vehicle motion control: actuator coordination | Shared actuators, safety case |
| G2-022 | HPC: resource allocation, scheduling, thermal | Real-time AI on shared accelerators |
| G2-024 | Vehicle security: attack surface | AI models as attack target |
| G2-031 | ADAS scenario validation and coverage gap | Unbounded scenario space |
| W1-004 | ML/AI-specific requirements and traceability | Data, ODD, monitors not in classic RE |
| W1-010 | Timing analysis unsolved | Perception-to-actuation latency |
| W4-017 | SOTIF validation doesn't scale | Unknown unsafe scenarios |
| W5-005 | AI/ML engineering capabilities gap | Few engineers bridge ML and safety |
| W5-019 | AI-generated code safety and certification gap | Probabilistic vs. deterministic |
| G3-012 | Remote diagnostics and predictive maintenance | Fleet learning and feedback |
| G3-013 | Digital twin sync and fidelity | Simulation and validation support |

---

### 32.3 Perception, sensor fusion, and ODD (G2-001, W1-004)

ADAS perception must fuse heterogeneous sensors into a consistent world model and manage ML model training, data, and ODD (G2-001). ML components are data-driven and opaque; requirements on data, ODD, and monitors are not handled by classic RE (W1-004). ODD boundaries are poorly specified; training data quality and curation are ongoing burdens; model drift over vehicle life is a field risk (G2-001).

The research record is explicit that **ODD specification is a problem, not a solved input**: how OEMs currently specify training-data requirements is an unanswered question (G2-001 unanswered). Without a well-specified ODD, safety claims cannot be bounded.

---

### 32.4 Scenario coverage and validation (G2-031, W4-017, W3-018–W3-020)

ADAS validation requires scenario coverage across ODD, weather, lighting, and road conditions; the scenario space is effectively unbounded (G2-031, W3-018). Real-world testing is expensive and slow; simulation fidelity is limited; edge cases are rare but critical (G2-031, W3-019, W3-020). SOTIF (ISO 21448) addresses unknown unsafe scenarios but does not scale to enumeration (W4-017).

AI can assist scenario generation and anomaly detection (G2-031), but coverage metrics are immature and simulation is not accepted as sole evidence (G2-031, W4-017). The research record does not establish achievable scenario coverage or regulatory acceptance of virtual validation.

---

### 32.5 Real-time execution on shared hardware (G2-022, W1-010, G2-002)

Physical AI must run in real time on shared SoC/GPU/NPU with mixed-criticality workloads. The problems:

- **Real-time guarantees on GPU/NPU** are not established (G2-022).
- **Resource allocation, scheduling, and thermal management** under combined AI/graphics/control workloads are unsolved (G2-022, G2-019).
- **Timing analysis** for perception-to-actuation chains is unsolved (W1-010, G2-002).
- **Software portability across SoC generations** is limited, locking OEMs to silicon vendors (G2-022).

The research record treats these as open engineering problems, not as solved by faster silicon alone.

---

### 32.6 Safety certification of ML (W5-019, W4-017, W1-004)

ML-based perception cannot be certified with conventional ISO 26262 evidence: ISO 26262 is deterministic/evidence-based; ML is probabilistic/data-driven (W5-019). Standards (SOTIF ISO 21448, ISO/PAS 8800) only partially address; published ML-enhancements to ISO 26262 cover only ASIL A/B (W5-019, S13). Assessors and engineers lack ML safety expertise (W5-005, W5-019).

The consequence is that ML-based features are often scoped to lower ASIL levels or restricted ODDs to remain certifiable (W5-019). Full ASIL-D ML certification is not established as feasible (W5-019 unanswered).

---

### 32.7 Cybersecurity and data (G2-024, G3-003, G3-012)

Physical AI models are attack targets (adversarial inputs, model extraction) and depend on data pipelines:

- **Attack surface** expands with connectivity and central compute (G2-024, G3-009).
- **Data volume** from perception (video, lidar) overwhelms cloud pipelines (G3-003).
- **Fleet learning** requires data collection, model updates, and validation loops that are not standardised (G3-012, G3-013).
- **Data quality (veracity)** at fleet scale is a governance problem (G3-020).

The research record does not establish secure, scalable fleet-learning pipelines for physical AI.

---

### 32.8 Evidence limitations

- Physical AI capability claims are largely vendor/OEM-reported; independent benchmarks are scarce.
- Achievable scenario coverage, real-time performance on shared accelerators, and full ASIL-D ML certification feasibility are not established.
- The long-term equilibrium between rule-based and learning-based control is not settled.

---

### 32.9 Cross-references

- **Chapter 28 (SDV):** the platform on which physical AI runs.
- **Chapter 29 (Zonal/Centralised):** the compute substrate.
- **Chapter 30 (AI-Native):** the engineering-side AI context.
- **Chapter 33 (Autonomous Workflows):** how physical AI is developed and validated.

---

### Problem IDs referenced in this chapter

W1-004, W1-010; W4-017; W5-005, W5-019; G2-001, G2-002, G2-006, G2-010, G2-014, G2-022, G2-024, G2-031; G3-003, G3-009, G3-012, G3-013, G3-020.

---

*End of Chapter 32.*