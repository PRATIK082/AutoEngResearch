# Part VII — Research Conclusions

## Chapter 44 — Opportunities for Further Investigation

### Summary

This chapter identifies **opportunities for further investigation** — areas where the research record is incomplete, contradictory, or where new evidence would materially change the conclusions of Parts I–VI. It is deliberately not a solution or product roadmap (Task.md §17). It consolidates (a) the 72 cross-domain questions (CROSS_QUESTIONS.md), prioritised P1/P2/P3, (b) the coverage gaps identified in CROSS_CHECK §3, (c) the hidden/emerging problems in Task.md §12, and (d) the unanswered questions embedded in each problem record. The chapter is organised by investigation theme and includes a prioritised research agenda.

---

### 44.1 How to read this chapter

Each opportunity is stated as a question or investigation, with:
- **Domains** (workstreams involved),
- **Why it matters** (impact on the book's conclusions),
- **Existing evidence** (what is already known),
- **Gap** (what must be found),
- **Priority** (P1 critical, P2 important, P3 nice-to-have).

P1 items are those whose answer would change the ranking in Chapter 41 or the gap classification in Chapter 42. The full 72 cross-domain questions (CQ-001 … CQ-072) are in `CROSS_QUESTIONS.md`; this chapter summarises and prioritises them rather than duplicating all text.

---

### 44.2 Priority 1 investigations (P1) — 18 items

These are the highest-leverage investigations. Their answers would change the book's core conclusions.

| # | Investigation | Domains | Problem IDs | Why it matters |
|---|---|---|---|---|
| 1 | **How much of test scenario incompleteness traces to ambiguous requirements vs. test-generation limits?** | W1, W3, W4 | W1-001, W3-018, W3-020, W4-017 | Decides whether to invest in RE quality or test generation (CQ-001) |
| 2 | **Automotive-specific cost-of-change multiplier (requirement change at HIL/vehicle vs. requirements phase)** | W1, W3, W4 | W1-005, W1-012, W3-031, W4-012 | The 35–45% rework figure is cross-industry; automotive curve missing (CQ-002) |
| 3 | **How much of V&V time is requirements-driven rework vs. test-infrastructure unreliability?** | W1, W3, W4 | W1-012, W3-027, W3-031, W4-011 | Prioritises which bottleneck to attack first (CQ-004) |
| 4 | **Is Adaptive adoption blocked more by spec immaturity, skill gap, or toolchain maturity?** | W2, W5, W4 | W2-003, W2-016, W5-002, W5-024, W4-004, W4-025 | Determines whether to wait for spec stabilisation or invest now (CQ-007) |
| 5 | **What fraction of Classic ECUs are migratable to Adaptive within 10 years, and what binds?** | W2, W5, W4 | W2-016, W2-002, W2-018, W5-002, W5-010, W4-019 | Hybrid reality depends on the answer (CQ-008) |
| 6 | **Does multicore distribution get worse or better under zonal/centralised compute, and can timing analysis close the gap?** | W2, W1, W3 | W2-006, W2-013, W1-010, W1-016, W3-012 | Consolidation is risky if timing is unsolved (CQ-009) |
| 7 | **True end-to-end cost of ARXML-related rework across requirements→release** | W1, W2, W3, W4 | W1-013, W2-001, W2-015, W3-030, W4-002, W4-003, W4-005 | Justifies investment in canonicalisation (CQ-013) |
| 8 | **End-to-end timing verification across MIL→SIL→HIL→vehicle: where does it break?** | W1, W2, W3 | W1-010, W1-018, W2-007, W2-008, W2-012, W3-005, W3-013 | Timing is safety-relevant and unsolved (CQ-017) |
| 9 | **When (if ever) can ML-based safety functions reach ASIL D certification?** | W1, W4, W5, W2 | W1-004, W4-017, W5-019, W5-020, W2-018 | Defines ML safety ceiling (CQ-021) |
| 10 | **What fraction of compliance effort is genuine safety work vs. avoidable overhead, and what is automatable?** | W1, W3, W4, W5 | W1-014, W3-025, W4-008, W4-011, W4-013, W4-014, W4-015, W4-016, W4-029, W5-025 | Core Part VII question; drives automation prioritisation (CQ-026) |
| 11 | **Quantified cost of variant explosion per vehicle program** | W1, W2, W3, W4 | W1-011, W2-011, W3-011, W4-005 | Variant explosion cited everywhere but never quantified (CQ-030) |
| 12 | **Is there a vECU fidelity threshold (per ASIL) at which regulators accept vECU evidence, replacing HIL?** | W3, W4, W5, W2 | W3-004, W3-010, W4-019, W5-003, W2-018 | Determines virtualisation ROI (CQ-034) |
| 13 | **What can be safely automated vs. must remain under human authority — is there industry consensus?** | W5, W4, W1, W2 | W5-025, W4-030, W1-014, W2-001, CQ-043 | Defines automation ceiling; no governance exists today (CQ-043) |
| 14 | **Will SDV/centralised computing increase or decrease total software engineering effort, and over what horizon?** | W1, W2, W5, W4 | W1-016, W2-016, W5-024, W4-019 | Central Part V question; evidence is mixed (CQ-050) |
| 15 | **Are SOTIF unknown-unsafe-scenario and ADAS scenario-coverage problems worsened by ML nondeterminism in a way that requires new safety concepts?** | W4, W3, W5, W1 | W4-017, W3-018, W3-019, W3-029, W5-019, W1-004 | May require abandoning deterministic safety arguments (CQ-051) |
| 16 | **Can the talent-shortage statistics (80% unfilled, 40% demand surge, 25% vacancy, 30% shortfall) be independently corroborated?** | W5, W4, W1 | W5-001, W5-004, W5-005, W5-022, W1-015 | Book must not propagate unverified marketing statistics (CQ-056) |
| 17 | **How should the book weight problems by frequency × severity × cost when most figures are qualitative or single-source?** | All | CQ-071 | Part VII prioritisation requires a defensible ranking method (CQ-071) |
| 18 | **What is the single most significant unsolved problem category — do the five workstreams agree?** | All | CQ-072 | Part VII conclusion 41 asks for the most significant categories (CQ-072) |

---

### 44.3 Priority 2 investigations (P2) — selected 31 items

P2 items are important but their answers refine rather than change the core conclusions. The full list is in CROSS_QUESTIONS.md §15. Selected high-value P2 investigations:

| # | Investigation | Domains | Problem IDs |
|---|---|---|---|
| 19 | Does horizontal traceability across MIL–SIL–HIL fail because of missing req→test links or tool/time-base heterogeneity? | W1, W3, W4 | W1-003, W3-023, W3-005, W4-007 (CQ-003) |
| 20 | Can AI-generated test scenarios be safely grounded in requirements given hallucination and no automotive datasets? | W1, W3, W5, W4 | W1-001, W3-026, W5-017, W5-018, W4-030 (CQ-005) |
| 21 | What fraction of late integration defects trace to incomplete requirements vs. implementation errors, and how does shift-left change it? | W1, W3 | W1-006, W1-002, W3-031 (CQ-006) |
| 22 | Is Adaptive blocked more by spec immaturity or by skill/toolchain gaps? (causal attribution) | W2, W5, W4 | W2-003, W5-002, W5-024, W4-004, W4-025 (CQ-007, dup) |
| 23 | Can hypervisors be certified as SEooC at acceptable cost given safety-engineer scarcity and tool-qualification burden? | W2, W5, W4 | W2-009, W2-018, W5-003, W4-020, W4-011 (CQ-010) |
| 24 | How much of AUTOSAR configuration can be safely automated by AI without invalidating the safety case? | W2, W5, W4 | W2-001, W2-012, W5-017, W5-019, W4-030, W4-011 (CQ-011) |
| 25 | If ARXML serialization were canonicalised, how much glue work and VCS friction would disappear, and would traceability improve? | W2, W4, W1 | W2-015, W4-002, W4-028, W1-013 (CQ-014) |
| 26 | Can variant management be unified across system, AUTOSAR binding-time, and test dimensions? | W1, W2, W3, W4 | W1-011, W2-011, W3-011, W4-005 (CQ-015) |
| 27 | Does PLM↔ARXML integration failure amplify configuration drift and safety-case maintenance? | W4, W1, W2 | W4-024, W1-013, W2-001, W4-012 (CQ-016) |
| 28 | Do TSN+SOME/IP field results meet ADAS timing requirements, or does DDS dominate HPC in practice? | W2, W1, W3 | W2-007, W2-008, W1-018, W3-013 (CQ-018) |
| 29 | Under zonal/centralised compute, does the "thinking vs acting" separation create new timing failure modes not covered by SOTIF? | W1, W4, W2 | W1-016, W4-017, W2-018 (CQ-020) |
| 30 | Does integrating ISO/SAE 21434 into ISO 26262 processes create more value than running them in parallel? | W4, W5, W2 | W4-014, W4-011, W4-015, W5-004, W2-010 (CQ-022) |
| 31 | Can AI-assisted HARA/test-artifact generation be certified as part of a safety case, or is it assistive-only? | W5, W4, W1 | W5-003, W5-017, W4-011, W4-030, W1-001 (CQ-023) |
| 32 | How does cybersecurity talent shortage interact with UNECE R155 CSMS supply-chain burden and OTA security? | W5, W4, W2 | W5-004, W4-015, W2-010 (CQ-024) |
| 33 | Can automated evidence collection reduce compliance documentation from 41% to a manageable level without weakening the safety case? | W3, W4, W1 | W3-025, W4-013, W4-029, W1-014 (CQ-027) |
| 34 | Does AI standards-aware assistance reduce compliance effort, or does verification of AI output consume the savings? | W5, W4, W1 | W5, W4-030, W1-014 (CQ-028) |
| 35 | Can model-based testing handle hybrid discrete-continuous systems well enough to reduce variant test explosion? | W3, W1, W4 | W3-035, W3-017, W4-017 (CQ-031) |
| 36 | Does configuration-time variant binding cause more defects than test-time variant explosion? | W2, W3, W4 | W2-011, W3-011, W4-005 (CQ-032) |
| 37 | What organisational changes enable shift-left in legacy OEMs — cultural, financial, or skill-based? | W3, W5, W4, W1 | W3-031, W5-007, W5-009, W5-013, W4-019, W1-016 (CQ-033) |
| 38 | Does HIL cost/scalability get solved by cloud HIL or by vECUs? | W3, W4, W5 | W3-010, W3-015, W4-018, W4-019 (CQ-035) |
| 39 | Does the lack of a vECU standard reflect technical disagreement or commercial lock-in? | W3, W4, W2 | W3-007, W4-004, W2-005 (CQ-036) |
| 40 | What is the bus factor / single-person expertise risk quantified per critical tool/domain, and cost of a key expert leaving? | W5, W4, W2, W3 | W5-012, W4-022, W2-019, W3-038 (CQ-037) |
| 41 | Can AI knowledge capture reduce onboarding time by the claimed 40% given confidentiality barriers? | W5, W4, W1 | W5-012, W5-013, W5-016, W4-030, W1-015 (CQ-038) |
| 42 | Does compensation disadvantage cause more damage through attrition or hiring gaps, and which is cheaper to fix? | W5, W4, W2 | W5-015, W5-001, W5-012, W4-022, W4-027, W2-019 (CQ-039) |
| 43 | How does the education pipeline mismatch affect Classic vs Adaptive adoption? | W5, W2, W1 | W5-022, W2, W1-009 (CQ-040) |
| 44 | What is the net productivity effect of LLM code generation in automotive including verification cost? | W5, W4, W3, W2 | W5, W4-030, W3-024, W2-001 (CQ-041) |
| 45 | Can RAG-based compliance Q&A be made sufficiently grounded to avoid hallucination? | W5, W4, W1 | W5, W4-030, W1-014 (CQ-042) |
| 46 | Can AI-based requirements ambiguity detection be validated sufficiently for safety-relevant requirements processes? | W1, W5, W4 | W1-001, W5, W4-011 (CQ-045) |
| 47 | Where does engineering ownership get lost between OEM–Tier-1–Tier-2, and can a shared data model fix it? | W1, W4, W5, W2 | W1-006, W4-023, W5-014, W2-005 (CQ-046) |
| 48 | What is the cost/frequency of OEM–supplier data exchange failures, and does ARXML version fragmentation dominate? | W4, W2, W1 | W4-023, W4-002, W2-015, W1-013 (CQ-047) |
| 49 | Can suppliers deliver vECUs in a standard format, and what IP barriers block it? | W3, W4, W5, W2 | W3-007, W4-019, W5-016, W2-020 (CQ-048) |
| 50 | Does UNECE R156/ISO 24089 require OEMs to control supplier ECU state, and is that feasible? | W4, W2, W5 | W4-016, W2-010, W5-014 (CQ-049) |
| 51 | Are SOTIF and ADAS scenario coverage worsened by ML nondeterminism requiring new safety concepts? | W4, W3, W5, W1 | W4-017, W3-018, W3-019, W3-029, W5-019, W1-004 (CQ-051) |
| 52 | What problems will emerge in the next 5–10 years not yet visible? | All | W1-016, W2-003, W3-015, W4-030, W5-021 (CQ-052) |
| 53 | Does "no plans to replace Classic" plus SDV skill gap create a two-tier industry with shrinking Classic talent pool? | W2, W5, W1 | W2, W5-023, W1-015 (CQ-053) |
| 54 | How will AI governance and emerging AI regulations affect tool certification and safety cases? | W5, W4, W1 | W5-025, W4-030, W4-020, W4-011, W1-014 (CQ-054) |
| 55 | Is MBSE adoption low because skills are missing, or are skills missing because MBSE adoption is low? | W1, W5, W4 | W1-009, W5-007, W4-006 (CQ-055) |
| 56 | Can CI/CD for safety-critical embedded include HIL-in-the-loop without losing determinism? | W4, W3, W2 | W4-020, W3-003, W2-006 (CQ-058) |
| 57 | What is the total cost of a vehicle software release including safety re-assessment, compliance evidence, and OTA readiness? | W4, W2, W1, W3 | W4-011, W4-016, W2-010, W1-014, W3-025 (CQ-060) |
| 58 | Can fault injection be automated enough to satisfy ISO 26262 without the manual expertise bottleneck? | W3, W2, W4, W5 | W3-033, W2-006, W2-010, W4-011, W5-009 (CQ-061) |
| 59 | Does 43.6% simulation flakiness in CARLA invalidate simulation-based safety evidence? | W3, W4, W5 | W3-019, W4-017, W5-019 (CQ-062) |
| 60 | What is the optimal level of AUTOSAR compliance ("how much is enough") and its trade-off with speed/cost? | W2, W4, W5, W1 | W2-019, W4-011, W5-002, W1-014 (CQ-066) |
| 61 | Can ARXML intelligence and standards-aware assistants be combined into a single engineering copilot without violating confidentiality and certification? | W2, W5, W4, W1 | W2-001, W5-016, W4-030 (CQ-067) |
| 62 | What is the minimum viable skill set for an engineer in a modern automotive program, and how long to acquire? | W5, W4, W2, W1 | W5-013, W4-022, W2-019, W1-015 (CQ-069) |
| 63 | Does "requirements as code / formal methods" scale to thousands of requirements per vehicle? | W1, W5, W4 | W1-001, W5-006, W4-030 (CQ-070) |

---

### 44.4 Priority 3 investigations (P3) — selected

P3 items are nice-to-have. Full list in CROSS_QUESTIONS.md §15. Selected:

| # | Investigation | Problem IDs |
|---|---|---|
| 64 | Does Classic OS scheduling limitation force hypervisor complexity that drives the SDV skill gap? | W2-013, W2-009, W5-024, W1-016 (CQ-012) |
| 65 | Does PLM↔ARXML integration failure amplify drift and safety-case maintenance? (deeper) | W4-024, W4-012 (CQ-016) |
| 66 | How much of HIL model-fidelity gap is timing/latency vs. omitted physics? | W3-012, W3-013, W3-001, W1-010 (CQ-019) |
| 67 | What is the safety-case maintenance cost of zonal/centralised vs. distributed architecture? | W4-012, W1-016, W2-009, W5-003 (CQ-025) |
| 68 | How do ASPICE assessment gaming and process fatigue interact with genuine quality improvement? | W4-013, W3-032 (CQ-029) |
| 69 | Can digital twins close the model-fidelity gap enough to reduce proving-ground testing? | W3-001, W3-012, W3-017, W4-019 (CQ-063) |
| 70 | How does semiconductor supply chain and chip obsolescence interact with software architecture longevity? | W1 (hardware), W2-016, W5-010, W4-027 (CQ-064) |
| 71 | Are open-source AUTOSAR tools viable for safety-critical production, and what would it take? | W4-025, W4-011, W4-020, W2-005, W5-002 (CQ-065) |
| 72 | How does organisational structure affect feasibility of end-to-end traceability and knowledge capture? | W4-007, W5-012, W1-002, W5-014, W2-005 (CQ-068) |

---

### 44.5 Hidden problems and coverage gaps (investigation opportunities)

CROSS_CHECK §3 identifies topics not covered or only superficially covered. Each is an investigation opportunity:

| # | Hidden/under-covered topic | Why it matters | Suggested investigation |
|---|---|---|---|
| H1 | **Hardware/semiconductor lifecycle and supply chain** | Chip shortage, silicon revision management, foundry dependencies treated as software problems | Deepen G1 with primary OEM/supplier interviews; quantify obsolescence cost per program |
| H2 | **Cloud/backend and connected-services software** | Telematics, V2X, backend services, fleet learning, data pipelines essentially absent beyond G3 | Expand G3 with backend architecture, data pipeline, and cloud cost studies |
| H3 | **Energy, power, and thermal software** | Only mentioned in W1-016; no dedicated problem record beyond G4 | Deepen G4 with thermal-aware scheduling, power management software, energy budgets |
| H4 | **Open-source software governance and licensing in production ECUs** | W4-025 covers tooling only; OSS component use, license compliance, SBOM not covered | Investigate OSS governance, SBOM management, license compliance in automotive |
| H5 | **Software architecture quality, modularity, technical-debt metrics** | No workstream records modularity, coupling, or technical-debt measurement | Define and measure architecture-quality KPIs; investigate technical-debt quantification |
| H6 | **Developer experience / ergonomics of engineering tools** | Only W4-006 (requirements tool UX); not generalised | Investigate DX of AUTOSAR config, ARXML editors, HIL tooling |
| H7 | **Cost/benefit and ROI quantification** | Every workstream marks cost impact "unquantified"; no measurement framework | Build a cost-of-quality measurement framework for automotive software |
| H8 | **Multi-language and globalization of software** | Absent | Investigate localization, internationalization in automotive software |
| H9 | **Stakeholder and supply-chain ownership mapping** | Partial in DOMAIN_MAP; no deep analysis | Map OEM–Tier-1–Tier-2 accountability and engineering ownership loss (CQ-046) |
| H10 | **Contradiction resolution** | MBSE adoption (66% reliance vs. 22% definitional clarity); Adaptive vs. zonal tension; testing cost share (50% vs. 35–45%); shift-left progress; HIL vs. SiL; talent statistics | Reconcile quantitative anchors and staleness (CROSS_CHECK §5.2) |

---

### 44.6 Emerging future risks (Task.md §12)

Problems that will become more serious over the next 5–10 years (CQ-052). Each needs investigation:

| Risk | Evidence | Investigation needed |
|---|---|---|
| SDV/centralised compute increases engineering effort before decreasing it | W1-016, W2-016, W5-024 | Longitudinal study of SDV program effort vs. legacy (CQ-050) |
| ADAS/ADS scenario completeness worsens with ODD expansion and ML nondeterminism | W3-018, W3-019, W3-020, W4-017, W5-019 | Integrated analysis of SOTIF + ML nondeterminism (CQ-051) |
| Semiconductor obsolescence accelerates | G1-007, G1-008, G1-009, G1-003 | Obsolescence cost per program; effectiveness of LTSAs |
| AI adoption without governance risks quality erosion and audit failures | W5-025, W4-030, W5-017, W5-019, W5-020 | AI governance framework; certification precedents |
| Skills obsolescence: Classic expertise ages while SDV skills scarce | W5-021, W5-023, W2 cross-cutting, CQ-053 | Demographic analysis by platform skill |
| Cloud/backend scale challenges as fleets grow | G3-003, G3-007, G3-008, G3-009, G3-028 | Cloud cost and architecture studies at fleet scale |
| Energy/thermal complexity grows with fast charging, V2G, thermal runaway safety | G4-004, G4-006, G4-017, G4-030 | Thermal-aware scheduling and containment software |
| Memory wall worsens as AI inference moves into vehicles | G1-017, G1-018, G1-019 | Memory hierarchy optimisation for zonal compute |
| Two-tier industry: Classic experts vs. SDV experts with shrinking Classic pool | W2, W5-023, CQ-053 | Workforce planning by platform skill |

---

### 44.7 Unanswered questions from the problem records

Every problem record in W1–W5 and G1–G4 ends with an "Unanswered questions" field. These are not duplicated here; instead, themes are grouped. The most frequently recurring unanswered themes:

1. **Quantification of cost/time impact** (almost every record) — no measurement exists for most problems.
2. **Effectiveness of existing solutions** — no pilot data or comparative studies.
3. **Adoption rates** of MBSE, vECU, shift-left, cloud HIL, open-source AUTOSAR tools.
4. **Causal attribution** — which of several candidate causes dominates (e.g., CQ-001, CQ-006, CQ-007).
5. **Regulator positions** on vECU fidelity, ML safety, AI in engineering.
6. **Organisational and supply-chain** feasibility of shared data models and ownership.
7. **Longitudinal and demographic** data on skills and workforce.

These themes map directly to P1/P2/P3 investigations above.

---

### 44.8 Research limitations of this book

- No primary interviews or OEM-internal data were used (DATA_VERIFY, CROSS_QUESTIONS §16).
- Workforce statistics are single-source commercial reports, flagged unverified (DATA_VERIFY §3.1).
- Several quantitative anchors are stale (NHTSA 2020 OTA) or unverified (43.6% CARLA flakiness, 80% integration issues, KPIT 2025, HCLTech 2025).
- Counts of problem records (258) are not comparable across workstreams due to granularity differences.
- No ROI or effectiveness quantification exists for any solution category.
- Priority ratings (P1/P2/P3) are the subagent's judgement, not industry-validated.
- Answering P1 questions will require new empirical research, not just literature review.

---

### 44.9 Cross-references

- **Chapter 41 (this Part):** problem categories.
- **Chapter 42 (this Part):** insufficient solutions.
- **Chapter 43 (this Part):** technology and process gaps.
- **Chapter 45 (this Part):** open research questions (consolidated list).
- **CROSS_QUESTIONS.md:** full 72 cross-domain questions with fields.
- **CROSS_CHECK.md:** contradictions, overlaps, gaps, inconsistencies.
- **DATA_VERIFY.md:** evidence verification and staleness flags.
- **TASK_COVERAGE.md:** coverage checklist against Task.md.

---

### Problem IDs referenced in this chapter

W1-001, W1-002, W1-003, W1-004, W1-005, W1-006, W1-009, W1-010, W1-012, W1-013, W1-014, W1-015, W1-016, W1-018; W2-001, W2-002, W2-003, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-013, W2-015, W2-016, W2-018, W2-019, W2-020; W3-001, W3-003, W3-004, W3-005, W3-007, W3-010, W3-011, W3-012, W3-013, W3-015, W3-017, W3-018, W3-019, W3-020, W3-023, W3-024, W3-025, W3-026, W3-027, W3-029, W3-030, W3-031, W3-033, W3-035, W3-036, W3-037, W3-038; W4-002, W4-004, W4-005, W4-006, W4-007, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-017, W4-018, W4-019, W4-020, W4-023, W4-025, W4-027, W4-028, W4-029, W4-030; W5-001, W5-002, W5-003, W5-004, W5-005, W5-006, W5-007, W5-009, W5-012, W5-013, W5-014, W5-016, W5-017, W5-018, W5-019, W5-020, W5-021, W5-022, W5-023, W5-024, W5-025; G1-003, G1-007, G1-008, G1-009, G1-010, G1-011, G1-013, G1-016, G1-017, G1-018, G1-019; G2-001, G2-031; G3-001 … G3-028; G4-004, G4-006, G4-017, G4-030; CQ-001 … CQ-072.

---

### 44.10 Sources

- `research/CROSS_QUESTIONS.md` (72 questions, P1/P2/P3).
- `research/CROSS_CHECK.md` (§3 gaps, §5 recommendations, §4 inconsistencies).
- `research/DATA_VERIFY.md`.
- `research/TASK_COVERAGE.md`.
- `research/KNOWLEDGE_INDEX.md`.
- `Task.md` §8, §12, §13, §16.

---

*End of Chapter 44.*