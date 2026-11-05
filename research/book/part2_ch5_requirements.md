# Part II — Chapter 5: Requirements Engineering

**Book:** *The State of Automotive Software Engineering*  
**Part:** II — Complete Automotive Software Lifecycle  
**Chapter:** 5 — Requirements Engineering  
**Source workstreams:** W1 (Requirements & System Engineering), W4 (Toolchain & Standards), W5 (Skills & AI)  
**Problem IDs in scope:** W1-001, W1-002, W1-003, W1-004, W1-005, W1-006, W1-012, W1-014, W1-015, W4-006, W4-007, W5-006

---

## 5.0 Chapter purpose and problem map

This chapter examines requirements engineering (RE) as the first and most consequential stage of the automotive software lifecycle. The research record shows that RE is not a documentation exercise that precedes "real" engineering; it is the stage at which ambiguity, ownership gaps, and abstraction mismatches are introduced and then amplified through every downstream stage. The problems below recur across OEM, Tier-1, and Tier-2 contexts and are cited by multiple independent sources.

| Problem ID | Title | Primary root cause (short) |
|---|---|---|
| W1-001 | Ambiguous, underspecified natural-language requirements | Natural language at intake; no shared operational context |
| W1-002 | Unconnected abstraction levels across OEM/Tier-1/Tier-2 | Organisational and tool boundaries; manual refinement |
| W1-003 | Requirements traceability burden and link quality | Manual linking cost; change propagation not automated |
| W1-004 | ML/AI-specific requirements and traceability gaps | RE methods designed for deterministic software |
| W1-005 | Late and uncontrolled requirement changes | No authority to reject architecture-breaking changes |
| W1-006 | Incomplete requirements handed to suppliers (assumption-driven development) | OEM capacity/time; unclear system-requirements ownership |
| W1-012 | Requirements-driven rework cost and late defect discovery | Compounding effect of W1-001/005/006 |
| W1-014 | Compliance overhead and standards fragmentation | Multiple overlapping standards; OEM-specific interpretation |
| W1-015 | System engineering skill and workforce gaps | Education lags industry; turnover |
| W4-006 | Requirements management tools legacy-heavy, hard to onboard | Legacy DOORS-centric tooling; fragmented ALM landscape |
| W4-007 | End-to-end traceability gaps (req → code → tests → safety) | Manual links; no semantic validation of links |
| W5-006 | Requirements engineering skills gaps | Training and onboarding deficits |

---

## 5.1 The automotive requirements stack

Automotive requirements are not a single list. They are a layered, multi-organisation construct that typically includes:

1. **Stakeholder requirements (SHRQ)** — customer, regulatory, and brand-level expectations.
2. **System requirements (SYRQ)** — vehicle- and function-level behaviour, performance, safety, and interface constraints.
3. **Software requirements (SWRQ)** — ECU/application-level behaviour, often decomposed by supplier.
4. **Hardware requirements (HWRQ)** — silicon, board, sensor, and actuator constraints.

In practice these layers are owned by different organisations (OEM, Tier-1, Tier-2, silicon vendors) and stored in different tools. Wang et al. (ASE 2026 Industry Showcase) report empirical decomposition ratios of up to 67 product requirements per stakeholder requirement in extreme cases across 8,082 stakeholder and 5,870 product requirements at Infineon. The abstraction gap is therefore not a paperwork issue; it is a structural feature of the supply chain (W1-002, W1-006).

A recurring pattern is that OEMs deliver high-level specifications and suppliers fill gaps with assumptions. When interactions between functions are not specified, integration becomes discovery (W1-006). This is the single most cited root cause of late integration surprises in the research record.

---

## 5.2 Problem deep dives

### 5.2.1 Ambiguity and underspecification (W1-001, W1-005, W1-006)

Requirements are predominantly written in natural language. They are susceptible to ambiguity, underspecification, and implicit assumptions. ALM tools (DOORS, Polarion, Codebeamer, Jama Connect, Visure, reqSuite) store text but do not enforce semantic quality (W1-001). Reviews happen late, and there is no mandatory context enrichment at intake.

The consequence is that OEMs deliver specs that are intentionally or unintentionally incomplete; suppliers implement to the written text; and unconsidered interactions surface at integration (W1-006). Late changes then arrive without cross-domain impact analysis (W1-005). Liebel et al. (Requirements Eng 23(1):145–167, 2018) document organisation and communication problems in automotive RE; Habibullah et al. (Requirements Eng 29:25–48, 2024) document perception-system RE difficulties including breakdown and opacity.

**AI applicability (W1-001):** High for drafting and clarity checks; low for safety-critical sign-off. NLP-based quality checking is experimental and not safety-certified. Human approval is required for all safety-relevant requirements.

### 5.2.2 Abstraction-level disconnect and traceability (W1-002, W1-003, W4-007)

Stakeholder, system, software, and hardware requirements are managed in separate tools and teams with mismatched granularity. Traceability links are sparse, stale, or wrong. Raiqon's cross-artifact analysis finding (cited in W1-002) is that links can be present but semantically wrong — a more dangerous state than missing links.

Traceability is mandated by ISO 26262 and ASPICE, yet creating and maintaining links is cumbersome. Engineers often create no links at all because of fear of creating wrong ones (CEUR automated-traceability paper, cited in W1-003). Cross-domain traceability — covering ML models, datasets, operational design domains (ODDs), and scenarios — is harder still (Habibullah 2024).

**Existing solutions and why they are insufficient:** Polarion, Codebeamer, and Jama traceability modules store links but do not validate semantic correctness; automated traceability is research-stage; ML elements are not covered by classic traceability (W1-003, W4-007).

### 5.2.3 ML/AI-specific requirements (W1-004)

Machine-learning components require requirements on data, ODD, monitors, and KPIs that classic RE does not handle. Traceability must include models, datasets, and scenarios. Failures are hard to trace to source because ML is data-driven and opaque. Standards (SOTIF ISO 21448, ISO/PAS 8800) are still maturing and tooling is largely absent (W1-004, W1-014). This is a growing gap as ADAS/ADS content increases.

### 5.2.4 Late and uncontrolled change (W1-005, W1-017)

Requirements change during development. Suppliers may not receive updates in time; changes ignore cross-domain impacts; system-level requirements are not updated after changes; there is no authority to reject architecture-breaking changes (W1-005, W1-017). Change control boards exist but are slow, and tooling does not propagate impact automatically. Spread.ai (2025) notes dependencies buried in disconnected legacy tools, making change impact unclear and causing redundant tests.

### 5.2.5 Requirements tooling (W4-006)

Requirements management tools in automotive are legacy-heavy (DOORS Classic/Next, Polarion, Codebeamer) and hard to onboard. New engineers face steep learning curves, and tool fragmentation means requirements, architecture, and test artifacts live in different systems with brittle cross-tool links. This compounds every other RE problem (W4-006, W4-001).

### 5.2.6 Compliance and standards fragmentation (W1-014)

ISO 26262, ASPICE, ISO/SAE 21434, SOTIF (ISO 21448), ISO 24089, ISO/PAS 8800, UNECE R155/R156, and MISRA create a large documentation and evidence burden. Overlap and cross-references are complex; ISO 26262 Ed.3 adds SOTIF/AI cross-references. The boundary between genuine safety work and avoidable administrative overhead is organisation-specific and poorly measured (W1-014). See also Chapter 6 and Part IV.

### 5.2.7 Requirements-driven rework cost (W1-012)

Requirements-driven rework is the single largest rework category, estimated at 35–45% of total rework cost (Capers Jones/Namcook Analytics, cited in W1-012). This figure is cross-industry, not automotive-specific, and must not be treated as a verified automotive statistic. Nevertheless, multiple independent sources agree that late requirement changes multiply cost and that re-planning overhead is largely invisible (W1-005, W1-012).

### 5.2.8 Skills (W5-006, W1-015)

Requirements engineering skill gaps are documented across W1-015 and W5-006: insufficient systems-thinking training, weak requirements-writing skill, and onboarding delays. Communication gaps between software, hardware, and system teams amplify ambiguity (W1-015, W5-014). These are structural rather than individual: education lags industry needs, and industry changes faster than curricula.

---

## 5.3 Cross-cutting root causes

1. **Natural language is the default intake format**, and no universal automotive requirement ontology exists (W1-001).
2. **Tooling stores artifacts but does not enforce semantic consistency** across abstraction levels or organisations (W1-002, W1-003, W4-006, W4-007).
3. **Ownership of system-level requirements is unclear** between OEM and suppliers, producing assumption-driven development (W1-006).
4. **Change impact is not propagated automatically**, so late changes break integrated architecture (W1-005, W1-017).
5. **Compliance multiplies documentation burden** without clarifying the genuine-safety-work boundary (W1-014).
6. **RE skills are scarce and slow to develop** (W1-015, W5-006).

---

## 5.4 Evidence limitations

- The 35–45% rework figure is cross-industry (Capers Jones), not automotive-specific (W1-012).
- Survey and market statistics (e.g., MBSE reliance 66%) are flagged as unverified in W1 and CROSS_CHECK.
- No direct access to OEM proprietary data; findings rely on published studies, vendor blogs, and open papers.
- Tool-specific claims reflect vendor positioning and should be treated as indicative.

---

## 5.5 Cross-references

- **Chapter 6 (System Engineering and Architecture):** interface governance (W1-007), architecture consistency (W1-008, W1-017), MBSE adoption (W1-009), timing (W1-010), zonal architecture (W1-016).
- **Chapter 7 (Software Design and Development):** embedded skills and code quality (W5-001, W4-008–W4-010).
- **Chapter 8 (AUTOSAR and Middleware):** ARXML/config drift (W1-013) and classic/adaptive interop (W2-002).
- **Chapter 9 (Configuration and Engineering Data):** variant explosion (W1-011) and ARXML interoperability (W1-013).
- **Chapter 11 (Testing and Validation):** test spec quality and traceability (W3-024, W3-023) are downstream of RE quality.
- **Part VI:** requirements ambiguity → test spec quality → traceability is a documented cross-domain dependency (CROSS_QUESTIONS CQ-001, CQ-003, CQ-006).

---

*End of Chapter 5.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

