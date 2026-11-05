# Part V — Future Automotive Engineering

## Chapter 30 — AI-Native Automotive Engineering

### Summary

"AI-native" automotive engineering means AI is not an add-on feature but a foundational capability across the engineering lifecycle and the vehicle itself: AI-assisted requirements analysis, code generation, test generation, compliance checking, and in-vehicle perception, prediction, and control. The research record shows AI is already delivering measurable value in narrow, low-risk tasks (code generation for simulation/config scripts, test scenario generation, requirements summarisation, RAG-based compliance Q&A, documentation) but is blocked from safety-critical use by confidentiality/IP constraints, hallucination, lack of automotive datasets, certification gaps, and absence of AI governance (W5-016–W5-020, W5-025, W4-030). AI-native engineering is therefore a research and governance problem as much as a technology problem.

---

### 30.1 Where AI is already delivering value

Per the S9 survey (Petrovic et al., arXiv:2507.15025), GenAI is already delivering value in:

- Code generation (simulation and configuration scripts) — GPT-4 dominant (S9).
- Test scenario generation (LLM/RAG) (S9).
- Requirements summarisation/extraction (S9).
- RAG-based compliance Q&A (S9).
- Code analysis/optimisation (S9).
- Documentation generation (S9).

These are predominantly **non-safety-critical, human-reviewed** tasks. Only 1 of 9 surveyed organisations used GenAI for requirements handling, because requirements are high-value IP protected by NDAs (W5-016, S9). The value is real but narrow.

---

### 30.2 AI-native problem map

| Problem ID | Title | AI-native relevance |
|---|---|---|
| W1-001 | Ambiguous, underspecified requirements | AI can assist clarity checks; cannot resolve safety intent |
| W1-004 | ML/AI-specific requirements and traceability gaps | New RE categories (data, ODD, monitors) not handled by classic RE |
| W3-029 | Non-determinism and flaky tests in ML systems | ML testing fundamentally different from deterministic testing |
| W4-007 | End-to-end traceability gaps | AI-generated artifacts need provenance |
| W4-008 | Static analysis false positives / manual deviation | AI triage emerging; not certified |
| W4-030 | AI-assisted engineering tooling trust/certification/IP | Core blocker for AI-native adoption |
| W5-005 | AI/ML engineering capabilities gap | Few engineers bridge ML and functional safety |
| W5-016 | AI adoption barriers: confidentiality/IP/NDA | Cloud LLMs unusable for IP-protected requirements |
| W5-017 | LLM hallucination in safety-critical engineering | Systematic risk in every GenAI use |
| W5-018 | Lack of automotive datasets for fine-tuning | Main bottleneck for local LLM adoption |
| W5-019 | AI-generated code safety and certification gap | Probabilistic vs. deterministic; standards evolving |
| W5-020 | Explainability and accountability of AI outputs | No provenance; confidence poorly calibrated |
| W5-025 | Absence of AI governance in engineering workflows | No policy on which tasks may use AI |
| G2-001 | ADAS perception: sensor fusion, ML lifecycle, ODD | AI-native vehicle capability |
| G2-031 | ADAS scenario validation and coverage gap | AI-generated scenarios; coverage metrics immature |

---

### 30.3 The confidentiality/IP barrier (W5-016, W5-018)

Automotive requirements and code are high-value IP protected by NDAs. Exposing them to third-party cloud LLMs is unacceptable (W5-016). The alternative — locally deployable (fine-tuned) models — requires datasets, GPU hardware, and time that most organisations lack (W5-018). The S9 survey found only 1 of 9 organisations used GenAI for requirements handling for exactly this reason (W5-016).

This creates a **two-tier AI adoption pattern**: cloud LLMs are used where data can leave the organisation (simulation scripts, documentation), and are blocked where it cannot (requirements, safety-critical code). AI-native engineering cannot be achieved until this barrier is resolved — via federated/private deployments, synthetic data, or industry data consortia (W5-016, W5-018). None of these are mature.

---

### 30.4 Hallucination and verification (W5-017, W5-020)

LLMs are statistical next-token predictors, not symbolic reasoners. They hallucinate in believable ways (W5-017, S9). In automotive engineering this is not a cosmetic issue: an LLM can generate MISRA-compliant C that compiles but encodes wrong safety logic, passing naive review and surfacing only in HIL (W5-017).

Mitigations exist (RAG grounding, multi-agent debate, self-consistency, formal methods, uncertainty evaluation) but **reduce, do not eliminate**, hallucination (W5-017). There is no established AI verification workflow in the industry (W5-017, W5-025). Until formal-verification-in-the-loop becomes standard, AI-generated artifacts in safety-critical paths require human expert review — and verification skills are themselves scarce (W5-003, W5-009).

---

### 30.5 Certification and functional safety (W5-019, W4-030)

ISO 26262 is deterministic and evidence-based; ML is probabilistic and data-driven. The framework has gaps for the ML lifecycle; published enhancements cover only ASIL A/B (W5-019, S13). Certification of AI-assisted or AI-implemented safety functions is unresolved (W5-019, S12). SOTIF (ISO 21448) and ISO/PAS 8800 (AI safety) are still maturing (W1-004, W4-017, G2-001).

The consequence: AI can assist analysis (HARA artifact generation, test case generation) but cannot certify. Safety case ownership must remain human (W5-003, W5-019). Any "AI-native" claim that implies autonomous safety sign-off is not supported by the research record.

---

### 30.6 ML-specific requirements and traceability (W1-004, W4-007)

Machine-learning components require requirements on **data, ODD, monitors, and KPIs** that classic requirements engineering does not handle (W1-004). Traceability must cover models, datasets, and scenarios (W1-004, W4-007). ALM tools trace code and tests but not models, datasets, or ODDs (W1-004, G2-001). This is a growing gap as ADAS/ADS content increases (G2-001, G2-031).

AI-native engineering therefore needs an expanded notion of traceability — one that the current toolchain does not provide (W4-007, W4-001). This is as much a standards problem (SOTIF, ISO/PAS 8800) as a tooling problem (W1-014).

---

### 30.7 AI governance (W5-025, W5-020)

There is no established governance for AI use in safety-critical engineering: which tasks may use AI, required verification, approval gates, IP handling, audit trails (W5-025). Different teams use GenAI inconsistently; no audit trail; auditors cannot verify AI-assisted artifacts (W5-025, S9). AI outputs lack explainability and accountability (W5-020).

AI-native engineering cannot scale without governance. The research record calls for: an AI usage policy, verification gates, provenance tracking, confidence calibration, and training (W5-020, W5-025). None of these are standardised today.

---

### 30.8 Evidence limitations

- AI capability claims are largely vendor-reported; independent automotive benchmarks are scarce (W4-030, S9).
- Hallucination rates on automotive tasks are not measured (W5-017).
- Certification of AI-assisted artifacts is not established (W5-019).
- The cost of on-prem LLM deployment and the availability of automotive datasets are not quantified (W5-016, W5-018).
- AI-native engineering as a fully realised state does not yet exist; it is an aspiration with partial, narrow realisations.

---

### 30.9 Cross-references

- **Chapter 31 (Agentic Engineering Systems):** multi-agent orchestration of AI in engineering.
- **Chapter 32 (Physical AI):** AI in the vehicle.
- **Chapter 33 (Autonomous Workflows):** automation of engineering processes.
- **Chapter 34 (Future Workforce):** AI literacy and skill gaps.
- **Part IV, Chapter 26 (Current AI Adoption):** where AI stands today.

---

### Problem IDs referenced in this chapter

W1-001, W1-004, W1-014; W3-029; W4-001, W4-007, W4-008, W4-017, W4-030; W5-003, W5-005, W5-009, W5-016, W5-017, W5-018, W5-019, W5-020, W5-025; G2-001, G2-031.

---

*End of Chapter 30.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

