# Part II — Chapter 6: System Engineering and Architecture

**Book:** *The State of Automotive Software Engineering*  
**Part:** II — Complete Automotive Software Lifecycle  
**Chapter:** 6 — System Engineering and Architecture  
**Source workstreams:** W1 (Requirements & System Engineering), W2 (AUTOSAR & Middleware), W4 (Toolchain & Standards), W5 (Skills & AI)  
**Problem IDs in scope:** W1-006, W1-007, W1-008, W1-009, W1-010, W1-011, W1-015, W1-016, W1-017, W1-018, W2-009, W2-016, W2-018, W4-012, W5-007

---

## 6.0 Chapter purpose and problem map

System engineering (SE) and architecture define how vehicle functions are decomposed, allocated to ECUs and compute platforms, and integrated. The research record shows that automotive SE is characterised by weak interface governance, unsolved timing analysis, slow MBSE adoption, and an architecture in transition from distributed ECUs to zonal/centralised compute. These problems are coupled: incomplete requirements (Chapter 5) + no system-level architecture + no interface governance = integration as discovery.

| Problem ID | Title | Primary root cause (short) |
|---|---|---|
| W1-006 | Incomplete requirements handed to suppliers | OEM capacity/time; unclear system-requirements ownership |
| W1-007 | Unstable interfaces and interface governance gaps | Distributed development; lack of governance authority |
| W1-008 | Inconsistent architecture across suppliers/OEM | No top-down architecture authority; late architecture definition |
| W1-009 | MBSE adoption barriers (cultural, skill, tool, governance) | Perception of MBSE as diagramming; lack of ROI evidence |
| W1-010 | Timing analysis and predictability unsolved for AUTOSAR | Mismatch between industry needs and academic timing analysis |
| W1-011 | Variant explosion and configuration management | Product-line complexity; ARXML variation-point complexity |
| W1-015 | System engineering skill and workforce gaps | Education lags industry; high complexity |
| W1-016 | Transition to zonal/centralised architecture | Consolidation onto fewer, more powerful computers |
| W1-017 | Architecture consistency and change management without visibility | Lack of systems visibility; no single source of truth |
| W1-018 | Network architecture and timing of communication | Heterogeneous legacy networks; timing not captured in AUTOSAR |
| W2-009 | Hypervisor/mixed-criticality consolidation challenges | Certification uncertainty; real-time virtualisation overhead |
| W2-016 | Classic-to-Adaptive migration requires full communication redesign | Paradigm shift (signal→service) with no automated path |
| W2-018 | Safety integrity level gap: Adaptive targets ASIL-B | Adaptive's dynamic model less suited to hard safety |
| W4-012 | Safety case maintenance and change-impact re-assessment cost | Change propagation through safety evidence is manual |
| W5-007 | System/architecture engineering skills gaps | Scarce systems engineers and MBSE modellers |

---

## 6.1 Interface governance (W1-007, W1-008, W1-017)

Interface definitions are frequently ambiguous, allowing different interpretations. Requirements and interfaces change mid-development; suppliers may not receive updates; and there is no single source of truth for interfaces (W1-007). The root causes are distributed development by many teams, lack of interface governance authority, and proprietary formats.

The consequences include mismatched interfaces, integration failures, and over-engineering (oversized ECUs, redundant sensors) (W1-007). BMW's DATE paper (cited in W1-007) notes that precise interface description is a prerequisite for integration and that standardised BSW simplifies integration — but AUTOSAR standardises BSW, not application interfaces, and ARXML interface descriptions drift.

Architecture consistency across suppliers is similarly weak: suppliers develop to their own architectural vision, misaligned with the OEM's evolving architecture, with no shared system-level requirements and no architecture governance (W1-008). Winner & Maurer (cited in W1-008, W1-017) describe "no authority to reject changes that break architecture" and technical debt accumulation. Change requests ignore cross-domain impacts; system-level requirements are not updated after changes; version control is incoherent across suppliers (W1-017).

**Automation feasibility:** Medium–High for dependency graphs and change-impact analysis; Low for architecture authority (judgement-heavy).

---

## 6.2 MBSE adoption (W1-009, W5-007)

Model-Based Systems Engineering (MBSE) is widely proposed as the cure for document-centric silos, but adoption is limited by:

- Cultural resistance and perception of MBSE as diagramming rather than engineering.
- Modelling skill gaps and lack of skilled modellers.
- Tool integration gaps and model governance immaturity.
- Cost and regulatory acceptance uncertainty.

Only 22% of systems-engineering participants could cite a clear MBSE definition (Purdue dissertation, Maheshwari, cited in W1-009), while a market statistic claims 66% of enterprises rely on MBSE frameworks — these two figures are not reconciled and the latter is flagged as unverified (CROSS_CHECK C1, C6). MBSE is therefore in a state where nominal adoption may exceed genuine practice (W1-009).

SysML v2 (2025 compatibility updates) is a significant migration with uncertain uptake and cost. Cultural change is slow regardless of tooling (W1-009, W5-007).

---

## 6.3 Timing analysis (W1-010, W1-018, W2-006, W2-013)

Timing predictability and timing analysis remain largely unsolved for automotive/AUTOSAR software (W1-010):

- The AUTOSAR standard lacks timing aspects; application timing depends on implementation details (ECU mapping, bus) that AUTOSAR abstracts away.
- Cache effects complicate analysis; 80–100 ECUs in high-end cars multiply interactions.
- Network timing across distributed ECUs, gateways, and Ethernet is not captured in AUTOSAR (W1-018).
- Classic OS (OSEK) fixed-priority, partitioned scheduling has no dynamic task migration; multicore distribution is manual and non-trivial (W2-006, W2-013).
- SOME/IP and DDS middleware introduce discovery latency and jitter (W2-007, W2-008), compounding the problem.

Timing is safety-relevant. Late timing violations surface during integration, not design, and the AUTOSAR timing extensions (R24-11) document use cases but acknowledge complexity. Research-stage methods (formal timing verification, probabilistic timing analysis) are not integrated into standard toolchains and are not certified (W1-010).

**Cross-reference:** Chapter 8 (W2-006, W2-007, W2-008, W2-013); Chapter 11 (temporal synchronisation in XiL, W3-005, W3-013).

---

## 6.4 Variant explosion (W1-011, W2-011, W4-005)

Vehicle variants (hardware, software, region, regulation) explode combinatorially. ARXML variant management and binding are complex (multiple binding times: SystemDesign, CodeGen, PreCompile, Link, PostBuild), and configuration consistency across tools is fragile (W1-011, W2-011, W4-005). Variant explosion is cited by W1, W2, W3, and W4 with different emphases (system, AUTOSAR binding-time, test environment reconfiguration, config diff/merge).

Holsten et al. (2023) note the lack of concepts for integrated systems-platform engineering and that variants split due to diverging requirements. No single source of truth for variant conditions exists across tools (W2-011). This multiplies combinatorial testing (Chapter 11) and configuration error risk (Chapter 9).

---

## 6.5 Zonal and centralised architecture (W1-016, W2-009, W2-016, W2-018)

The shift from distributed ECUs to zonal/centralised high-performance computing introduces:

- Mixed-criticality integration on shared compute (safety + QM on the same SoC).
- Resource allocation, scheduling, and timing predictability challenges.
- Redundancy and fault tolerance (single point of failure risk).
- Thermal and power management of consolidated compute.
- Cybersecurity of consolidated attack surface.
- Supplier ecosystem disruption ("thinking vs acting" separation, Bosch).
- Hypervisor certification uncertainty (W2-009) and real-time virtualisation overhead.
- Adaptive Platform ASIL target gap (ASIL-B target vs Classic's ASIL-D) (W2-018).
- Migration cost: Classic-to-Adaptive requires full communication redesign (signal→service) (W2-016).

Promwad (2025), Accelerat (2025), Frontiers (2025), and ArXiv 2412.01840 document these challenges. McKinsey (2026) frames zonal/central computing as a market direction, but the engineering reality is hybrid: most vehicles run Classic + Adaptive + non-AUTOSAR (Linux/QNX/Android) with gateways (W2 cross-cutting). SystemWeaver (2025) confirms Classic is not going away.

**Open question:** whether SDV/centralised computing increases or decreases total software engineering effort, and over what time horizon (CROSS_QUESTIONS CQ-050).

---

## 6.6 Safety case maintenance (W4-012, W2-018, W4-011)

Safety case maintenance and change-impact re-assessment are costly and largely manual. When architecture or requirements change, safety evidence must be re-assessed; the propagation of change through safety artifacts is not automated (W4-012). This interacts with the ASIL gap (W2-018) and ISO 26262 overhead (W4-011). The cost of safety case maintenance for zonal/centralised architectures vs. distributed ECU architectures is unquantified (CQ-025).

---

## 6.7 Skills (W1-015, W5-007)

System engineering skill and workforce gaps are documented (W1-015, W5-007): shortage of systems engineers, MBSE modellers, and requirements engineers; onboarding/training time; knowledge silos and single-person expertise; communication gaps between software/hardware/system teams. Training takes years; industry changes faster than curricula (W1-015).

---

## 6.8 Cross-cutting root causes

1. **No single source of truth for architecture or interfaces** across OEM and suppliers (W1-007, W1-008, W1-017).
2. **Timing analysis is safety-relevant and largely unsolved**, underpinning the zonal transition (W1-010, W1-018).
3. **MBSE is blocked by culture, skills, tooling, and governance**, despite being the proposed cure (W1-009).
4. **Variant explosion is poorly supported** by configuration/variant management (W1-011).
5. **Zonal/centralised consolidation introduces new failure modes** (mixed-criticality, thermal, redundancy) that current standards and tools do not fully address (W1-016, W2-009).
6. **Safety case maintenance is manual and expensive**, and its cost for consolidated architectures is unquantified (W4-012).

---

## 6.9 Evidence limitations

- MBSE adoption statistics (66% reliance vs. 22% definitional clarity) are unreconciled (CROSS_CHECK C1, C6).
- Zonal/centralised architecture challenges are documented by vendor and academic sources; failure rates and cost of consolidated compute are unquantified.
- Timing analysis findings rely on academic papers and AUTOSAR technical reports; prevalence of timing violations in production is unknown.

---

## 6.10 Cross-references

- **Chapter 5 (Requirements Engineering):** incomplete requirements and late changes (W1-005, W1-006) feed architecture inconsistency (W1-008).
- **Chapter 7 (Software Design and Development):** multicore distribution (W2-006) and embedded skills (W5-001) are downstream of architecture decisions.
- **Chapter 8 (AUTOSAR and Middleware):** classic/adaptive migration (W2-016), ASIL gap (W2-018), hypervisors (W2-009).
- **Chapter 9 (Configuration and Engineering Data):** variant management (W1-011, W2-011) and ARXML drift (W1-013).
- **Chapter 11/12 (Testing, HIL, Vehicle):** scenario completeness and timing verification (CQ-017, CQ-020).
- **Part V (Future Engineering):** SDV and zonal computing (W1-016, W5-024).

---

*End of Chapter 6.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

