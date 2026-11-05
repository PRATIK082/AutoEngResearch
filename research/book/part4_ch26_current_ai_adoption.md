# Part IV — Existing Industry Solutions

## Chapter 26 — Current AI Adoption in Automotive Engineering

### Summary

AI adoption in automotive software engineering is **real but narrow**: it is delivering value in assistive, low-risk, non-safety-critical tasks (code generation for simulation/config scripts, test scenario generation, requirements summarisation/extraction, RAG-based compliance Q&A, code analysis/optimisation, documentation) and is **experimental or blocked** in everything that touches safety evidence, certification, or IP-protected requirements (W5, S9). The boundary between "delivering value" and "not yet reliable" is defined by four barriers: **hallucination** (W5-017), **data confidentiality/IP** (W5-016), **absence of automotive datasets** (W5-018), and the **probabilistic-vs-deterministic safety gap** (W5-019). A small industry survey (n=9, S9) found GenAI used for code generation by most respondents but for requirements handling by only one, precisely because requirements are IP-protected. No AI tool is certified for safety-critical engineering use today (W4-030). Governance for AI use in engineering is absent (W5-025). This chapter catalogues where AI is and is not delivering value, the tools and techniques in use, and the barriers that define its current ceiling.

---

### 26.1 Purpose and scope

This chapter surveys current AI adoption in automotive engineering: LLM-based assistants, RAG over engineering data, AI code generation, AI test generation, AI requirements handling, AI static analysis, multi-agent approaches, and vendor AI features. It distinguishes "delivering value now", "experimental", and "not yet reliable" per W5/S9, and interlinks the problem IDs that define AI's current limits.

### 26.2 Evidence base and its limitations

The primary evidence source is an arXiv survey of GenAI for automotive software development (Petrovic et al., arXiv:2507.15025, GACLM 2025) comprising a literature review plus a small industry survey (n=9). Findings are indicative, not representative (W5 §7). Additional sources: W4-030, W5-016–W5-025. No primary OEM interviews were conducted. Treat adoption claims as directional.

### 26.3 Where AI is delivering value now

Per W5/S9, AI is delivering value in:

1. **Code generation for simulation and configuration scripts** (GPT-4 dominant). Used for non-safety-critical glue code, test scripts, and simulation harnesses.
2. **Test scenario generation** (LLM/RAG). Accelerates scenario authoring for SiL/HIL and ADS testing.
3. **Requirements summarisation and extraction.** Condensing large requirement sets; extracting structured information from natural-language specs.
4. **RAG-based compliance Q&A.** Answering standards/regulation questions grounded in retrieved documents.
5. **Code analysis and optimisation.** Refactoring suggestions, complexity reduction, documentation.
6. **Documentation generation.** Release notes, API docs, test reports.

These are all **assistive** uses in which a human engineer reviews and approves output, and in which errors are caught by existing verification (compilation, static analysis, test execution) rather than by safety argumentation.

### 26.4 Where AI is experimental

Per W5/S9:

1. **Requirements handling with local fine-tuned models.** Blocked by data confidentiality/IP (W5-016) and lack of datasets (W5-018); small models underperform.
2. **MISRA-compliant embedded C generation.** LLMs can generate C that compiles and passes naive review but may encode wrong safety logic; requires full verification (W5-017).
3. **Multi-agent debate for hallucination reduction.** Mitigates but does not eliminate hallucination (W5-017).
4. **VLM (vision-language model) diagram extraction.** Extracting information from engineering diagrams; accuracy variable.
5. **AI-assisted HARA/test-artifact generation.** Reported (AEGIS) but certification unresolved (W5-003, CQ-023).
6. **AI-assisted traceability link suggestion.** Emerging; requires human approval (W4-007, W4-030).

### 26.5 Where AI is not yet reliable

1. **Safety-critical code without verification.** Probabilistic generation cannot be trusted for safety logic (W5-019).
2. **Certification evidence generation.** No certified AI tooling; assessors cannot accept AI-generated safety evidence (W4-030, W5-019).
3. **ML-based safety functions under ISO 26262.** Only ASIL A/B covered in literature; ASIL C/D unresolved (W5-019, CQ-021).
4. **Architecture decisions.** Require human judgement and accountability (W5-007, W5-020).
5. **Cybersecurity architecture.** Human authority required (W5-004).
6. **Undecidable MISRA rule decisions.** Human judgement required (W4-010).

### 26.6 Techniques and tools in use

- **LLMs (GPT-4 dominant)** for code generation, summarisation, Q&A (S9).
- **RAG (retrieval-augmented generation)** over internal engineering documents and standards for grounded Q&A.
- **Local fine-tuned models (Llama 3)** where data cannot leave premises (W5-016).
- **Multi-agent debate / self-consistency** for hallucination reduction (W5-017).
- **Engineering knowledge graphs** for structured retrieval (W5-012).
- **Vendor AI features** in commercial tools: Polarion Copilot, Codebeamer AI, Trace.Space, and similar ALM AI layers (W4-030, W4-006).
- **On-prem LLM deployment** for IP-sensitive work (W5-016, CQ-044).

### 26.7 The four barriers that define AI's current ceiling

#### 26.7.1 Hallucination (W5-017)

LLMs are statistical next-token predictors, not symbolic reasoners. They hallucinate in believable ways. Direct unverified use in automotive engineering is "almost impossible"; human expert in the loop is required (S9). Mitigations (RAG grounding, multi-agent debate, self-consistency, uncertainty evaluation, formal methods) reduce but do not eliminate hallucination; certification is not established. Hallucination rates on automotive tasks are unknown (unanswered).

#### 26.7.2 Data confidentiality and IP (W5-016)

Requirements are high-value IP protected by NDAs; exposing them to third-party cloud LLMs is unacceptable. Locally deployable fine-tuned models are required, which demand datasets, hardware, and time (S9). Only 1 of 9 surveyed organisations used GenAI for requirements handling, precisely because of this barrier. This explains the asymmetry: code generation (where cloud LLMs are more acceptable) advances faster than requirements handling (where data cannot leave premises).

#### 26.7.3 Absence of automotive datasets (W5-018)

The main bottleneck for local LLM adoption is the lack of publicly available specialised automotive datasets for fine-tuning, due to IP/confidentiality (S9). No industry data commons exists. Synthetic data and RAG over internal docs are partial mitigations; quality gaps remain.

#### 26.7.4 Probabilistic vs. deterministic safety (W5-019)

ISO 26262 is deterministic and evidence-based; ML is probabilistic and data-driven. The standard has gaps for the ML lifecycle (ASIL A/B focus in literature; ASIL C/D unresolved). Certification of AI-assisted or AI-implemented safety functions is unresolved. SOTIF (ISO 21448) partially addresses unknown unsafe scenarios but does not close the gap (W4-017, CQ-021).

### 26.8 Additional AI barriers

- **Explainability and accountability** (W5-020): AI outputs lack provenance; confidence is poorly calibrated; standards require traceable evidence; no uncertainty quantification in most tools.
- **AI governance absence** (W5-025): no established policy on which tasks may use AI, required verification, approval gates, IP handling, audit trails; adoption outpaces process definition.
- **Trust and certification** (W4-030): no safety-certified AI assistants; on-prem options limited; standards silent on AI in engineering.
- **Skill gaps** (W5-005): few engineers bridge ML and functional safety; ML education focuses on generic data science.
- **Cost** (W4-027, CQ-044): on-prem LLM deployment cost and quality gap vs. cloud models are unquantified.

### 26.9 The asymmetry of AI adoption

AI adoption is asymmetric by design: it advances fastest where (a) data can leave premises or is non-sensitive, (b) errors are caught by deterministic verification (compilation, static analysis, tests), and (c) safety evidence is not required. It stalls where (a) data is IP-protected, (b) errors are semantic and not caught by existing verification, and (c) safety certification is required. This asymmetry is not a temporary state; it reflects structural properties of LLMs and of the safety standards. The boundary is likely to move only as (i) on-prem models improve, (ii) standards address AI in engineering, and (iii) certification precedents accumulate (CQ-043, CQ-054).

### 26.10 Vendor and tool landscape

- **ALM AI features:** Polarion Copilot, Codebeamer AI, Trace.Space, and similar (W4-006, W4-030).
- **Code assistants:** GitHub Copilot, Cursor, and similar used for non-safety glue code and scripts (W4-030).
- **RAG/compliance Q&A prototypes** over standards and internal docs (W5-006, W5-016).
- **On-prem LLM deployments** (Llama 3 fine-tunes) for IP-sensitive work (W5-016, W5-018).
- **Multi-agent and knowledge-graph research prototypes** (W5-012, W5-017).

None of these is certified for safety-critical engineering use (W4-030).

### 26.11 Net-effect question (unresolved)

Whether AI reduces net engineering effort once verification cost is included is unresolved (CQ-041). AI can generate code in seconds, but verifying that code against safety requirements, MISRA rules, and system intent may consume the savings. No net-effect study exists. Similarly, whether RAG-based compliance Q&A can be made sufficiently grounded to avoid hallucination given IP and data barriers is unresolved (CQ-042).

### 26.12 Problem ID index for this chapter

W1-001, W1-004; W3-024, W3-026, W3-029; W4-006, W4-007, W4-008, W4-010, W4-011, W4-017, W4-030; W5-003, W5-005, W5-006, W5-007, W5-009, W5-016, W5-017, W5-018, W5-019, W5-020, W5-025; CQ-005, CQ-011, CQ-023, CQ-028, CQ-041, CQ-042, CQ-043, CQ-044, CQ-045, CQ-054.

### 26.13 Sources

- research/W5_skills_ai.md (§2 W5-016..W5-020, W5-025; §4 AI applicability summary; S9 survey)
- research/W4_toolchain_standards.md (W4-006, W4-007, W4-008, W4-010, W4-011, W4-017, W4-030)
- research/W1_requirements_system.md (W1-001, W1-004)
- research/W3_testing_validation.md (W3-024, W3-026, W3-029)
- Task.md §7 (AI, Automation and Future Engineering)
- CROSS_QUESTIONS.md (CQ-005, CQ-011, CQ-023, CQ-028, CQ-041, CQ-042, CQ-043, CQ-044, CQ-045, CQ-054)
- Petrovic et al., "Survey of GenAI for Automotive Software Development" (arXiv:2507.15025, 2025)

---

*End of Chapter 26.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

