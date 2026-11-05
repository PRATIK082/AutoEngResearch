# Part IV — Existing Industry Solutions

## Chapter 25 — Existing Automation Approaches

### Summary

Automation in automotive software engineering is **real but unevenly distributed**: it is mature in build and test execution, partial in configuration and requirements handling, and largely absent in safety argumentation, architecture decisions, and certification. The industry has invested heavily in CI/CD pipelines (Jenkins, GitLab, GitHub Actions), test automation (Tessy, VectorCAST, Parasoft), build caching, and ARXML scripting, and these deliver measurable value in developer productivity. But the same automation is **poorly suited to safety-critical embedded software** (W4-020): tool qualification, reproducibility, and HIL integration are bolted on rather than native. Shift-left adoption—the organisational prerequisite for automation to pay off—stands at only ~33% (W3-031). The result is an automation landscape that speeds up the activities that were already fast (compilation, unit-test execution) while leaving the slow, manual, expensive activities (HIL setup, safety-case maintenance, requirements quality, configuration merge, scenario coverage) largely untouched. This chapter catalogues existing automation approaches, their value, and their limits.

---

### 25.1 Purpose and scope

This chapter surveys automation that exists today across the lifecycle: build and CI/CD, test automation, ARXML/configuration automation, code generation, HIL automation, scenario generation, traceability automation, and documentation generation. It distinguishes *mechanical* automation (high feasibility) from *semantic* automation (low feasibility) following the automation-feasibility summary in W4 §4.4, and interlinks the problem IDs that automation has and has not resolved.

### 25.2 The automation feasibility landscape (from W4 §4.4)

| Feasibility | Activities |
|---|---|
| **High** | Mechanical data transformation, evidence aggregation, coverage reporting, CI build caching, documentation generation |
| **Medium** | ARXML validation/migration, traceability link suggestion, HIL scheduling, vECU integration, TARA assistance, requirements handling (LLM/RAG) |
| **Low** | Safety judgement, compliance interpretation, undecidable rule decisions, certification of AI, architecture decisions, safety-case argumentation |

This table is the organising frame for the rest of the chapter: high-feasibility automation is widely deployed and works; medium-feasibility automation is emerging and partial; low-feasibility automation is not credible today.

### 25.3 Build and CI/CD automation

**What exists:** Jenkins/GitLab/GitHub Actions pipelines, containerised builds (Docker), distributed builds, ccache, incremental builds, automated unit-test execution, coverage reporting.

- **Value delivered:** faster feedback, reproducible builds, automated regression, artefact management.
- **Limitations:** generic pipelines are unsuitable for safety-critical embedded software without bolting on qualification and traceability (W4-020); build times for large AUTOSAR codebases remain long because generated code invalidates caches (W4-021); HIL-in-CI integration is immature; tool qualification evidence is manual.
- **What remains manual:** tool qualification, environment reproducibility, safety-impact analysis, HIL-in-the-loop.
- **Problem IDs:** W4-020, W4-021, W4-028, W3-003, W3-027, CQ-058.

### 25.4 Test automation

**What exists:** automated unit/component test execution (Tessy, VectorCAST, Parasoft, GoogleTest), regression suites, coverage (MC/DC) measurement, test-data management, automated test prioritisation (partial).

- **Value delivered:** repeatable execution, coverage evidence, faster regression, reduced manual execution.
- **Limitations:** test-case design remains manual and quality varies (W3-024); regression scalability and overhead persist (W3-028); invalid test failures (up to 91%) undermine trust (W3-027); test tool interoperability and evidence aggregation are manual (W4-029); fault injection is manual and expertise-dependent (W3-033); model-based testing does not yet handle hybrid systems (W3-035).
- **What remains manual:** test-case design, coverage adequacy judgement, fault-model definition, evidence assembly, safety validation sign-off.
- **Problem IDs:** W3-024, W3-026, W3-027, W3-028, W3-032, W3-033, W3-035, W3-036, W3-037, W4-029, W4-030.

### 25.5 HIL automation

**What exists:** automated HIL test execution, remote HIL access, bench scheduling tools, vECU-based SIL substitution (partial), cloud HIL (emerging).

- **Value delivered:** unattended test execution, better bench utilisation, early (SiL) feedback.
- **Limitations:** benches are scarce and scheduled (W4-018); setup and reconfiguration take weeks (W3-011); model fidelity gaps (W3-012); vECU fidelity and integration friction (W4-019); cloud HIL has latency, cyber, and compliance barriers (W3-015); no clear criteria for when vECU evidence is acceptable (CQ-034).
- **What remains manual:** bench configuration, plant model development, scenario authoring, scheduling optimisation, fidelity judgement.
- **Problem IDs:** W3-010, W3-011, W3-012, W3-013, W3-014, W3-015, W3-016, W4-018, W4-019, W3-004, W3-007.

### 25.6 ARXML and configuration automation

**What exists:** ARXML parsers/validators, code generators, configuration scripts, semantic diff tools (dSPACE Compare, IAV Kedu), CI validation gates, automated refactoring attempts.

- **Value delivered:** catches invalid ARXML early; enables bulk edits; supports code generation; reduces manual entry.
- **Limitations:** ARXML version/dialect fragmentation persists (W4-002); manual authoring is still error-prone (W4-003); diff/merge and variant management are immature (W4-005); configuration consistency validation gaps (W2-012); round-trip friction (W2-015).
- **What remains manual:** semantic reconciliation, variant binding decisions, safe merge of configurations, migration between AUTOSAR releases.
- **Problem IDs:** W4-002, W4-003, W4-005, W2-001, W2-011, W2-012, W2-015, W1-013, CQ-013, CQ-014.

### 25.7 Code generation and model-based automation

**What exists:** Simulink/TargetLink code generation, AUTOSAR RTE/BSW generators, configuration code generators, model-based testing (partial).

- **Value delivered:** eliminates hand-written boilerplate; enforces architecture; accelerates development; supports certification when qualified.
- **Limitations:** generated code is large and slow to compile (W4-021); generated code is not modularised for incremental builds; timing analysis of generated code is unsolved (W1-010); model-to-code fidelity must be validated (W3-001, W3-002).
- **What remains manual:** model validation, timing verification, safety evidence for generated code, generator qualification.
- **Problem IDs:** W4-021, W1-010, W3-001, W3-002, W3-008, W4-020.

### 25.8 Requirements and traceability automation

**What exists:** ALM traceability features, automated link suggestions (emerging), impact analysis (partial), requirements import/export, RAG-based compliance Q&A (experimental).

- **Value delivered:** reduces manual link creation; supports audit evidence; emerging AI assistance.
- **Limitations:** traceability links are manual and brittle (W4-007); links go stale after changes (W1-003); cross-tool links are fragile; AI-assisted handling is experimental and blocked by confidentiality/IP (W5-016) and hallucination (W5-017).
- **What remains manual:** intent-level trace links, impact judgement, compliance sign-off, AI output verification.
- **Problem IDs:** W4-007, W1-003, W4-030, W5-006, W5-016, W5-017, W5-020, W5-025, CQ-003, CQ-045.

### 25.9 Scenario generation and ADAS validation automation

**What exists:** scenario databases, scenario-based testing frameworks, simulation platforms (CARLA, Gazebo), OpenSCENARIO/OpenDRIVE, automated scenario generation (emerging, AI-assisted).

- **Value delivered:** scales scenario testing beyond manual authoring; enables regression; supports SOTIF process.
- **Limitations:** scenario space is effectively unbounded (W3-018, W4-017); simulation nondeterminism/flakiness (W3-019, up to 43.6%); test scenario reuse is unsafe across systems (W3-020); coverage completeness is unknown (W3-018); AI-generated scenarios require safety validation (CQ-005).
- **What remains manual:** scenario adequacy judgement, coverage assessment, safety validation sign-off, unknown-unsafe-scenario reasoning.
- **Problem IDs:** W3-017, W3-018, W3-019, W3-020, W4-017, W5-009, CQ-001, CQ-005, CQ-051, CQ-062.

### 25.10 Documentation and knowledge automation

**What exists:** LLM-based documentation generation, automated API docs, RAG over internal docs, knowledge-graph prototypes, AI-assisted onboarding.

- **Value delivered:** reduces documentation overhead; improves searchability; promising for knowledge capture (W5-012).
- **Limitations:** accuracy must be verified (W5-017); data confidentiality/IP barriers (W5-016); lack of automotive datasets (W5-018); no proven 40% training-time reduction in automotive (CQ-038).
- **What remains manual:** safety rationale documentation, review and approval, governance.
- **Problem IDs:** W5-012, W5-013, W5-016, W5-017, W5-018, W5-020, W5-023, CQ-038.

### 25.11 Shift-left: automation's prerequisite that is not met

Automation's value depends on testing and validating early. Shift-left adoption is low (~33% implemented; 15% have no plan, up 6% YoY) (W3-031). Barriers: culture, investment, talent, regulation, vECU fidelity, and HIL availability (W3-031, CQ-033). Without shift-left, automation accelerates late-stage activities rather than preventing defects. This is a primary reason existing automation has not reduced industry-wide defect escape or release delays.

### 25.12 What existing automation solves vs. what it does not

**Solves (mechanical, high feasibility):**
- Build and unit-test execution speed.
- Coverage measurement and reporting.
- Mechanical data transformation and CI gates.
- Code generation from models.
- Unattended HIL test execution.
- Basic documentation generation.

**Does not solve (semantic, safety, governance):**
- Requirements quality and ambiguity (W1-001, W5-006).
- Architecture consistency and governance (W1-008, W1-017).
- Timing analysis (W1-010, W2-007, W2-008).
- Safety-case maintenance and change-impact (W4-012).
- Compliance interpretation and evidence judgement (W4-011, W4-013, W4-029).
- Scenario coverage completeness (W3-018, W4-017).
- Knowledge capture and tacit expertise (W5-012, W5-013).
- AI certification and trust (W4-030, W5-019, W5-020, W5-025).

### 25.13 Problem ID index for this chapter

W1-001, W1-003, W1-008, W1-010, W1-013, W1-017; W2-001, W2-007, W2-008, W2-011, W2-012, W2-015; W3-001, W3-002, W3-003, W3-004, W3-007, W3-010, W3-011, W3-012, W3-013, W3-014, W3-015, W3-016, W3-017, W3-018, W3-019, W3-020, W3-024, W3-026, W3-027, W3-028, W3-031, W3-032, W3-033, W3-035, W3-036, W3-037; W4-002, W4-003, W4-005, W4-007, W4-011, W4-012, W4-013, W4-017, W4-018, W4-019, W4-020, W4-021, W4-028, W4-029, W4-030; W5-006, W5-009, W5-012, W5-013, W5-016, W5-017, W5-018, W5-020, W5-023, W5-025; CQ-001, CQ-003, CQ-005, CQ-013, CQ-014, CQ-034, CQ-038, CQ-045, CQ-051, CQ-058, CQ-062.

### 25.14 Sources

- research/W4_toolchain_standards.md (W4 §4.4 automation feasibility; W4-002, W4-003, W4-005, W4-007, W4-011, W4-012, W4-013, W4-017, W4-018, W4-019, W4-020, W4-021, W4-028, W4-029, W4-030)
- research/W3_testing_validation.md (W3-001..W3-038)
- research/W2_autosar_middleware.md (W2-001, W2-007, W2-008, W2-011, W2-012, W2-015)
- research/W1_requirements_system.md (W1-001, W1-003, W1-008, W1-010, W1-013, W1-017)
- research/W5_skills_ai.md (W5-006, W5-009, W5-012, W5-013, W5-016, W5-017, W5-018, W5-020, W5-023, W5-025)
- Task.md §7 (AI, Automation and Future Engineering), §8 (Time, Cost, Productivity)
- CROSS_QUESTIONS.md (CQ-001, CQ-003, CQ-005, CQ-013, CQ-014, CQ-034, CQ-038, CQ-045, CQ-051, CQ-058, CQ-062)

---

*End of Chapter 25.*