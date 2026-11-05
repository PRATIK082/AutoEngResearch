# W4 — Engineering Tools and Standards/Compliance

**Workstream:** W4 (Toolchain & standards)  
**Scope:** Automotive engineering tool ecosystem and standards/regulatory compliance affecting software development, integration, validation, release and maintenance.  
**Status:** Research report (problem discovery; no solutions proposed).  
**Method:** Web research across vendor documentation, standards bodies, industry publications, practitioner forums, academic papers. Evidence tier: Verify (task-directed; cross-checked where feasible).  
**Problem ID prefix:** W4-  
**Date compiled:** 2026-11-05

---

## 1. Executive summary

Automotive software engineering is constrained less by the absence of tools than by the **fragmentation, heterogeneity and poor interoperability** of the tools that exist, combined with **compliance regimes whose administrative overhead scales faster than engineering value**. The dominant pain points are:

1. **Toolchain fragmentation and proprietary data islands** — OEMs and Tier-1s assemble 10–30+ tools (requirements, modelling, AUTOSAR config, network analysis, HIL, static analysis, CI) that exchange data via file-based, often lossy transformations; no end-to-end digital thread exists in practice.
2. **AUTOSAR configuration tool lock-in** — DaVinci/EB tresos/SystemDesk dominate Classic AUTOSAR BSW configuration; high licence cost and proprietary project formats make switching or parallel use painful; ARXML itself is standardised but tool-specific extensions and version drift fragment it.
3. **Standards/compliance overhead** — ISO 26262, ASPICE, ISO/SAE 21434, UNECE R155/R156, ISO 24089, SOTIF and MISRA/CERT impose documentation, traceability and process evidence demands that are largely manual and disproportionate to the underlying safety value in many areas.
4. **Validation infrastructure scarcity** — HIL benches are capital-intensive and scheduled; vECUs promise shift-left but fidelity and integration gaps limit trust; scenario coverage for SOTIF (unknown unsafe scenarios) does not scale.
5. **Embedded/CI mismatch** — generic CI/CD pipelines are unsuitable for safety-critical embedded software; certification-grade traceability, reproducibility and tool qualification are bolted on rather than native.

The problems below are recorded in the structured problem-record format requested by the master prompt.

---

## 2. Research methodology and evidence notes

- Sources: vendor pages (dSPACE, Vector, ETAS, Elektrobit, MathWorks, Parasoft, LDRA, Perforce/PRQA, PTC, Siemens, IBM), standards bodies (ISO, UNECE, AUTOSAR, MISRA), industry publications (Automotive IQ, dSPACE Insights, MDPI, arXiv, Nature), practitioner forums (Reddit, StackOverflow), open-source projects (GitHub topics, ARForge, ParseX).
- Quantitative claims are only included where a source supports them; otherwise marked as estimates/hypotheses.
- Confidence: High = multiple independent authoritative sources; Medium = single authoritative or several secondary; Low = practitioner anecdote or single secondary source.
- Limitations: No access to paid market reports; OEM-internal data unavailable; figures from vendor marketing treated cautiously.

---

## 3. Problem catalogue

### W4-001 — Fragmented heterogeneous toolchain with poor end-to-end interoperability

- **Domain:** Engineering tools / cross-lifecycle
- **Lifecycle stage:** All stages (requirements → release)
- **Problem description:** Automotive programmes assemble dozens of specialised tools (requirements management, system modelling, AUTOSAR authoring, network simulation, static analysis, HIL, PLM) that were not designed to interoperate; data moves via manual export/import, scripts and spreadsheets, producing a non-digital, error-prone thread.
- **Actual engineering scenario:** An OEM updates a signal definition in a system model; the change must be re-entered in CAN database (DBC), ARXML, ECU configuration, test cases and safety analysis by different engineers using different tools, with no automated propagation.
- **Who experiences:** System engineers, integration engineers, test engineers, configuration managers.
- **Root cause:** Historical domain-specific tool evolution; vendor lock-in and proprietary formats; absence of a neutral, versioned, machine-readable engineering data backbone; organisations optimising tool purchase per function rather than flow.
- **Contributing factors:** Legacy programmes; supplier-specific toolchains; compliance needing evidence trails; M&A inheriting disparate stacks.
- **Technical limitations:** File-based exchange; schema/version mismatches; no semantic diff; loss of metadata during conversion.
- **Human skill limitations:** Engineers become tool operators; few understand cross-tool data lineage.
- **Tool limitations:** Tools optimise single-domain UX; APIs/licensing restrict automation; batch/CLI interfaces often missing.
- **Process limitations:** Change control processes assume manual handoffs; no single source of truth.
- **Frequency/severity:** Continuous; high severity for large programmes.
- **Time/cost impact:** Hypothesis: 10–30% of engineering effort spent on data transformation/reconciliation (not quantified by independent source; treat as estimate).
- **Downstream effects:** Late defect discovery; configuration drift; release delays; audit findings.
- **Existing solutions:** PLM/ALM suites (Teamcenter, Windchill), AUTOSAR standardised ARXML, tool-specific importers/exporters, EAI middleware.
- **Why insufficient:** PLM covers product data not engineering semantics; importers are version-lagged and lossy; no semantic reconciliation.
- **Potential improvement areas:** Common engineering data model; semantic diff/merge; automated change propagation; graph-based traceability.
- **Automation feasibility:** Medium-High for data movement; low for semantic reconciliation without domain models.
- **AI applicability:** Medium — LLMs can assist mapping/transformation but require validation.
- **Human approval requirements:** High for any automated change to safety-relevant configuration.
- **Evidence/sources:**
  - Autodesk whitepaper "Automotive supply chain challenges and software solutions" (fragmentation/incompatibility): https://damassets.autodesk.net/content/dam/autodesk/www/pdf/automotive-supply-chain-challenges-and-software-solutions-whitepaper-en.pdf
  - Porsche Consulting "Software-Defined Vehicles in the Supplier Ecosystem": https://www.porsche-consulting.com/international/en/publication/software-defined-vehicles-supplier-ecosystem
  - MDPI "Review and Prospect of Integration Compatibility in Digital Vehicles": https://www.mdpi.com/2075-1702/13/9/786
- **Confidence:** High
- **Unanswered:** Quantified share of effort lost to transformation; degree of standardisation actually achieved in practice.

### W4-002 — Proprietary data formats and ARXML version/dialect fragmentation

- **Domain:** Configuration & engineering data
- **Lifecycle stage:** Configuration, integration
- **Problem description:** Although AUTOSAR standardises ARXML, in practice files are version-specific (R4.0/R4.1/R4.2/R4.3/R20-11 etc.), tool-specific (DaVinci vs EB tresos extensions), and frequently invalid against the XSD; exchange between tools/organisations fails or requires manual repair.
- **Actual engineering scenario:** A Tier-1 receives an OEM system extract in R4.3 ARXML but their BSW tool supports R4.0; they must downgrade/convert, losing or corrupting elements; validation fails late.
- **Who experiences:** Configuration engineers, integration engineers, suppliers.
- **Root cause:** AUTOSAR releases evolve; tool vendors lag; real projects pin to older releases; no universal migration tooling; strictness of XSD validation vs lenient tool parsers.
- **Contributing factors:** Multi-vehicle reuse across releases; long programme cycles; supplier heterogeneity.
- **Technical limitations:** No canonical intermediate representation; namespace/version detection not universal; schema validation cost on 100MB+ files.
- **Human skill limitations:** Engineers must understand XSD cardinality/namespaces manually.
- **Tool limitations:** Converters are vendor-specific and partial; generic editors (selfdata-style) exist but immature.
- **Process limitations:** Baseline/version control of ARXML often treated as opaque blobs.
- **Frequency/severity:** High in multi-party programmes.
- **Time/cost impact:** Hours–days per conversion incident; unquantified.
- **Downstream effects:** Integration delays; invalid configurations reaching HIL.
- **Existing solutions:** AUTOSAR serialization rules; AUTOSAR compare tools (dSPACE AUTOSAR Compare); IAV Kedu; open parsers (AutosarFactory, ParseX, ARForge).
- **Why insufficient:** Compare tools detect but do not reconcile; open parsers lack semantic validation; version migration unsupported generally.
- **Potential improvement areas:** Schema-independent DOM + validation; automated version migration; semantic diff/merge.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (assisted conversion with validation).
- **Human approval:** High.
- **Evidence/sources:**
  - AUTOSAR ARXML Serialization Rules: https://www.autosar.org/fileadmin/standards/R20-11/FO/AUTOSAR_TPS_ARXMLSerializationRules.pdf
  - dSPACE AUTOSAR Compare: https://www.dspace.com/en/inc/home/products/sw/system_architecture_software/dspace_autosar_compare.cfm
  - arXiv "An Investigation of the AUTOSAR Adaptive Platform" (ARXML heaviness): https://arxiv.org/html/2607.05227v1
  - Reddit open-source AUTOSAR SWC tooling (ARForge): https://www.reddit.com/r/embedded/comments/1s35zsy/i_built_an_open_source_autosar_classic_swc_design/
- **Confidence:** High
- **Unanswered:** Prevalence of invalid ARXML in the field; cost of manual repair.

### W4-003 — Manual ARXML authoring/editing is error-prone and unscalable

- **Domain:** Configuration & engineering data
- **Lifecycle stage:** Software design/configuration
- **Problem description:** Despite tooling, substantial ARXML editing is performed by hand or via fragile scripts; the schema's thousands of types and repeated field groups make manual edits risky; errors surface late in validation/build.
- **Actual engineering scenario:** Engineer edits a 40MB system ARXML in a text editor to rename a port; introduces duplicate UUID; discovered only at ECU build or HIL.
- **Who experiences:** Configuration engineers, SWC developers.
- **Root cause:** ARXML verbosity; tool UX not suited to bulk/programmatic edits; lack of safe, schema-aware editors; generated code unreadable.
- **Contributing factors:** Need for automation in CI; lack of idempotent transformations.
- **Technical limitations:** 130MB generated models; high parse/validation cost; path-based addressing fragile.
- **Human skill limitations:** Deep XSD/AUTOSAR knowledge required.
- **Tool limitations:** Generic XML editors lack AUTOSAR semantics; vendor tools lock edits into project DBs.
- **Process limitations:** No review-friendly diffs for ARXML.
- **Frequency/severity:** Frequent in integration phases.
- **Time/cost impact:** Unquantified; hours per incident.
- **Downstream effects:** Build breaks; integration delays.
- **Existing solutions:** arxml-cli style tools; AUTOSAR compare; generic DOM libraries.
- **Why insufficient:** Limited semantic awareness; no safe refactoring.
- **Potential improvement areas:** Schema-aware CLI/GUI; semantic diff; automated refactoring; CI validation gates.
- **Automation feasibility:** Medium-High.
- **AI applicability:** Medium (assisted edits with validation).
- **Human approval:** High.
- **Evidence/sources:**
  - MDPI "Automating AUTOSAR BSW Configuration Generation" (ARXML complex/error-prone/costly): https://www.mdpi.com/2076-3417/16/17/8443
  - selfdata project notes (14,600 structs, 130MB generated): repository README.
- **Confidence:** Medium
- **Unanswered:** Share of edits done manually vs tool-mediated.

### W4-004 — AUTOSAR configuration tool lock-in and licensing cost

- **Domain:** Toolchain / commercial tools
- **Lifecycle stage:** BSW configuration, ECU integration
- **Problem description:** Classic AUTOSAR BSW configuration is dominated by commercial tools (DaVinci Developer/Configurator, EB tresos Studio, SystemDesk); high licence costs, proprietary project formats and vendor-specific workflows create lock-in and barrier to entry (especially for startups/SMEs).
- **Actual engineering scenario:** Small EV startup cannot afford DaVinci + EB tresos seats; engineers resort to open-source/partial stacks and manual ARXML, slowing development.
- **Who experiences:** Tier-2s, startups, engineering service providers, cost-constrained OEMs.
- **Root cause:** AUTOSAR standard specifies artefacts but not authoring UX; tool vendors monetise configuration complexity; network effects.
- **Contributing factors:** Certification/qualification expectations; supplier mandates.
- **Technical limitations:** Project formats not portable; generated code tied to tool versions.
- **Human skill limitations:** Tool-specific expertise not transferable.
- **Tool limitations:** Limited free/community editions; steep learning curves.
- **Process limitations:** Procurement cycles; vendor support dependencies.
- **Frequency/severity:** Structural.
- **Time/cost impact:** Licence costs per seat (not quantified here); training months.
- **Downstream effects:** Supplier tool heterogeneity; integration friction.
- **Existing solutions:** Open-source AUTOSAR (OpenBSW, S-CORE, AGL), ARForge, community parsers.
- **Why insufficient:** Incomplete BSW coverage; no certification evidence; community support uneven.
- **Potential improvement areas:** Open-source reference toolchains; standardised authoring APIs; vendor-neutral project formats.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - Reddit "Designing AUTOSAR Classic SWCs Without DaVinci": https://dev.to/bzivkovic86/designing-autosar-classic-swcs-without-davinci-developer-4g61
  - GitHub topics autosar-classic: https://github.com/topics/autosar-classic
  - Quora "free tool available for AUTOSAR": https://www.quora.com/How-can-I-start-development-in-AUTOSAR-in-my-own-microcontroller-development-board-Is-there-any-free-tool-available-for-AUTOSAR
- **Confidence:** Medium
- **Unanswered:** Actual licence cost ranges; open-source adoption rates.

### W4-005 — Configuration diff/merge and variant management in ARXML are immature

- **Domain:** Configuration & engineering data
- **Lifecycle stage:** Configuration, release
- **Problem description:** Comparing and merging two ARXML configurations (e.g., OEM baseline vs supplier delivery, or variant A vs B) is hard; generic diff tools produce noise; variant explosion (vehicle lines, options) multiplies configurations; no robust 3-way merge.
- **Actual engineering scenario:** Merging a supplier's updated BSW configuration into an OEM baseline requires manual reconciliation of hundreds of ARXML elements; errors introduce regressions.
- **Who experiences:** Configuration managers, integration engineers.
- **Root cause:** ARXML's hierarchical verbosity; lack of semantic identity keys across versions; variant dimensions not modelled as first-class.
- **Contributing factors:** Late changes; multi-variant product lines.
- **Technical limitations:** No semantic diff algorithms in mainstream tools; whitespace/ordering noise.
- **Human skill limitations:** Need deep AUTOSAR knowledge to interpret diffs.
- **Tool limitations:** dSPACE Compare detects differences but merge workflows limited; Git handles text but not semantics.
- **Process limitations:** Variant management often in spreadsheets.
- **Frequency/severity:** High for multi-variant programmes.
- **Time/cost impact:** Days per merge; unquantified.
- **Downstream effects:** Configuration drift; release risk.
- **Existing solutions:** dSPACE AUTOSAR Compare; IAV Kedu; Git + custom scripts.
- **Why insufficient:** Detection ≠ reconciliation; no automated safe merge.
- **Potential improvement areas:** Semantic 3-way merge; variant-aware configuration model; automated conflict classification.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - dSPACE AUTOSAR Compare: https://www.dspace.com/en/inc/home/products/sw/system_architecture_software/dspace_autosar_compare.cfm
  - IAV Kedu: https://www.iav.com/products-and-services/iav-kedu/
  - arXiv variant-rich CI/CD paper: https://arxiv.org/html/2507.19446v1
- **Confidence:** Medium
- **Unanswered:** Typical number of variants per programme; merge failure rates.

### W4-006 — Requirements management tools are legacy-heavy, hard to onboard

- **Domain:** Requirements engineering tools
- **Lifecycle stage:** Requirements
- **Problem description:** Incumbent tools (IBM DOORS, Siemens Polarion, PTC Codebeamer) are powerful but have steep learning curves, dated UX, heavy administration; onboarding new engineers is rough; teams reassess/replatform frequently.
- **Actual engineering scenario:** New engineer takes weeks to become productive in DOORS; team debates migration to Polarion/Codebeamer; migration risk of losing traceability.
- **Who experiences:** Requirements engineers, system engineers, project managers.
- **Root cause:** Tools evolved from document-centric to ALM but carry legacy architectures; customisation debt; vendor lock-in.
- **Contributing factors:** Compliance needing specific workflows; org-specific tailoring.
- **Technical limitations:** Poor APIs for automation; performance on large modules; weak modern UX.
- **Human skill limitations:** Tool-specific expertise; onboarding time.
- **Tool limitations:** High admin overhead; limited AI assistance historically.
- **Process limitations:** Tool selection driven by compliance checklists not usability.
- **Frequency/severity:** Structural.
- **Time/cost impact:** Onboarding weeks; migration months/years.
- **Downstream effects:** Requirements quality suffers; traceability gaps.
- **Existing solutions:** Jama, Codebeamer, Polarion, DOORS Next, modern AI-native entrants (Trace.Space).
- **Why insufficient:** Migration cost; feature parity gaps; compliance evidence.
- **Potential improvement areas:** Better onboarding; AI-assisted requirements authoring; open data models.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium-High.
- **Human approval:** Medium.
- **Evidence/sources:**
  - Reddit "DOORS alternatives that don't feel ancient": https://www.reddit.com/r/embedded/comments/1s48jja/doors_alternatives_that_dont_feel_ancient/
  - mgtechsoft ALM comparison: https://mgtechsoft.com/blog/doors-next-vs-polarion-vs-codebeamer-vs-ptc-integrity/
  - raiqon "Keep Codebeamer, DOORS, Polarion or Jama. Put AI above": https://raiqon.com/resources/put-ai-above-alm/
- **Confidence:** Medium
- **Unanswered:** Quantified onboarding time; migration success rates.

### W4-007 — End-to-end traceability gaps between requirements, design, code, tests, safety

- **Domain:** Traceability / compliance
- **Lifecycle stage:** All stages
- **Problem description:** Bidirectional traceability from stakeholder requirements through system/software requirements, architecture, code, tests and safety analysis is required by ISO 26262/ASPICE but is maintained manually or partially; links break during change; evidence collection is painful.
- **Actual engineering scenario:** After a late requirement change, engineers cannot quickly identify affected tests/safety cases; regression scope is over- or under-estimated; audit finds dangling links.
- **Who experiences:** Safety managers, quality engineers, auditors, developers.
- **Root cause:** Tools store links in separate silos; identifiers inconsistent; manual link creation; no automated impact analysis.
- **Contributing factors:** Multi-toolchain; supplier boundaries; change volume.
- **Technical limitations:** No semantic identity across artefacts; link rot; query performance.
- **Human skill limitations:** Discipline to maintain links; understanding of trace granularity.
- **Tool limitations:** Traceability features exist but are cumbersome; reporting manual.
- **Process limitations:** Traceability treated as audit artefact not engineering aid.
- **Frequency/severity:** Continuous; high.
- **Time/cost impact:** Unquantified; significant in audits.
- **Downstream effects:** Over-testing or under-testing; compliance findings; release delays.
- **Existing solutions:** DOORS/Polarion/Codebeamer traceability; Parasoft bidirectional traceability with codeBeamer; Jama.
- **Why insufficient:** Links are manual and brittle; cross-tool links fragile; maintenance cost.
- **Potential improvement areas:** Automated trace generation from code/tests; impact analysis; graph-based traceability.
- **Automation feasibility:** Medium-High for code/test links; low for intent-level links.
- **AI applicability:** Medium-High (assisted link suggestions) with human approval.
- **Human approval:** High for compliance evidence.
- **Evidence/sources:**
  - Parasoft "Cut Compliance Costs... traceability codeBeamer": https://www.youtube.com/watch?v=Zv_u9-kY8jo
  - mgtechsoft compliance landscape: https://mgtechsoft.com/blog/automotive-software-compliance-landscape/
  - Jama ISO 26262 guide: https://www.jamasoftware.com/requirements-management-guide/automotive-engineering/iso-26262/
- **Confidence:** High
- **Unanswered:** Typical trace link maintenance effort; defect escape due to trace gaps.

### W4-008 — Static analysis false positives and manual deviation effort (MISRA/CERT)

- **Domain:** Code quality / standards compliance
- **Lifecycle stage:** Development, verification
- **Problem description:** MISRA/CERT compliance relies on static analysis tools that generate false positives and cannot decide undecidable rules; engineers spend substantial time triaging and justifying deviations; "zero false positives" is unattainable.
- **Actual engineering scenario:** A PRQA/Polyspace run on a 500k LOC codebase yields thousands of findings; team spends weeks classifying true vs false positives and writing deviation records; audit samples deviations.
- **Who experiences:** Developers, quality engineers, safety managers.
- **Root cause:** Static analysis is conservative; MISRA includes undecidable/semantic rules; tool precision varies; codebase scale.
- **Contributing factors:** Legacy code; third-party code; inconsistent coding practices.
- **Technical limitations:** Decidable vs undecidable rules; context sensitivity limits; tool-specific rule interpretations.
- **Human skill limitations:** Ability to judge false positives; deviation writing.
- **Tool limitations:** Every checker generates some false positives; rule coverage gaps.
- **Process limitations:** Deviation process overhead; audit scrutiny.
- **Frequency/severity:** Continuous in safety-critical codebases.
- **Time/cost impact:** Unquantified; potentially weeks per release.
- **Downstream effects:** Alert fatigue; real defects missed; compliance risk.
- **Existing solutions:** QA-MISRA (claims zero FP on syntactic rules), Parasoft, LDRA, QAC/PRQA, Polyspace; MISRA Compliance:2020 process.
- **Why insufficient:** Undecidable rules always need manual review; tool disagreement; false-positive rates non-zero in practice.
- **Potential improvement areas:** Better precision; ML-based false-positive reduction; standardised deviation exchange.
- **Automation feasibility:** Medium (triage assistance); low for final judgement.
- **AI applicability:** Medium (classification assistance) with human approval.
- **Human approval:** High.
- **Evidence/sources:**
  - Parasoft false positives blog: https://www.parasoft.com/blog/false-positives-in-static-code-analysis/
  - Perforce MISRA rules: https://www.perforce.com/blog/qac/misra-rules-misra-guidelines
  - MISRA Compliance:2020: https://misra.org.uk/app/uploads/2021/06/MISRA-Compliance-2020.pdf
  - arXiv MISRA C role of static analysis: https://arxiv.org/html/1809.00821v1
- **Confidence:** High
- **Unanswered:** False-positive rates per tool/rule; cost of deviation process.

### W4-009 — MISRA/CERT standard version churn and migration burden

- **Domain:** Standards / code quality
- **Lifecycle stage:** Development
- **Problem description:** MISRA C:2012 → MISRA C:2023, MISRA C++:2008 → MISRA C++:2023 (merged with AUTOSAR C++14), CERT updates; organisations must re-baseline, re-qualify tools, retrain and re-audit; legacy codebases pinned to old standards.
- **Actual engineering scenario:** Programme started under MISRA C++:2008; new standard requires C++17; compiler upgrade and rule re-mapping delay release; tool vendors lag in supporting new rules.
- **Who experiences:** Developers, quality managers, tool administrators.
- **Root cause:** Standards evolve to track language evolution; industry adoption lags; tool support lags; programme cycles long.
- **Contributing factors:** Multi-year vehicle programmes; supplier heterogeneity.
- **Technical limitations:** Rule mapping between versions incomplete; tool rule sets differ.
- **Human skill limitations:** Engineers must learn new rule sets; deviation re-justification.
- **Tool limitations:** Support for newest standards delayed; inconsistent implementations.
- **Process limitations:** Re-baselining effort; audit re-scoping.
- **Frequency/severity:** Periodic but high-impact.
- **Time/cost impact:** Unquantified; months for large codebases.
- **Downstream effects:** Delayed adoption of modern language features; technical debt.
- **Existing solutions:** Vendor migration guides (Qt/Parasoft); MISRA compliance process.
- **Why insufficient:** No automated rule migration; manual rework.
- **Potential improvement areas:** Automated rule mapping; incremental adoption; tool standardisation.
- **Automation feasibility:** Low-Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - Qt migration guide MISRA C++:2023: https://www.qt.io/quality-assurance/blog/migration-to-misra-c2023
  - Parasoft MISRA C++ 2023 guide: https://www.parasoft.com/blog/misra-cpp-2023-guide/
  - Perforce MISRA C++ 2023 intro: https://www.perforce.com/blog/qac/misra-cpp-2023-intro
  - StackOverflow C++14 vs MISRA C++:2023: https://stackoverflow.com/questions/78299202/can-i-use-misra-c2023-with-c14
- **Confidence:** Medium
- **Unanswered:** Adoption rates of new MISRA versions; tool support timelines.

### W4-010 — Undecidable MISRA rules require manual review and create inconsistency

- **Domain:** Code quality / standards
- **Lifecycle stage:** Verification
- **Problem description:** MISRA distinguishes decidable rules (automatable) from undecidable rules that always require manual review; manual reviews are inconsistent across reviewers and sites; coverage of undecidable rules is hard to evidence.
- **Actual engineering scenario:** Rule about control flow or pointer aliasing requires human judgement; different reviewers reach different conclusions; audit sampling exposes inconsistency.
- **Who experiences:** Developers, reviewers, auditors.
- **Root cause:** Fundamental limits of static analysis; reliance on human judgement for semantic properties.
- **Contributing factors:** Large codebases; distributed teams.
- **Technical limitations:** Undecidability; tool cannot prove compliance.
- **Human skill limitations:** Reviewer expertise variance.
- **Tool limitations:** Tools flag candidates only.
- **Process limitations:** Review checklists inconsistent.
- **Frequency/severity:** Continuous for undecidable rule sets.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Inconsistent compliance claims; audit findings.
- **Existing solutions:** Manual review, checklists, peer review.
- **Why insufficient:** Human inconsistency; cost; no evidence automation.
- **Potential improvement areas:** Structured review workflows; AI-assisted review suggestions; standardised evidence templates.
- **Automation feasibility:** Low for final decision; Medium for assistance.
- **AI applicability:** Medium (suggestions) with human approval.
- **Human approval:** High.
- **Evidence/sources:**
  - Qt blog (decidable vs undecidable): https://www.qt.io/quality-assurance/blog/build-safety-critical-software-with-misra-compliance
  - MISRA Compliance:2020: https://misra.org.uk/app/uploads/2021/06/MISRA-Compliance-2020.pdf
- **Confidence:** Medium
- **Unanswered:** Share of effort on undecidable rules.

### W4-011 — ISO 26262 compliance creates heavy documentation and process overhead

- **Domain:** Functional safety
- **Lifecycle stage:** All stages
- **Problem description:** ISO 26262 mandates hazard analysis, ASIL decomposition, safety requirements, verification/validation evidence and safety case; the administrative/documentation burden is large and often disproportionate to the engineering value, especially for lower ASILs or reused components.
- **Actual engineering scenario:** Team spends more time maintaining safety plans, checklists and traceability matrices than improving safety design; re-certification on minor changes.
- **Who experiences:** Safety engineers, developers, project managers.
- **Root cause:** Standard is process-heavy by design; evidence expectations interpreted conservatively by auditors/suppliers; lack of reusable safety assets.
- **Contributing factors:** OEM-specific interpretations; supplier cascade; defensive compliance culture.
- **Technical limitations:** No automated safety-case generation; manual evidence.
- **Human skill limitations:** Shortage of functional safety experts.
- **Tool limitations:** Safety tools (medini, PREEvision) help but manual data entry dominates.
- **Process limitations:** Conservative audit interpretation; change impact re-assessment.
- **Frequency/severity:** Continuous in safety-relevant programmes.
- **Time/cost impact:** Unquantified; significant share of programme cost (hypothesis).
- **Downstream effects:** Slower iteration; safety expertise bottleneck; over-engineering.
- **Existing solutions:** Safety toolchains (medini analyze, PREEvision), ALM traceability, ISO 26262 training/certification.
- **Why insufficient:** Tools are point solutions; evidence assembly manual; reuse of safety cases limited.
- **Potential improvement areas:** Reusable safety case modules; automated evidence collection; risk-proportionate tailoring.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (drafting assistance) with human approval.
- **Human approval:** High.
- **Evidence/sources:**
  - PTC ISO 26262 overview: https://www.ptc.com/en/blogs/alm/what-is-iso-26262
  - Jama ISO 26262: https://www.jamasoftware.com/requirements-management-guide/automotive-engineering/iso-26262/
  - Synopsys ISO 26262: https://www.synopsys.com/glossary/what-is-iso-26262.html
- **Confidence:** Medium
- **Unanswered:** Quantified compliance cost as % of programme; variation by ASIL.

### W4-012 — Safety case maintenance and change-impact re-assessment cost

- **Domain:** Functional safety
- **Lifecycle stage:** Change management, release
- **Problem description:** Any design/code change requires re-evaluating safety impact and updating the safety case; without automated impact analysis, teams either over-reassess (cost) or under-assess (risk).
- **Actual engineering scenario:** Minor bug fix in a non-safety module still triggers full safety re-review because dependency/impact is unclear; release delayed.
- **Who experiences:** Safety engineers, developers, release managers.
- **Root cause:** Safety cases are monolithic; dependency/impact analysis manual; no incremental safety argumentation.
- **Contributing factors:** Complex dependencies; late changes.
- **Technical limitations:** No automated safety-impact analysis tooling widely adopted.
- **Human skill limitations:** Safety engineer availability.
- **Tool limitations:** ALM tools track links but not safety argument validity.
- **Process limitations:** Conservative change policies.
- **Frequency/severity:** High in maintenance phase.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Release delays; safety team bottleneck.
- **Existing solutions:** Change-impact features in ALM; safety tooling.
- **Why insufficient:** Not safety-argument aware.
- **Potential improvement areas:** Incremental safety case; automated impact analysis; modular safety arguments.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** High.
- **Evidence/sources:** Inferred from ISO 26262 process descriptions (Jama/PTC); low direct evidence.
- **Confidence:** Low-Medium
- **Unanswered:** Frequency of over/under-assessment; cost data.

### W4-013 — Automotive SPICE assessment overhead and tailoring burden

- **Domain:** Process quality
- **Lifecycle stage:** Process management
- **Problem description:** ASPICE assessments require extensive process evidence, document trails and tailoring; organisations spend significant effort preparing for assessments rather than improving engineering; assessment gaming risk.
- **Actual engineering scenario:** Team spends months assembling assessment artefacts (plans, reviews, test reports) for ASPICE level 2/3; processes tailored to assessment expectations rather than value.
- **Who experiences:** Quality managers, process engineers, project teams.
- **Root cause:** Assessment model emphasises evidence; organisational incentives favour passing assessments; process overhead scales with assessment scope.
- **Contributing factors:** Customer mandates; supplier audits; frequent standard updates (ASPICE 4.0).
- **Technical limitations:** Evidence collection not automated; tool support partial.
- **Human skill limitations:** Process engineering expertise.
- **Tool limitations:** ALM tools support but manual curation remains.
- **Process limitations:** Tailoring complexity; assessment prep cycles.
- **Frequency/severity:** Periodic (assessment cycles) but continuous prep.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Process fatigue; assessment gaming; real quality not improved.
- **Existing solutions:** ASPICE consulting, training, ALM evidence automation.
- **Why insufficient:** Evidence still largely manual; assessment culture.
- **Potential improvement areas:** Automated evidence collection; continuous compliance; lighter tailoring.
- **Automation feasibility:** Medium-High for evidence collection.
- **AI applicability:** Low-Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - Tuxera ASPICE implementation factors: https://www.tuxera.com/blog/achieving-automotive-spice-an-implementation-overview/
  - DNV ASPICE assessment: https://www.dnv.com/services/automotive-spice-aspice-process-assessment/
  - GrammaTech static analysis in ASPICE: https://www.grammatech.com/learn/static-analysis-in-automotive-spice/
- **Confidence:** Medium
- **Unanswered:** Quantified prep effort; assessment pass/fail trends.

### W4-014 — ISO/SAE 21434 cybersecurity engineering integration cost

- **Domain:** Cybersecurity
- **Lifecycle stage:** All stages
- **Problem description:** ISO/SAE 21434 introduces cybersecurity goals, TARA, cybersecurity requirements, verification and monitoring across the lifecycle; integrating these into existing safety-focused processes is costly and skills are scarce; overlap/duplication with ISO 26262 evidence.
- **Actual engineering scenario:** Safety team and security team maintain parallel requirement sets and evidence; TARA workshops are manual and infrequent; threats identified late.
- **Who experiences:** Security engineers, safety engineers, architects.
- **Root cause:** New standard layered onto safety-centric processes; cybersecurity expertise shortage; tool support nascent.
- **Contributing factors:** Supply-chain scope; evolving threat landscape.
- **Technical limitations:** No unified safety+security modelling tools widely adopted.
- **Human skill limitations:** Shortage of automotive cybersecurity experts.
- **Tool limitations:** TARA tools (ThreatGet, etc.) exist but integration with ALM weak.
- **Process limitations:** Parallel processes; manual threat modelling.
- **Frequency/severity:** Continuous.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Late security findings; duplicated effort; audit risk.
- **Existing solutions:** TARA tools, ISO/SAE 21434 consulting, integrated safety/security platforms (emerging).
- **Why insufficient:** Immature integration; manual workshops.
- **Potential improvement areas:** Unified safety/security models; automated threat modelling; continuous TARA.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (threat suggestion) with human approval.
- **Human approval:** High.
- **Evidence/sources:**
  - mgtechsoft compliance landscape: https://mgtechsoft.com/blog/automotive-software-compliance-landscape/
  - ISO/SAE 21434 standard page (via TÜV SÜD): https://www.tuvsud.com/en/industries/automotive/iso-26262-for-automotives (context)
- **Confidence:** Medium
- **Unanswered:** Cost of ISO 21434 programmes; TARA tool adoption.

### W4-015 — UNECE R155 type-approval and CSMS supply-chain burden

- **Domain:** Cybersecurity regulation
- **Lifecycle stage:** Release, homologation
- **Problem description:** UNECE R155 requires a certified Cybersecurity Management System (CSMS) and vehicle type approval; obligations cascade to suppliers; evidence and audit readiness are heavy; smaller suppliers struggle.
- **Actual engineering scenario:** OEM requires all Tier-1s to demonstrate CSMS alignment; supplier must undergo audits and maintain evidence; delays vehicle approval.
- **Who experiences:** OEM compliance teams, suppliers, type-approval bodies.
- **Root cause:** Regulation makes OEM accountable for supply-chain cybersecurity; evidence requirements; limited supplier maturity.
- **Contributing factors:** Global applicability; tight timelines; evolving interpretations.
- **Technical limitations:** No standardised evidence exchange format.
- **Human skill limitations:** Cybersecurity expertise shortage.
- **Tool limitations:** CSMS tooling immature.
- **Process limitations:** Audit scheduling; supplier onboarding.
- **Frequency/severity:** Structural for new vehicle approvals.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Approval delays; supplier exclusion; cost increases.
- **Existing solutions:** CSMS consulting, ISO 21434 alignment, third-party audits (Bureau Veritas, Secura).
- **Why insufficient:** Manual evidence; supplier maturity gaps.
- **Potential improvement areas:** Standardised supplier evidence; continuous compliance monitoring.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** High.
- **Evidence/sources:**
  - Automotive IQ R155/R156: https://www.automotive-iq.com/cybersecurity/articles/unece-r155-r156-explained-automotive-cybersecurity-requirements-for-2027
  - UNECE R155 PDF: https://unece.org/sites/default/files/2023-02/R155e%20%282%29.pdf
  - Secura compliance guide: https://cybersecurity.bureauveritas.com/services/iot/automotive/how-to-reach-unece-compliance
- **Confidence:** Medium
- **Unanswered:** Approval delay statistics; supplier compliance rates.

### W4-016 — UNECE R156 / ISO 24089 software update engineering complexity

- **Domain:** Software update / OTA
- **Lifecycle stage:** Release, maintenance
- **Problem description:** R156/SUMS and ISO 24089 require managed software update processes (versioning, compatibility, rollback, state management, documentation); implementing compliant OTA across diverse ECUs and suppliers is complex and error-prone.
- **Actual engineering scenario:** OTA update fails on a subset of vehicles due to undocumented ECU state; rollback procedure untested; compliance evidence missing.
- **Who experiences:** OTA engineers, release managers, field support.
- **Root cause:** Vehicle software heterogeneity; ECU state variability; update engineering not first-class in development; immature tooling.
- **Contributing factors:** Legacy ECUs without update support; supplier variation.
- **Technical limitations:** No universal update state model; testing matrix explosion.
- **Human skill limitations:** Update engineering expertise.
- **Tool limitations:** OTA platforms (Mender, Redstone) help but compliance evidence manual.
- **Process limitations:** Update planning late in lifecycle.
- **Frequency/severity:** High for connected vehicles.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Failed updates; recalls; customer dissatisfaction.
- **Existing solutions:** OTA platforms; ISO 24089 training; SUMS consulting.
- **Why insufficient:** Tooling focuses on delivery not engineering evidence.
- **Potential improvement areas:** Update-aware development; automated compatibility verification; simulation of update scenarios.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - Redstone ISO 24089: https://www.redstoneota.com/iso-24089-automotive-software-update-engineering/
  - ModernRequirements ISO 24089: https://www.modernrequirements.com/blogs/iso-24089-managing-requirements-for-automotive-software-updates/
  - itemis glossary: https://www.itemis.com/en/glossary/iso-24089/
  - UNECE R156: https://unece.org/transport/documents/2021/03/standards/un-regulation-no-156-software-update-and-software-update
- **Confidence:** Medium
- **Unanswered:** OTA failure rates; compliance cost.

### W4-017 — SOTIF validation of unknown unsafe scenarios does not scale

- **Domain:** Safety / ADAS validation
- **Lifecycle stage:** Validation
- **Problem description:** ISO 21448 (SOTIF) requires addressing known and unknown unsafe scenarios arising from functional insufficiencies and misuse; identifying unknown unsafe scenarios is inherently open-ended; scenario-based testing cannot cover the space; evidence of sufficiency is hard.
- **Actual engineering scenario:** ADAS team tests thousands of scenarios but cannot demonstrate absence of unknown unsafe scenarios; safety argument relies on process evidence and heuristics.
- **Who experiences:** Safety engineers, ADAS validation engineers, regulators.
- **Root cause:** Infinite scenario space; limitations of simulation/field testing; absence of completeness metrics.
- **Contributing factors:** Environment variability; sensor limitations; ML nondeterminism.
- **Technical limitations:** Simulation fidelity; scenario generation coverage; oracle problem.
- **Human skill limitations:** Scenario engineering expertise.
- **Tool limitations:** Scenario databases and simulators help but coverage unknown.
- **Process limitations:** No agreed sufficiency criteria.
- **Frequency/severity:** Structural for ADAS/AD.
- **Time/cost impact:** Very high (AD programmes).
- **Downstream effects:** Delayed ADAS feature release; over-conservative systems.
- **Existing solutions:** Scenario-based testing, simulation platforms, SOTIF process, field monitoring.
- **Why insufficient:** Cannot enumerate unknowns; coverage unmeasurable.
- **Potential improvement areas:** Statistical safety arguments; continuous field feedback; better scenario generation.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (scenario generation) with human approval.
- **Human approval:** High.
- **Evidence/sources:**
  - dSPACE SOTIF: https://www.dspace.com/en/inc/home/news/engineers-insights/navigating-the-road-to-iso-21448.cfm
  - SRM Tech SOTIF: https://www.srmtech.com/knowledge-base/blogs/sotif-guidelines-ensuring-safety-beyond-system-failures/
  - ScienceDirect unknown unsafe scenarios: https://www.sciencedirect.com/science/article/abs/pii/S0001457523004578
  - TÜV SÜD SOTIF whitepaper: https://www.tuvsud.com/-/jssmedia/global/pdf-files/whitepaper-report-e-books/tuvsud-sotif.pdf
- **Confidence:** High
- **Unanswered:** How sufficiency is demonstrated in practice; regulator expectations.

### W4-018 — HIL test bench scarcity and scheduling bottlenecks

- **Domain:** Testing infrastructure
- **Lifecycle stage:** Integration, validation
- **Problem description:** HIL benches are capital-intensive and limited; shared resources create scheduling bottlenecks; test execution queues delay validation; hardware availability gates software progress.
- **Actual engineering scenario:** Multiple teams queue for the same HIL bench; software builds wait days for bench time; utilisation is uneven (peak vs idle).
- **Who experiences:** Test engineers, validation managers, developers.
- **Root cause:** High cost of HIL hardware; physical bench constraints; poor scheduling/remote access; test durations long.
- **Contributing factors:** Late validation strategy; hardware-dependent testing.
- **Technical limitations:** Real-time simulation performance; I/O interfaces; bench configurability.
- **Human skill limitations:** Bench setup expertise.
- **Tool limitations:** Scheduling tools limited; remote HIL access immature.
- **Process limitations:** Bench booking manual; no dynamic prioritisation.
- **Frequency/severity:** Continuous in integration phase.
- **Time/cost impact:** Unquantified; significant delays.
- **Downstream effects:** Release delays; over-reliance on SIL (fidelity gap).
- **Existing solutions:** Remote HIL, cloud HIL (emerging), vECUs, test automation.
- **Why insufficient:** Remote access latency; vECU fidelity; bench heterogeneity.
- **Potential improvement areas:** Cloud/remote HIL; better scheduling; vECU substitution where fidelity adequate.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** Medium.
- **Evidence/sources:**
  - Nature Scientific Reports HIL bottlenecks: https://www.nature.com/articles/s41598-025-21416-5
  - Automotive IQ HIL bottleneck article: https://www.automotive-iq.com/electrics-electronics/whitepapers/when-release-cycles-shrink-hil-validation-cant-be-the-bottleneck
  - OPAL-RT HIL advantages: https://www.opal-rt.com/blog/7-advantages-of-hil-testing/
- **Confidence:** High
- **Unanswered:** Utilisation rates; cost per bench; queue lengths.

### W4-019 — vECU fidelity gaps and integration friction with simulation

- **Domain:** Simulation / virtual validation
- **Lifecycle stage:** Development, validation
- **Problem description:** vECUs enable early validation but fidelity varies by level (L1–L4); integrating vECUs with existing simulation infrastructure is challenging; timing/RTOS behaviour, hardware peripherals and sensor/actuator interfaces are often abstracted inaccurately, limiting trust and substitution for HIL.
- **Actual engineering scenario:** Team runs SIL on vECU; tests pass; HIL reveals timing/peripheral issues; rework; trust in vECU erodes.
- **Who experiences:** Developers, validation engineers, simulation engineers.
- **Root cause:** Virtualisation trade-offs; lack of standardised vECU interfaces; toolchain integration gaps; cost of high-fidelity models.
- **Contributing factors:** ECU hardware diversity; RTOS/BSW complexity.
- **Technical limitations:** Timing accuracy; peripheral emulation; multi-ECU network simulation.
- **Human skill limitations:** Simulation modelling expertise.
- **Tool limitations:** Integration between vECU and plant models non-trivial.
- **Process limitations:** No clear criteria for when vECU evidence is acceptable.
- **Frequency/severity:** Growing as SDV adoption increases.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** False confidence or under-use of vECUs; continued HIL dependence.
- **Existing solutions:** dSPACE vECU, MathWorks Virtual ECU, Synopsys Silver, RemotiveLabs guidance.
- **Why insufficient:** Fidelity/cost trade-offs; integration effort.
- **Potential improvement areas:** Standardised vECU levels/interfaces; fidelity certification; automated integration.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - dSPACE vECU evolution: https://www.dspace.com/en/inc/home/news/engineers-insights/the-evolution-of-v-ecus.cfm
  - RemotiveLabs "vECUs are not a silver bullet": https://www.remotivelabs.com/blog/why-virtual-ecus-vecus-are-not-a-silver-bullet
  - MathWorks virtual ECU: https://www.mathworks.com/discovery/virtual-ecu.html
  - MathWorks accelerating defect detection with vECU: https://www.mathworks.com/videos/accelerating-automotive-software-defect-detection-via-shift-left-with-virtual-ecu-testing-1756218773487.html
- **Confidence:** Medium
- **Unanswered:** Fidelity acceptance criteria; cost/benefit data.

### W4-020 — Generic CI/CD pipelines are unsuitable for safety-critical embedded software

- **Domain:** DevOps / CI/CD
- **Lifecycle stage:** Build, verification, release
- **Problem description:** Generic CI/CD (Jenkins/GitLab/GitHub Actions) prioritises speed; safety-critical embedded software requires reproducible builds, tool qualification, traceability, and certification evidence; bolting these on is costly and fragile.
- **Actual engineering scenario:** Team runs GitHub Actions for builds/tests; auditors ask for qualified tools and reproducible environments; migration to qualified pipeline required.
- **Who experiences:** DevOps engineers, quality managers, developers.
- **Root cause:** CI/CD tools designed for web/cloud software; embedded/safety needs (cross-compilation, HIL, tool qualification) not native; certification toolchains immature.
- **Contributing factors:** Safety standards; long build times; hardware dependencies.
- **Technical limitations:** Tool qualification requirements; environment reproducibility; HIL-in-CI integration.
- **Human skill limitations:** DevOps + safety expertise combination rare.
- **Tool limitations:** Generic runners lack embedded toolchains; qualification evidence.
- **Process limitations:** No standard for safety-qualified CI/CD.
- **Frequency/severity:** Structural for safety programmes.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Slower adoption of modern DevOps; manual release processes.
- **Existing solutions:** LDRA CI, IAR embedded CI/CD, Parasoft CI integration, safety-qualified toolchains.
- **Why insufficient:** Qualification burden; HIL integration; cost.
- **Potential improvement areas:** Safety-qualified CI platforms; containerised qualified toolchains; HIL-as-a-service in CI.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** High.
- **Evidence/sources:**
  - LDRA CI for safety-critical: https://ldra.com/capabilities/continuous-integration/
  - Parasoft CI/CD guide: https://www.parasoft.com/white-paper/guide-to-ci-cd-for-automotive-devops/
  - IAR embedded CI/CD: https://www.iar.com/embedded-development-tools/embedded-ci-cd/
  - arXiv variant-rich CI/CD: https://arxiv.org/html/2507.19446v1
- **Confidence:** High
- **Unanswered:** Adoption rates of safety-qualified CI; cost.

### W4-021 — Long build/compilation times for large AUTOSAR codebases

- **Domain:** Build / tooling
- **Lifecycle stage:** Development
- **Problem description:** Generated AUTOSAR codebases (tens of thousands of structs, hundreds of MB) and large BSW configurations lead to long compile/link times, slowing iteration and CI feedback.
- **Actual engineering scenario:** Full rebuild of ECU software takes hours; developers avoid local builds; CI queues grow; feedback delayed.
- **Who experiences:** Developers, CI engineers.
- **Root cause:** AUTOSAR code generation produces huge code; monolithic builds; lack of incremental build discipline; template-heavy C++.
- **Contributing factors:** Configuration changes trigger full regeneration; tool-generated code not modularised.
- **Technical limitations:** Compiler/linker performance; build system complexity.
- **Human skill limitations:** Build engineering expertise.
- **Tool limitations:** Generators not optimised for incremental output.
- **Process limitations:** Full rebuilds in CI.
- **Frequency/severity:** Daily in large programmes.
- **Time/cost impact:** Hours per build (anecdotal).
- **Downstream effects:** Slower iteration; reduced testing frequency.
- **Existing solutions:** Incremental builds, ccache, distributed builds, build caching.
- **Why insufficient:** Generated code invalidates caches; configuration churn.
- **Potential improvement areas:** Incremental code generation; modular builds; better caching.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Low.
- **Evidence/sources:**
  - selfdata README (14,600 structs, ~130MB generated): repository.
  - General embedded CI literature (mgtechsoft): https://mgtechsoft.com/blog/ci-cd-for-embedded-systems/
- **Confidence:** Medium (partly anecdotal)
- **Unanswered:** Typical build times; effectiveness of caching in practice.

### W4-022 — Tool onboarding, training time and single-person expertise

- **Domain:** Human factors / tooling
- **Lifecycle stage:** All
- **Problem description:** Specialised automotive tools (DaVinci, CANoe, CANape, dSPACE, Tessy, LDRA) require months of training; organisations depend on a few experts; turnover causes knowledge loss and project risk.
- **Actual engineering scenario:** Key CANoe/CANalyzer expert leaves; remaining team cannot maintain test scripts; project stalls.
- **Who experiences:** Engineering managers, teams, new hires.
- **Root cause:** Tools are complex and vendor-specific; documentation uneven; training expensive; no standardised skill baseline.
- **Contributing factors:** Tool churn; limited training budgets.
- **Technical limitations:** Tool UX complexity; proprietary scripting.
- **Human skill limitations:** Shortage of trained engineers.
- **Tool limitations:** Poor in-tool guidance; steep curves.
- **Process limitations:** No structured onboarding; knowledge silos.
- **Frequency/severity:** Continuous.
- **Time/cost impact:** Months to productivity; unquantified.
- **Downstream effects:** Project delays; quality risk; vendor dependency.
- **Existing solutions:** Vendor training/certification; internal mentoring.
- **Why insufficient:** Costly; not scalable; knowledge leaves with people.
- **Potential improvement areas:** Better UX; AI assistants; standardised training; knowledge capture.
- **Automation feasibility:** Low for expertise; Medium for assistance.
- **AI applicability:** Medium (assistants) with human approval.
- **Human approval:** Medium.
- **Evidence/sources:**
  - Reddit DOORS onboarding: https://www.reddit.com/r/embedded/comments/1s48jja/doors_alternatives_that_dont_feel_ancient/
  - Veoneer Codebeamer onboarding success story: https://arorian.com/automotive-safety-codebeamer-alm/
- **Confidence:** Medium
- **Unanswered:** Training durations; turnover impact.

### W4-023 — Supplier/OEM toolchain mismatch and data exchange friction

- **Domain:** Supply chain / tooling
- **Lifecycle stage:** Integration
- **Problem description:** OEMs and suppliers use different tool stacks and versions; exchanging engineering data (ARXML, DBC, test results) requires conversion and reconciliation; mismatches cause integration failures.
- **Actual engineering scenario:** Supplier delivers test evidence in a format the OEM tool cannot import; manual rework; acceptance delayed.
- **Who experiences:** Integration engineers, quality engineers, suppliers.
- **Root cause:** No common toolchain standard; commercial interests; legacy investments.
- **Contributing factors:** Global supply chains; mergers.
- **Technical limitations:** Format incompatibilities; version skew.
- **Human skill limitations:** Cross-org coordination.
- **Tool limitations:** Importers/exporters lag.
- **Process limitations:** Interface control documents often incomplete.
- **Frequency/severity:** High in multi-tier programmes.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Integration delays; acceptance disputes.
- **Existing solutions:** Interface agreements; neutral formats (ARXML, ODX); PLM exchange.
- **Why insufficient:** Neutral formats still version/tool fragmented.
- **Potential improvement areas:** Common data contracts; automated validation at exchange; shared tool environments.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - Porsche Consulting supplier ecosystem: https://www.porsche-consulting.com/international/en/publication/software-defined-vehicles-supplier-ecosystem
  - Autodesk supply chain whitepaper: https://damassets.autodesk.net/content/dam/autodesk/www/pdf/automotive-supply-chain-challenges-and-software-solutions-whitepaper-en.pdf
- **Confidence:** Medium
- **Unanswered:** Frequency/cost of exchange failures.

### W4-024 — PLM/PDM vs engineering-data (ARXML) integration gaps

- **Domain:** Data management
- **Lifecycle stage:** Configuration, release
- **Problem description:** PLM systems manage product/mechanical data and versions well but do not natively understand AUTOSAR engineering semantics (ARXML, ECU configs); the digital thread between PLM and software engineering data is weak or manual.
- **Actual engineering scenario:** ECU software baseline stored in PLM as opaque file; change in PLM does not propagate to ARXML; software team maintains parallel versioning.
- **Who experiences:** Configuration managers, PLM admins, software teams.
- **Root cause:** PLM evolved from mechanical PDM; software engineering data models differ; integration projects costly.
- **Contributing factors:** Organisational silos (mechanical vs software).
- **Technical limitations:** No standardised PLM↔ARXML integration; semantic mismatch.
- **Human skill limitations:** Cross-discipline knowledge.
- **Tool limitations:** PLM connectors to ARXML immature.
- **Process limitations:** Parallel versioning systems.
- **Frequency/severity:** Structural in mechatronic programmes.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Baseline inconsistency; release errors.
- **Existing solutions:** PLM integrations (Polarion-Teamcenter), custom connectors.
- **Why insufficient:** Semantic gaps remain.
- **Potential improvement areas:** Software-aware PLM; unified engineering data backbone.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** High.
- **Evidence/sources:**
  - Automotive Manufacturing Solutions PLM evolution: https://www.automotivemanufacturingsolutions.com/partner-content/how-plm-is-evolving-to-meet-the-complexity-of-automotive-manufacturing-operations/2756626
  - mgtechsoft ELM vs PLM: https://mgtechsoft.com/blog/elm-vs-plm/
- **Confidence:** Medium
- **Unanswered:** Adoption of software-aware PLM.

### W4-025 — Open-source AUTOSAR tooling is immature and unsupported

- **Domain:** Open-source tooling
- **Lifecycle stage:** Development
- **Problem description:** Open-source AUTOSAR tools (parsers, generators, SWC designers) provide alternatives but lack completeness (BSW coverage), certification evidence, version support and sustained maintenance; community projects are often single-maintainer.
- **Actual engineering scenario:** Team adopts open-source ARXML parser; discovers missing support for a BSW module; must fork and maintain; risk for production use.
- **Who experiences:** Developers, startups, researchers.
- **Root cause:** AUTOSAR complexity; lack of funding/community; certification barriers; vendor dominance.
- **Contributing factors:** IP/licensing of standards; small community.
- **Technical limitations:** Partial schema coverage; no validation depth.
- **Human skill limitations:** Need to maintain forks.
- **Tool limitations:** Incomplete; documentation sparse.
- **Process limitations:** No support SLAs; audit risk.
- **Frequency/severity:** Structural for open-source adopters.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Limited adoption; duplicated community effort.
- **Existing solutions:** AutosarFactory, ParseX, ARForge, OpenBSW, S-CORE, AGL.
- **Why insufficient:** Coverage/support gaps.
- **Potential improvement areas:** Funded open-source reference toolchains; conformance test suites; community governance.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Medium.
- **Evidence/sources:**
  - Reddit ARForge: https://www.reddit.com/r/embedded/comments/1s35zsy/i_built_an_open_source_autosar_classic_swc_design/
  - GitHub autosar-classic topic: https://github.com/topics/autosar-classic
  - VDA Automotive Grade OSS MoU: https://www.vda.de/dam/jcr:50e82fa0-dd27-4db5-834f-80032c57b1b3/MoU%20Automotive%20Grade%20Open-Source%20Software%20Ecosystem%20signed.pdf
- **Confidence:** Medium
- **Unanswered:** Open-source adoption in production; maintenance sustainability.

### W4-026 — Standards evolution and regional regulatory fragmentation

- **Domain:** Standards/regulation
- **Lifecycle stage:** Compliance, release
- **Problem description:** Automotive software must comply with an evolving, overlapping set of standards (ISO 26262, ASPICE, ISO 21434, SOTIF, AUTOSAR, MISRA, UNECE R10/R155/R156) plus regional regulations; changes and regional differences force re-work and market-specific variants.
- **Actual engineering scenario:** A feature compliant in EU requires changes for China/US regulatory expectations; homologation re-run; software variant proliferation.
- **Who experiences:** Compliance teams, OEMs, suppliers.
- **Root cause:** Global market with regional regulators; standards update cycles; no harmonisation.
- **Contributing factors:** Emerging AI governance; cybersecurity.
- **Technical limitations:** No automated compliance mapping across standards.
- **Human skill limitations:** Regulatory expertise scarcity.
- **Tool limitations:** Compliance tools are standard-specific.
- **Process limitations:** Change management across standards.
- **Frequency/severity:** Continuous.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Market delays; variant explosion; compliance risk.
- **Existing solutions:** Regulatory consulting; compliance management tools.
- **Why insufficient:** Manual mapping; reactive.
- **Potential improvement areas:** Harmonised compliance frameworks; automated cross-standard traceability.
- **Automation feasibility:** Low-Medium.
- **AI applicability:** Medium (mapping assistance).
- **Human approval:** High.
- **Evidence/sources:**
  - VDA MoU (prevent regional fragmentation): https://www.vda.de/dam/jcr:50e82fa0-dd27-4db5-834f-80032c57b1b3/MoU%20Automotive%20Grade%20Open-Source%20Software%20Ecosystem%20signed.pdf
  - Automotive IQ R155/R156: https://www.automotive-iq.com/cybersecurity/articles/unece-r155-r156-explained-automotive-cybersecurity-requirements-for-2027
- **Confidence:** Medium
- **Unanswered:** Cost of regional variants; regulatory change frequency.

### W4-027 — Tool licensing, infrastructure and cost constraints

- **Domain:** Toolchain economics
- **Lifecycle stage:** All
- **Problem description:** Commercial automotive toolchains carry high per-seat licence costs, server/HIL infrastructure costs and vendor support contracts; budgets constrain tool adoption and push teams to suboptimal or outdated tooling.
- **Actual engineering scenario:** Team continues using older tool version because upgrade licence is unaffordable; misses bug fixes and new features; compatibility issues arise.
- **Who experiences:** Engineering managers, procurement, developers.
- **Root cause:** Concentrated tool vendor market; high development cost of specialised tools; certification/qualification overhead passed to customers.
- **Contributing factors:** Budget cycles; long programmes.
- **Technical limitations:** Floating licence contention; server infrastructure.
- **Human skill limitations:** N/A.
- **Tool limitations:** Licence servers; offline usage limits.
- **Process limitations:** Procurement lead times.
- **Frequency/severity:** Structural.
- **Time/cost impact:** Unquantified (licence costs not publicly available).
- **Downstream effects:** Outdated tooling; reduced capability; shadow IT.
- **Existing solutions:** Open-source alternatives; cloud licensing (emerging).
- **Why insufficient:** Coverage/support gaps.
- **Potential improvement areas:** Open-source reference tools; flexible licensing; shared tool environments.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Medium.
- **Evidence/sources:** Inferred from vendor market structure and open-source motivation (Reddit/Quora); low direct evidence.
- **Confidence:** Low-Medium
- **Unanswered:** Actual licence cost ranges; budget share.

### W4-028 — Manual data transformation via spreadsheets/scripts ("glue work")

- **Domain:** Process / tooling
- **Lifecycle stage:** All
- **Problem description:** Engineers spend significant time manually transforming data between tools using Excel/CSV and ad-hoc scripts; these transformations are undocumented, error-prone and become hidden dependencies.
- **Actual engineering scenario:** Signal list maintained in Excel; manually converted to DBC/ARXML; discrepancies discovered at integration; no version control.
- **Who experiences:** All engineers, especially integration.
- **Root cause:** Tools lack direct integration; organisations under-invest in automation; data models mismatched.
- **Contributing factors:** Time pressure; lack of build engineering skills.
- **Technical limitations:** No standard transformation framework; schema drift.
- **Human skill limitations:** Scripting skills uneven.
- **Tool limitations:** Missing APIs/exporters.
- **Process limitations:** Undocumented manual steps.
- **Frequency/severity:** Continuous.
- **Time/cost impact:** Unquantified; hypothesised significant.
- **Downstream effects:** Data inconsistency; audit findings; knowledge silos.
- **Existing solutions:** ETL scripts, middleware, ALM integrations.
- **Why insufficient:** Fragile; not maintained.
- **Potential improvement areas:** Standardised transformation pipelines; version-controlled glue code; data contracts.
- **Automation feasibility:** High for mechanical transforms; low for semantic.
- **AI applicability:** Medium.
- **Human approval:** Medium.
- **Evidence/sources:**
  - Autodesk supply chain (data silos): https://damassets.autodesk.net/content/dam/autodesk/www/pdf/automotive-supply-chain-challenges-and-software-solutions-whitepaper-en.pdf
  - arXiv requirements interoperability: https://arxiv.org/html/2512.23780v1
- **Confidence:** Medium
- **Unanswered:** Share of effort on glue work.

### W4-029 — Test tool interoperability and coverage evidence management

- **Domain:** Testing tools
- **Lifecycle stage:** Testing, compliance
- **Problem description:** Unit/component/integration test tools (Tessy, VectorCAST, Parasoft, GoogleTest) produce evidence in disparate formats; aggregating coverage (MC/DC) and linking to requirements/safety cases is manual; tool qualification evidence separate.
- **Actual engineering scenario:** Safety manager must manually compile coverage reports from three test tools into a safety case; discrepancies; audit questions.
- **Who experiences:** Test engineers, safety managers, quality engineers.
- **Root cause:** No common test evidence format; tool-specific coverage metrics; qualification requirements.
- **Contributing factors:** Multi-tool strategies; legacy tests.
- **Technical limitations:** Coverage metric differences (statement/branch/MC/DC); tool output schemas.
- **Human skill limitations:** Evidence assembly.
- **Tool limitations:** Export formats limited; integration weak.
- **Process limitations:** Manual evidence packs.
- **Frequency/severity:** Continuous in safety programmes.
- **Time/cost impact:** Unquantified.
- **Downstream effects:** Audit findings; over-testing.
- **Existing solutions:** Tool integrations; ALM test management; coverage aggregation tools.
- **Why insufficient:** Manual curation persists.
- **Potential improvement areas:** Common evidence format; automated aggregation; continuous compliance dashboards.
- **Automation feasibility:** Medium-High.
- **AI applicability:** Low-Medium.
- **Human approval:** High.
- **Evidence/sources:**
  - Parasoft automotive devops: https://www.parasoft.com/white-paper/guide-to-ci-cd-for-automotive-devops/
  - LDRA CI: https://ldra.com/capabilities/continuous-integration/
- **Confidence:** Medium
- **Unanswered:** Evidence assembly effort; tool qualification costs.

### W4-030 — AI-assisted engineering tooling: trust, certification, IP and data constraints

- **Domain:** AI in engineering tools
- **Lifecycle stage:** All
- **Problem description:** AI/LLM assistance in engineering (requirements, ARXML, code, tests) is promising but blocked by hallucination risk, lack of explainability, functional safety certification, IP/confidentiality constraints and absence of validation; organisations are cautious.
- **Actual engineering scenario:** Engineer uses LLM to draft ARXML or code; output must be fully validated; no certified AI tooling accepted in safety case; data cannot leave premises.
- **Who experiences:** Developers, safety managers, IT/security, management.
- **Root cause:** AI models not certified for safety-critical use; data governance; uncertainty; standards do not address AI in engineering.
- **Contributing factors:** IP protection; customer contracts; regulatory ambiguity.
- **Technical limitations:** Hallucination; nondeterminism; lack of provenance.
- **Human skill limitations:** Prompting/evaluation skills.
- **Tool limitations:** No safety-certified AI assistants; on-prem options limited.
- **Process limitations:** No approval framework for AI-generated engineering artefacts.
- **Frequency/severity:** Emerging but growing.
- **Time/cost impact:** N/A (potential).
- **Downstream effects:** Slow AI adoption; shadow usage risk.
- **Existing solutions:** Vendor AI features (Polarion Copilot, Codebeamer AI, etc.); on-prem LLMs; RAG over engineering data.
- **Why insufficient:** Not certified; trust gaps; integration.
- **Potential improvement areas:** Validated AI assistants; provenance/explainability; on-prem deployment; standards for AI in engineering.
- **Automation feasibility:** High for assistance; low for autonomous safety decisions.
- **AI applicability:** High (assistive) with strict human approval.
- **Human approval:** High.
- **Evidence/sources:**
  - raiqon AI above ALM: https://raiqon.com/resources/put-ai-above-alm/
  - Task.md section 7 AI/automation.
- **Confidence:** Medium
- **Unanswered:** AI adoption rates in automotive engineering; certified AI tooling timelines.

---

## 4. Cross-cutting analysis

### 4.1 Where toolchain problems concentrate
- **Configuration/ARXML layer** (W4-002, W4-003, W4-005) is the densest friction point because AUTOSAR's standardised but verbose XML is the lingua franca that every tool must handle, yet tool support is uneven.
- **Traceability layer** (W4-007, W4-029) is where compliance cost compounds; it spans every other domain.
- **Validation infrastructure** (W4-018, W4-019) is capital-gated and becomes the bottleneck as software volume grows.

### 4.2 Standards/compliance: genuine safety value vs administrative overhead
- **Genuine safety value:** hazard analysis, ASIL-appropriate verification, cybersecurity threat analysis, SOTIF scenario work, MISRA decidable rules, tool qualification.
- **Administrative overhead / candidate for automation:** evidence assembly, traceability link maintenance, deviation record management, assessment artefact preparation, manual data transformation, report formatting.
- Distinction matters: problems W4-008, W4-011, W4-013, W4-029 include both; automation should target evidence/administration, not safety judgement.

### 4.3 Root-cause themes
1. **Vendor lock-in and proprietary formats** (W4-001, W4-004, W4-023, W4-027).
2. **Standards/process weight without automation** (W4-011, W4-013, W4-015, W4-016).
3. **Semantic gap between tools** (W4-002, W4-005, W4-024, W4-028).
4. **Scarcity of specialised expertise and infrastructure** (W4-018, W4-022).
5. **AI/certification trust gap** (W4-030).

### 4.4 Automation feasibility summary
- High: mechanical data transformation, evidence aggregation, coverage reporting, CI build caching.
- Medium: ARXML validation/migration, traceability link suggestion, HIL scheduling, vECU integration, TARA assistance.
- Low: safety judgement, compliance interpretation, undecidable rule decisions, certification of AI.

---

## 5. Evidence and source index

| Problem | Key sources |
|---|---|
| W4-001 | Autodesk whitepaper; Porsche Consulting; MDPI integration compatibility |
| W4-002 | AUTOSAR serialization rules; dSPACE Compare; arXiv AUTOSAR Adaptive; Reddit ARForge |
| W4-003 | MDPI AUTOSAR BSW automation; selfdata README |
| W4-004 | Reddit/Dev.to ARForge; GitHub autosar-classic; Quora |
| W4-005 | dSPACE Compare; IAV Kedu; arXiv variant-rich CI/CD |
| W4-006 | Reddit DOORS; mgtechsoft; raiqon |
| W4-007 | Parasoft video; mgtechsoft; Jama |
| W4-008 | Parasoft false positives; Perforce MISRA; MISRA Compliance:2020; arXiv |
| W4-009 | Qt/Parasoft/Perforce migration guides; StackOverflow |
| W4-010 | Qt blog; MISRA Compliance:2020 |
| W4-011 | PTC/Jama/Synopsys ISO 26262 |
| W4-012 | Inferred (low evidence) |
| W4-013 | Tuxera; DNV; GrammaTech |
| W4-014 | mgtechsoft; TÜV SÜD (context) |
| W4-015 | Automotive IQ; UNECE R155 PDF; Secura |
| W4-016 | Redstone; ModernRequirements; itemis; UNECE R156 |
| W4-017 | dSPACE; SRM Tech; ScienceDirect; TÜV SÜD |
| W4-018 | Nature; Automotive IQ; OPAL-RT |
| W4-019 | dSPACE; RemotiveLabs; MathWorks |
| W4-020 | LDRA; Parasoft; IAR; arXiv |
| W4-021 | selfdata README; mgtechsoft |
| W4-022 | Reddit; Arorian/Codebeamer |
| W4-023 | Porsche Consulting; Autodesk |
| W4-024 | Automotive Manufacturing Solutions; mgtechsoft |
| W4-025 | Reddit ARForge; GitHub; VDA MoU |
| W4-026 | VDA MoU; Automotive IQ |
| W4-027 | Inferred (low evidence) |
| W4-028 | Autodesk; arXiv |
| W4-029 | Parasoft; LDRA |
| W4-030 | raiqon; Task.md |

---

## 6. Unanswered research questions (W4)

1. What is the quantified cost share of compliance administration vs engineering value across ASIL levels?
2. What are actual licence cost ranges and budget shares for major toolchains?
3. What are HIL utilisation rates and queue lengths in practice?
4. What are false-positive rates per MISRA tool/rule in real codebases?
5. What is the prevalence of invalid/non-conformant ARXML in the field?
6. How is SOTIF sufficiency demonstrated and accepted by regulators?
7. What are open-source AUTOSAR tool adoption rates in production?
8. What is the cost/frequency of supplier-OEM data exchange failures?
9. How much engineering effort is lost to glue work and manual transformation?
10. What are realistic AI assistant adoption and certification timelines in automotive engineering?

---

## 7. Notes on confidence and limitations

- Several problems (W4-012, W4-027) are inferred from industry structure with low direct evidence; marked accordingly.
- Quantitative impact figures are deliberately omitted where no reliable source exists (per master prompt rule: never invent statistics).
- Vendor marketing claims (e.g., "zero false positives") are treated as aspirational and cross-checked against MISRA Compliance:2020's statement that no tool is perfect.
- This report focuses on problem discovery only; no solutions or product recommendations are proposed.


---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

