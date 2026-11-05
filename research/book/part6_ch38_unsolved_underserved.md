# Part VI — Consolidated Problem Discovery

## Chapter 38 — Unsolved and Underserved Problems

**Book:** *The State of Automotive Software Engineering*  
**Part:** VI — Consolidated Problem Discovery  
**Chapter:** 38 — Unsolved and Underserved Problems  
**Sources:** Part IV Chapter 27 §27.6; KNOWLEDGE_INDEX; Part V; Part III Chapters 14–22

---

## 38.0 Chapter purpose

This chapter identifies the problems for which **no existing solution category is adequate**. It consolidates the twelve items from Chapter 27 §27.6 with additional gaps surfaced by Part V (future engineering) and the G-series workstreams (semiconductor, vehicle domains, cloud/fleet, energy/thermal). It is the "where the market has not delivered" view: the problems that persist *despite* the existence of commercial tools, open source, automation, AI, and standards.

The conclusion of Part IV stands: **the gap is not absence of tools; it is composition.** Tools do not compose into an end-to-end, semantically consistent, evidence-producing flow (Chapter 27 §27.5).

---

## 38.1 The twelve structural gaps (from Chapter 27 §27.6)

| # | Gap | Problem IDs | Why existing solutions fall short |
|---|---|---|---|
| U-01 | End-to-end digital thread | W4-001, W4-007, W4-024, W4-028 | Tools are strong in isolation; data moves by file export/import and glue work; no shared semantic model |
| U-02 | Semantic reconciliation across tools | W4-002, W4-005, W4-024 | ARXML is not canonical; PLM does not understand AUTOSAR semantics; no semantic diff/merge |
| U-03 | Automated safety evidence and safety-case maintenance | W4-011, W4-012, W4-029 | Safety cases are maintained manually; change-impact re-assessment is costly; no standard evidence format |
| U-04 | Certified AI in engineering | W4-030, W5-019, W5-020, W5-025 | No AI tool is certified for safety-critical use; hallucination, IP, governance gaps |
| U-05 | Scenario coverage completeness for ADAS/SOTIF | W3-018, W4-017 | Scenario space is unbounded; no completeness metric; SOTIF does not scale |
| U-06 | Timing analysis across XiL | W1-010, W2-007, W2-008, W3-005 | Timing tools do not compose with configuration and CI; end-to-end budgets are manual |
| U-07 | vECU fidelity standards and acceptance criteria | W4-019, CQ-034 | vECU interfaces are not standardised (FMI vs AUTOSAR); no regulator-accepted fidelity criteria |
| U-08 | Knowledge capture and tacit expertise at scale | W5-012, W5-013 | Knowledge is tacit and concentrated; documentation is a by-product; RAG quality depends on poor docs |
| U-09 | Requirements quality automation | W1-001, W5-006 | ALM tools store but do not validate; ambiguity detection is not automated |
| U-10 | Architecture governance enforcement | W1-008, W1-017 | Architecture is defined at OEM level but implemented at supplier level; no enforcement tooling |
| U-11 | Variant management across the lifecycle | W1-011, W2-011, W3-011, W4-005 | Variant dimensions are managed independently; binding time is implicit |
| U-12 | HIL scarcity and scheduling | W4-018, W3-011 | Benches are capital-intensive and non-virtualisable; scheduling is manual |

---

## 38.2 Additional unsolved/underserved problems from G-series and Part V

Beyond the twelve, the following remain unsolved by any existing solution category:

### 38.2.1 Semiconductor and hardware (G1)

| Gap | Problem IDs | Why unsolved |
|---|---|---|
| Obsolescence and 15+ year service life vs <7 year semiconductor EOL | G1-007, G1-008, G1-009 | Last-time buys and ECU redesign are the only levers; no industry-standard obsolescence-management protocol |
| MCAL vendor lock-in | G1-020, G1-021, W2-005 | AUTOSAR standardises interfaces, not implementations; changing silicon vendor requires re-qualifying MCAL |
| Hardware/software co-design silos | G1-015, G1-022, G1-023, G1-031 | BSP patches break compatibility; bring-up surprises surface at integration |
| Sensor/actuator interface standardisation | G1-027, G1-028 | Proprietary SerDes; no standardised hardware interfaces; scales with ADAS sensor count |
| Memory wall and embedded-flash scalability | G1-017, G1-018, G1-019 | Limits model complexity and feature growth; no standard mitigation |
| Mixed-criticality consolidation certification | G1-016, G1-024, G2-027, W2-009 | Freedom from interference must be proven per platform; hypervisor evidence lags hardware |
| Semiconductor talent shortage | G1-034, G3-026 | Automotive is a secondary customer; expertise concentrated in few regions |

### 38.2.2 Vehicle domains (G2)

| Gap | Problem IDs | Why unsolved |
|---|---|---|
| ADAS perception/ML lifecycle and ODD specification | G2-001, G2-031, W1-004, W3-018 | ODD specified informally; ML safety (ISO/PAS 8800) evolving; scenario coverage unmeasurable |
| Brake-by-wire / steer-by-wire validation | G2-007, G2-008 | Fault-tolerance and haptics are safety-critical and hard to validate; no standard method |
| EV coupled control loops (inverter/BMS/charging/thermal/energy) | G2-010, G2-011, G2-012, G2-013, G2-025, G4-001…G4-032 | Cross-domain actuator conflicts (G2-032) more frequent as domains consolidate |
| Zonal/centralised migration | G2-019, G2-021, G2-022, W1-016 | Re-architecture, not refactor; timing, redundancy, thermal, single point of failure |
| Cross-domain actuator conflicts | G2-032, G2-006, G2-014 | Current ECU-per-function architecture does not support cross-domain coordination natively |
| Commercial/off-highway and two-/three-wheeler toolchains | G2-029, G2-030 | Passenger-car toolchain does not serve extreme variant diversity and cost constraints |

### 38.2.3 Cloud, backend and fleet (G3)

| Gap | Problem IDs | Why unsolved |
|---|---|---|
| OTA as a safety-aware, stateful lifecycle (not file distribution) | G3-001, G3-018, G3-019, W2-010, W4-016 | Bootloader outside AUTOSAR scope; safe-state OEM-specific; no standard rollback protocol; most OEMs OTA only infotainment |
| Connected-vehicle data volume vs legacy pipelines | G3-003, G3-006, G3-028 | 25 GB/hour/vehicle exceeds batch ETL; streaming-first requires skills and cost |
| Real-time latency for vehicle-relevant decisions | G3-004, G3-016 | Cloud-first assumes connectivity; physics of distance; no deterministic QoS end-to-end |
| Data silos (engineering/customer/geographic/vendor/temporal) | G3-005, G3-020 | Organisational, not just technical; requires data mesh and domain ownership |
| Fleet-management platform fragmentation (mixed-OEM) | G3-010, G3-011, G3-024 | OEMs treat connected data as proprietary; no open data standard; competitive differentiation |
| Cloud cybersecurity at fleet scale | G3-009, W4-014, W4-015 | vSOC different from IT SOC; API security immature in automotive; skills scarce |
| Privacy/consent/sovereignty enforcement at scale | G3-008, G3-022, G3-023 | Multi-party enforcement; evolving regulation; no central consent registry |
| Digital twin value and synchronisation | G3-013 | Cost vs fidelity; sync not tied to OTA/config events; unclear ROI |
| SUMS evidence and software identity at fleet scale | G3-017, W4-016 | R156 specifies activities, not implementation; evidence scattered across PLM/cloud/supplier systems |
| Cloud HIL certification and latency | G3-014, W3-015 | HIL requires deterministic low-latency I/O; cloud introduces non-determinism and trust boundaries |
| OEM business-model shift to platform operator | G3-027 | Hardware-era organisation; lack of software platform operating model; skills |

### 38.2.4 Energy and thermal (G4)

| Gap | Problem IDs | Why unsolved |
|---|---|---|
| SoC/SoH estimation accuracy | G4-001, G4-002, G2-011 | Estimation error propagates into range anxiety, charging scheduling, safety decisions |
| Thermal runaway detection and containment | G4-004, G4-005, G4-030 | Safety-critical; detection latency; false positives/negatives; software response |
| Charging interoperability (protocols, chargers, grid) | G4-012, G4-015, G4-016, G4-032 | Depends on charger firmware and grid state; software alone cannot fully solve |
| ISO 15118 PKI / OCPP fragmentation | G4-013, G4-014 | Version fragmentation; certificate management complexity |
| V2G bidirectional charging | G4-017 | Grid safety; complexity; standards immature |
| High-voltage safety (ASIL) of charging/BMS | G4-024, G4-018 | Safety case for charging and BMS software; ASIL allocation |
| Charging cybersecurity | G4-025 | Attack surface at charging interfaces |
| Energy/thermal HIL and vECU validation | G4-031 | Hard to validate energy/thermal models in HIL |

### 38.2.5 Human skills, AI and future engineering (W5, Part V)

| Gap | Problem IDs | Why unsolved |
|---|---|---|
| Structural talent shortage (embedded, AUTOSAR, safety, cyber, AI/ML, systems) | W5-001…W5-005, W5-024, G1-034, G3-026 | Education and compensation lag by years; not solvable by hiring alone |
| Knowledge silos and turnover-driven loss | W5-012, W5-023 | Tacit knowledge; documentation is a by-product; bus factor |
| AI adoption barriers (confidentiality, IP, hallucination, certification, governance) | W5-016…W5-020, W5-025, W4-030 | No certified AI; no automotive datasets; no governance framework |
| Education pipeline mismatch | W5-022, W5-021 | University curricula lag industry; 30% shortfall in EV/smart-mfg fields |
| Compensation disadvantage | W5-015 | Pay parity not realistic near term; HR practices not adapted |
| SDV skill gap | W5-024, W5-007 | Hypervisor, SOA, OTA, cloud, data skills scarce |

---

## 38.3 Stakeholder view: who is underserved

| Stakeholder | Most underserved problems |
|---|---|
| OEMs | End-to-end digital thread (U-01), variant management (U-11), OTA lifecycle (G3-001, G3-018), SDV skills (W5-024), business-model shift (G3-027) |
| Tier-1 suppliers | Toolchain mismatch (W4-023), MCAL lock-in (G1-020, G1-021), safety evidence (U-03), HIL scarcity (U-12) |
| Tier-2 / semiconductor suppliers | Obsolescence (G1-007…G1-009), supply opacity (G1-004), talent (G1-034) |
| Engineering service providers | Onboarding/training (W5-013, W4-022), toolchain knowledge (W5-011), skills gaps (W5-006…W5-011) |
| Tool vendors | Interoperability expectations (W4-001, W4-002), open-source competition (W4-025), AI trust (W4-030) |
| Testing service providers | HIL scarcity (U-12), scenario coverage (U-05), vECU fidelity (U-07), cloud HIL (G3-014) |
| Startups / SMEs | Licensing/cost (W4-027, W4-004), open-source immaturity (W4-025), skills (W5-001) |
| EV manufacturers | Energy/thermal (G4), charging interop (G4-032), BMS (G2-011, G4-001, G4-002) |
| Autonomous-driving companies | Scenario coverage (U-05), ML safety (G2-001, W1-004), sensor interfaces (G1-027) |
| Fleet operators | Mixed-OEM fragmentation (G3-010), predictive maintenance (G3-012), connectivity (G3-016) |

---

## 38.4 Why these remain unsolved: structural barriers

Drawing on Chapter 36 (root causes) and Chapter 27 (limitations):

1. **Composition gap, not tool gap.** Tools exist; they do not compose (RC-1). Solving composition requires standardisation and governance that no single vendor has incentive to provide.
2. **Safety evidence is a liability, not a feature.** Vendors cannot certify AI or vECU fidelity without regulatory frameworks that do not yet exist (RC-5, RC-8).
3. **Economics of lock-in.** Vendor lock-in (W4-004, W4-027) is economically rational; switching costs are high; procurement is compliance-driven.
4. **Skills are the binding constraint.** Even where technology exists, the people to operate it do not (RC-4). Training takes years.
5. **Standards evolve slower than technology.** Regulatory and standards cycles (RC-5) lag the SDV/AI transition by years.
6. **Organisational inertia.** Hardware-era organisations, contracts, and incentives (RC-3) resist the socio-technical changes SDV requires.
7. **Data confidentiality is a hard wall.** IP and NDA constraints (W5-016) prevent the data sharing needed for AI and cross-org learning (RC-8).

---

## 38.5 The "completeness illusion" (from Chapter 27 §27.5)

A recurring trap: assuming that because a tool exists for a problem, the problem is solved.

- Requirements tools exist, yet ambiguity and traceability burden persist (W1-001, W1-003, W4-006, W4-007).
- AUTOSAR exists, yet configuration complexity and lock-in persist (W2-001, W4-004).
- Static analysers exist, yet false positives and deviation effort persist (W4-008).
- HIL exists, yet bench scarcity and fidelity gaps persist (W4-018, W4-019).
- AI assistants exist, yet hallucination and certification gaps persist (W5-017, W4-030).

The gap is not absence of tools; it is **composition**: tools do not compose into an end-to-end, semantically consistent, evidence-producing flow.

---

## 38.6 What can be automated vs. what requires human judgement

From the problem records (automation feasibility and AI applicability fields):

| Category | Can be automated (with caveats) | Requires human judgement / approval |
|---|---|---|
| Mechanical data transformation | Glue work (W4-028), ARXML bulk edits (W4-003), build/test execution | Safety-critical decisions |
| Orchestration | HIL scheduling (W3-010, W3-016), OTA campaigns (G3-001) | Safety activation, rollback (W2-010, G3-018) |
| Detection/anomaly | Fleet health (G3-021), data quality (G3-020), defect prediction | Incident response, recall scope |
| Generation (assistive) | Code for scripts (W4-030), test scenarios (W3-018), documentation | Safety-critical code (W5-019), certification evidence |
| Verification | Static analysis triage (W4-008), trace link suggestion (W4-007) | Safety case argumentation (W4-012), SOTIF completeness (W4-017) |
| Governance | Cost optimisation (G3-007), retention tiering (G3-028) | Policy, legal, contract decisions (G3-008, G3-022) |

**Human authority must remain** for: safety-critical activation/rollback, certification sign-off, policy/legal/contract decisions, SOTIF completeness judgement, AI governance, and strategic decisions. (Task.md §5: identify what engineers can realistically automate, what requires expert judgement, and what should remain under human authority.)

---

## 38.7 Evidence limitations

- The "unsolved" assessment is based on Part IV's solution analysis (Chapters 23–27) and the problem records; no primary vendor interviews or hands-on tool evaluations were conducted (Chapter 27 §27.7).
- Licence costs and adoption rates are not publicly available (W4-027).
- AI adoption evidence derives from a small survey (n=9, S9); treat as directional.
- The matrix maps solution categories to problem IDs; it does not quantify effectiveness or ROI.

---

## 38.8 Cross-references

- **Chapter 35 (Catalogue):** full ID list.
- **Chapter 36 (Root-Cause):** root-cause families.
- **Chapter 37 (Cross-Domain Dependency):** dependency maps and solution-vs-gap matrix.
- **Chapter 39 (Emerging):** problems that will become more serious.
- **Chapter 40 (Consolidated Findings):** final synthesis.
- **Part IV (Chapters 23–27):** existing solutions and limitations.

---

*End of Chapter 38.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

