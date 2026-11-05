# Part VII — Research Conclusions

## Chapter 45 — Open Research Questions

### Summary

This chapter consolidates the open research questions accumulated across the entire research effort: the "Unanswered questions" fields of 258 problem records (W1–W5, G1–G4), the 72 cross-domain questions (CQ-001 … CQ-072) in `CROSS_QUESTIONS.md`, and the contradictions, gaps, and inconsistencies flagged in `CROSS_CHECK.md`. It is the final chapter of the book and its purpose is to make explicit what is **not known**, so that no reader (or future decision-maker) mistakes the research record for a complete answer. The questions are grouped by theme; a full index of all 72 cross-domain questions is provided in §45.3.

---

### 45.1 The most critical open questions (top 12)

These are the questions whose answers would most change the conclusions of this book. They are drawn from the P1 set (CROSS_QUESTIONS §15) and from contradictions that must be resolved before the book's claims can be considered consistent.

1. **What is the single most significant unsolved problem category, and do the workstreams agree?** (CQ-072) — Part VII's central conclusion depends on this.
2. **How should problems be weighted by frequency × severity × cost when most figures are qualitative or single-source?** (CQ-071) — without a defensible ranking method, prioritisation is judgement.
3. **What fraction of compliance effort is genuine safety work vs. avoidable overhead, and what is automatable?** (CQ-026) — the boundary is "organisation-specific and poorly measured" (W1-014).
4. **Can the talent-shortage statistics be independently corroborated?** (CQ-056) — the book must not propagate unverified marketing statistics.
5. **When (if ever) can ML-based safety functions reach ASIL D certification?** (CQ-021) — defines the ML safety ceiling.
6. **Is there a vECU fidelity threshold (per ASIL) at which regulators accept vECU evidence?** (CQ-034) — determines virtualisation ROI.
7. **What can be safely automated vs. must remain under human authority — is there industry consensus?** (CQ-043) — no governance exists today.
8. **Will SDV/centralised computing increase or decrease total software engineering effort?** (CQ-050) — central Part V question; evidence is mixed.
9. **Are SOTIF unknown-unsafe-scenario problems worsened by ML nondeterminism requiring new safety concepts?** (CQ-051) — may require abandoning deterministic safety arguments.
10. **How much of test scenario incompleteness traces to ambiguous requirements vs. test-generation limits?** (CQ-001) — decides investment priority.
11. **What is the automotive-specific cost-of-change multiplier?** (CQ-002) — the 35–45% rework figure is cross-industry.
12. **What is the true end-to-end cost of ARXML-related rework?** (CQ-013) — the single most frequently recorded problem family lacks a number.

---

### 45.2 Contradictions that must be resolved

CROSS_CHECK §1 lists seven direct contradictions. Each is an open research question:

| # | Contradiction | Sources | Reconciliation needed |
|---|---|---|---|
| C1 | MBSE: 66% reliance vs. 22% definitional clarity | W1-009 | Define "reliance" (nominal vs. genuine MBSE) (CQ-055) |
| C2 | Adaptive "stalled" (W2) vs. zonal/centralised "active transition" (W1, W5) | W2 cross-cutting, W1-016, W5-024 | State which zonal platforms are Adaptive vs. non-AUTOSAR (CQ-007, CQ-008) |
| C3 | 45% of roles automatable (W5-021) vs. AI not reliable for safety-critical code (W5-017, W5-019) | W5 | State whether the 45% excludes safety-critical roles (CQ-043) |
| C4 | Tesla-only safety-critical OTA (NHTSA 2020) vs. broad OTA compliance urgency (W4-016) | W2-010, W4-016 | Refresh the 2020 anchor or remove it (C4) |
| C5 | 35–45% requirements-driven rework (W1-012) vs. 50% testing cost (W3) | W1-012, W3 | Reconcile denominators (rework cost vs. dev cost) (CQ-004) |
| C6 | MBSE "blocked cure" vs. 66% reliance | W1-009 | Same as C1 |
| C7 | 80% integration issues found 6 months pre-SOP (W3-031) vs. "late defect discovery" (W1) | W3-031, W1-012 | Define "late" relative to fix cost, not schedule |

---

### 45.3 Full index of cross-domain questions (CQ-001 … CQ-072)

All 72 cross-domain questions from `CROSS_QUESTIONS.md`. Priority: P1 (critical), P2 (important), P3 (nice-to-have). Full text in `CROSS_QUESTIONS.md`.

| ID | Question (short) | Priority |
|---|---|---|
| CQ-001 | How much of test scenario incompleteness traces to ambiguous requirements vs. test-generation limits? | P1 |
| CQ-002 | Automotive-specific cost-of-change multiplier for late requirement changes? | P1 |
| CQ-003 | Does horizontal XiL traceability fail due to missing req→test links or tool/time-base heterogeneity? | P2 |
| CQ-004 | How much of the 47% V&V time is requirements-driven rework vs. test-infra unreliability? | P1 |
| CQ-005 | Can AI-generated test scenarios be safely grounded in requirements? | P2 |
| CQ-006 | What fraction of late integration defects trace to incomplete requirements vs. implementation errors, and how does shift-left change it? | P2 |
| CQ-007 | Is Adaptive adoption blocked more by spec immaturity, skill gap, or toolchain maturity? | P1 |
| CQ-008 | What fraction of Classic ECUs are migratable to Adaptive within 10 years, and what binds? | P1 |
| CQ-009 | Does multicore distribution get worse or better under zonal/centralised compute, and can timing analysis close the gap? | P1 |
| CQ-010 | Can hypervisors be certified as SEooC at acceptable cost? | P2 |
| CQ-011 | How much AUTOSAR configuration can be safely automated by AI without invalidating the safety case? | P2 |
| CQ-012 | Does Classic OS scheduling limitation force hypervisor complexity that drives the SDV skill gap? | P3 |
| CQ-013 | True end-to-end cost of ARXML-related rework? | P1 |
| CQ-014 | If ARXML serialization were canonicalised, how much glue work and VCS friction would disappear? | P2 |
| CQ-015 | Can variant management be unified across system, AUTOSAR binding-time, and test dimensions? | P2 |
| CQ-016 | Does PLM↔ARXML integration failure amplify configuration drift and safety-case maintenance? | P3 |
| CQ-017 | End-to-end timing verification across MIL→SIL→HIL→vehicle: where does it break? | P1 |
| CQ-018 | Do TSN+SOME/IP field results meet ADAS timing requirements, or does DDS dominate HPC? | P2 |
| CQ-019 | How much of HIL model-fidelity gap is timing/latency vs. omitted physics? | P3 |
| CQ-020 | Under zonal/centralised compute, does "thinking vs acting" separation create new timing failure modes not covered by SOTIF? | P2 |
| CQ-021 | When (if ever) can ML-based safety functions reach ASIL D certification? | P1 |
| CQ-022 | Does integrating ISO/SAE 21434 into ISO 26262 processes create more value than running them in parallel? | P2 |
| CQ-023 | Can AI-assisted HARA/test-artifact generation be certified, or is it assistive-only? | P2 |
| CQ-024 | How does cybersecurity talent shortage interact with UNECE R155 CSMS burden and OTA security? | P2 |
| CQ-025 | What is the safety-case maintenance cost of zonal/centralised vs. distributed architecture? | P3 |
| CQ-026 | What fraction of compliance effort is genuine safety work vs. avoidable overhead, and what is automatable? | P1 |
| CQ-027 | Can automated evidence collection reduce compliance documentation from 41% to a manageable level? | P2 |
| CQ-028 | Does AI standards-aware assistance reduce compliance effort, or does verification consume the savings? | P2 |
| CQ-029 | How do ASPICE assessment gaming and process fatigue interact with genuine quality improvement? | P3 |
| CQ-030 | Quantified cost of variant explosion per vehicle program? | P1 |
| CQ-031 | Can model-based testing handle hybrid discrete-continuous systems to reduce variant test explosion? | P3 |
| CQ-032 | Does configuration-time variant binding cause more defects than test-time variant explosion? | P3 |
| CQ-033 | What organisational changes enable shift-left in legacy OEMs? | P2 |
| CQ-034 | Is there a vECU fidelity threshold (per ASIL) at which regulators accept vECU evidence? | P1 |
| CQ-035 | Does HIL cost/scalability get solved by cloud HIL or by vECUs? | P2 |
| CQ-036 | Does the lack of a vECU standard reflect technical disagreement or commercial lock-in? | P3 |
| CQ-037 | What is the bus factor / single-person expertise risk per critical tool/domain? | P2 |
| CQ-038 | Can AI knowledge capture reduce onboarding time by the claimed 40%? | P3 |
| CQ-039 | Does compensation disadvantage cause more damage through attrition or hiring gaps? | P3 |
| CQ-040 | How does education pipeline mismatch affect Classic vs Adaptive adoption? | P3 |
| CQ-041 | Net productivity effect of LLM code generation in automotive? | P2 |
| CQ-042 | Can RAG-based compliance Q&A be made sufficiently grounded? | P2 |
| CQ-043 | What can be safely automated vs. must remain under human authority — is there industry consensus? | P1 |
| CQ-044 | Does on-prem LLM deployment solve the confidentiality barrier at acceptable cost? | P3 |
| CQ-045 | Can AI-based requirements ambiguity detection be validated for safety-relevant processes? | P2 |
| CQ-046 | Where does engineering ownership get lost between OEM–Tier-1–Tier-2? | P2 |
| CQ-047 | What is the cost/frequency of OEM–supplier data exchange failures? | P3 |
| CQ-048 | Can suppliers deliver vECUs in a standard format, and what IP barriers block it? | P2 |
| CQ-049 | Does UNECE R156/ISO 24089 require OEMs to control supplier ECU state? | P3 |
| CQ-050 | Will SDV/centralised computing increase or decrease total software engineering effort? | P1 |
| CQ-051 | Are SOTIF and ADAS scenario coverage worsened by ML nondeterminism requiring new safety concepts? | P1 |
| CQ-052 | What problems will emerge in the next 5–10 years not yet visible? | P2 |
| CQ-053 | Does "no plans to replace Classic" plus SDV skill gap create a two-tier industry? | P3 |
| CQ-054 | How will AI governance and emerging AI regulations affect tool certification and safety cases? | P3 |
| CQ-055 | Is MBSE adoption low because skills are missing, or are skills missing because MBSE adoption is low? | P2 |
| CQ-056 | Can the talent-shortage statistics be independently corroborated? | P1 |
| CQ-057 | What is the actual defect-reproduction time in automotive? | P2 |
| CQ-058 | Can CI/CD for safety-critical embedded include HIL-in-the-loop without losing determinism? | P2 |
| CQ-059 | How do environmental/EMC/durability physical tests interact with SOTIF scenario-based testing? | P3 |
| CQ-060 | What is the total cost of a vehicle software release? | P2 |
| CQ-061 | Can fault injection be automated enough to satisfy ISO 26262? | P2 |
| CQ-062 | Does 43.6% simulation flakiness in CARLA invalidate simulation-based safety evidence? | P2 |
| CQ-063 | Can digital twins close the model-fidelity gap enough to reduce proving-ground testing? | P3 |
| CQ-064 | How does the semiconductor supply chain and chip obsolescence interact with software architecture longevity? | P3 |
| CQ-065 | Are open-source AUTOSAR tools viable for safety-critical production? | P3 |
| CQ-066 | What is the optimal level of AUTOSAR compliance ("how much is enough")? | P2 |
| CQ-067 | Can ARXML intelligence and standards-aware assistants be combined into a single engineering copilot? | P3 |
| CQ-068 | How does organisational structure affect feasibility of end-to-end traceability and knowledge capture? | P3 |
| CQ-069 | What is the minimum viable skill set for an engineer in a modern automotive program? | P2 |
| CQ-070 | Does "requirements as code / formal methods" scale to thousands of requirements per vehicle? | P3 |
| CQ-071 | How should the book weight problems by frequency × severity × cost? | P1 |
| CQ-072 | What is the single most significant unsolved problem category? | P1 |

---

### 45.4 Open questions by theme (from problem records)

Beyond the 72 cross-domain questions, every problem record ends with "Unanswered questions." These are grouped by theme rather than listed individually (258 records). The most recurring themes:

**A. Quantification (appears in nearly every record)**
- What is the cost/time impact of this problem per program? (unquantified in all workstreams)
- What is the frequency/severity distribution?
- What are the defect escape rates and rework percentages?
- What is the ROI of proposed improvements?

**B. Adoption and effectiveness**
- What is the adoption rate of MBSE, vECU, shift-left, cloud HIL, open-source AUTOSAR tools?
- Are existing solutions effective in practice (pilot data)?
- What is the optimal SiL/HIL split per ASIL and system type?

**C. Causal attribution**
- Which candidate cause dominates (e.g., requirement ambiguity vs. test-generation limits)?
- Is the barrier cultural, financial, or skill-based?

**D. Regulator positions**
- Will regulators accept vECU evidence, and at what fidelity threshold?
- When (if ever) will ML safety functions reach ASIL C/D?
- What is the certification path for AI tools in engineering?

**E. Organisational and supply-chain feasibility**
- Can a shared engineering data model fix OEM–Tier-1–Tier-2 ownership loss?
- Is OEM control of supplier ECU state feasible under UNECE R156?
- Can suppliers deliver vECUs in a standard format given IP constraints?

**F. Longitudinal and demographic**
- Will SDV/centralised compute increase or decrease total engineering effort?
- What is the demographic outlook for Classic vs. SDV skills?
- What problems will emerge in 5–10 years?

**G. Evidence and methodology**
- Can single-source statistics be corroborated?
- Are stale anchors (NHTSA 2020 OTA) still valid?
- How should qualitative findings be ranked?

---

### 45.5 What would be needed to answer these questions

Answering the P1 set (18 questions) requires **new empirical research**, not further literature review:

- Automotive-specific cost-of-change and cost-of-quality studies (CQ-002, CQ-004, CQ-013, CQ-030, CQ-060).
- Regulator positions and standards development (CQ-021, CQ-034, CQ-043, CQ-051).
- Causal attribution studies (CQ-001, CQ-006, CQ-007, CQ-009).
- Independent corroboration of workforce statistics (CQ-056).
- Longitudinal studies of SDV program effort (CQ-050).
- Industry consensus processes on automation boundaries (CQ-043).

The P2/P3 sets (54 questions) can be partially addressed by targeted literature review, surveys, and case studies, but many also require primary data.

---

### 45.6 Limitations of this chapter

- The 72 cross-domain questions and their priorities are the subagent's judgement, not industry-validated (CROSS_QUESTIONS §16).
- The problem-record "Unanswered questions" are qualitative prompts, not structured research proposals.
- No primary data was collected; all questions derive from published evidence.
- Priority ratings may not reflect industry consensus.
- Answering P1 questions is beyond the scope of this research effort and requires dedicated empirical programs.

---

### 45.7 Cross-references

- **Chapter 41 (this Part):** problem categories.
- **Chapter 42 (this Part):** insufficient solutions.
- **Chapter 43 (this Part):** technology and process gaps.
- **Chapter 44 (this Part):** investigation opportunities.
- **CROSS_QUESTIONS.md:** full 72 questions with domains, evidence, gaps, and priorities.
- **CROSS_CHECK.md:** contradictions, overlaps, gaps, inconsistencies.
- **DATA_VERIFY.md:** evidence verification and staleness.
- **TASK_COVERAGE.md:** coverage checklist.
- **KNOWLEDGE_INDEX.md:** problem catalogue index.

---

### Problem IDs referenced in this chapter

All problem IDs in the corpus are potentially relevant; the most frequently cited in this chapter: W1-001, W1-002, W1-003, W1-004, W1-005, W1-006, W1-009, W1-010, W1-012, W1-013, W1-014, W1-015, W1-016, W1-018; W2-001, W2-002, W2-003, W2-004, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-013, W2-015, W2-016, W2-018, W2-019, W2-020; W3-001, W3-003, W3-004, W3-005, W3-007, W3-010, W3-011, W3-012, W3-013, W3-017, W3-018, W3-019, W3-020, W3-023, W3-025, W3-026, W3-027, W3-029, W3-030, W3-031, W3-033, W3-035, W3-036, W3-037, W3-038; W4-002, W4-004, W4-005, W4-006, W4-007, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-017, W4-018, W4-019, W4-020, W4-023, W4-025, W4-027, W4-029, W4-030; W5-001, W5-002, W5-003, W5-004, W5-005, W5-006, W5-007, W5-009, W5-012, W5-013, W5-014, W5-016, W5-017, W5-018, W5-019, W5-020, W5-021, W5-022, W5-023, W5-024, W5-025; G1-003, G1-007, G1-008, G1-009, G1-013, G1-016, G1-017, G1-018, G1-019; G2-001, G2-031; G3-003, G3-007, G3-008, G3-009, G3-028; G4-004, G4-006, G4-017, G4-030; CQ-001 … CQ-072.

---

### 45.8 Sources

- `research/CROSS_QUESTIONS.md` (72 questions).
- `research/CROSS_CHECK.md` (contradictions, overlaps, gaps, inconsistencies).
- `research/DATA_VERIFY.md`.
- `research/KNOWLEDGE_INDEX.md`.
- `research/TASK_COVERAGE.md`.
- `research/W1_requirements_system.md` … `research/W5_skills_ai.md`.
- `research/G1_semiconductor_hardware.md` … `research/G4_energy_thermal.md`.
- `Task.md` §8, §11, §12, §13, §16, §17.

---

*End of Chapter 45. End of Part VII. End of book.*

---

### Book completion note

With Chapters 41–45, Part VII (Research Conclusions) is complete. The full book structure (Task.md §14) now has Parts I–VII drafted in `research/book/`. Part VI (Chapters 35–40, consolidated problem discovery) remains to be written by a subsequent pass; Part II Chapters 10–13 (lifecycle deep-dives) are also pending. The problem IDs, cross-references, and evidence limitations in this Part are consistent with the research corpus and with `KNOWLEDGE_INDEX.md`, `CROSS_CHECK.md`, `CROSS_QUESTIONS.md`, and `DATA_VERIFY.md`.

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

