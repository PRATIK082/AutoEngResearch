# Part V — Future Automotive Engineering

## Chapter 31 — Agentic Engineering Systems

### Summary

Agentic engineering systems use AI agents — autonomous or semi-autonomous LLM-based systems that can plan, use tools, and act over multiple steps — to perform engineering tasks such as requirements analysis, ARXML validation, dependency/change-impact analysis, test generation, and documentation. The research record shows the building blocks are emerging (LLM/RAG, multi-agent debate, engineering knowledge graphs, tool integration) but that agentic systems in safety-critical automotive engineering face the same blockers as AI more broadly — confidentiality, hallucination, certification, governance — plus new ones: tool reliability, auditability, and the risk of uncontrolled autonomous actions on engineering baselines (W5-017, W5-020, W5-025, W4-001, W4-007). Agentic systems are therefore a promising but immature area, with no production-scale deployment in safety-critical engineering workflows evidenced.

---

### 31.1 What "agentic" means in this context

An agentic system goes beyond single-shot LLM prompting: it plans a sequence of actions, calls tools (search, code execution, ARXML parsers, requirements tools), observes results, and iterates. In automotive engineering, candidate applications include:

- **Requirements handling:** summarisation, ambiguity detection, formalisation, traceability link suggestion (W1-001, W1-003, W4-007).
- **ARXML/configuration intelligence:** validation, diff/merge, consistency checks (W2-001, W4-002, W4-003, W4-005).
- **Dependency and change-impact analysis:** propagating changes across requirements, architecture, code, tests (W1-005, W1-017, W4-012).
- **Test generation and optimisation:** scenario generation, test-case prioritisation (W3-024, W3-026, W3-037).
- **Knowledge capture and retrieval:** RAG over internal docs, engineering knowledge graphs (W5-012, W5-014).
- **Documentation generation:** automated, standards-aware (W5-012, W3-025).

The S9 survey positions these as emerging/experimental, with LLM/RAG for requirements handling still a gap (S9).

---

### 31.2 Agentic systems problem map

| Problem ID | Title | Agentic relevance |
|---|---|---|
| W1-003 | Requirements traceability burden | Agents can suggest links; semantic correctness unverified |
| W1-005 | Late and uncontrolled changes | Agents can do impact analysis; cannot enforce change control |
| W1-007 | Unstable interfaces and governance gaps | Agents depend on stable interfaces to act reliably |
| W4-001 | Fragmented toolchain, poor interoperability | Agents need tool APIs; fragmentation blocks integration |
| W4-007 | End-to-end traceability gaps | Agents generate artifacts that must be traceable/provenanced |
| W4-028 | Manual data transformation ("glue work") | Prime target for agentic automation |
| W5-017 | LLM hallucination | Compounds over multi-step agent plans |
| W5-020 | Explainability and accountability | Agents act autonomously; accountability unclear |
| W5-025 | Absence of AI governance | No policy on autonomous agent actions |
| W3-023 | Horizontal XiL traceability gaps | Agents can correlate XiL artifacts if data accessible |
| W2-001 | ARXML configuration complexity | High-value target for agentic assistance |
| W4-030 | AI tooling trust/certification/IP | Same blockers as single-model AI |

---

### 31.3 Why multi-step amplifies risk

Agentic systems compound the risks of single-model AI:

- **Hallucination compounds** across steps: an error in step 2 propagates to steps 3–N, and the final output may look coherent (W5-017, S9 Degeneration-of-Thought).
- **Tool errors** (wrong ARXML parse, stale requirement) are ingested as facts and amplified (W4-001, W4-003).
- **Confidence calibration** is poor; agents report high confidence on wrong plans (W5-020).
- **No standardised verification workflow** exists for agent outputs (W5-017, W5-025).

Multi-agent designs (debate, self-consistency, LLM-as-judge) are proposed mitigations but are research-stage and do not eliminate risk (W5-017, S9).

---

### 31.4 Tool integration and fragmentation (W4-001, W4-028)

Agents are only as good as the tools they can call. The automotive toolchain is fragmented, proprietary, and poorly interoperable (W4-001). Requirements, architecture, configuration, code, and test artifacts live in different systems with brittle cross-tool links (W4-006, W4-007, W4-024). Manual "glue work" (spreadsheets, scripts) is the current integration mechanism (W4-028).

An agentic system that cannot call a requirements tool, an ARXML parser, or a test runner cannot act. Therefore, **agentic engineering presupposes toolchain integration** that does not yet exist (W4-001, W4-028). This is a prerequisite problem, not a detail.

---

### 31.5 Human authority and approval gates

The research record is consistent that certain decisions must stay under human authority regardless of agent capability (W5 cross-cutting table):

- Safety requirements approval (W5-003, W5-019).
- Architecture decisions (W5-007).
- Safety-critical code approval (W5-019).
- Safety validation sign-off (W5-009).
- Compliance sign-off (W5-025).
- Certification (W5-019).

Agentic systems must be designed with explicit approval gates, not as autonomous replacements for engineers. The research record does not support "lights-out" engineering; it supports **augmentation** with human-in-the-loop gates (W5-025, S9).

---

### 31.6 Auditability and provenance

For agentic systems to be used in safety-critical engineering, every action must be auditable: which agent, which tools, which inputs, which outputs, which human approvals (W5-020, W5-025, W4-007). Current LLMs provide no provenance; confidence is poorly calibrated (W5-020). ALM tools store artifacts but do not capture agent provenance (W4-007, W4-030).

Without provenance, assessors cannot verify AI-assisted artifacts, and adoption in safety-critical paths is blocked (W5-025, W4-030). This is a tooling and process gap, not just a model gap.

---

### 31.7 Knowledge graphs and RAG as enabling infrastructure

Engineering knowledge graphs and RAG over internal documentation are repeatedly cited as enabling infrastructure for agentic systems (W5-012, W5-014, Task.md §7). They address knowledge silos and single-person expertise (W5-012) and enable standards-aware assistance (W5-006). However:

- Building and maintaining an engineering knowledge graph requires structured data that most organisations do not have (W4-024, W4-028).
- RAG quality depends on document quality, which is poor (W5-012, W4-006).
- Hallucination persists even with RAG (W5-017).

Knowledge graphs are a promising direction but not a solved enabler.

---

### 31.8 Evidence limitations

- No production-scale deployment of agentic systems in safety-critical automotive engineering is evidenced (W4-030, S9).
- Hallucination rates on multi-step automotive tasks are not measured (W5-017).
- Tool-integration prerequisites are not quantified (W4-001).
- Governance frameworks for autonomous agent actions do not exist (W5-025).

---

### 31.9 Cross-references

- **Chapter 30 (AI-Native):** the broader AI context.
- **Chapter 33 (Autonomous Workflows):** end-to-end automation of engineering processes.
- **Chapter 34 (Future Workforce):** AI literacy and governance.
- **Part IV, Chapter 26 (Current AI Adoption):** where AI stands today.

---

### Problem IDs referenced in this chapter

W1-001, W1-003, W1-005, W1-007; W2-001; W3-023, W3-024, W3-026, W3-037; W4-001, W4-002, W4-003, W4-005, W4-006, W4-007, W4-012, W4-024, W4-028, W4-030; W5-003, W5-006, W5-007, W5-009, W5-012, W5-014, W5-017, W5-019, W5-020, W5-025.

---

## 31.x Agentic automation for bootloader/flash (W6)

Future agentic systems can target bootloader/flash workflows: automated memory-map verification (W6-002), UDS flash conformance checks (W6-007), power-fault test generation (W6-014), manufacturing/EOL traceability (W6-015), and image-integrity verification (W6-028). See `research/SOLUTIONS_BOOTLOADER_FLASH.md`.

*End of Chapter 31.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

