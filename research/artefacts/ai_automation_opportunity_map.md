# AI and Automation Opportunity Map

**Artefact:** #10 (Task.md §15)  
**Source basis:** research/W1–W5, G1–G4, KNOWLEDGE_INDEX.md, part5 (AI-native/agentic), part4 (existing AI adoption), part6 (unsolved/underserved)  
**Date:** 2026-11-05  
**Scope:** Map of where AI and automation can genuinely improve automotive software engineering, where they are experimental, and where they cannot yet replace conventional engineering methods. Opportunity IDs (OPP-*) are artefact-local and cross-reference problem IDs.

---

## 1. How to read this map

- **OPP-ID:** artefact-local opportunity identifier.
- **Problem IDs:** canonical problem records the opportunity addresses.
- **Opportunity:** what could be automated or AI-assisted.
- **Type:** Automation (deterministic) / AI-assisted (LLM/ML) / Agentic (multi-step) / Hybrid.
- **Feasibility:** Low / Medium / High (research estimate; includes data, tooling, certification constraints).
- **Human approval:** whether safety-critical or compliance sign-off must remain human.
- **Key risks:** hallucination, certification, IP/confidentiality, data availability, explainability.

**Boundary rule (Task.md §7):** AI must not be assumed to solve every problem. Safety-critical sign-off, hazard analysis, safety case approval, and certification evidence remain human responsibilities. AI is a tool, not an authority (W5-017, W5-019, W5-020, W5-025).

---

## 2. Requirements engineering opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-01 | W1-001, W5-006 | Ambiguity/underspecification detection in natural-language requirements | AI-assisted | Medium | Required | False confidence; domain context |
| OPP-02 | W1-001, W1-005 | Requirement quality scoring and completeness checks at intake | AI-assisted | Medium | Required | Hallucinated completeness |
| OPP-03 | W1-002, W1-003 | Semantic link suggestion between abstraction levels (SHRQ→SyRQ→SWRQ) | AI-assisted | Medium | Required | Wrong links; staleness |
| OPP-04 | W1-003, W4-007 | Automated traceability link maintenance and staleness detection | Automation + AI | Medium | Required (safety evidence) | False positives; audit acceptance |
| OPP-05 | W1-004, W3-018 | ML/AI-specific requirement templates (ODD, data, monitors, KPIs) | Automation | Medium | Required | Standards still maturing |
| OPP-06 | W1-005, W1-017 | Change-impact analysis across requirements, architecture, tests | Automation + AI | Medium–High | Required | Incomplete dependency graph |
| OPP-07 | W1-006 | Detection of incomplete requirements / assumption gaps before supplier handoff | AI-assisted | Medium | Required | Implicit context not captured |
| OPP-08 | W1-011, W2-011 | Variant consistency checking and binding-time validation | Automation | Medium | Required | Variant combinatorics |
| OPP-09 | W1-014, W4-011, W4-013 | Standards-aware assistants (ISO 26262, ASPICE, 21434 cross-references) | AI-assisted | Medium | Required | Hallucinated citations; outdated standards |

---

## 3. System engineering and architecture opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-10 | W1-007, W1-008 | Interface governance: single source of truth; drift detection between ICDs and ARXML | Automation | Medium–High | Required | Enforcement authority unclear |
| OPP-11 | W1-009, W5-007 | MBSE model generation assistance from requirements | AI-assisted | Medium | Required | Cultural adoption; model governance |
| OPP-12 | W1-010, W1-018 | Timing analysis tooling integration (AUTOSAR timing extensions) | Automation | Medium | Required | Analysis unsolved; not certified |
| OPP-13 | W1-013, W4-002, W4-003 | ARXML canonicalization, diff normalization, consistency checks | Automation | High | For safety-relevant config | Low (deterministic) |
| OPP-14 | W1-016, W2-009, W5-024 | Architecture impact analysis for zonal/centralised consolidation | AI-assisted | Medium | Required | New failure modes; judgement-heavy |
| OPP-15 | W1-017 | Engineering knowledge graph / dependency graph across artefacts | Automation + AI | Medium–High | Required | Data silos; governance |
| OPP-16 | W1-008, W5-017 | Architecture review support (consistency checks, debt detection) | AI-assisted | Medium | Required | Hallucination; authority |

---

## 4. AUTOSAR and middleware opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-17 | W2-001, W4-003 | Automated ARXML generation/validation from higher-level models | Automation | Medium–High | Required | Tool lock-in |
| OPP-18 | W2-011, W2-015, W4-005 | Semantic ARXML diff/merge and variant reconciliation | Automation | High | Required | Variant combinatorics |
| OPP-19 | W2-012 | RTE generation consistency validation (SWC vs BSW) | Automation | Medium–High | Required | Late surface |
| OPP-20 | W2-002, W2-016 | Classic/Adaptive gateway translation assistance (interface mapping) | AI-assisted | Medium | Required | Semantic/timing loss |
| OPP-21 | W2-005, W4-004 | Configuration portability checks across BSW vendors | Automation | Medium | Required | Vendor lock-in |
| OPP-22 | W2-010, W4-016, G3-018 | OTA update impact analysis and safe-state planning | Automation + AI | Medium | Required | Fleet-scale risk |
| OPP-23 | W2-019, W5-002 | AUTOSAR configuration assistants (guided, explainable) | AI-assisted | Medium | Required | Hallucination; certification |
| OPP-24 | W2-007, W2-008 | Middleware timing analysis and service-discovery tuning | Automation | Medium | Required | Real-time constraints |

---

## 5. Software development and code quality opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-25 | W4-008, W4-009, W4-010 | Static analysis triage: false-positive reduction, deviation drafting | AI-assisted | Medium | Required (safety) | Missed violations |
| OPP-26 | W4-008 | Automated code review assistance (MISRA/CERT) | AI-assisted | Medium | Required | Hallucinated compliance |
| OPP-27 | W4-021 | Build time optimization (incremental, caching, parallelization) | Automation | Medium–High | Not safety-critical | Toolchain constraints |
| OPP-28 | W4-020 | Safety-critical CI/CD gating and evidence collection | Automation | Medium | Required | Pipelines lack safety gating |
| OPP-29 | W5-019, W4-030 | AI-generated code safety screening and certification support | AI-assisted | Low–Medium | Mandatory | Certification gap; hallucination |
| OPP-30 | G1-020, G1-021 | MCAL configuration validation and portability checks | Automation | Medium | Required | Vendor lock-in |

---

## 6. Integration opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-31 | W1-006, W1-007, W5-008 | Integration "discovery" detection: flag unverified cross-component interactions before integration | AI-assisted | Medium | Required | Implicit interactions |
| OPP-32 | W4-023, W4-028 | Automated data transformation between OEM/supplier toolchains | Automation | Medium–High | Required | Format drift |
| OPP-33 | W4-024, W4-028 | PLM↔ARXML synchronization automation | Automation | Medium | Required | Data ownership |
| OPP-34 | G1-015, G1-022, G1-023 | Hardware/software co-design interface validation (pinout, memory map, timing) | Automation | Medium | Required | Late hardware changes |
| OPP-35 | G2-032, G2-014 | Cross-domain actuator conflict detection (motion/energy/ADAS) | Simulation + AI | Medium | Required | Combinatorial complexity |
| OPP-36 | W5-012, W5-014 | Knowledge capture and onboarding assistants for integration expertise | AI-assisted | Medium | Required | Knowledge loss; confidentiality |

---

## 7. Testing and validation opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-37 | W3-001, W3-012 | Plant model fidelity checking and scenario-based validation | Automation + AI | Medium | Required | Model accuracy |
| OPP-38 | W3-002, W3-023 | Test case reuse across MIL/SIL/HIL with traceability preservation | Automation | Medium | Required | Reuse safety |
| OPP-39 | W3-003, W3-030 | Reproducible test environments (version-controlled XiL setups) | Automation | Medium–High | Not safety-critical | Tooling gaps |
| OPP-40 | W3-010, W3-016, W4-018 | HIL scheduling and utilization optimization | Automation | Medium | Not safety-critical | Bench constraints |
| OPP-41 | W3-011, W1-011 | Automated HIL environment generation for variants | Automation | Medium | Required | Variant explosion |
| OPP-42 | W3-018, W4-017 | Scenario generation and coverage estimation for ADAS/ADS | AI-assisted | Medium | Required | Completeness unknown |
| OPP-43 | W3-019, W3-029 | Flaky-test detection and nondeterminism source identification | Automation + AI | Medium | Required | False attribution |
| OPP-44 | W3-024 | Test case specification quality checking | AI-assisted | Medium | Required | Ambiguous tests |
| OPP-45 | W3-026, W3-037 | Risk-based test prioritization and coverage gap detection | AI-assisted | Medium | Required | Underdeveloped methods |
| OPP-46 | W3-027, W3-036 | Invalid-failure triage and test infrastructure health monitoring | Automation | Medium–High | Not safety-critical | Infrastructure variance |
| OPP-47 | W3-028 | Regression test selection and optimization | Automation + AI | Medium–High | Required | Safety-critical subset |
| OPP-48 | W3-030, W3-036 | Test data management and artefact versioning across XiL | Automation | Medium–High | Required | No standard TDM |
| OPP-49 | W3-032, W4-013 | Unit testing strategy assistance and evidence generation | AI-assisted | Medium | Required | Weak evidence |
| OPP-50 | W3-033 | Fault injection scenario generation from failure models | AI-assisted | Medium | Required | Incomplete models |
| OPP-51 | W3-034, W4-012 | Safety validation test volume optimization (statistical methods) | Automation | Low–Medium | Required | Statistical maturity |
| OPP-52 | W3-036, W4-029 | Test tool interoperability adapters and result normalization | Automation | Medium | Not safety-critical | Vendor formats |
| OPP-53 | W3-038, W5-009 | Testing knowledge capture and skill support assistants | AI-assisted | Medium | Required | Expertise gaps |

---

## 8. Compliance, safety and cybersecurity opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-54 | W4-011, W4-012 | Safety case evidence collection and change-impact re-assessment support | Automation + AI | Medium | Mandatory (human approval) | Certification acceptance |
| OPP-55 | W4-012 | Automated safety-case delta analysis after changes | Automation | Medium | Mandatory | Missed impacts |
| OPP-56 | W4-014, W4-015 | Cybersecurity TARA assistance and threat-to-requirement mapping | AI-assisted | Medium | Mandatory | Hallucinated threats |
| OPP-57 | W4-017, W3-018 | SOTIF scenario enumeration support (unknown unsafe scenarios) | AI-assisted | Low–Medium | Mandatory | Incompleteness |
| OPP-58 | W4-025 | Open-source AUTOSAR tooling maturity support | Automation | Medium | Required | Community support |
| OPP-59 | W4-026 | Regulatory change monitoring and impact alerting | Automation + AI | Medium | Required | Regional fragmentation |

---

## 9. Release, OTA and fleet opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-60 | W2-010, W4-016, G3-017 | Release baseline and software identity tracking across fleet | Automation | Medium–High | Required | Version confusion |
| OPP-61 | G3-001, G3-018, G3-019 | OTA campaign orchestration and targeting optimization | Automation + AI | Medium | Required | Fleet-scale risk |
| OPP-62 | G3-012, G3-021 | Remote diagnostics and predictive maintenance pipelines | AI-assisted | Medium | Required | Data quality |
| OPP-63 | G3-013 | Digital twin synchronization and fidelity monitoring | Automation + AI | Medium | Required | Twin drift |
| OPP-64 | G3-015, W3-022 | Test fleet management and real-world data collection optimization | Automation | Medium | Required | Privacy/consent |
| OPP-65 | G4-026, G4-001, G4-002 | Fleet energy management and predictive analytics | AI-assisted | Medium | Required | Model accuracy |

---

## 10. Human skills, knowledge and AI governance opportunities

| OPP-ID | Problem IDs | Opportunity | Type | Feasibility | Human approval | Key risks |
|---|---|---|---|---|---|---|
| OPP-66 | W5-012, W5-013, W4-022 | Knowledge capture and onboarding assistants (engineering wikis, Q&A) | AI-assisted | Medium | Required | Confidentiality; IP |
| OPP-67 | W5-014, W5-007 | Cross-discipline communication support (shared glossary, interface summaries) | AI-assisted | Medium | Required | Misinterpretation |
| OPP-68 | W5-016, W4-030 | Confidential/on-prem LLM deployment for engineering data | Automation (infra) | Medium | Required | Cost; data residency |
| OPP-69 | W5-018 | Automotive-specific fine-tuning datasets and retrieval (RAG) over engineering artefacts | AI-assisted | Medium | Required | Data silos; IP |
| OPP-70 | W5-020, W5-025 | AI output explainability and accountability frameworks | Automation + governance | Medium | Mandatory | Accountability gap |
| OPP-71 | W5-025, W4-030 | AI governance in engineering workflows (policies, audit trails, human-in-the-loop) | Automation + governance | Medium | Mandatory | Adoption resistance |
| OPP-72 | W5-005, W5-009 | AI-assisted skill support for ADAS/ML and testing | AI-assisted | Medium | Required | Skill gaps persist |

---

## 11. What AI cannot (yet) reliably do

| Area | Why not (problem IDs) | Boundary |
|---|---|---|
| Safety-critical code generation sign-off | W5-019, W4-030, W5-017 | AI-generated code lacks certification; human approval mandatory |
| Hazard analysis and safety case approval | W5-003, W4-011, W4-012 | Requires expert judgement and accountability |
| Functional safety certification evidence | W4-011, W5-019 | Certification bodies require human-accountable evidence |
| Real-time timing proof for safety-critical systems | W1-010, W2-004 | Timing analysis unsolved; AI cannot prove determinism |
| Freedom-from-interference evidence | W2-004, W2-018 | Mixed-criticality safety case is judgement-heavy |
| SOTIF completeness (unknown unsafe scenarios) | W4-017, W3-018 | Scenario enumeration inherently incomplete |
| Physical EMC/durability validation | W3-021, W3-017 | Physical constraints; cannot be simulated fully |
| Hardware obsolescence and supply-chain decisions | G1-007–G1-009, G1-004 | Business/strategic judgement |
| Regulatory/legal interpretation | W4-026, W1-014 | Jurisdiction-specific; human authority |
| Safety-critical test oracle for novel scenarios | W3-018, W3-019 | Oracle problem remains |

---

## 12. Opportunity count summary

| Lifecycle area | OPP-IDs | Count |
|---|---|---|
| Requirements engineering | OPP-01 … OPP-09 | 9 |
| System engineering & architecture | OPP-10 … OPP-16 | 7 |
| AUTOSAR & middleware | OPP-17 … OPP-24 | 8 |
| Software development & code quality | OPP-25 … OPP-30 | 6 |
| Integration | OPP-31 … OPP-36 | 6 |
| Testing & validation | OPP-37 … OPP-53 | 17 |
| Compliance, safety & cybersecurity | OPP-54 … OPP-59 | 6 |
| Release, OTA & fleet | OPP-60 … OPP-65 | 6 |
| Human skills, knowledge & AI governance | OPP-66 … OPP-72 | 7 |
| **Total** | OPP-01 … OPP-72 | **72** |

---

## 13. Evidence and source references (selected)

- research/W1–W5 reports (problem records, automation feasibility, AI applicability)
- research/G1–G4 reports (hardware, vehicle domains, cloud/fleet, energy/thermal)
- research/part4_ch26_current_ai_adoption.md
- research/part4_ch27_limitations_existing_solutions.md
- research/part5_ch30_ai_native.md, part5_ch31_agentic_systems.md, part5_ch33_autonomous_workflows.md
- research/part6_ch38_unsolved_underserved.md
- research/part7_ch44_opportunities.md
- research/KNOWLEDGE_INDEX.md (cross-cutting themes 6, 9, 11)
- research/DATA_VERIFY.md

---

*End of artefact: ai_automation_opportunity_map.md*


---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

