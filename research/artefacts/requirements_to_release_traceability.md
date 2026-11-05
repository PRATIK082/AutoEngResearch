# Requirements-to-Release Traceability Map

**Artefact:** #6 (Task.md §15)  
**Source basis:** research/W1_requirements_system.md, W2_autosar_middleware.md, W3_testing_validation.md, W4_toolchain_standards.md, W5_skills_ai.md, part2_ch10_integration.md, part2_ch11/12/13, KNOWLEDGE_INDEX.md  
**Date:** 2026-11-05  
**Scope:** End-to-end traceability from stakeholder/customer requirements through system, software, hardware and configuration artefacts to released software, flash images, OTA packages and field evidence.

---

## 1. Purpose

This artefact maps **what must be traceable** across the automotive software lifecycle, **which artefacts carry the links**, **where links break**, and **which problem IDs explain the breaks**. It is a research artefact, not a tool specification. It deliberately records gaps and unanswered questions rather than proposing a product.

Traceability in automotive software is mandated by ISO 26262, Automotive SPICE, ISO/SAE 21434 and UNECE R155/R156, yet the practice is fragmented: links are manual, brittle, frequently stale, and often absent because engineers fear creating wrong links (W1-003, W4-007).

---

## 2. Traceability model (levels)

| Level | Artefact / abstraction | Typical owner | Representative tools | Problem IDs |
|---|---|---|---|---|
| L0 | Customer / market / regulatory requirements | Product planning, OEM | Spreadsheets, PLM | W1-001, W1-004, W1-014 |
| L1 | Stakeholder requirements (SHRQ) | System engineering (OEM) | DOORS, Polarion, Codebeamer, Jama | W1-001, W1-002, W1-005, W1-006 |
| L2 | System requirements (SyRQ) | System engineering (OEM/Tier-1) | DOORS, Polarion, MBSE (SysML) | W1-002, W1-006, W1-007, W1-008 |
| L3 | Software requirements (SWRQ) | Software engineering | DOORS, Codebeamer, Polarion | W1-001, W1-003, W1-004 |
| L4 | Hardware requirements (HWRQ) | Hardware engineering | PLM/PDM, requirements tools | G1-015, G1-022, G1-033 |
| L5 | Architecture / interface definitions | System / software architecture | Enterprise Architect, Rhapsody, MBSE | W1-007, W1-008, W1-017, W1-018 |
| L6 | AUTOSAR / configuration descriptions (ARXML, DBC) | Configuration engineering | EB tresos, DaVinci, SystemDesk, PREEvision | W1-013, W2-001, W2-011, W2-012, W2-015, W4-002–W4-005 |
| L7 | Source code / models (C/C++, Simulink, Rust) | Developers | Git, IDEs, MATLAB/Simulink | W4-008, W4-009, W4-010, W4-020, W4-021 |
| L8 | Tests (unit, component, integration, system, vehicle) | Test engineering | Tessy, VectorCAST, Parasoft, LDRA, GoogleTest | W3-023, W3-024, W3-026, W3-030, W3-032, W3-036, W4-029 |
| L9 | Safety / cybersecurity evidence | Safety & security engineering | Safety case tools, FMEA tools | W4-011, W4-012, W4-014, W4-015, W5-003, W5-004 |
| L10 | Build / release artefacts (flash image, OTA package) | Release engineering | Build systems, bootloader tooling | W2-010, W4-016, W4-020, W3-030 |
| L11 | Field evidence (telemetry, DTCs, OTA status) | Fleet / service | Cloud platforms, diagnostics | G3-012, G3-017, G3-021, G4-029 |

**Key gap:** No single tool enforces semantic consistency across L0–L11; links are stored but not validated for correctness (W1-002, W1-003, W4-001, W4-007).

---

## 3. End-to-end traceability chain (requirements → release)

| Stage | Input artefacts | Output artefacts | Trace link required | Where it breaks (problem IDs) |
|---|---|---|---|---|
| Requirements engineering | Customer/OEM specs, regulations | SHRQ → SyRQ → SWRQ/HWRQ | Vertical link between abstraction levels | W1-001, W1-002, W1-005, W1-006 |
| System engineering & architecture | Requirements, constraints | System architecture, interface definitions (ICDs), network architecture | Req → architecture elements; interface → components | W1-007, W1-008, W1-016, W1-017, W1-018 |
| Software design & development | SWRQ, architecture | SWCs, BSW config, models, source code | Req → design → code | W1-003, W2-001, W2-012, W4-007 |
| Configuration & data management | Architecture, variants | ARXML, DBC, ECU extracts, variant bindings | Config → requirements; variant → test scope | W1-011, W1-013, W2-011, W2-015, W4-002–W4-005 |
| Software integration | Components, configs, hardware | Integrated ECU/network/vehicle software | Interface → integration test | W1-007, W2-012, W4-023, W5-008 |
| Testing & validation (XiL) | Requirements, code, configs | Test cases, results, coverage evidence | Req → test; test → result; MIL/SIL/HIL horizontal | W3-023, W3-024, W3-026, W3-030, W3-036, W4-029 |
| Safety & cybersecurity assurance | Requirements, hazards, threats | Safety case, CSMS evidence, FMEA/FMEDA | Hazard → requirement → mitigation → test | W1-014, W4-011, W4-012, W4-014, W4-015, W5-003, W5-004 |
| Release & manufacturing | Validated software, configs | Baseline, flash image, OTA package, EOL test scripts | Release → requirements/tests/configuration baseline | W2-010, W4-016, W4-020, W3-030 |
| Field monitoring & maintenance | Released software, telemetry | Defect reports, OTA status, warranty data | Field defect → requirement/design → fix | G3-012, G3-017, G3-021, G4-029 |

---

## 4. Horizontal traceability across MIL–SIL–HIL–vehicle

| From | To | What must be preserved | Status / problem IDs |
|---|---|---|---|
| MIL test cases | SIL verification | Reuse of model-level test cases against generated code | W3-002 (not reused); W3-003 (model instability) |
| SIL (vECU) | HIL | Fidelity of vECU vs target ECU; timing | W3-004, W3-007, W4-019 |
| HIL test cases | Vehicle tests | Scenario reuse safety; environment fidelity | W3-020, W3-012, W3-013, W3-014 |
| Test data & artefacts | All XiL stages | Versioning and reproducibility of inputs/outputs | W3-030 |
| Requirements | Tests at every XiL level | Req → test mapping survives model/code changes | W1-003, W3-023, W4-007 |

**Key gap:** Horizontal traceability across MIL–SIL–HIL is explicitly documented as lacking (W3-023); test reuse across levels is unsafe or not performed (W3-002, W3-020).

---

## 5. Standards requiring traceability (selected)

| Standard | Traceability requirement | Lifecycle scope | Problem IDs |
|---|---|---|---|
| ISO 26262 | Req → hazard → safety goal → safety requirement → design → implementation → test → evidence | All | W4-011, W4-012, W1-014 |
| Automotive SPICE (SWE.1–SWE.6) | Req → design → code → unit/integration test; bidirectional trace | Development | W4-013, W3-032 |
| ISO/SAE 21434 | Threat → requirement → control → verification → evidence | Cybersecurity | W4-014, W4-015, W5-004 |
| ISO 21448 (SOTIF) | Scenario → triggering condition → mitigation → validation evidence | ADAS/ADS | W4-017, W3-018, W1-004 |
| ISO 24089 / UNECE R156 | Software update package → version → vehicle configuration → SUMS evidence | Release/OTA | W4-016, W2-010, G3-017 |
| UNECE R155 | CSMS → vehicle type approval → supply-chain evidence | Cybersecurity | W4-015, W5-004 |
| MISRA C/C++ | Rule → deviation record → justification → verification | Code quality | W4-008, W4-009, W4-010 |

---

## 6. Tools involved and their traceability limitations

| Tool category | Examples | Strength | Limitation (problem IDs) |
|---|---|---|---|
| Requirements management | DOORS (Classic/Next), Polarion, Codebeamer, Jama Connect, Visure | Stores requirements and links | Legacy-heavy, hard to onboard (W4-006); links not semantically validated (W1-002, W1-003) |
| MBSE / systems modelling | Rhapsody, Enterprise Architect, Cameo/SysML | Architecture consistency | Low adoption, skill/tool gaps (W1-009, W5-007) |
| AUTOSAR configuration | EB tresos, DaVinci Developer/Configurator, SystemDesk | BSW/RTE generation | Lock-in, licensing cost (W4-004, W4-027); ARXML drift (W1-013, W4-002, W4-005) |
| Static analysis / code quality | QAC/PRQA, LDRA, Parasoft, Tessy | Rule checking | False positives, manual deviation (W4-008); undecidable rules (W4-010) |
| Test management / execution | VectorCAST, CANoe, Tessy, GoogleTest | Test execution | Interoperability gaps (W3-036, W4-029); coverage evidence management (W4-029) |
| CI/CD | Jenkins, GitLab CI, GitHub Actions | Automation | Generic pipelines unsuitable for safety-critical embedded (W4-020) |
| PLM/PDM | PLM systems | Product data | Integration gaps with ARXML engineering data (W4-024) |
| AI-assisted tooling | LLM-based assistants | Drafting, analysis | Trust, certification, IP/data constraints (W4-030, W5-016–W5-020, W5-025) |

---

## 7. Quantitative evidence (sourced, indicative)

| Metric | Value | Source | Confidence |
|---|---|---|---|
| Requirements-driven rework share of total rework | 35–45% | Capers Jones / Namcook Analytics (cited W1-012) | Medium (cross-industry, not automotive-specific) |
| Max product requirements per stakeholder requirement | up to 67 | Wang et al. 2026 (W1-002) | Medium–High |
| Practitioner agreement on "unconnected abstraction levels" | 72%+ | Liebel 2018 (W1-002) | High |
| Integration issues surfacing ~6 months before SOP | ~80% (unverified; no URL) | KPIT 2025 (cited W3) | Low–Medium (unverified) |
| Traditional feedback to dev team | 4–6 weeks | KPIT/HCLTech 2025 | Low–Medium (unverified) |
| SOP delays due to late software errors | >2 years | HCLTech 2025 | Low–Medium (unverified) |
| Only 22% could cite a clear MBSE definition | 22% | Purdue dissertation (W1-009) | Medium |

---

## 8. Gap summary (traceability-specific)

| Gap ID | Description | Primary problem IDs |
|---|---|---|
| TG-01 | No end-to-end traceability across L0–L11; tools store links but do not enforce semantic consistency | W1-003, W4-007, W4-001 |
| TG-02 | Links go stale after changes; no automated change-impact propagation | W1-005, W1-017, W4-007 |
| TG-03 | ML/AI elements (models, datasets, ODDs, scenarios) not covered by classic traceability | W1-004, W3-018, W3-029, W4-007 |
| TG-04 | Horizontal XiL traceability (MIL→SIL→HIL→vehicle) lacking | W3-023, W3-002, W3-020, W3-030 |
| TG-05 | Variant binding and configuration drift break req→config→test trace | W1-011, W2-011, W4-005, W4-002 |
| TG-06 | Safety/cybersecurity evidence maintenance under change is costly | W4-011, W4-012, W4-014, W4-015 |
| TG-07 | Supplier/OEM data exchange friction prevents cross-organisation traceability | W4-023, W4-024, W4-028, W5-014 |
| TG-08 | Compliance overhead vs. genuine safety work boundary unmeasured | W1-014, W4-011, W4-013 |

---

## 9. Unanswered research questions (traceability)

1. What is the actual density and correctness of trace links in production automotive programs? (W1-003)
2. What fraction of integration defects trace to incomplete/ambiguous requirements vs. implementation? (W1-006, CQ-006)
3. What is the automotive-specific cost-of-change multiplier (early vs. late requirement change)? (W1-012)
4. How often are trace links wrong vs. missing in production programs? (W1-002)
5. What fraction of compliance effort is genuine safety work vs. avoidable overhead? (W1-014)
6. How does ML traceability (models, datasets, ODDs) work in practice in ADAS programs? (W1-004)
7. What is the true cost/benefit of SiL-first vs. HiL-first integration across vehicle programs? (ch. 10)

---

## 10. Evidence and source references (selected)

- W1_requirements_system.md (W1-001 … W1-018)
- W2_autosar_middleware.md (W2-001, W2-010–W2-012, W2-015, W2-011)
- W3_testing_validation.md (W3-023, W3-024, W3-026, W3-030, W3-032, W3-036)
- W4_toolchain_standards.md (W4-001–W4-030)
- W5_skills_ai.md (W5-003, W5-004, W5-016–W5-020, W5-025)
- part2_ch10_integration.md (§10.6 quantitative evidence)
- KNOWLEDGE_INDEX.md (cross-cutting themes 1, 2, 6)
- DATA_VERIFY.md (verification status of numerical claims)

---

*End of artefact: requirements_to_release_traceability.md*
