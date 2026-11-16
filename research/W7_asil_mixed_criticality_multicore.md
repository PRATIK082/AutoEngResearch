# W7 — ASIL A/B/C/D, Mixed-Criticality, Multicore & Multi-Partition Integration

**Workstream:** W7 (ASIL / mixed-criticality / multicore / multi-partition / SWC-BSW-MCAL integration)
**Scope:** Problem discovery, root-cause analysis, evidence and unanswered questions for ASIL A/B/C/D implications, testing and cross-ASIL/QM issues in SW component design, multicore and multi-partition systems, application integration, BSW stack integration and MCAL challenges — across the lifecycle (architecture, design, integration, V&V, production, service, OTA).
**Date:** 2026-11-16
**Status:** Verify (Tier 2) — in progress (Phase A seed: §1 + W7-001..W7-010). Full target: 30 records.
**Method:** ISO 26262 series, AUTOSAR Classic/Adaptive SWS, MCU safety manuals, and repo-internal cross-reference (W1/W2/W3/W4/W5/W6, G1–G4). No numbers invented; quantitative claims only where sourced.

---

## 1. Workstream Summary

ASIL assignment changes everything downstream: architecture partitioning, SWC granularity, runnable-to-task mapping, BSW/MCAL selection, multicore allocation, interference analysis, test depth, tool qualification, and work products. QM is not "no safety" — it is the interference source that ASIL software must be proven free from. Multicore and multi-partition (OS-Applications, memory partitions, hypervisor guests) multiply the interference surface (memory, timing, execution, data consistency). BSW/MCAL integration is where ASIL claims most often break: driver behaviour, configuration consistency, and RTE contract gaps invalidate otherwise-correct application software.

Key tensions:

- **ASIL rigour vs. reuse:** higher ASIL demands stricter process/evidence, but platforms push common SWC/BSW across ASILs and QM.
- **Consolidation vs. isolation:** zonal/HPC concentrates mixed-criticality on shared SoCs; freedom from interference (FFI) is the price.
- **Performance vs. determinism:** multicore throughput tempts dynamic behaviour that safety analysis cannot bound.
- **Configuration vs. code:** BSW/MCAL safety lives mostly in configuration; review focuses on code and misses it.
- **Testing vs. analysis:** testing alone cannot prove FFI; analysis alone cannot prove timing — both are required, neither is sufficient.

This workstream owns ASIL/multicore truth. It complements W2 (AUTOSAR), W3 (testing), G1 (silicon), G2 (domains), W6 (bootloader). Canonical cross-refs: W2-004 (FFI), W2-006/W2-013 (multicore/scheduling), W2-009 (hypervisor), W2-012 (RTE contract), W2-018 (ASIL-B gap), W1-010 (timing), G1-020/021 (MCAL), W6-017/W6-021 (MCAL/BSW-bootloader).

---

## 2. Problem Catalogue

### W7-001 — ASIL-A implication set (design, process, testing, evidence)

- **Domain:** Functional safety / ASIL-A
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Teams treat ASIL-A as "QM with extra paperwork" or copy ASIL-B/C practices at full cost. ASIL-A has reduced but non-zero obligations (systematic faults, verification, integration testing, configuration management); under-scoping leaves systematic gaps, over-scoping wastes effort.
- **Who:** Safety managers, SW architects, SWC developers, test engineers.
- **Root cause:** No consolidated per-ASIL implication delta in project assets; ISO 26262 ASIL-dependent tables (methods, work products) not translated into team checklists.
- **Contributing factors:** Reuse of QM components without gap analysis; test strategy copied from QM; tool confidence (TCL) ignored.
- **Technical limitations:** ASIL-A still requires systematic-fault controls and integration/verification evidence; FFI obligations apply where coexisting with higher ASILs.
- **Tool limitations:** ALM templates default to QM or ASIL-D with no ASIL-A profile; trace tools do not flag ASIL-dependent method gaps.
- **Process limitations:** Tailoring rationale rarely documented; ASIL-A reviews skipped as "low risk".
- **Frequency/severity:** Pervasive (body/comfort, non-critical functions); low per-item severity, high aggregate exposure.
- **Time/cost impact:** Rework when audit finds missing work products; or 2–3× overspend from ASIL-D practices applied blindly.
- **Downstream effects:** Audit findings; late evidence generation; cost overrun.
- **Existing solutions:** ISO 26262 ASIL tables, ASPICE, company safety plans.
- **Why insufficient:** Tables state *what* is required, not *how much* for this component; no team-level delta checklist.
- **Automation feasibility:** High (ASIL-profile checklists, work-product completeness checks).
- **AI applicability:** Medium (tailoring assistant, evidence-gap finder; human signs off).
- **Human approval:** Mandatory (safety manager confirms ASIL-A tailoring).
- **Evidence/sources:** ISO 26262 (ASIL-dependent requirements); W1-014 (compliance overhead); W2-018 (ASIL gap context).
- **Confidence:** High.
- **Unanswered questions:** What is the measured cost delta ASIL-A vs QM in series programs?
- **Lesson learnt:** ASIL-A fails at audits, not on the road — missing tailoring rationale and thin integration-test evidence are the two repeat findings.
- **AI-ready guideline:** IF component is ASIL-A THEN enforce ASIL-A work-product checklist (tailoring rationale + systematic-fault measures + integration test evidence) WITH checklist artefact in ALM GATE safety-manager approval.

### W7-002 — ASIL-B implication set (design, process, testing, evidence)

- **Domain:** Functional safety / ASIL-B
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** ASIL-B is the most common target for consolidated/zonal software (and Adaptive's historical ceiling — W2-018), yet teams lack a crisp B-vs-A and B-vs-C boundary: which analyses (FMEA/FTA depth, dependent-failure analysis scope), which test methods (fault injection, robustness), and which architectural mechanisms (redundancy vs. monitoring) are mandatory at B.
- **Who:** Safety architects, system engineers, V&V leads.
- **Root cause:** ASIL-B sits at the inflection where single-point-fault metrics and FFI obligations bite, but guidance is interpreted per-project.
- **Contributing factors:** Adaptive-targets-B legacy; mixed B/D consolidation without re-analysis; supplier B-claims without evidence packages.
- **Technical limitations:** SPFM/LFM targets step up from A; dependent-failure analysis required in scope; FFI must be argued where B coexists with C/D or QM.
- **Tool limitations:** Safety-analysis tools compute metrics but do not validate architectural assumption coverage.
- **Process limitations:** Supplier safety case exchange (SEooC) incomplete at B; assumptions (AoU) untracked.
- **Frequency/severity:** High (zonal controllers, ADAS functions, powertrain auxiliaries).
- **Time/cost impact:** Late architecture rework when B metrics fail; supplier evidence loops measured in months.
- **Downstream effects:** Launch delay; consolidation blocked; cost of added monitors/redundancy late.
- **Existing solutions:** ISO 26262 hardware-architectural-metrics + FTA/FMEA practice; SEooC exchange.
- **Why insufficient:** Metric computation decoupled from software architecture decisions; AoU tracking manual.
- **Automation feasibility:** Medium (metric calculators tied to architecture model; AoU tracker).
- **AI applicability:** Medium (assumption-coverage checks, evidence-package completeness).
- **Human approval:** Mandatory (safety case sign-off).
- **Evidence/sources:** ISO 26262-4/-5/-9; W2-018; W1-016 (zonal challenges).
- **Confidence:** High.
- **Unanswered questions:** What fraction of ASIL-B claims on consolidated SoCs survive independent assessment first pass?
- **Lesson learnt:** ASIL-B programs die in the assumption log — untracked AoUs from SEooC components invalidate the safety case at assessment time.
- **AI-ready guideline:** IF ASIL-B on shared compute THEN require SPFM/LFM computation + dependent-failure analysis + AoU register WITH safety-case artefact GATE independent safety assessor.

### W7-003 — ASIL-C implication set (design, process, testing, evidence)

- **Domain:** Functional safety / ASIL-C
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** ASIL-C demands near-D rigour (systematic capability, verification independence, robustness/fault-injection testing) but teams plan it as "B plus a bit". Shortfalls concentrate in software systematic faults (design reviews, semi-formal methods), verification independence, and hardware-software interface (HSI) validation.
- **Who:** SW safety engineers, architects, integrators.
- **Root cause:** C-vs-D boundary misunderstood; independence and systematic-fault method tables under-applied.
- **Contributing factors:** Legacy B processes stretched to C; HSI owned by nobody; fault-injection planned but de-scoped under schedule pressure.
- **Technical limitations:** Systematic-fault controls step up (reviews, analyses, semi-formal notations where applicable); HSI verification is integration-critical.
- **Tool limitations:** Requirements/design tools do not enforce independence (author vs. reviewer vs. tester separation).
- **Process limitations:** Independence claimed but not evidenced (same engineer, different hat).
- **Frequency/severity:** Medium frequency; high severity (braking/steering auxiliaries, BMS, powertrain).
- **Time/cost impact:** Assessment failures; re-verification with independent testers late in program.
- **Downstream effects:** SOP risk; duplicate V&V spend.
- **Existing solutions:** ISO 26262-6 (software), -8 (verification independence), HSI specifications.
- **Why insufficient:** Independence and HSI treated as documentation, not as engineered activities with owners and time.
- **Automation feasibility:** Medium (independence matrix checks, HSI coverage tracking).
- **AI applicability:** Medium (review-coverage analysis, HSI gap detection).
- **Human approval:** Mandatory (independent assessor).
- **Evidence/sources:** ISO 26262-6/-8; W3 (testing depth context); G2 safety gradient.
- **Confidence:** High.
- **Unanswered questions:** What fault-injection coverage is actually achieved for ASIL-C software in series programs?
- **Lesson learnt:** ASIL-C assessments fail on independence and HSI — the code is usually fine, the verification organisation and interface evidence are not.
- **AI-ready guideline:** IF ASIL-C THEN enforce verification-independence matrix (author≠reviewer≠tester) + HSI test coverage WITH independence/HSI evidence GATE independent assessor.

### W7-004 — ASIL-D implication set (design, process, testing, evidence)

- **Domain:** Functional safety / ASIL-D
- **Lifecycle stage:** Architecture, design, integration, V&V, production, operation
- **Problem:** ASIL-D requires the full method set (highest systematic capability, independence, comprehensive analyses, full requirements-based + robustness + fault-injection testing, complete work products). Common failure: claiming D while inheriting B/C-grade software, tools, or processes without a documented upgrade path.
- **Who:** Safety managers, assessors, architects, all SW roles on D functions.
- **Root cause:** D treated as a label on unchanged engineering practice; upgrade deltas (methods, independence, tools, evidence) not planned or costed.
- **Contributing factors:** Reuse of proven-in-use arguments without usage-profile evidence; tool qualification (TCL) skipped; Freedom from interference assumed.
- **Technical limitations:** Full single-point/latent-fault metrics, full systematic-fault controls, full FFI argumentation, full change/impact analysis discipline.
- **Tool limitations:** Unqualified tools in D toolchain; coverage tools themselves untrusted.
- **Process limitations:** Change control laxity; proven-in-use claims without exposure data.
- **Frequency/severity:** Lower frequency; highest severity (braking, steering, vehicle motion control).
- **Time/cost impact:** Largest V&V spend in program; assessment rework measured in quarters.
- **Downstream effects:** SOP block; recall/field risk if D claimed but not earned.
- **Existing solutions:** ISO 26262 full method set; proven-in-use (26262-8 §14); tool qualification (26262-8 §11).
- **Why insufficient:** Upgrade path from B/C practice to D practice is project folklore, not a plan.
- **Automation feasibility:** Medium (D-completeness dashboard across methods, tools, evidence).
- **AI applicability:** Low-Medium (completeness checks only; no AI safety judgement).
- **Human approval:** Mandatory at every D gate (assessor).
- **Evidence/sources:** ISO 26262 (all parts); W2-004/W2-018; W3 testing depth; G2-ASIL-D domains.
- **Confidence:** High.
- **Unanswered questions:** What is the true cost multiplier QM→D for equivalent software functionality in series production?
- **Lesson learnt:** ASIL-D is a process and evidence discipline first, a technical challenge second — programs that plan the D upgrade path explicitly pass; those that "test into D" fail.
- **AI-ready guideline:** IF ASIL-D THEN require D-completeness dashboard (methods + independence + tool qualification + FFI + change discipline) green before assessment WITH dashboard export GATE external assessor.

### W7-005 — QM components: obligations, limits, and misuse

- **Domain:** Functional safety / QM
- **Lifecycle stage:** Architecture, design, integration, sourcing
- **Problem:** QM is misread as "no obligations". QM software coexisting with ASIL software creates FFI obligations on the ASIL side; QM components in safety-related functions need documented rationale; QM reuse into ASIL contexts without gap analysis is endemic.
- **Who:** Architects, safety managers, suppliers, purchasers.
- **Root cause:** "QM = no safety work" shorthand; safety/non-safety boundary drawn at component label instead of at interference paths.
- **Contributing factors:** Cost pressure to keep functions QM; infotainment/QM stacks consolidated with ASIL functions on HPC.
- **Technical limitations:** QM software needs no safety measures per se, but any shared resource (CPU, memory, bus, power) with ASIL software triggers FFI requirements on the ASIL partition.
- **Tool limitations:** Component catalogues carry QM/ASIL labels but not interference-relevance metadata.
- **Process limitations:** QM→ASIL reuse approved without impact analysis.
- **Frequency/severity:** Pervasive; severity inherited from the highest ASIL sharing the resource.
- **Time/cost impact:** Late FFI analysis and partitioning rework.
- **Downstream effects:** Safety-case invalidation; consolidation rollback.
- **Existing solutions:** ISO 26262 coexistence/FFI clauses; partitioning mechanisms.
- **Why insufficient:** Boundary drawn on paper, not verified on shared resources.
- **Automation feasibility:** High (shared-resource vs ASIL matrix → FFI obligation flag).
- **AI applicability:** Medium (interference-path enumeration assistant).
- **Human approval:** Required (safety manager confirms QM/ASIL boundary rationale).
- **Evidence/sources:** ISO 26262-6 Ann. D (coexistence); W2-004; W2-009.
- **Confidence:** High.
- **Unanswered questions:** How many QM→ASIL reuses in current platform programs have documented gap analyses?
- **Lesson learnt:** Every painful FFI finding I have seen started as "it's only QM" — the label was right, the shared-resource analysis was missing.
- **AI-ready guideline:** IF QM software shares any resource with ASIL software THEN raise FFI obligation on the ASIL side WITH shared-resource matrix GATE safety-manager approval.

### W7-006 — Freedom from interference: memory interference

- **Domain:** Mixed-criticality / FFI
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Shared memory/interconnect (caches, SRAM, DRAM, MMU/MPU configuration, DMA masters) allows QM or lower-ASIL partitions to corrupt or starve higher-ASIL partitions. MPU/MMU regions misconfigured, DMA unchecked, cache-partitioning absent.
- **Who:** Safety architects, MCAL/integration engineers, SoC engineers.
- **Root cause:** Interference channels enumerated late or never; MPU/MMU/DMA treated as bring-up detail, not safety mechanism.
- **Contributing factors:** Complex SoC interconnects; vendor safety-manual assumptions unimplemented; multi-vendor BSW touching MPU setup.
- **Technical limitations:** Requires memory partitioning + verified configuration + DMA control + stack/heap isolation; cache effects complicate worst-case reasoning (W1-010).
- **Tool limitations:** Static configuration checks exist but cross-tool MPU consistency (BSW vs. app vs. bootloader) rarely verified end-to-end.
- **Process limitations:** No memory-interference test/analysis owner.
- **Frequency/severity:** High on consolidated SoCs; severity up to ASIL-D.
- **Time/cost impact:** Late re-partitioning; re-verification of entire software load.
- **Downstream effects:** Safety case fails; performance regressions from added isolation.
- **Existing solutions:** MPU/MMU partitioning, MCAL MPU drivers, hypervisor memory isolation, ISO 26262-6 Ann. D.
- **Why insufficient:** Mechanisms exist; end-to-end configuration + verification is routinely incomplete.
- **Automation feasibility:** High (MPU-map consistency checks, DMA-master inventory, forbidden-access tests).
- **AI applicability:** Medium (configuration cross-checks, interference-path enumeration).
- **Human approval:** Mandatory (safety architect signs memory-partitioning argument).
- **Evidence/sources:** ISO 26262-6 Ann. D; MCU safety manuals; W2-004; G1 (memory wall/interconnect).
- **Confidence:** High.
- **Unanswered questions:** What is the field escape rate attributable to memory-partitioning misconfiguration?
- **Lesson learnt:** Memory FFI is a configuration problem wearing an architecture costume — the diagram is always correct, the MPU table is where it breaks.
- **AI-ready guideline:** IF mixed-criticality on shared memory THEN require machine-checked MPU/MMU map + DMA-master allow-list + forbidden-access negative tests WITH map + test report GATE safety architect.

### W7-007 — Freedom from interference: timing interference

- **Domain:** Mixed-criticality / FFI
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Shared CPUs, buses, and interconnects let QM/lower-ASIL load delay higher-ASIL execution beyond deadlines. WCET estimates invalidated by cache, contention, and interrupt storms; no timing budget enforcement at runtime.
- **Who:** Timing analysts, OS/integration engineers, safety architects.
- **Root cause:** Timing analysis unsolved for modern SoCs (W1-010); budgets assigned but not measured or enforced.
- **Contributing factors:** Multicore contention; Ethernet/CAN congestion; badly-bounded interrupt loads; middleware jitter (W2-007/W2-008).
- **Technical limitations:** Requires bounded interference (time partitioning, budget enforcement, priority design) + measurement + analysis; full proof generally infeasible — argument is assurance-case based.
- **Tool limitations:** Timing tools model single-core well, multicore/shared-interconnect poorly.
- **Process limitations:** Timing validation left to HIL/vehicle testing; no timing owner per cause chain.
- **Frequency/severity:** High; severity up to ASIL-D (braking/steering/motion).
- **Time/cost impact:** Late timing fixes (re-mapping, re-prioritisation) cascade through integration.
- **Downstream effects:** Deadline misses; degraded fallback; safety-case timing annex fails.
- **Existing solutions:** Time partitioning, execution-time budgets, watchdogs/deadline monitoring, TSN where applicable.
- **Why insufficient:** Budgets without enforcement and measurement are wishes; analysis without contention modelling is fiction.
- **Automation feasibility:** Medium (budget tracking, schedule simulation, runtime deadline-miss logging).
- **AI applicability:** Medium (timing-anomaly detection from traces; no timing proof).
- **Human approval:** Mandatory (timing safety case).
- **Evidence/sources:** ISO 26262-6; W1-010; W1-018; W2-006/W2-013; SAE timing literature.
- **Confidence:** High.
- **Unanswered questions:** What deadline-miss rates are observed in-field for consolidated mixed-criticality ECUs?
- **Lesson learnt:** Timing FFI is won by budgets with teeth — runtime enforcement (budget monitors, watchdogs) catches what analysis misses, and measurement on target beats simulation every time.
- **AI-ready guideline:** IF mixed-criticality shares execution resources THEN require per-partition timing budgets + runtime enforcement + target measurement WITH timing annex GATE safety architect.

### W7-008 — Freedom from interference: execution / control-flow interference

- **Domain:** Mixed-criticality / FFI
- **Lifecycle stage:** Design, integration, V&V
- **Problem:** Errant QM/lower-ASIL code (runaway tasks, stuck ISRs, corrupted function pointers, misuse of supervisor calls) disrupts higher-ASIL execution: locks held, OS objects abused, mode changes triggered, watchdogs serviced incorrectly.
- **Who:** OS/B
...[truncated 6154 chars]