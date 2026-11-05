# Part VII — Research Conclusions

## Chapter 43 — Technology and Process Gaps

### Summary

This chapter separates the industry's gaps into **technology gaps** (capabilities that do not exist or are not mature enough) and **process gaps** (activities that are performed manually, inconsistently, or without evidence). The distinction matters: technology gaps require research and standardisation; process gaps require organisational change and measurement. Both are prerequisites for any future improvement. The chapter also catalogues what is **still manual** across the lifecycle, since manual work is the most direct indicator of a gap. Sources: problem records W1–W5, G1–G4, CROSS_CHECK, CROSS_QUESTIONS.

---

### 43.1 Technology gaps

Technology gaps are capabilities whose absence or immaturity blocks progress, independent of organisational will.

| # | Technology gap | Affected problems | Why it persists | Evidence |
|---|---|---|---|---|
| T1 | **End-to-end timing analysis across heterogeneous compute/network/XiL** | W1-010, W1-018, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014, W2-017, G1-016, G1-017, G2-002 | Tools do not compose; timing is checked after design, not designed in; FFI on Adaptive unsolved | W1-010 ("still largely unsolved"); CQ-017 |
| T2 | **vECU fidelity standards and regulator acceptance criteria** | W3-004, W3-007, W4-019, CQ-034, CQ-036 | No consolidated standard (FMI vs AUTOSAR); no fidelity KPI; no regulator position | W4-019 ("no clear criteria"); W3-004 (levels 0–4) |
| T3 | **Scenario coverage measurement for ADAS/SOTIF** | W3-018, W3-019, W3-020, W4-017, G2-001, G2-031, W3-029, W5-019 | Coverage is unmeasurable; simulation nondeterminism; ML nondeterminism; no determinism requirement standard | W3-018 (Euro NCAP "barely scratch the surface"); CQ-062 |
| T4 | **Certified AI / ML safety at ASIL C/D** | W1-004, W5-019, W3-029, W5-005, CQ-021, CQ-051 | No safety-case precedent; monitors/ODD not standardised; ISO/PAS 8800 emerging; probabilistic vs deterministic | W5-019 (only ASIL A/B in literature); CQ-021 |
| T5 | **Canonical, versioned, machine-readable engineering data backbone** | W4-001, W4-002, W4-005, W2-015, W1-013, W4-024, W4-028 | ARXML not canonical; no semantic diff/merge; PLM lacks AUTOSAR semantics | W4-002; CQ-014 |
| T6 | **Semiconductor obsolescence prediction and LTB optimisation** | G1-007, G1-008, G1-009, G1-003, G1-004 | Component lifecycle monitoring immature; demand uncertainty; no standard obsolescence risk process | G1-007; CQ-064 |
| T7 | **Memory and storage for SDV/ADAS compute** | G1-017, G1-018, G1-019, G1-016, G2-022 | Compute outpaces memory; eFlash doesn't scale; xSPI too slow for XiP; emerging NVM not qualified | G1-017, G1-018, G1-019 |
| T8 | **Chiplet and advanced-node automotive qualification** | G1-013, G1-014, G1-010, G1-011, G1-012 | Standards immature; AEC-Q100 overburdens advanced nodes; mission-profile not standardised | G1-010, G1-011, G1-013, G1-014 |
| T9 | **Mixed-criticality consolidation (hypervisor safety certification)** | W2-009, W2-004, W1-016, G1-016, G2-019, G2-027 | Hypervisor SEooC certification uncertain; FFI on Adaptive unsolved; cost unknown | W2-009; CQ-010 |
| T10 | **Multi-tier supply-chain visibility (semiconductor BOM)** | G1-004, G1-005, G1-006, W4-024 | No shared semiconductor BOM standard; proprietary data; contractual barriers | G1-004; CQ-064 |
| T11 | **Cloud/backend vehicle-data standard and interoperability** | G3-011, G3-010, G3-024, G3-008, G3-009 | No common vehicle-data standard; siloed systems; privacy/compliance fragmentation | G3-011, G3-005, G3-007 |
| T12 | **Hardware/software co-design tooling integration** | G1-015, G1-022, G1-023, G1-031, W5-010 | EDA not integrated with software toolchains; VCS not suited to chip design; virtual platform fidelity gaps | G1-015, G1-022; W3-004, W4-019 |
| T13 | **MCAL portability and open implementation** | G1-020, G1-021, W2-005, W4-004, W4-025 | MCAL vendor-specific; no cross-vendor portability; open-source MCAL immature | G1-021; W4-025 |
| T14 | **Sensor/actuator interface standardisation** | G1-027, G1-028, G2-020 | Proprietary SerDes; no standard hardware interfaces; scales with ADAS sensor count | G1-027, G1-028 |
| T15 | **AI governance and certification framework for engineering** | W5-025, W4-030, W5-017, W5-019, W5-020, CQ-043, CQ-054 | No industry consensus; no standard; no regulator roadmap | W5-025; W4-030 |

---

### 43.2 Process gaps

Process gaps are activities that are performed manually, inconsistently, or without evidence, and that could be improved by process change even without new technology.

| # | Process gap | Affected problems | Why it persists | Evidence |
|---|---|---|---|---|
| P1 | **Shift-left adoption (testing early)** | W3-031, W3-001, W3-002, W3-031, W4-019, W3-004 | Culture, investment, talent, regulation; only 33% implemented; 15% have no plan (up 6% YoY, unverified) | W3-031; CQ-033 |
| P2 | **End-to-end traceability (requirements→code→tests→safety)** | W1-003, W3-023, W4-007, W4-029, W1-002 | Links created late or not at all; tools do not enforce; horizontal XiL trace missing | W1-003; W4-007; CQ-003 |
| P3 | **Requirements quality control at intake** | W1-001, W1-005, W1-006, W5-006, CQ-045, CQ-070 | Natural language irreducible at scale; NLP checkers experimental; no quality gate | W1-001; CQ-070 |
| P4 | **Interface governance and architecture conformance** | W1-007, W1-008, W1-017, W2-012, W4-005 | Specs drift; review boards lack enforcement; no automated conformance checking | W1-007, W1-008; CQ-016 |
| P5 | **Variant management across lifecycle dimensions** | W1-011, W2-011, W3-011, W4-005, CQ-015, CQ-030, CQ-032 | System/AUTOSAR/test variant models not unified; combinatorial explosion; binding-time confusion | W1-011, W2-011; CQ-015 |
| P6 | **Compliance evidence collection and safety-case maintenance** | W4-011, W4-012, W4-013, W4-029, W3-025, W1-014, CQ-026, CQ-027 | Manual documentation; no automated evidence; re-assessment cost unquantified | W3-025 (41% time); W4-012 |
| P7 | **Knowledge capture and succession planning** | W5-012, W5-014, W5-023, W4-022, W4-006, CQ-037, CQ-038 | Tacit knowledge undocumented; bus-factor risk; no standard process; AI capture depends on doc quality | W5-012; CQ-037 |
| P8 | **HIL scheduling and capacity management** | W3-010, W3-011, W3-016, W4-018, CQ-035 | Benches scarce; setup weeks; manual scheduling; no optimisation | W3-016; W4-018 |
| P9 | **Test case specification and review quality** | W3-024, W3-026, W3-032, W3-027 | Test specs informal; review weak; invalid failures high; unit testing informal | W3-024; W3-027 (up to 91% invalid, single company) |
| P10 | **Fault injection and robustness testing** | W3-033, W3-034, W2-006, W2-010, CQ-061 | Manual, expertise-dependent; incomplete fault models; AUTOSAR-specific faults not modelled | W3-033; CQ-061 |
| P11 | **Regression testing scope and prioritisation** | W3-028, W3-026, W3-037, W3-024 | Re-run everything; risk-based testing underdeveloped; no prioritisation method | W3-028; W3-037 |
| P12 | **Architecture review and change-impact visibility** | W1-017, W1-008, W1-005, W4-012, CQ-016 | No visibility of change impact; late defect discovery; safety re-assessment cost | W1-017; W4-012 |
| P13 | **Supplier/OEM data exchange and ownership** | W1-006, W4-023, W5-014, W2-005, CQ-046, CQ-047 | Ownership lost between orgs; toolchain mismatch; communication gaps | W1-006; W4-023; CQ-046 |
| P14 | **MBSE adoption (culture/skill/tool/governance)** | W1-009, W5-007, W4-006, CQ-055 | Blocked by culture, skills, tooling, governance; definitional confusion | W1-009; CQ-055 |
| P15 | **AI-in-engineering governance** | W5-025, W4-030, W5-016, W5-017, W5-019, W5-020, CQ-043, CQ-054 | No policies, verification gates, provenance, audit trails; adoption ahead of governance | W5-025; W4-030 |
| P16 | **Onboarding and competency development** | W5-013, W4-022, W2-019, W5-022, W5-023, CQ-069 | 12–24 months to component ownership; no defined skill taxonomy; education mismatch | W5-013; CQ-069 |
| P17 | **Cost/ROI measurement and quantification** | All (W1-012, W3-025, W4-001, W4-028, CQ-002, CQ-004, CQ-013, CQ-030, CQ-057, CQ-060, CQ-071) | Every workstream marks cost impact "unquantified"; no measurement framework; survey figures untriangulated | CROSS_CHECK §3.1, §3.3; CQ-071 |

---

### 43.3 What is still manual (the manual-work inventory)

The research record identifies the following as still predominantly manual. This is the most direct evidence of gaps.

**Requirements and system engineering:**
- Requirements authoring in natural language with quality control (W1-001, W4-006).
- Trace link creation and maintenance (W1-003, W4-007).
- Architecture conformance and change-impact analysis (W1-008, W1-017).
- Timing budget computation (W1-010).
- Variant and configuration reconciliation (W1-011, W4-005).

**AUTOSAR and middleware:**
- ARXML authoring, editing, and merging (W4-003, W4-005, W2-001, W2-015).
- BSW and RTE configuration (W2-001, W2-012, W4-004).
- Classic/Adaptive gateway translation (W2-002).
- MCAL configuration (G1-020, G1-021).

**Testing and validation:**
- Test case specification and review (W3-024).
- Test scenario authoring for ADAS (W3-018, W3-020).
- Fault injection design and execution (W3-033).
- HIL bench setup and reconfiguration (W3-011, W4-018).
- Safety and cybersecurity test evidence aggregation (W3-025, W4-029).
- Regression test selection and prioritisation (W3-028, W3-037).

**Toolchain and data:**
- Data transformation and reconciliation ("glue work") (W4-028).
- Compliance documentation (W3-025, W4-011, W4-013).
- Safety-case maintenance and re-assessment (W4-012).
- Build and release approval cycles (W4-020, W4-021).

**Knowledge and workforce:**
- Onboarding and tacit knowledge transfer (W5-012, W5-013).
- Documentation authoring and verification (W5-017, W5-020).

---

### 43.4 Gaps that will worsen in the next 5–10 years

Task.md §12 asks for problems emerging over the next 5–10 years. The research record points to:

1. **SDV/centralised compute** increases multicore pressure, mixed-criticality risk, and hypervisor certification burden (W1-016, W2-009, W2-016, W5-024, G1-016, G2-019, G2-022, G2-027) while timing analysis remains unsolved (W1-010).
2. **ADAS/ADS scenario completeness** becomes harder as ODDs expand and ML nondeterminism grows (W3-018, W3-019, W3-020, W4-017, W5-019, G2-001, G2-031).
3. **Semiconductor obsolescence** accelerates as vehicle service life stays long and EOL shortens (G1-007, G1-008, G1-009, G1-003).
4. **AI adoption without governance** risks quality erosion and audit failures as AI enters safety-relevant workflows (W5-025, W4-030, W5-017, W5-019, W5-020).
5. **Skills obsolescence** as Classic AUTOSAR expertise ages while SDV skills are scarce (W5-021, W5-023, W2 cross-cutting "Classic not going away", CQ-053).
6. **Cloud/backend and connected-services** scale challenges as fleets grow and data volumes explode (G3-003, G3-007, G3-008, G3-009, G3-028).
7. **Energy/thermal** complexity grows with fast charging, V2G, and thermal runaway safety (G4-004, G4-006, G4-017, G4-030).
8. **Memory wall** worsens as AI inference moves into vehicles (G1-017, G1-018, G1-019).

---

### 43.5 Cross-cutting gaps (appearing in ≥3 workstreams)

CROSS_CHECK §5.5 and §3 identify the following as cross-cutting. They are the highest-leverage targets:

1. Toolchain fragmentation and proprietary formats (W4-001, W4-002, W4-004, W4-023, W4-027, W4-028, W1-013, W2-005, W2-015, W3-036).
2. ARXML/configuration complexity (W1-013, W2-001, W2-011, W2-012, W2-015, W4-002, W4-003, W4-005).
3. Compliance administration overhead (W1-014, W3-025, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-026).
4. Timing analysis unsolved (W1-010, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014).
5. Scenario completeness unknown (W3-018, W3-019, W3-020, W4-017, G2-001, G2-031).
6. Skills and workforce gaps (W5-001 … W5-011, W5-013, W5-015, W5-021 … W5-024, W1-015, W2-019, W3-038, W4-022, G1-034, G3-026).
7. Knowledge silos and turnover (W5-012, W5-014, W5-023, W4-022, W1-015, W2-019, W3-038).

---

### 43.6 Coverage gaps in the research itself

CROSS_CHECK §3.1 notes topics not covered or only superficially covered. These are themselves gaps in the evidence base and should be treated as open research areas:

- Hardware/semiconductor lifecycle and supply chain (partially covered by G1; cloud/backend by G3; energy/thermal by G4).
- Cloud/backend and connected-services software (G3 covers 28 records but depth varies).
- Energy, power, and thermal software (G4 covers 32 records).
- Open-source software governance and licensing in production ECUs (SBOM) — not covered.
- Software architecture quality, modularity, and technical-debt metrics — not covered.
- Multi-language and globalization — not covered.
- Developer experience/ergonomics of engineering tools — partially (W4-006).
- Cost/benefit and ROI quantification — every workstream marks cost impact "unquantified"; no measurement framework exists.

---

### 43.7 Evidence limitations

- Technology gaps are identified from problem records and cross-check; no independent technology-readiness assessment was conducted.
- Process gaps rely on survey data (Perforce, electronica, S9 n=9) that are single-source and untriangulated.
- "Still manual" claims are based on problem-record descriptions, not time-motion studies.
- Emerging-risk claims (§43.4) are hypotheses, not forecasts.

---

### 43.8 Cross-references

- **Chapter 41 (this Part):** problem categories.
- **Chapter 42 (this Part):** insufficient solutions.
- **Chapter 44 (this Part):** investigation opportunities.
- **Chapter 45 (this Part):** open research questions.
- **Chapter 27 (Part IV):** root-cause themes for insufficient solutions.
- **CROSS_CHECK.md §3:** coverage gaps.
- **CROSS_QUESTIONS.md:** P1/P2 questions mapping to gaps.

---

### Problem IDs referenced in this chapter

W1-001, W1-002, W1-003, W1-005, W1-006, W1-007, W1-008, W1-010, W1-011, W1-013, W1-014, W1-015, W1-016, W1-017, W1-018; W2-001, W2-002, W2-004, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-011, W2-012, W2-013, W2-014, W2-015, W2-016, W2-017; W3-001, W3-002, W3-004, W3-005, W3-007, W3-010, W3-011, W3-015, W3-016, W3-018, W3-019, W3-020, W3-023, W3-024, W3-025, W3-026, W3-027, W3-028, W3-029, W3-031, W3-032, W3-033, W3-034, W3-035, W3-036, W3-037, W3-038; W4-001, W4-002, W4-003, W4-004, W4-005, W4-006, W4-007, W4-008, W4-009, W4-010, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-017, W4-018, W4-019, W4-020, W4-021, W4-022, W4-023, W4-024, W4-025, W4-026, W4-027, W4-028, W4-029, W4-030; W5-001 … W5-015, W5-016, W5-017, W5-019, W5-020, W5-021, W5-022, W5-023, W5-024, W5-025; G1-003, G1-004, G1-007, G1-008, G1-009, G1-010, G1-011, G1-012, G1-013, G1-014, G1-015, G1-016, G1-017, G1-018, G1-019, G1-020, G1-021, G1-022, G1-023, G1-027, G1-028, G1-031, G1-034; G2-001, G2-002, G2-019, G2-020, G2-022, G2-027, G2-031; G3-003, G3-005, G3-007, G3-008, G3-009, G3-010, G3-011, G3-024, G3-026, G3-028; G4-004, G4-006, G4-017, G4-030, G4-031, G4-032; CQ-001, CQ-003, CQ-009, CQ-014, CQ-015, CQ-016, CQ-017, CQ-021, CQ-026, CQ-027, CQ-030, CQ-032, CQ-033, CQ-034, CQ-035, CQ-037, CQ-038, CQ-043, CQ-045, CQ-046, CQ-047, CQ-051, CQ-053, CQ-054, CQ-057, CQ-060, CQ-061, CQ-062, CQ-064, CQ-069, CQ-070, CQ-071.

---

### 43.9 Sources

- `research/W1_requirements_system.md` … `research/W5_skills_ai.md`.
- `research/G1_semiconductor_hardware.md` … `research/G4_energy_thermal.md`.
- `research/CROSS_CHECK.md` (§3 gaps, §5.5 root causes, §3.1 missing topics).
- `research/CROSS_QUESTIONS.md` (CQ-001 … CQ-072, priority-labelled).
- `research/KNOWLEDGE_INDEX.md` (cross-cutting themes).
- `research/DATA_VERIFY.md` (evidence limitations).
- `Task.md` §4, §8, §12, §16.

---

*End of Chapter 43.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

