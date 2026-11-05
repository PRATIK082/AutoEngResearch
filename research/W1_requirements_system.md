# W1 — Requirements Engineering and System Engineering/Architecture

**Workstream:** W1 (Requirements & System Engineering)  
**Scope:** Problem discovery, root-cause analysis, evidence, unanswered questions  
**Date:** 2026-11-05  
**Status:** Verify (Tier 2) — findings cross-checked against multiple sources; numerical claims only where sourced.

---

## 1. Workstream Summary

Requirements engineering (RE) and system engineering (SE) in automotive software are characterised by:

- **Multi-tier, multi-organisation information flows** (OEM → Tier-1 → Tier-2 → silicon vendors) with mismatched abstraction levels and weak feedback loops.
- **Natural-language requirements** that are ambiguous, underspecified, and implicitly assume context; refinement from stakeholder to product requirements is driven by architectural scope and missing context, not verbosity.
- **Tooling fragmentation**: DOORS (Classic/Next), Polarion, Codebeamer, Jama Connect, Visure, reqSuite, and spreadsheets coexist; traceability is manual, brittle, and often incomplete.
- **MBSE adoption is slow** due to cultural resistance, tool-integration gaps, model-governance immaturity, and lack of skilled modellers.
- **Architecture is in flux**: distributed ECUs → domain controllers → zonal/centralised HPC, introducing mixed-criticality integration, timing predictability, redundancy, and thermal/power challenges.
- **Timing analysis** remains largely unsolved for AUTOSAR software despite being safety-relevant.
- **Variant explosion** (hardware, software, region, regulation) is poorly supported by configuration/variant management.
- **Compliance overhead** (ISO 26262, ASPICE, ISO/SAE 21434, UNECE R155/R156) is large but partly genuine safety work; the boundary is unclear.

---

## 2. Problem Catalogue

### W1-001 — Ambiguous, underspecified natural-language requirements

- **Domain:** Requirements engineering
- **Lifecycle stage:** Requirements engineering (stakeholder → system → software)
- **Problem:** Requirements are predominantly written in natural language, making them susceptible to ambiguity, underspecification, and implicit assumptions. OEMs deliver high-level specs; suppliers fill gaps with assumptions.
- **Who experiences it:** OEM requirements engineers, Tier-1 systems engineers, embedded software developers.
- **Root cause:** Absence of formal/structured requirement languages at intake; organisational pressure to ship specs fast; lack of shared operational context.
- **Contributing factors:** Multi-tier handoffs; domain knowledge silos; legacy document-centric culture.
- **Technical limitations:** NLP-based quality checking is experimental; no universal automotive requirement ontology.
- **Human skill limitations:** Requirements-writing skill gaps; insufficient systems-thinking training.
- **Tool limitations:** ALM tools store text but do not enforce semantic quality.
- **Process limitations:** Reviews happen late; no mandatory context enrichment at intake.
- **Frequency/severity:** Pervasive; cited across multiple independent studies (Liebel 2018; Habibullah 2024; Wang 2026).
- **Time/cost impact:** Rework from requirements-driven defects is typically the largest rework category (35–45% of total rework cost per Capers Jones/Namcook Analytics, cited in W1-012).
- **Downstream effects:** Late defect discovery, integration failures, recalls, warranty cost.
- **Existing solutions:** DOORS templates, Polarion/Codebeamer workflows, AI requirement-quality assistants (experimental).
- **Why insufficient:** Templates enforce structure not semantics; AI assistants are not safety-certified.
- **Automation feasibility:** Medium (NLP quality checks, ambiguity detection).
- **AI applicability:** High for drafting/clarity; low for safety-critical sign-off.
- **Human approval:** Required for all safety-relevant requirements.
- **Evidence/sources:**
  - Liebel et al., "Organisation and Communication Problems in Automotive Requirements Engineering," Requirements Eng 23(1):145–167 (2018). https://link.springer.com/article/10.1007/s00766-016-0261-7
  - Habibullah et al., "Requirements and software engineering for automotive perception systems: an interview study," Requirements Eng 29:25–48 (2024). https://link.springer.com/article/10.1007/s00766-023-00410-1
  - Wang et al., "Bridging Stakeholder and Product Requirements: An Empirical Study of Requirement Engineering in the Automotive Industry," arXiv:2607.05632 (ASE 2026 Industry Showcase). https://arxiv.org/abs/2607.05632
- **Confidence:** High.
- **Unanswered questions:** What proportion of automotive requirements are genuinely ambiguous vs. intentionally flexible? How much rework is attributable to requirement ambiguity vs. late change?

### W1-002 — Unconnected abstraction levels across OEM/Tier-1/Tier-2

- **Domain:** Requirements engineering / system engineering
- **Lifecycle stage:** Requirements decomposition and system architecture
- **Problem:** Stakeholder (SHRQ), system, software (SWRQ), and hardware requirements are managed in separate tools/teams with mismatched granularity; traceability links are sparse or stale.
- **Root cause:** Organisational boundaries (OEM vs suppliers) and tool boundaries; no shared system model; refinement is manual.
- **Evidence:** Liebel P3 "Unconnected Abstraction Levels" (72%+ agreement among 14-case study practitioners). Wang et al. find refinement complexity driven by architectural scope and missing context, not verbosity; max 67 product requirements per stakeholder requirement in extreme decomposition cases.
- **Contributing factors:** Reusable spec requirements instantiated across many product contexts; no automated impact propagation.
- **Tool limitations:** DOORS/Polarion/Codebeamer store links but do not validate semantic correctness; cross-tool linking is brittle.
- **Process limitations:** Reviews per abstraction level; no end-to-end ownership.
- **Impact:** Late integration surprises; assumption-based updates.
- **Existing solutions:** MBSE (SysML), Polarion/Codebeamer traceability, Raiqon cross-artifact analysis.
- **Why insufficient:** MBSE adoption low; tool links can be present but wrong (Raiqon finding).
- **Automation feasibility:** Medium–High.
- **AI applicability:** Medium (semantic link suggestion, gap detection).
- **Human approval:** Required.
- **Confidence:** High.
- **Unanswered questions:** What is the actual density/completeness of trace links in production programs? How often are links wrong vs. missing?

### W1-003 — Requirements traceability burden and link quality

- **Domain:** Requirements engineering / compliance
- **Lifecycle stage:** All (requirements → design → code → test)
- **Problem:** Traceability is mandated by ISO 26262/ASPICE but creating and maintaining links is cumbersome; engineers fear creating wrong links, so often no links are created; links go stale after changes.
- **Root cause:** Manual linking cost; tool friction; change propagation not automated.
- **Evidence:** CEUR traceability paper (automated traceability between requirements and AUTOSAR models) notes engineers often create no trace links because of fear of wrong links. Visure, mgtechsoft, Wind River blogs document fragmented toolchains and manual processes. SodiusWillert/DOORS integration notes.
- **Tool limitations:** Links stored but not semantically validated; cross-domain (ML models, datasets, ODDs) traceability is harder (Habibullah 2024).
- **Impact:** Compliance evidence gaps; audit findings; recall risk.
- **Existing solutions:** Polarion/Codebeamer/Jama traceability modules; automated traceability research (CEUR).
- **Why insufficient:** Automation is research-stage; ML elements (models, datasets, ODDs) not covered by classic traceability.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (link suggestion, staleness detection).
- **Human approval:** Required for safety evidence.
- **Confidence:** High.
- **Unanswered questions:** Quantified cost of traceability maintenance per program? False-positive rate of automated link suggestion?

### W1-004 — ML/AI-specific requirements and traceability gaps (ADAS/perception)

- **Domain:** Requirements engineering (AI/ML)
- **Lifecycle stage:** Requirements and validation
- **Problem:** ML systems require requirements on data, ODD, monitors, and KPIs that classic RE does not handle; traceability must include models, datasets, scenarios; failures are hard to trace to source.
- **Root cause:** RE methods designed for deterministic software; ML is data-driven and opaque.
- **Evidence:** Habibullah et al. 2024 (Requirements Eng): difficulties in breakdown, ML opaqueness, more elements to trace (ODDs, scenarios, training data); traceability discussed in 4/7 interviews. SAE deep-learning requirements analysis paper (2023).
- **Impact:** Safety case gaps for ADAS/AV; regulatory uncertainty.
- **Existing solutions:** Emerging SOTIF (ISO 21448), ISO/PAS 8800 (AI safety) references in ISO 26262 Ed.3.
- **Why insufficient:** Standards still maturing; tooling absent.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Medium (as tool, not authority).
- **Human approval:** Mandatory.
- **Confidence:** Medium–High.
- **Unanswered questions:** How do OEMs currently specify training-data requirements? What is the state of practice for ODD specification?

### W1-005 — Late and uncontrolled requirement changes

- **Domain:** Requirements engineering / change management
- **Lifecycle stage:** All stages
- **Problem:** Requirements change during development; suppliers may not receive updates in time; changes ignore cross-domain impacts; system-level requirements not updated after changes.
- **Root cause:** No authority to reject architecture-breaking changes; assumption-based updates without verification; lack of systems visibility.
- **Evidence:** LinkedIn systems-engineering article (Winner & Maurer): change requests ignore cross-domain impacts; late alterations break integrated architecture; incoherent version control across suppliers; no authority to reject changes. mgtechsoft: requirement updated by OEM without supplier understanding impact.
- **Impact:** Rework, schedule slip, invalid test results, integration failures.
- **Existing solutions:** Change control boards, ALM change management (Polarion/Codebeamer).
- **Why insufficient:** CCBs are slow; tooling does not propagate impact automatically.
- **Automation feasibility:** Medium (change-impact analysis).
- **AI applicability:** Medium (impact prediction).
- **Human approval:** Required.
- **Confidence:** High.
- **Unanswered questions:** Frequency and cost of late changes per program; effectiveness of CCBs.

### W1-006 — Incomplete requirements handed to suppliers (assumption-driven development)

- **Domain:** System engineering / supplier management
- **Lifecycle stage:** System requirements and architecture
- **Problem:** OEMs provide high-level specs leaving suppliers to fill details based on assumptions; interactions between functions not specified.
- **Root cause:** OEM capacity/time; unclear ownership of system-level requirements; inside-out component design.
- **Evidence:** LinkedIn SE article: "Incomplete requirements: OEMs provide high-level specifications, leaving suppliers to fill in details based on assumptions." Root-cause example: OEM specified individual functions but not interactions; supplier implemented to written requirements; integration revealed unconsidered interactions.
- **Impact:** Integration becomes discovery; redesign, rework, retest; launch slip; warranty cost.
- **Existing solutions:** Systems engineering frameworks, MBSE, interface control documents (ICDs).
- **Why insufficient:** ICDs are static and drift; MBSE adoption low.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** High.
- **Unanswered questions:** What % of integration defects trace to incomplete requirements vs. implementation errors?

### W1-007 — Unstable interfaces and interface governance gaps

- **Domain:** System engineering / interface management
- **Lifecycle stage:** Architecture and integration
- **Problem:** Interface definitions ambiguous, allowing different interpretations; requirements/interfaces change mid-development; suppliers may not receive updates; no single source of truth for interfaces.
- **Root cause:** Distributed development by many teams; lack of interface governance authority; proprietary formats.
- **Evidence:** LinkedIn SE article (unstable interfaces); BMW DATE paper (precise interface description prerequisite for integration; standardized BSW simplifies); SemiEngineering E/E architecture complexity.
- **Impact:** Mismatched interfaces; integration failures; over-engineering (oversized ECUs, redundant sensors).
- **Existing solutions:** AUTOSAR standardized interfaces; ICDs; ARXML interface descriptions.
- **Why insufficient:** AUTOSAR standardizes BSW not application interfaces; ARXML drift; ICDs not enforced.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** High.
- **Unanswered questions:** How much over-engineering cost is attributable to interface uncertainty?

### W1-008 — Inconsistent architecture across suppliers/OEM

- **Domain:** System engineering / architecture
- **Lifecycle stage:** Architecture
- **Problem:** Suppliers develop to their own architectural vision, misaligned with OEM's evolving architecture; no shared system-level requirements; no architecture governance.
- **Root cause:** Lack of top-down architecture authority; late architecture definition; organisational silos.
- **Evidence:** LinkedIn SE article; Winner & Maurer: "No authority to reject changes that break architecture"; technical debt accumulates.
- **Impact:** Technical debt, future cost growth, recalls.
- **Existing solutions:** Architecture review boards, MBSE, platform strategies.
- **Why insufficient:** Review boards lack enforcement; platforms are slow to evolve.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Low (judgement-heavy).
- **Human approval:** Required.
- **Confidence:** Medium–High.
- **Unanswered questions:** How is architecture authority structured in practice across OEMs?

### W1-009 — MBSE adoption barriers (cultural, skill, tool, governance)

- **Domain:** System engineering / MBSE
- **Lifecycle stage:** Architecture and design
- **Problem:** MBSE promised to replace document-centric engineering but adoption is limited by cultural resistance, modelling skill gaps, tool integration, model governance, and cost; only 22% of systems-engineering participants could cite a clear MBSE definition.
- **Root cause:** Perception of MBSE as diagramming; lack of demonstrated ROI; training gaps; regulatory acceptance uncertainty.
- **Evidence:** Purdue dissertation (Maheshwari): industry perceives MBSE as diagramming; skepticism about regulatory acceptance. SodiusWillert: resistance to change is primary challenge; workload perception. MDPI 2025: limited tool expertise and skilled personnel shortage. BusinessResearchInsights: 66% of enterprises rely on MBSE frameworks (market stat; verify). MG Tech Soft: legacy data, lack of modelling expertise, organisational resistance, tool integration, model governance, cross-functional adoption.
- **Impact:** Continued document-centric silos; traceability gaps; rework.
- **Existing solutions:** Cameo/SysML tools, cloud MBSE, SysML v2 (2025 compatibility updates).
- **Why insufficient:** SysML v2 migration cost; cultural change slow; tool interoperability still partial.
- **Automation feasibility:** Low (human change).
- **AI applicability:** Medium (model generation assistance).
- **Human approval:** Required.
- **Confidence:** Medium (market stats need verification).
- **Unanswered questions:** Actual MBSE adoption rate in automotive vs. aerospace; ROI evidence; SysML v2 uptake.

### W1-010 — Timing analysis and predictability unsolved for AUTOSAR

- **Domain:** System engineering / architecture / timing
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** Timing predictability and timing analysis remain largely unsolved for automotive/AUTOSAR software; AUTOSAR standard lacks timing aspects; cache effects complicate analysis; application timing depends on implementation details (ECU mapping, bus) that AUTOSAR abstracts away.
- **Root cause:** Mismatch between industry requirements and academic timing analysis; high complexity; abstraction mismatch.
- **Evidence:** TU Munich timing challenges paper (mediatum): timing analysis "important and still largely unsolved problem"; 80–100 ECUs in high-end cars; cache effects. TU Braunschweig: "AUTOSAR standard lacks aspects of timing and performance." AUTOSAR FO Timing Analysis TR (R24-11) documents use cases but acknowledges complexity. TUM paper: need for timing model bridging system and implementation.
- **Impact:** Unsafe timing estimates; pessimistic/unsafe estimations; late timing violations.
- **Existing solutions:** AUTOSAR timing extensions; formal timing verification research (KAIST); probabilistic timing analysis (HAL 2025).
- **Why insufficient:** Research-stage; not integrated into standard toolchains; not certified.
- **Automation feasibility:** Medium (tooling).
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Confidence:** High.
- **Unanswered questions:** How often do timing violations surface in integration vs. design? Adoption of AUTOSAR timing extensions?

### W1-011 — Variant explosion and configuration management

- **Domain:** System engineering / configuration management
- **Lifecycle stage:** Architecture, configuration, release
- **Problem:** Vehicle variants (hardware, software, region, regulation) explode combinatorially; ARXML variant management and binding are complex; configuration consistency across tools is fragile.
- **Root cause:** Product-line complexity; lack of integrated systems-platform engineering (Holsten 2023); ARXML variation points/binding complexity.
- **Evidence:** Holsten et al. 2023 (E/E platforms): lack of concepts for integrated systems-platform engineering; variants split due to diverging requirements. AUTOSAR ARXML Serialization Rules: variant binding/variation points complexity. ARXML guide: tag reordering causes noisy diffs; sync with Simulink models.
- **Impact:** Configuration errors; invalid test coverage; release delays.
- **Existing solutions:** ARXML EvaluatedVariantSet/VariationPoint; MATLAB/Simulink AUTOSAR export; PLM variant management.
- **Why insufficient:** Tool-specific; no cross-tool variant consistency checking; noisy diffs impede review.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (variant consistency checks).
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Quantified cost of variant management; number of variants per platform in practice.

### W1-012 — Requirements-driven rework cost and late defect discovery

- **Domain:** Cross-cutting
- **Lifecycle stage:** All
- **Problem:** Requirements-driven rework is the single largest rework category (35–45% of total rework cost); late changes multiply cost; re-planning overhead is invisible.
- **Root cause:** See W1-001/005/006.
- **Evidence:** ReworkCost.com citing Capers Jones/Namcook Analytics: requirements-driven rework 35–45% of total rework cost across industries. General software rule: late changes cost far more than early (cost-of-change curve); automotive-specific quantification not found in this pass.
- **Impact:** Cost overruns, schedule slip.
- **Existing solutions:** Agile/short cycles (reduces cost per change, not volume).
- **Why insufficient:** Agile conflicts with hardware cycles; volume of changes preserved/increased.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** Medium (figure is cross-industry, not automotive-specific).
- **Unanswered questions:** Automotive-specific rework percentages; cost-of-change multiplier in automotive.

### W1-013 — ARXML tool interoperability and configuration drift

- **Domain:** Configuration and engineering data management
- **Lifecycle stage:** Design, integration
- **Problem:** ARXML files are central but tools generate XML tags in different orders causing noisy diffs; interface definitions in ARXML drift from Simulink models; consistency/compatibility across tools and teams is hard.
- **Root cause:** No canonical serialization/enforcement; tool-specific generators; manual sync.
- **Evidence:** ARXML guide (autosar.io): tag reordering; Simulink/ARXML sync; integration failures when interface definitions mismatch. ZipRecruiter ARXML challenges. DBC Utility blog: format confusion at handoffs.
- **Impact:** Review difficulty; integration failures; handoff errors.
- **Existing solutions:** AUTOSAR serialization rules; MATLAB export.
- **Why insufficient:** Rules not enforced by all tools; drift persists.
- **Automation feasibility:** High (canonicalization, diff normalization, consistency checks).
- **AI applicability:** Medium.
- **Human approval:** For safety-relevant config.
- **Confidence:** High.
- **Unanswered questions:** Rate of ARXML-related integration failures; tool compliance with serialization rules.

### W1-014 — Compliance overhead and standards fragmentation

- **Domain:** Standards/regulations
- **Lifecycle stage:** All
- **Problem:** ISO 26262, ASPICE, ISO/SAE 21434, ISO 21448 (SOTIF), ISO 24089, ISO/PAS 8800, UNECE R155/R156, MISRA create large documentation/evidence burden; overlap and cross-references are complex; ISO 26262 Ed.3 adds SOTIF/AI cross-references.
- **Root cause:** Multiple standards with overlapping scopes; safety + cybersecurity + AI governance; OEM-specific interpretations.
- **Evidence:** mgtechsoft compliance landscape; SRES ISO 26262 Ed.3 (cross-references to SOTIF, ISO/PAS 8800); Automotive IQ ISO 26262 guide; Ansys/Parasoft.
- **Impact:** Administrative overhead; slower release; evidence gaps.
- **Existing solutions:** Integrated compliance tooling (Visure FMEA integration); ASPICE assessments.
- **Why insufficient:** Overlap not eliminated; tailoring is organisation-specific; AI governance emerging.
- **Automation feasibility:** Medium (evidence collection, cross-reference mapping).
- **AI applicability:** Medium (standards-aware assistants).
- **Human approval:** Mandatory.
- **Confidence:** Medium.
- **Unanswered questions:** What fraction of compliance effort is genuine safety work vs. avoidable overhead? Cost of ASPICE assessments?

### W1-015 — System engineering skill and workforce gaps

- **Domain:** Human skills
- **Lifecycle stage:** System engineering
- **Problem:** Shortage of systems engineers, MBSE modellers, requirements engineers; onboarding/training time; knowledge silos and single-person expertise; communication gaps between software/hardware/system teams.
- **Root cause:** Education lags industry needs; high complexity; turnover.
- **Evidence:** MDPI 2025 MBSE workforce competencies (limited tool expertise, skilled personnel shortage); LinkedIn SE article; Purdue dissertation.
- **Impact:** Dependency on individuals; quality variance; delays.
- **Existing solutions:** Training programs, certifications.
- **Why insufficient:** Training takes years; industry changes faster than curricula.
- **Automation feasibility:** Low (human judgement).
- **AI applicability:** Medium (assistants, knowledge capture).
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Quantified shortage; training time; turnover impact.

### W1-016 — Transition to zonal/centralised architecture: new engineering challenges

- **Domain:** System engineering / architecture
- **Lifecycle stage:** Architecture, integration, validation
- **Problem:** Shift from distributed ECUs to zonal/centralised HPC introduces mixed-criticality integration on shared compute, resource allocation/scheduling, redundancy/fault tolerance, thermal/power management, cybersecurity, and supplier ecosystem disruption; "thinking vs acting" separation.
- **Root cause:** Consolidation of functions onto fewer powerful computers; need for virtualisation; new failure modes (single point of failure).
- **Evidence:** Promwad 2025 (redundancy critical; heat/power; single processor failure risk); Accelerat 2025 (mixed-criticality integration, cybersecurity, runtime reliability); Frontiers 2025 (resource allocation, scheduling); ArXiv 2412.01840 (heat management, ECU integration, cybersecurity); Bosch (thinking/acting separation); McKinsey 2026 market outlook (zonal/central computing).
- **Impact:** New failure modes; cost; validation complexity; supplier renegotiation.
- **Existing solutions:** Virtualisation, hypervisors, zonal ECUs, service-oriented architecture (SOA).
- **Why insufficient:** Virtualisation overhead; SOA portability limited (Tieto 2025: apps still platform-specific); standards evolving.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** Medium–High.
- **Unanswered questions:** Failure rates of consolidated compute; cost of redundancy; maturity of automotive virtualisation.

### W1-017 — Architecture consistency and change management without systems visibility

- **Domain:** System engineering / change management
- **Lifecycle stage:** Architecture
- **Problem:** Change requests ignore cross-domain impacts; system-level requirements not updated after changes; incoherent version control across suppliers; assumption-based updates without verification; no authority to reject architecture-breaking changes.
- **Root cause:** Lack of systems visibility; no single source of truth; distributed ownership.
- **Evidence:** LinkedIn SE article (Winner & Maurer): change management without systems visibility is major source of instability; list of problems. Spread.ai 2025: dependencies buried in disconnected legacy tools; change impacts unclear; redundant tests.
- **Impact:** Late integration errors; schedule slip; cost overruns.
- **Existing solutions:** PLM, ALM, digital thread initiatives.
- **Why insufficient:** Tools are siloed; no end-to-end dependency graph.
- **Automation feasibility:** Medium–High (dependency graph, change-impact analysis).
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Adoption and effectiveness of digital-thread initiatives.

### W1-018 — Network architecture and timing of communication (bus/Ethernet)

- **Domain:** System engineering / network
- **Lifecycle stage:** Architecture, integration
- **Problem:** Integrating new networks (Ethernet) into legacy multi-bus architectures; timing constraints across distributed ECUs; function-based vs ECU-based timing constraints; gateway design.
- **Root cause:** Heterogeneous legacy networks; distributed development by many teams; timing constraints not captured in AUTOSAR.
- **Evidence:** AUTOSAR FO Timing Analysis TR; DATE 2009 timing paper (FlexRay scheduling); TUM timing paper.
- **Impact:** Timing violations; network congestion; integration delays.
- **Existing solutions:** AUTOSAR communication stacks; timing extensions; network simulation.
- **Why insufficient:** Timing analysis unsolved; gateway bottlenecks.
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Confidence:** Medium.
- **Unanswered questions:** Prevalence of network timing violations in production.

---

## 3. Cross-Cutting Observations

1. **Requirements and architecture problems are coupled**: incomplete requirements + no system-level architecture + no interface governance = integration as discovery (LinkedIn SE root-cause example).
2. **Tool fragmentation is a root cause, not just a symptom**: ALM tools store artifacts but do not enforce semantic consistency; cross-tool links are brittle; ARXML serialization is not canonical.
3. **Compliance is both genuine safety work and avoidable overhead**; the boundary is organisation-specific and poorly measured.
4. **MBSE is the proposed cure but is itself blocked** by culture, skills, tooling, and governance.
5. **Timing analysis is a safety-relevant unsolved problem** that underpins the zonal/centralised transition.
6. **ML/AI introduces new RE categories** (data, ODD, monitors) that classic RE and traceability do not cover.

---

## 4. Evidence and Source Index (selected)

- Liebel et al. 2018, Requirements Eng 23(1):145–167 — organisation/communication problems in automotive RE.
- Habibullah et al. 2024, Requirements Eng 29:25–48 — automotive perception systems RE interview study.
- Wang et al. 2026, arXiv:2607.05632 (ASE 2026) — stakeholder vs product requirements empirical study (Infineon, 8,082 SHRQ / 5,870 PRQ).
- Broy, "Challenges in Automotive Software Engineering," ICSE (TU Munich).
- TU Munich timing challenges paper (mediatum 1197943).
- TU Braunschweig, "The Need of a Timing Model for the AUTOSAR software."
- AUTOSAR FO Timing Analysis TR (R24-11).
- Holsten et al. 2023, E/E platforms in automotive industry.
- Maheshwari (Purdue dissertation), industrial MBSE adoption challenges.
- MDPI 2025, MBSE workforce competencies.
- SodiusWillert, MBSE challenges.
- MG Tech Soft, MBSE guide and compliance landscape.
- Promwad 2025, Accelerat 2025, Frontiers 2025, ArXiv 2412.01840 — zonal/centralised architecture challenges.
- Spread.ai 2025 — SDV execution gap.
- ReworkCost.com / Capers Jones — requirements-driven rework 35–45% (cross-industry).
- AUTOSAR ARXML Serialization Rules; autosar.io ARXML guide; DBC Utility blog.
- SRES ISO 26262 Ed.3; Automotive IQ ISO 26262 guide.

---

## 5. Unanswered Research Questions (consolidated)

1. What is the actual density and correctness of trace links in production automotive programs?
2. What fraction of integration defects trace to incomplete/ambiguous requirements vs. implementation?
3. What is the automotive-specific cost-of-change multiplier (early vs. late requirement change)?
4. What is the real MBSE adoption rate and demonstrated ROI in automotive vs. aerospace?
5. How often do timing violations surface in production (design vs. integration)?
6. What fraction of compliance effort is genuine safety work vs. avoidable overhead?
7. How are architecture authority and interface governance structured across OEMs?
8. What is the failure rate and cost of consolidated (zonal/centralised) compute?
9. How do OEMs currently specify training-data and ODD requirements for ML components?
10. What is the effectiveness of change control boards in automotive programs?

---

## 6. Limitations of This Workstream

- Numerical figures (35–45% rework; 66% MBSE adoption; 22% MBSE definition) are from secondary/cross-industry or market sources and are not automotive-specific verified data.
- No direct access to OEM proprietary data; findings rely on published studies, vendor blogs, and open papers.
- Tool-specific claims reflect vendor positioning; treat as indicative.
- Some sources are vendor marketing; contradictory evidence not fully resolved.
- Tier 2 (Verify) depth; Tier 3 (Auditor) would require full pagination of all sources and additional independent verification.
---

*End of W1 report.*

---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

