# Part VII — Research Conclusions

## Chapter 42 — Areas with Insufficient Existing Solutions

### Summary

Part IV (Chapters 23–27) established that the industry is not short of tools, standards, or methods, but that **none of them composes** into an end-to-end, semantically consistent, evidence-producing flow. Chapter 27 lists twelve items that no existing solution category resolves. This chapter expands that list into a consolidated view of **areas where existing solutions are insufficient or absent**, distinguishing (a) areas where a solution exists but is immature/incomplete, (b) areas where solutions exist in silos but do not compose, and (c) areas where no solution exists at all. The classification is the input to Chapter 43 (gaps) and Chapter 44 (investigation opportunities). The central finding is unchanged: **the gap is not absence of tools; it is composition, semantics, and certified evidence.**

---

### 42.1 Purpose and method

This chapter maps each major engineering need (derived from Parts II–III problem records) against the solution categories assessed in Part IV (commercial tools, open source, automation, AI, standards). Three statuses are used:

- **Solved (S):** solution is mature and widely effective.
- **Partial (P):** solution exists but is immature, incomplete, or non-composing.
- **Insufficient/Absent (I):** no effective solution exists; or existing solutions do not address the need.

No area in this chapter is classified S; the research record contains no problem category that is fully solved industry-wide.

---

### 42.2 Insufficient-solution matrix

| # | Area / need | Status | Existing (partial) solutions | Why insufficient | Problem IDs |
|---|---|---|---|---|---|
| 1 | End-to-end digital thread (requirements→code→tests→safety evidence) | I | ALM tools, traceability links | Links manual/brittle; no semantic backbone; horizontal XiL trace missing | W4-001, W4-007, W4-024, W4-028, W1-003, W3-023, W4-029 |
| 2 | Semantic reconciliation across tools | I | ARXML, ICDs, gateways | Version skew, tool extensions, no semantic diff/merge; PLM lacks AUTOSAR semantics | W4-002, W4-005, W4-024, W1-007, W1-008, W2-002 |
| 3 | Automated safety evidence and safety-case maintenance | I | ISO 26262 processes, tool qualification | Manual documentation; re-assessment cost unquantified; no automation precedent | W4-011, W4-012, W4-029, W3-025, W1-014, W4-017 |
| 4 | Certified AI in engineering | I | LLM assistants, code gen, RAG | Hallucination, IP, no datasets, no certification; probabilistic vs deterministic | W4-030, W5-016, W5-017, W5-018, W5-019, W5-020, W5-025, CQ-041, CQ-042 |
| 5 | Scenario coverage completeness for ADAS/SOTIF | I | OpenSCENARIO, OpenDRIVE, simulators | Coverage unmeasurable; sim flakiness; scenario reuse unsafe; ML nondeterminism | W3-018, W3-019, W3-020, W4-017, W3-029, G2-001, G2-031, W5-019 |
| 6 | Timing analysis across XiL | I | chronVAL, SymTA/S, Network Designer | Tools do not compose; manual budgets; not validated at runtime; FFI unsolved on Adaptive | W1-010, W2-004, W2-006, W2-007, W2-008, W3-005, CQ-017 |
| 7 | vECU fidelity standards and acceptance criteria | I | vECU levels 0–4, FMI | No consolidated standard; no regulator acceptance threshold; fidelity unmeasured | W3-004, W3-007, W4-019, CQ-034, CQ-036 |
| 8 | Knowledge capture and tacit expertise at scale | I | RAG, knowledge graphs, docs | Document quality poor; tacit knowledge undocumented; no standardised process | W5-012, W5-013, W5-014, W5-023, W4-006, CQ-038 |
| 9 | Requirements quality automation | I | NLP checkers, ALM templates | Experimental; safety-process evidence missing; formal methods not at intake | W1-001, W1-003, W1-004, W5-006, CQ-045, CQ-070 |
| 10 | Architecture governance enforcement | P | Architecture review boards, ICDs | Lack enforcement; static specs drift; no automated conformance | W1-007, W1-008, W1-017, W2-012, W4-005 |
| 11 | Variant management across lifecycle | P | Config tools, binding-time mechanisms | System/AUTOSAR/test variant models not unified; combinatorial explosion | W1-011, W2-011, W3-011, W4-005, CQ-015, CQ-030, CQ-032 |
| 12 | HIL scarcity and scheduling | P | HIL farms, Rapid HIL, cloud HIL | Cost/scarcity; setup weeks; cloud HIL latency/cyber/compliance; vECU not trusted | W3-010, W3-011, W3-015, W3-016, W4-018, CQ-035 |
| 13 | ARXML/configuration automation | P | Config tools, scripts, open parsers | Lock-in; cost; friction; semantic reconciliation manual; no canonical form | W2-001, W2-015, W4-002, W4-003, W4-004, W4-005, W4-025, CQ-013, CQ-014 |
| 14 | Static analysis and code quality evidence | P | QAC/PRQA, LDRA, Parasoft, Tessy | False positives; undecidable rules; version churn; deviation effort; evidence aggregation | W4-008, W4-009, W4-010, W4-029 |
| 15 | MBSE adoption | P | SysML/Rhapsody, Enterprise Architect | Culture/skill/tool/governance blockers; definitional confusion; partial/ad-hoc use | W1-009, W5-007, CQ-055 |
| 16 | ML safety certification (ASIL C/D) | I | SOTIF, ISO/PAS 8800 (emerging) | No ASIL C/D ML safety-case precedent; monitors and ODD not standardised | W1-004, W5-019, W3-029, W5-005, CQ-021, CQ-051 |
| 17 | Semiconductor obsolescence management | P | LTB, redesign, continuation supply, predictive analytics | LTB capital/risk; redesign months; no standard obsolescence risk process; immature analytics | G1-007, G1-008, G1-009, G1-003, G1-004, CQ-064 |
| 18 | Multi-tier supply-chain visibility | P | OEM-foundry partnerships, multi-sourcing | Covers few parts; visibility partial; no shared semiconductor BOM standard | G1-004, G1-005, G1-006, W4-024 |
| 19 | Cloud/backend and connected-services software | I | Cloud platforms, telematics stacks | Fragmented standards, data silos, cost/lock-in, privacy/compliance, cybersecurity; no common vehicle-data standard | G3-001 … G3-028, W4-024, W4-015 |
| 20 | Energy/thermal software validation | P | HIL, vECU, simulation | Energy/thermal HIL and vECU validation gaps; coupled multi-domain control hard to test | G4-031, G4-032, G4-004, G4-005, W3-012, W4-019 |

---

### 42.3 Why existing solutions are insufficient — the six root causes

Repeating and extending Chapter 27 §27.2, the six root causes are:

1. **Fragmentation and proprietary formats** (W4-001, W4-002, W4-004, W4-023, W4-027). Tools do not compose because data does not flow semantically between them.
2. **Semantic gaps between tools** (W4-002, W4-005, W4-024, W4-028). Even neutral formats (ARXML) are not semantically reconciled; PLM does not understand AUTOSAR.
3. **Standards and process weight without automation** (W4-011, W4-013, W4-015, W4-016, W4-026). Compliance overhead scales faster than engineering value; the genuine/avoidable boundary is unmeasured (W1-014).
4. **Scarcity of specialised expertise** (W4-018, W4-022, W5-001 … W5-011). Tools require months of training; organisations depend on few experts.
5. **AI and certification trust gap** (W4-030, W5-016 … W5-020, W5-025). No AI tool is certified for safety-critical engineering use.
6. **Validation infrastructure scarcity** (W4-018, W4-019, W3-010, W3-015). HIL benches are capital-intensive; vECUs are not trusted; scenario coverage does not scale.

These are **socio-technical**: they involve organisations, contracts, skills, and incentives as much as technology. No single tool or vendor can resolve them (Chapter 27 §27.2).

---

### 42.4 The "completeness" illusion (reprise)

Chapter 27 §27.5 lists the trap: because a tool exists for a problem, the problem is assumed solved. The evidence does not support this:

- Requirements tools exist, yet ambiguity and traceability burden persist (W1-001, W1-003, W4-006, W4-007).
- AUTOSAR exists, yet configuration complexity and lock-in persist (W2-001, W4-004).
- Static analysers exist, yet false positives and deviation effort persist (W4-008).
- HIL exists, yet bench scarcity and fidelity gaps persist (W4-018, W4-019).
- AI assistants exist, yet hallucination and certification gaps persist (W5-017, W4-030).

The gap is **composition**: tools do not compose into an end-to-end, semantically consistent, evidence-producing flow.

---

### 42.5 Areas where no solution exists at all (the "I" rows)

Rows 1–9 and 16, 19 in the matrix above are classified I (insufficient/absent). These are the highest-leverage targets for further investigation (Chapter 44) and for any future capability development:

1. End-to-end digital thread.
2. Semantic reconciliation across tools.
3. Automated safety evidence and safety-case maintenance.
4. Certified AI in engineering.
5. Scenario coverage completeness for ADAS/SOTIF.
6. Timing analysis across XiL.
7. vECU fidelity standards and acceptance criteria.
8. Knowledge capture and tacit expertise at scale.
9. Requirements quality automation.
10. ML safety certification (ASIL C/D).
11. Cloud/backend and connected-services software (fragmented, no common standard).

---

### 42.6 What "sufficient" would require (without prescribing solutions)

For each I-row, the research record implies necessary conditions (not solutions):

- **Digital thread:** a shared, versioned, machine-readable engineering data backbone with semantic identity (CQ-014, CQ-068).
- **Semantic reconciliation:** canonical ARXML serialization plus schema/version governance (CQ-014, CQ-047).
- **Safety evidence:** automated evidence collection with safety-case precedent and regulator acceptance (CQ-027, CQ-023).
- **Certified AI:** governance framework, verification gates, provenance, IP handling, audit trails, human-approval requirements (W5-025, W4-030, CQ-043, CQ-054).
- **Scenario completeness:** measurable coverage criteria and determinism requirements for simulation (CQ-001, CQ-062).
- **Timing:** a unified timing-verification method across XiL levels (CQ-017, CQ-009).
- **vECU fidelity:** regulator-accepted fidelity thresholds per ASIL (CQ-034).
- **Knowledge capture:** quality-gated documentation and engineering knowledge graphs (CQ-038).
- **Requirements quality:** validated NLP/LLM quality checking in safety-relevant processes (CQ-045).
- **ML safety:** safety-case precedent for ML at ASIL C/D and monitor standardisation (CQ-021, CQ-051).
- **Cloud/backend:** common vehicle-data standard and interoperability framework (G3-011).

These are research and standardisation prerequisites, not product specifications.

---

### 42.7 Evidence limitations

- No ROI or effectiveness quantification exists for any solution category (Chapter 27 §27.7).
- AI adoption evidence derives from a small survey (n=9, S9); treat as directional.
- Open-source maturity assessments reflect the 2026 state; the VDA MoU may change the trajectory.
- The matrix maps categories to problem IDs; it does not quantify effectiveness.

---

### 42.8 Cross-references

- **Chapter 27 (Part IV):** limitations of existing solutions (source of the twelve unsolved items).
- **Chapters 23–26 (Part IV):** per-category capabilities and limits.
- **Chapter 41 (this Part):** problem categories.
- **Chapter 43 (this Part):** technology and process gaps.
- **Chapter 44 (this Part):** investigation opportunities.
- **Chapter 45 (this Part):** open research questions.

---

### Problem IDs referenced in this chapter

W1-001, W1-003, W1-004, W1-007, W1-008, W1-010, W1-011, W1-013, W1-014, W1-017; W2-001, W2-002, W2-004, W2-006, W2-007, W2-008, W2-011, W2-012, W2-015; W3-004, W3-005, W3-007, W3-010, W3-011, W3-012, W3-015, W3-016, W3-018, W3-019, W3-020, W3-023, W3-025, W3-029, W3-031; W4-001, W4-002, W4-003, W4-004, W4-005, W4-006, W4-007, W4-008, W4-009, W4-010, W4-011, W4-012, W4-015, W4-016, W4-017, W4-018, W4-019, W4-023, W4-024, W4-025, W4-026, W4-027, W4-028, W4-029, W4-030; W5-001 … W5-006, W5-012, W5-013, W5-014, W5-016, W5-017, W5-018, W5-019, W5-020, W5-023, W5-025; G1-003, G1-004, G1-007, G1-008, G1-009; G2-001, G2-031; G3-001 … G3-028; G4-004, G4-005, G4-031, G4-032; CQ-001, CQ-009, CQ-013, CQ-014, CQ-015, CQ-017, CQ-021, CQ-023, CQ-027, CQ-030, CQ-032, CQ-034, CQ-035, CQ-036, CQ-038, CQ-041, CQ-042, CQ-045, CQ-051, CQ-054, CQ-062, CQ-064, CQ-068, CQ-070.

---

### 42.9 Sources

- `research/book/part4_ch27_limitations_existing_solutions.md` (§27.2, §27.4, §27.6).
- `research/CROSS_CHECK.md` (§2 overlaps, §3 gaps, §5 recommendations).
- `research/KNOWLEDGE_INDEX.md`.
- `research/CROSS_QUESTIONS.md` (CQ-001, CQ-009, CQ-013, CQ-014, CQ-015, CQ-017, CQ-021, CQ-023, CQ-027, CQ-030, CQ-032, CQ-034, CQ-035, CQ-036, CQ-038, CQ-041, CQ-042, CQ-045, CQ-051, CQ-054, CQ-062, CQ-064, CQ-068, CQ-070).
- `research/G3_cloud_fleet.md`, `research/G4_energy_thermal.md`.
- `Task.md` §4, §12, §16 (artefact 12).

---

*End of Chapter 42.*