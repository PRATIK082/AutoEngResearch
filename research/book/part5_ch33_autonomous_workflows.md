# Part V — Future Automotive Engineering

## Chapter 33 — Autonomous Engineering Workflows

### Summary

Autonomous engineering workflows aim to automate large parts of the automotive software lifecycle — requirements analysis, code generation, build, integration, testing, HIL orchestration, release — with minimal human intervention. The research record shows that automation is already strong in narrow, well-defined tasks (static analysis, MISRA checks, documentation, knowledge capture, test execution) but weak or absent where judgement, safety, and certification are involved (W5 cross-cutting table). The biggest automation opportunities are in the "glue work" between tools (W4-028), build/CI/CD (W4-020, W4-021), HIL orchestration (W3-010, W3-011, W3-016), and traceability (W1-003, W3-023, W4-007). The blockers are tool fragmentation (W4-001), immature shift-left (W3-031), HIL scarcity (W3-010, W4-018), and the absence of AI governance (W5-025). "Lights-out" engineering is not supported by the evidence; augmentation with human gates is.

---

### 33.1 The automation spectrum

The W5 cross-cutting table summarises what can be automated today:

| Activity | Automatable now | Needs expert judgement | Must stay human authority |
|---|---|---|---|
| Requirements handling | Medium (LLM/RAG emerging) | Yes — ambiguity resolution | Safety requirements approval |
| Compliance checking | Medium-High (RAG) | Yes — interpretation | Compliance sign-off |
| Code generation (sim/config) | Medium (GPT-4 dominant) | Yes — safety logic | Safety-critical code approval |
| Test scenario generation | Medium-High (LLM/RAG) | Yes — coverage adequacy | Safety validation sign-off |
| Static analysis / MISRA | High (existing tools) | Yes — triage | Release decision |
| Documentation generation | High (LLM) | Yes — accuracy | Release notes approval |
| Knowledge capture / search | High | Yes — relevance | N/A |
| Safety case argumentation | Low | Yes | Mandatory human |
| Cybersecurity architecture | Low-Medium | Yes | Mandatory human |
| Architecture decisions | Low-Medium | Yes | Mandatory human |
| Certification | None | N/A | Mandatory human (assessors) |

The pattern: automation is high where tasks are **syntactic and well-scoped**, and low where they are **semantic, safety-relevant, or require judgement** (W5-003, W5-007, W5-019).

---

### 33.2 Autonomous workflows problem map

| Problem ID | Title | Workflow relevance |
|---|---|---|
| W3-001 | MIL plant/control models simplified, validated late | Shift-left blocked by model fidelity |
| W3-002 | MIL test cases not reused for SIL | Wasted test effort across XiL |
| W3-003 | MIL/SIL model instability prevents deterministic CI | Non-deterministic pipelines |
| W3-010 | HIL cost and scalability | Bottleneck for automated testing |
| W3-011 | HIL setup latency, reconfiguration, variants | Automation of bench management |
| W3-015 | Cloud-based HIL challenges | Remote execution barriers |
| W3-016 | HIL test environment management | Bench scheduling and utilisation |
| W3-023 | Horizontal XiL traceability gaps | Automated evidence collection |
| W3-028 | Regression testing scalability and overhead | Automation of regression selection |
| W3-031 | Shift-left adoption incomplete | Testing still late |
| W4-001 | Fragmented toolchain, poor interoperability | Automation prerequisite |
| W4-020 | Generic CI/CD unsuitable for safety-critical embedded | Pipeline engineering gap |
| W4-021 | Long build/compilation times | Build automation and caching |
| W4-028 | Manual data transformation ("glue work") | Prime automation target |
| W4-030 | AI tooling trust/certification/IP | AI automation blockers |
| W1-003 | Requirements traceability burden | Automated link suggestion |
| W1-005 | Late and uncontrolled changes | Automated impact analysis |
| W4-007 | End-to-end traceability gaps | Automated evidence |
| W5-012 | Knowledge silos, single-person expertise | Automated knowledge capture |
| W5-013 | Training and onboarding time/cost | Automated onboarding assistance |
| W5-025 | Absence of AI governance | Policy for autonomous actions |

---

### 33.3 Shift-left and CI/CD for embedded (W3-031, W4-020, W4-021)

Shift-left ambition far exceeds reality: only ~33% implemented; "not testing early enough" is rising (W3-031). Generic CI/CD pipelines (Jenkins, GitLab, GitHub Actions) are unsuitable for safety-critical embedded: they lack deterministic builds, tool qualification, and evidence generation (W4-020). Build times for large AUTOSAR codebases are long, blocking fast feedback (W4-021).

Autonomous workflows require **safety-aware CI/CD** that produces qualified, traceable evidence — a capability the industry does not yet have as a standard (W4-020, W4-007). This is a high-value, high-effort gap.

---

### 33.4 HIL orchestration and automation (W3-010, W3-011, W3-016, W4-018)

HIL is the dominant cost and schedule driver for testing (up to 50% of dev cost) and is scarce (W3-010, W4-018). Bench setup is slow and variant-heavy (W3-011); utilisation is poor (W3-016). Cloud HIL is blocked by latency, cybersecurity, and compliance (W3-015, G3-014).

Autonomous HIL workflows require: automated bench provisioning, variant-aware test scheduling, remote execution with compliance evidence, and AI-assisted test prioritisation. These are partially available but not integrated (W3-010, W3-011, W3-016, W3-037). HIL automation is a prime opportunity but depends on resolving cloud-HIL barriers (W3-015).

---

### 33.5 Traceability and evidence automation (W1-003, W3-023, W4-007)

Traceability is mandated by ISO 26262 and ASPICE but is created manually and is sparse, stale, or wrong (W1-003, W3-023, W4-007). Automated traceability is research-stage; ML elements (models, datasets, ODDs) are not covered by classic traceability (W1-004, W3-023).

Autonomous workflows cannot be audited without automated evidence. The research record treats automated traceability as a high-value, unsolved problem (W4-007, W1-003). AI can assist (link suggestion, gap detection) but cannot self-certify (W5-017, W4-030).

---

### 33.6 Knowledge capture and onboarding automation (W5-012, W5-013, W5-014)

Knowledge silos and single-person expertise are chronic (W5-012). AI knowledge capture (RAG, engineering knowledge graphs) can reduce training time by up to 40% (W5-011, W5-012, S11). Onboarding takes 12–24 months for an ASIL B component owner (W5-013). Communication gaps across disciplines and organisations are chronic (W5-014).

Automation here is high-feasibility for documentation capture and search, medium for tacit knowledge (W5-012). But RAG quality depends on document quality, which is poor (W5-012, W4-006). This is a high-value, medium-difficulty area.

---

### 33.7 Glue work automation (W4-028, W4-001, W4-024)

Manual data transformation via spreadsheets and scripts ("glue work") is a pervasive, low-value activity (W4-028). It exists because tools do not interoperate (W4-001) and PLM/PDM is not integrated with engineering data (W4-024). Automating glue work is high-feasibility and high-value, and is a prerequisite for higher-level agentic automation (W4-028, Chapter 31).

---

### 33.8 Evidence limitations

- Automation feasibility ratings are qualitative assessments from the research reports, not measured.
- Shift-left adoption statistics (~33%) are survey-based and may not be automotive-specific.
- Cloud HIL and automated traceability are research-stage; no production-scale evidence cited.
- AI governance for autonomous workflows does not exist (W5-025).

---

### 33.9 Cross-references

- **Chapter 30 (AI-Native):** AI capabilities and blockers.
- **Chapter 31 (Agentic Systems):** multi-agent orchestration.
- **Chapter 34 (Future Workforce):** skills for automated workflows.
- **Part IV, Chapter 25 (Existing Automation):** what automation exists today.

---

### Problem IDs referenced in this chapter

W1-003, W1-004, W1-005; W3-001, W3-002, W3-003, W3-010, W3-011, W3-015, W3-016, W3-023, W3-028, W3-031, W3-037; W4-001, W4-007, W4-018, W4-020, W4-021, W4-024, W4-028, W4-030; W5-003, W5-007, W5-009, W5-012, W5-013, W5-014, W5-017, W5-019, W5-025.

---

*End of Chapter 33.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

