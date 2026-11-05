# Part V — Future Automotive Engineering

## Chapter 29 — Zonal and Centralised Computing

### Summary

Zonal and centralised computing move vehicle intelligence from dozens of distributed, function-specific ECUs to a small number of zone controllers plus one or more central high-performance computers (HPCs), connected by an Automotive Ethernet backbone. The motivation is wiring reduction, cost, and software updatability; the cost is concentrated risk: mixed-criticality interference, timing unpredictability, thermal and power limits, supplier-boundary disruption, and migration of legacy software that was never designed for zonal deployment (W1-016, G2-019, G2-021, G2-022). The research record treats zonal architecture as an emerging, not solved, transition: timing analysis is unsolved, hypervisor certification is costly and uncertain, and real-time guarantees on shared GPUs/NPUs are not established (W1-010, W2-004, W2-009, G2-022).

---

### 29.1 Why zonal architecture is hard

Zonal architecture is not a packaging change. It changes the fundamental assumptions of every layer:

- **Physical:** wiring harnesses shrink, but zone controllers must survive harsh environments at the vehicle corners; thermal and power delivery to HPCs become design drivers (G2-019, G2-022).
- **Software:** functions previously owned by a single ECU are now distributed across zone controllers and central compute; interfaces multiply; timing budgets must be re-derived (W1-016, W1-018, G2-020, G2-021).
- **Safety:** mixed-criticality workloads share silicon; freedom from interference must be demonstrated (W2-004, W2-009, G2-027).
- **Supply chain:** Tier-1s that owned domain ECUs must renegotiate ownership; OEMs must absorb platform software (G2-019, W5-024).
- **Migration:** legacy software is tightly coupled to domain ECUs and does not port (G2-021, W2-016).

---

### 29.2 Zonal/centralised problem map

| Problem ID | Title | Centralised-computing relevance |
|---|---|---|
| W1-010 | Timing analysis unsolved for AUTOSAR | End-to-end budgets across zone/HPC boundaries |
| W1-016 | Zonal/centralised architecture new challenges | Mixed criticality, timing, redundancy, thermal, SPOF |
| W1-018 | Network architecture and timing | Ethernet backbone, gateway latency |
| W2-004 | Freedom from Interference on Adaptive unsolved | Shared compute interference analysis |
| W2-006 | Multicore distribution complexity in Classic | Manual partitioning across cores |
| W2-009 | Hypervisor/mixed-criticality consolidation | Virtualisation overhead and certification |
| W2-013 | Classic OS (OSEK) scheduling limitations | Static scheduling vs. dynamic workloads |
| W4-019 | vECU fidelity gaps | Virtual validation of consolidated stacks |
| G2-019 | Gateway and central computing consolidation | Resource contention, thermal, supplier disruption |
| G2-020 | Vehicle networking multi-bus gateways | Bandwidth and timing across heterogeneous networks |
| G2-021 | Zonal architecture software migration | Legacy software not designed for zonal deployment |
| G2-022 | HPC resource allocation / scheduling / thermal | Real-time guarantees on shared accelerators |
| G2-027 | Safety-critical mixed criticality and FFI | Partitioning, certification, interference |
| G2-028 | Aerospace-inspired certification/reuse gap | IMA/ARINC 653 inspiration vs. automotive cost |
| W5-024 | SDV/centralised computing skill gap | Hypervisor, SOA, OTA skills scarce |

---

### 29.3 Timing and predictability

Timing is the load-bearing unsolved problem underneath zonal architecture. The research record is explicit:

- AUTOSAR Classic has **no standardised timing model** bridging system and implementation; timing analysis is largely unsolved (W1-010, W2-004).
- **Cache effects, multicore interference, and bus contention** make worst-case execution time (WCET) analysis unreliable on consolidated hardware (W2-006, W1-010).
- **SOME/IP service discovery** introduces latency and jitter that are not bounded by the standard (W2-007); **DDS** real-time determinism and threading complexity are similarly unresolved (W2-008).
- **Classic OS (OSEK)** is static and priority-based, not suited to dynamic, service-oriented workloads (W2-013).

In a zonal architecture, a timing violation in a shared HPC can propagate across domains that were previously physically isolated. The research record therefore treats timing analysis as **safety-relevant**, not merely a performance concern (W1-010, G2-002, G2-020).

---

### 29.4 Hypervisors, virtualisation, and mixed criticality

Consolidation requires a hypervisor to host ASIL-D safety functions alongside QM infotainment and ADAS on the same SoC. The problems:

- **Virtualisation overhead** and determinism are not yet at automotive-grade levels for all workloads (W2-009, G2-022).
- **Safety certification of virtualised stacks** is costly and the certification path is not well established (W2-009, G2-027, G2-028).
- **Resource allocation** (CPU, GPU, NPU, memory bandwidth) across mixed-criticality workloads with AI/ML growth is unsolved (G2-022).
- **Thermal management** of HPCs under combined AI/graphics/control workloads is a design driver, not an afterthought (G2-019, G2-022).

Aerospace-inspired partitioning (IMA, ARINC 653) is frequently cited as a model, but the certification regimes differ (ISO 26262 vs. DO-178C) and automotive cost constraints are far tighter (G2-028). Reuse is limited.

---

### 29.5 Network backbone: Ethernet, gateways, and timing

Zonal architecture depends on an Automotive Ethernet backbone. The problems:

- **Gateway design** is a bottleneck: translating between CAN/CAN FD/LIN/FlexRay and Ethernet at the required latency is hard (G2-020, W1-018).
- **Bandwidth vs. determinism trade-off**: Ethernet provides bandwidth (100 Mbps–1 Gbps) but automotive real-time guarantees require TSN, which is still being adopted (G2-020).
- **Timing constraints are not captured in AUTOSAR** (W1-018, W1-010), so network-level timing violations are discovered late.
- **Multi-bus legacy** will persist for years, so gateways remain in the architecture (G2-020).

---

### 29.6 Migration cost and legacy software

Moving from domain-based to zonal architecture requires re-partitioning software that was designed around single-ECU ownership:

- **Legacy software is tightly coupled to domain ECUs** and does not port (G2-021, W2-016).
- **Service-oriented portability is limited**: applications are still platform-specific despite SOA intentions (G2-019, G2-022).
- **Safety certification reuse** is limited; re-certification cost is high (G2-021, G2-027).
- **Supplier IP and ownership** must be renegotiated; Tier-1s lose domain control (G2-019, W5-014).

Migration cost per program and success rates are **not quantified** in the research record (G2-021 unanswered questions). This is a significant evidence gap for planning.

---

### 29.7 Single point of failure and redundancy

Centralisation concentrates risk:

- **Single point of failure (SPOF)**: an HPC failure can disable multiple domains simultaneously (W1-016, G2-019).
- **Redundancy cost and weight** for brake-by-wire and steer-by-wire are already high; centralising does not reduce them (G2-007, G2-008).
- **Fail-operational design** for central compute is immature (G2-019, G2-022).

The research record does not establish whether centralised architectures improve or worsen fleet-level reliability; this is an open question (G2-019 unanswered).

---

### 29.8 Evidence limitations

- Zonal architecture is an emerging transition; most production vehicles are hybrid (domain + some centralisation). Independent, production-scale reliability data is scarce.
- Timing-analysis gaps are reported as unsolved but their quantitative impact on programs is not measured.
- Migration cost and success rates are not quantified (G2-021).
- Hypervisor certification paths are evolving; claims of "certified" virtualised stacks should be verified against specific standards and ASIL levels.

---

### 29.9 Cross-references

- **Chapter 28 (SDV):** the broader re-platforming context.
- **Chapter 30 (AI-Native):** AI workloads on HPC.
- **Chapter 32 (Physical AI):** on-vehicle AI execution.
- **Chapter 34 (Future Workforce):** SDV/centralised computing skill gaps.
- **Chapter 1 (Evolution):** the architectural generations leading to zonal.

---

### Problem IDs referenced in this chapter

W1-010, W1-016, W1-018; W2-004, W2-006, W2-007, W2-008, W2-009, W2-013, W2-016; W4-019; W5-024; G2-002, G2-007, G2-008, G2-019, G2-020, G2-021, G2-022, G2-027, G2-028.

---

*End of Chapter 29.*