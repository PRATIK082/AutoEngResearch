# W7 — ASIL A/B/C/D, Mixed-Criticality, Multicore & Multi-Partition Integration

**Workstream:** W7 (ASIL / mixed-criticality / multicore / multi-partition / SWC-BSW-MCAL integration)
**Scope:** Problem discovery, root-cause analysis, evidence and unanswered questions for ASIL A/B/C/D implications, testing and cross-ASIL/QM issues in SW component design, multicore and multi-partition systems, application integration, BSW stack integration and MCAL challenges — across the lifecycle (architecture, design, integration, V&V, production, service, OTA).
**Date:** 2026-11-16
**Status:** Verify (Tier 2) — records complete (W7-001..W7-030). Integration into indices/artefacts/book (Phase D) pending.
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
- **Who:** OS/integration engineers, safety architects, BSW developers.
- **Root cause:** OS objects and MCU control interfaces are globally reachable; no capability discipline, and QM code runs at the same privilege as ASIL code on many Classic ECUs.
- **Contributing factors:** Third-party QM stacks with supervisor access; watchdog serviced from QM context; mode-management APIs callable from any partition.
- **Technical limitations:** Requires privilege separation (user/supervisor), OS-Application access control, watchdog-manager separation (ASIL vs QM supervised entities), and safe-state enforcement independent of QM cooperation.
- **Tool limitations:** OS configuration tools generate access masks but rarely verify least-privilege completeness.
- **Process limitations:** No reviewer owns the OS-access/privilege matrix.
- **Frequency/severity:** Medium; severity up to ASIL-D.
- **Time/cost impact:** OS re-configuration late; re-test of whole integration.
- **Downstream effects:** Deadlock, missed deadlines, defeated watchdogs, uncontrolled mode changes.
- **Existing solutions:** AUTOSAR OS-Applications, trusted/untrusted functions,driving-mode managers, external window watchdogs.
- **Why insufficient:** Mechanisms configured permissively by default; watchdog placed where QM can kick it.
- **Automation feasibility:** High (privilege/access-matrix checks, watchdog-independence checks).
- **AI applicability:** Medium (over-permissive access detection).
- **Human approval:** Mandatory (safety architect signs execution-FFI argument).
- **Evidence/sources:** AUTOSAR SWS OS; ISO 26262-6 Ann. D; W2-004/W2-006; W7-006/W7-007.
- **Confidence:** High.
- **Unanswered questions:** How many production OS configurations grant QM code supervisor-mode access?
- **Lesson learnt:** The watchdog must never trust the watched — every watchdog-defeat incident traces to QM-context servicing or shared-window configuration.
- **AI-ready guideline:** IF mixed-criticality shares an OS THEN require least-privilege access matrix + ASIL-owned watchdog supervision WITH OS-access audit + watchdog-independence test GATE safety architect.

### W7-009 — QM–ASIL coupling through shared libraries and common-cause paths

- **Domain:** Mixed-criticality / dependent failure
- **Lifecycle stage:** Architecture, design, sourcing, V&V
- **Problem:** ASIL and QM partitions share libraries (math, crypto, middleware), compilers, tools, and sometimes requirements authors — a single systematic fault couples them. Dependent-failure analysis (DFA) is hardware-focused; software common-cause paths are hand-waved.
- **Who:** Safety analysts, architects, supplier quality.
- **Root cause:** DFA scope stops at hardware; shared software/toolchain/people paths treated as independent by assertion.
- **Contributing factors:** Common compiler/toolchain; shared open-source components; same team developing ASIL + QM variants (W5 reuse risk).
- **Technical limitations:** Requires systematic-fault controls proportionate to ASIL (reviews, diverse verification) plus explicit common-cause argument for shared elements.
- **Tool limitations:** DFA tools model hardware coupling; software coupling captured in prose, if at all.
- **Process limitations:** No coupling register linking shared elements to ASIL claims.
- **Frequency/severity:** Medium; severity inherits highest ASIL.
- **Time/cost impact:** Assessment findings; late diversification or re-verification.
- **Downstream effects:** Safety-case dependent-failure annex fails; common-cause single point of failure.
- **Existing solutions:** ISO 26262-9 §7 (DFA), checklists for coupling factors, diverse redundancy where justified.
- **Why insufficient:** Checklists filled generically; software coupling factors rarely evidenced.
- **Automation feasibility:** Medium (shared-element inventory vs. ASIL claims → coupling flags).
- **AI applicability:** Medium (coupling-path enumeration from BOM/dependency data).
- **Human approval:** Mandatory (DFA sign-off).
- **Evidence/sources:** ISO 26262-9 §7; W5 (reuse/systematic faults); W7-002/W7-004.
- **Confidence:** High.
- **Unanswered questions:** What share of assessed programs evidence software common-cause paths vs. asserting independence?
- **Lesson learnt:** Independence is guilty until proven innocent — every shared compiler, library, and author is a coupling path until the DFA says otherwise with evidence.
- **AI-ready guideline:** IF ASIL claim coexists with QM/shared elements THEN require coupling register (shared libs, tools, people, requirements) + DFA annex WITH coupling evidence GATE safety assessor.

### W7-010 — SWC partitioning by ASIL (granularity, allocation, interface discipline)

- **Domain:** SW component design / mixed-criticality
- **Lifecycle stage:** Architecture, design
- **Problem:** SWCs are cut along functional or team lines, not ASIL lines — one SWC mixes ASIL-D logic with QM diagnostics, forcing the whole SWC (and its tests, reviews, tools) to the highest ASIL and exploding cost; or ASIL logic leaks across SWC boundaries, defeating partitioning.
- **Who:** SW architects, safety engineers, function owners.
- **Root cause:** No ASIL-driven decomposition rule; functional decomposition inherited from legacy ECUs.
- **Contributing factors:** Supplier SWC boundaries fixed by contract; model-based codegen at wrong granularity; calibration sprawl across SWCs.
- **Technical limitations:** SWC granularity must balance ASIL homogeneity (fine) against RTE/COM overhead and scheduling complexity (coarse); interfaces must carry ASIL-relevant contracts (ranges, freshness, failure modes).
- **Tool limitations:** Architecture tools show functional decomposition, not ASIL-homogeneity heatmaps.
- **Process limitations:** Partitioning decided once at architecture freeze, never revisited after ASIL allocation changes.
- **Frequency/severity:** Pervasive on consolidated ECUs; severity = cost explosion or safety-case weakness.
- **Time/cost impact:** Whole-SWC uplift to ASIL-D multiplies V&V cost; re-partitioning late breaks interfaces.
- **Downstream effects:** V&V cost overrun; FFI argument complicated by mixed-ASIL SWCs.
- **Existing solutions:** ASIL-homogeneous decomposition guidance, AUTOSAR SWC/ports, safety contracts per port.
- **Why insufficient:** Guidance not enforced; no homogeneity metric tracked.
- **Automation feasibility:** High (ASIL-homogeneity metric per SWC; mixed-ASIL interface flags).
- **AI applicability:** Medium (re-partitioning proposals; human decides).
- **Human approval:** Required (architect + safety manager approve decomposition).
- **Evidence/sources:** ISO 26262-6 (software architectural design); W2-011 (SWC design); W2-012 (RTE); W7-005.
- **Confidence:** High.
- **Unanswered questions:** What ASIL-homogeneity threshold (e.g. % QM code in ASIL SWC) is economically optimal?
- **Lesson learnt:** Mixed-ASIL SWCs tax every change — splitting by ASIL at architecture time is an order of magnitude cheaper than uplifting a mixed SWC under audit pressure.
- **AI-ready guideline:** IF SWC contains mixed ASILs THEN flag for decomposition review and require per-port safety contract WITH homogeneity report GATE architect + safety manager.

### W7-011 — Runnable-to-task mapping under ASIL constraints

- **Domain:** SW component design / AUTOSAR OS
- **Lifecycle stage:** Design, integration
- **Problem:** Runnable entities with different ASILs are mapped to shared OS tasks without ASIL-aware rules — high-ASIL runnables inherit priority inversion, preemption delay, or blocking from QM runnables in the same task, and task-level failure handling cannot distinguish ASIL severities.
- **Who:** SW architects, RTE/OS integrators, safety engineers.
- **Root cause:** Mapping driven by rate/period convenience, not ASIL, criticality, or blocking analysis; no allocation constraint recorded.
- **Contributing factors:** Legacy task sets reused on consolidated ECUs; model-based tools auto-map runnables by sample time; mixed-ASIL mode and error handling share one task body.
- **Technical limitations:** Requires per-task ASIL ceiling, priority/preemption design, blocking budgets, and separation of error handling by ASIL; coarse tasks reduce overhead but couple criticalities.
- **Tool limitations:** RTE/mapping tools optimise for CPU load, not ASIL separation or preemption analysis.
- **Process limitations:** No reviewer owns the runnable-to-task allocation argument; mapping changes post-integration without re-analysis.
- **Frequency/severity:** Common on consolidated ECUs; severity up to ASIL-D where braking/steering runnables share tasks.
- **Time/cost impact:** Late re-mapping forces RTE regeneration, re-test, and timing re-validation; extent unquantified.
- **Downstream effects:** Priority inversion; FFI timing argument fails; W7-007 timing interference worsens.
- **Existing solutions:** ASIL-aware task allocation, AUTOSAR OS priorities/preemption, blocking analysis, W7-010 SWC partitioning.
- **Why insufficient:** Allocation rules exist as guidance but are not machine-checked against the actual mapping.
- **Automation feasibility:** High (mixed-ASIL task flags, priority/blocking consistency checks).
- **AI applicability:** Medium (re-mapping proposals; human decides).
- **Human approval:** Required (architect + safety manager approve task map).
- **Evidence/sources:** ISO 26262-6 (software architectural design); AUTOSAR SWS OS; W2-011; W2-012; W7-007; W7-010.
- **Confidence:** High.
- **Unanswered questions:** What re-mapping cost is typical once integration testing has started?
- **Lesson learnt:** Tasks are criticality boundaries, not just rate groups — a QM runnable in an ASIL-D task buys the whole task's failure modes.
- **AI-ready guideline:** IF runnable set maps to shared OS tasks THEN require per-task ASIL ceiling + priority/preemption rationale WITH mapping report GATE architect + safety manager.

### W7-012 — Data consistency across ASIL boundaries

- **Domain:** Mixed-criticality / RTE communication
- **Lifecycle stage:** Design, implementation, V&V
- **Problem:** Concurrent read/write across ASIL boundaries corrupts shared data — implicit vs. explicit RTE communication chosen by default, unprotected global buffers, torn reads on multi-word signals, and race conditions between QM producers and ASIL-D consumers.
- **Who:** Application engineers, RTE integrators, safety analysts.
- **Root cause:** Data-consistency mechanism (Ioc, implicit buffers, exclusive areas, lock-free discipline) not selected per safety relevance; coherence treated as performance tuning.
- **Contributing factors:** Implicit communication assumed sufficient; calibration and XCP writes bypass protection; multicore concurrent access multiplies races.
- **Technical limitations:** Requires per-data consistency classification (explicit/implicit, coherency class, protection scope) plus verification of atomicity and freshness; overhead trades against determinism.
- **Tool limitations:** RTE generators emit configured mechanism correctly but do not flag safety-relevant data left unprotected.
- **Process limitations:** No data-consistency owner per signal group; reviews check names and scaling, not concurrency semantics.
- **Frequency/severity:** Frequent on consolidated stacks; severity up to ASIL-D on actuation paths.
- **Time/cost impact:** Race fixes late require interface redesign and regression; extent unquantified.
- **Downstream effects:** Corrupted actuation commands; nondeterministic test results; safety-case data argument fails.
- **Existing solutions:** AUTOSAR implicit/explicit communication, IOC, exclusive areas, E2E on top for transmission faults (W7-013).
- **Why insufficient:** Mechanisms configured per signal, but safety-driven selection criteria and cross-core verification are routinely skipped.
- **Automation feasibility:** High (unprotected shared-data detection, coherency-class consistency checks).
- **AI applicability:** Medium (race-path enumeration from access graphs; human confirms).
- **Human approval:** Required (safety engineer signs data-consistency classification).
- **Evidence/sources:** ISO 26262-6 Ann. D; AUTOSAR SWS RTE; W2-012; W7-006; W7-011.
- **Confidence:** High.
- **Unanswered questions:** What share of field data-corruption escapes trace to implicit/explicit misconfiguration?
- **Lesson learnt:** Every cross-ASIL datum needs a named consistency mechanism, or the integration test will name it for you.
- **AI-ready guideline:** IF data crosses ASIL boundaries THEN require declared coherency class + protection mechanism WITH consistency report GATE safety engineer.

### W7-013 — End-to-end protection (E2E profiles, CRC/sequence)

- **Domain:** Safety communication / E2E
- **Lifecycle stage:** Design, integration, V&V
- **Problem:** Safety-relevant signals travel COM, Pdus, gateways, and RTE without E2E protection, or with wrong profile/counter/CRC configuration — systematic communication faults (repetition, loss, delay, masquerade, incorrect addressing) pass undetected across QM intermediaries.
- **Who:** Safety engineers, COM/RTE integrators, gateway engineers.
- **Root cause:** E2E treated as library call rather than end-to-end safety mechanism; protect/check placement and profile selection not derived from fault model.
- **Contributing factors:** Gateway repacking strips E2E headers; variant/calibration changes alter PDU layout without E2E regeneration; QM middleware assumed benign.
- **Technical limitations:** Requires profile selection per ISO 26262-6 and AUTOSAR E2E (counter, CRC, timeout, address checks), correct protect/check partitioning, and residual-fault reasoning; adds bus and CPU overhead.
- **Tool limitations:** E2E generators configure chosen profile but do not verify end-to-end coverage across gateways and repacking.
- **Process limitations:** E2E verification left to fault-injection campaigns that arrive too late to fix placement errors.
- **Frequency/severity:** Common on gatewayed and zonal architectures; severity up to ASIL-D.
- **Time/cost impact:** Late profile or placement fixes cascade through COM/PduR/gateway configuration; extent unquantified.
- **Downstream effects:** Undetected corruption or staleness reaches actuation; freedom-from-systematic-fault argument fails.
- **Existing solutions:** AUTOSAR E2E profiles, COM signal protection, deadline/freshness monitoring.
- **Why insufficient:** Library availability confused with end-to-end coverage; gateway and repacking paths escape the protected region.
- **Automation feasibility:** High (protect/check placement checks, profile-consistency verification, coverage-gap flags).
- **AI applicability:** Medium (E2E gap detection; no residual-fault proof).
- **Human approval:** Mandatory (safety manager signs E2E argument).
- **Evidence/sources:** ISO 26262-4/6; AUTOSAR SWS E2E; W2-012; W2-013; W7-012.
- **Confidence:** High.
- **Unanswered questions:** What residual systematic-fault rate remains for gatewayed E2E paths in production?
- **Lesson learnt:** E2E protects a path, not a function call — every gateway, repack, or variant fork redraws the path and must redraw the protection.
- **AI-ready guideline:** IF signal is safety-relevant THEN require E2E profile + protect/check placement covering full path WITH E2E coverage report GATE safety manager.

### W7-014 — Multicore core allocation for mixed-criticality

- **Domain:** Multicore architecture / mixed-criticality
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** ASIL-D, lower-ASIL, and QM software are placed on cores by load balancing alone — safety-relevant workloads share cores with noisy QM neighbours, lockstep cores wasted on QM, and allocation carries no isolation rationale traceable to the safety case.
- **Who:** System architects, safety architects, SoC/integration engineers.
- **Root cause:** No core-allocation policy linking ASIL and FFI to lockstep vs. performance cores, cache/interconnect sharing, and degradation strategy.
- **Contributing factors:** Vendor core recommendations ignored; hypervisor guest placement decided by team ownership; performance benchmarks override isolation needs.
- **Technical limitations:** Requires per-core ASIL capability model (lockstep, memory protection, timing isolation), interference budgets per core pair, and fallback allocation on core failure; isolation costs throughput.
- **Tool limitations:** Allocation tools balance load and affinity; none enforce ASIL-to-core capability rules.
- **Process limitations:** Core allocation frozen by performance testing before safety review; re-allocation treated as retune, not re-argument.
- **Frequency/severity:** Pervasive on multicore consolidation; severity up to ASIL-D.
- **Time/cost impact:** Late re-allocation invalidates timing, FFI, and V&V evidence; extent unquantified.
- **Downstream effects:** W7-006/W7-007 interference arguments collapse; lockstep benefit unrealised; W7-015 timing analysis invalid.
- **Existing solutions:** Lockstep cores, ASIL-capability allocation, hypervisor pinning, MCU safety manuals.
- **Why insufficient:** Hardware capability exists; allocation rationale and interference accounting per core pair are missing.
- **Automation feasibility:** Medium (capability-rule checks, forbidden co-location flags, allocation-trace generation).
- **AI applicability:** Medium (allocation optimisation under isolation constraints; human approves).
- **Human approval:** Mandatory (safety architect signs core-allocation argument).
- **Evidence/sources:** ISO 26262-6 Ann. D; MCU safety manuals; AUTOSAR SWS OS multicore; G1 silicon; W7-006; W7-007.
- **Confidence:** Medium.
- **Unanswered questions:** What performance penalty do production projects accept for ASIL-driven core isolation?
- **Lesson learnt:** Cores are safety devices first and compute pools second — placing by load and bolting on isolation afterwards never survives the audit.
- **AI-ready guideline:** IF mixed-criticality uses multicore THEN require ASIL-to-core capability allocation + co-location exclusions WITH allocation rationale GATE safety architect.

### W7-015 — Multicore scheduling and timing analysis gaps

- **Domain:** Multicore timing / AUTOSAR OS
- **Lifecycle stage:** Design, integration, V&V
- **Problem:** Global vs. partitioned scheduling chosen by default, AUTOSAR OS ScheduleTables and spinlock discipline misaligned with timing budgets, and single-core WCET estimates reused on multicore — contention, migration, and blocking erode every margin.
- **Who:** Timing analysts, OS engineers, integrators.
- **Root cause:** Multicore timing treated as scaled single-core timing; scheduling policy, migration, cross-core blocking, and contention absent from analysis.
- **Contributing factors:** Spinlocks held across preemption; ScheduleTable expiry jitter; inter-core communication latency unbudgeted; measurement on unloaded benches.
- **Technical limitations:** Requires partitioned-vs-global rationale, per-core WCET with contention bounds, spinlock/blocking analysis, and ScheduleTable verification; full multicore timing proof generally infeasible (W1-010).
- **Tool limitations:** Single-core WCET and schedule simulation mature; multicore contention and cross-core blocking modelled poorly or not at all.
- **Process limitations:** Timing owned by no one across cores; ScheduleTable changes without timing re-signoff.
- **Frequency/severity:** High on multicore AD/ADAS and zonal controllers; severity up to ASIL-D.
- **Time/cost impact:** Late schedule rework cascades through tasks, cores, and communication; extent unquantified.
- **Downstream effects:** Deadline misses under load; degraded modes trigger spuriously; timing safety case rejected.
- **Existing solutions:** Partitioned scheduling, ScheduleTables, spinlock protocols, execution-budget monitoring, W7-007 timing budgets.
- **Why insufficient:** Mechanisms configured without multicore-aware analysis; measurement rarely covers worst-case contention.
- **Automation feasibility:** Medium (ScheduleTable consistency checks, spinlock-hold audits, deadline-miss logging).
- **AI applicability:** Medium (timing-anomaly detection from traces; no WCET proof).
- **Human approval:** Mandatory (timing safety case).
- **Evidence/sources:** ISO 26262-6; AUTOSAR SWS OS; W1-010; W1-018; W2-006; W7-007; W7-014.
- **Confidence:** High.
- **Unanswered questions:** What contention margins do production multicore timing cases actually apply?
- **Lesson learnt:** Multicore timing is an integration measurement discipline, not a calculation — budgets, ScheduleTables, and spinlock rules only hold if re-verified under full contention.
- **AI-ready guideline:** IF multicore scheduling carries ASIL deadlines THEN require scheduling-policy rationale + contention-aware budgets + ScheduleTable verification WITH timing report GATE safety architect.

### W7-016 — Inter-core communication and spinlocks (shared-memory hazards, lock ordering, deadlock)

- **Domain:** Multicore / mixed-criticality
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Cores exchange data through shared memory guarded by AUTOSAR OS spinlocks or lock-free IOC; lock ordering violations, unbounded spin-waits, missing deadlock analysis, and unprotected cross-core paths let a low-ASIL core corrupt or stall a high-ASIL core.
- **Who:** Multicore architects, OS/RTE integrators, safety engineers.
- **Root cause:** Inter-core data flows designed as functional signals, not as concurrency hazards — lock scope, ordering, and blocking budgets never specified per ASIL.
- **Contributing factors:** Fine-grained sharing for performance; nested spinlock acquisition across modules; vendor BSW acquiring locks opaquely; cross-core FFI argument missing (W7-006, W7-008).
- **Technical limitations:** Spinlock hold times must be bounded and analyzable; lock-free IOC buffers need single-writer discipline plus overflow policy; cross-core blocking feeds response-time analysis (W1-010).
- **Tool limitations:** Spinlock call-graph and lock-order extraction is manual; RTE/IOC generators emit code without blocking-time budgets or ordering reports.
- **Process limitations:** No owner for the global lock-ordering and inter-core data-flow inventory across suppliers.
- **Frequency/severity:** Common on multicore consolidation; severity up to ASIL-D (deadlock or data corruption).
- **Time/cost impact:** Late deadlock discovery forces redesign of task allocation and re-verification of timing; extent unquantified.
- **Downstream effects:** Timing safety case fails; ordering fixes ripple into scheduling and core allocation (W2-006, W2-013).
- **Existing solutions:** AUTOSAR OS spinlocks with defined ordering, IOC cross-core communication, lock-free single-writer patterns, ISO 26262-6 Ann. D interference guidance.
- **Why insufficient:** Mechanisms exist but system-wide ordering, hold-time budgets, and negative tests are routinely omitted.
- **Automation feasibility:** High (lock-order graph checks, shared-variable inventory, missing-guard flags).
- **AI applicability:** Medium (interference-path enumeration, ordering proposals; human decides).
- **Human approval:** Required (safety architect approves lock-ordering and blocking-time argument).
- **Evidence/sources:** AUTOSAR SWS OS (spinlocks, IOC); ISO 26262-6 Ann. D; W2-006; W2-013; W7-006; W7-008; W1-010.
- **Confidence:** High.
- **Unanswered questions:** What fraction of field-relevant multicore faults trace to spinlock ordering versus unguarded sharing?
- **Lesson learnt:** Multicore fails at the lock table, not the core map — every shared word needs one named guard and one ordering rule, or the integration test will find the missing one at the worst moment.
- **AI-ready guideline:** IF cores share memory across ASILs THEN require enumerated guards plus machine-checked lock ordering plus blocking budgets WITH lock-order graph and guard inventory GATE safety architect.

### W7-017 — Multicore timing verification (contention, cache effects, measurement vs analysis)

- **Domain:** Multicore timing / V&V
- **Lifecycle stage:** Integration, verification, validation
- **Problem:** Shared interconnect, memory, and cache contention make single-core WCET estimates invalid on multicore; teams rely on measurement-only timing that misses rare contention alignments, or on analysis that ignores multicore interference, leaving cross-ASIL timing FFI unproven.
- **Who:** Timing engineers, integrators, safety assessors.
- **Root cause:** Timing verification planned per task, not per interference channel — contention sources treated as noise rather than bounded analysis inputs.
- **Contributing factors:** Cache partitioning absent; DMA and accelerator masters unmodeled; core allocation changing late; trace coverage insufficient for rare alignments (W7-007).
- **Technical limitations:** Requires bounded interference model (arbitration, DRAM refresh, cache behaviour) plus combined measurement-and-analysis argument; full multicore WCET analysis remains impractical for complex SoCs.
- **Tool limitations:** Timing tools model single-core well but multicore contention analysis and worst-case alignment generation are immature; trace tools lack interference attribution.
- **Process limitations:** No timing-interference budget owner; timing sign-off decoupled from core-allocation decisions.
- **Frequency/severity:** Pervasive on consolidated multicore; severity up to ASIL-D deadline miss.
- **Time/cost impact:** Late contention discovery forces re-allocation, cache partitioning retrofit, or hardware change; extent unquantified.
- **Downstream effects:** Schedule overrun; degraded performance from added isolation; safety case rejection at assessment.
- **Existing solutions:** Response-time analysis with blocking terms, measurement-based timing with interference stress tests, cache locking/partitioning, ISO 26262-6 timing analysis methods.
- **Why insufficient:** Neither pure measurement nor pure analysis covers multicore contention; combined method is guidance, not enforced practice.
- **Automation feasibility:** Medium (interference-channel checklist, trace-coverage gaps, budget-vs-measurement comparison).
- **AI applicability:** Medium (contention-scenario generation, anomaly triage; human signs argument).
- **Human approval:** Mandatory (safety assessor approves multicore timing argument).
- **Evidence/sources:** ISO 26262-6 (timing analysis); AUTOSAR timing extensions; W1-010; W2-013; W7-007; G1 (interconnect, memory wall).
- **Confidence:** High.
- **Unanswered questions:** Which combined measurement-plus-analysis pattern gives assessor-acceptable multicore evidence at lowest cost?
- **Lesson learnt:** Measurement shows the timing you got, not the timing you could get — on multicore, unstressed contention channels always reappear as rare deadline misses in the field.
- **AI-ready guideline:** IF ASIL tasks share interconnect or memory THEN require bounded interference model plus stressed measurement evidence WITH timing-interference report GATE safety assessor.

### W7-018 — OS-Applications and memory partitions (trusted vs untrusted, restart, containment)

- **Domain:** Multi-partition / OS separation
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** AUTOSAR OS-Applications isolate memory and execution by trust level, but trusted/untrusted classification drifts, partition restart and termination policies are unspecified, and a faulted QM partition escapes containment or takes down an ASIL partition with it.
- **Who:** OS integrators, safety architects, BSW engineers.
- **Root cause:** Partitions drawn around suppliers or features rather than ASIL and fault-containment needs; restart behaviour treated as integration detail, not safety requirement.
- **Contributing factors:** Overuse of trusted partitions for convenience; cross-partition service calls bypassing isolation; MPU region sprawl shared with W7-006; supplier binaries demanding trusted status.
- **Technical limitations:** Requires least-privilege partition design, verified MPU/MMU regions per OS-Application, specified restart/terminate semantics, and proven non-interference of restart on surviving partitions.
- **Tool limitations:** OS configuration tools generate regions but lack cross-checks against the ASIL allocation and least-privilege violations.
- **Process limitations:** No gate reviews trusted-status grants; partition-restart testing owned by nobody.
- **Frequency/severity:** Frequent on multi-supplier ECUs; severity up to ASIL-D containment breach.
- **Time/cost impact:** Late re-partitioning invalidates integration tests and safety analyses; extent unquantified.
- **Downstream effects:** Dependent-failure argument collapses; restart defects surface as cascading resets in the field.
- **Existing solutions:** AUTOSAR OS OS-Applications with trusted/untrusted, memory protection, partition restart/termination, ISO 26262-6 Ann. D.
- **Why insufficient:** Mechanism widely available; least-privilege discipline plus restart verification is rarely completed end to end.
- **Automation feasibility:** High (trusted-surface inventory, region-vs-allocation cross-check, restart-path coverage).
- **AI applicability:** Medium (privilege-escalation path enumeration, restart-test generation).
- **Human approval:** Required (safety architect approves partition and restart argument).
- **Evidence/sources:** AUTOSAR SWS OS; ISO 26262-6 Ann. D; W2-004; W7-006; W7-009; W6-024 (partition-aware V&V).
- **Confidence:** Medium.
- **Unanswered questions:** What restart-latency and surviving-partition interference bounds do assessors accept for ASIL-D containment claims?
- **Lesson learnt:** Trusted is a debt instrument — every trusted partition granted for schedule convenience is repaid with interest in fault-injection testing and assessor scrutiny.
- **AI-ready guideline:** IF partition hosts mixed trust or ASIL THEN require least-privilege region proof plus specified restart semantics plus fault-injection evidence WITH partition argument GATE safety architect.

### W7-019 — Hypervisor guests for mixed-criticality (ASIL guest beside QM Linux/Android)

- **Domain:** Virtualization / mixed-criticality
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** An ASIL guest (e.g. AUTOSAR Classic or ASIL MCU firmware) shares a SoC with a QM Linux or Android guest; virtio devices, shared memory, virtual interrupts, and scheduler configuration create interference and corruption paths, while the hypervisor itself is claimed as safety element without proven isolation evidence.
- **Who:** Hypervisor integrators, SoC architects, safety managers.
- **Root cause:** Hypervisor adopted as consolidation mechanism first and safety mechanism second — guest interference channels and hypervisor safety case scoped after guest bring-up.
- **Contributing factors:** Virtio queue and backend sharing; paravirtualised I/O through QM guests; vCPU overcommit and scheduling jitter; hypervisor configuration sprawling across vendors (W2-009).
- **Technical limitations:** Requires spatial isolation per guest, bounded virtual-interrupt latency, virtio backend hardening or mediation, and hypervisor developed or qualified to the needed ASIL with independent assessment.
- **Tool limitations:** Guest-interference analysis tooling is immature; virtio data-flow and shared-page tracking across guest images lacks standard checks.
- **Process limitations:** No single owner for the cross-guest FFI argument spanning hypervisor vendor, guest OS vendors, and application suppliers.
- **Frequency/severity:** Growing with HPC and zonal consolidation; severity up to ASIL-D (QM guest corrupting safety guest).
- **Time/cost impact:** Hypervisor re-qualification or re-architecture late is effectively a platform restart; extent unquantified.
- **Downstream effects:** Type-approval and safety-case blockage; performance loss from added mediation and pinning.
- **Existing solutions:** Type-1 hypervisors with static partitioning, vCPU pinning, device passthrough for safety I/O, ISO 26262-8 qualification and SEooC arguments.
- **Why insufficient:** Static partitioning guidance exists but virtio and shared-service paths routinely reintroduce coupling that the partitioning claim ignores.
- **Automation feasibility:** Medium (shared-page inventory, virtio-path enumeration, vCPU-config consistency checks).
- **AI applicability:** Medium (cross-guest interference enumeration; human assesses safety case).
- **Human approval:** Mandatory (safety assessor approves hypervisor-as-safety-element claim).
- **Evidence/sources:** ISO 26262-8 (qualification, SEooC); hypervisor safety manuals; W2-009; W7-006; W7-007; W7-009; G1 (HPC silicon).
- **Confidence:** Medium.
- **Unanswered questions:** Which virtio device classes have assessor-accepted hardening patterns for ASIL guests beside QM Linux?
- **Lesson learnt:** The hypervisor diagram always shows isolation and the virtio backend always shares something — safety guests survive only when every shared queue has a named mediator and a corruption test.
- **AI-ready guideline:** IF ASIL guest shares SoC with QM guest THEN require static partition config plus enumerated virtio and shared-memory paths plus hypervisor safety case WITH isolation evidence GATE safety assessor.

### W7-020 — Inter-partition communication gateways (I/O routing, FFI boundary, covert channels)

- **Domain:** Multi-partition communication / mixed-criticality
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Gateways route signals, PDUs, and service calls between partitions and guests of different ASILs; the gateway becomes an unverified FFI boundary — unvalidated routing lets faulty QM data poison ASIL functions, gateway overload propagates timing faults, and shared gateway state opens covert or side channels.
- **Who:** System architects, COM/gateway engineers, safety engineers.
- **Root cause:** Gateway specified as connectivity feature (route this frame here) rather than as safety boundary with validation, quota, and failure semantics per flow.
- **Contributing factors:** Shared buffers and routing tables across ASILs; end-to-end protection terminated and re-originated at the gateway without analysis (W7-013); gateway hosted in a QM partition for convenience.
- **Technical limitations:** Requires per-flow validation (range, freshness, plausibility), temporal quotas and overload policy, E2E protection continuity decisions, and information-flow analysis for covert channels.
- **Tool limitations:** Gateway configuration tools check routability, not per-flow ASIL contracts, quota sufficiency, or information-flow leakage.
- **Process limitations:** Gateway treated as BSW configuration owned by integration, with no safety sign-off per routed flow.
- **Frequency/severity:** Common on zonal and HPC topologies; severity up to ASIL-D (systematic cross-partition corruption).
- **Time/cost impact:** Late gateway hardening forces re-routing, re-validation, and re-timing of many flows; extent unquantified.
- **Downstream effects:** Cascade failures across partitions; timing FFI argument fails; cybersecurity-safety interface findings (ISO/SAE 21434 coupling).
- **Existing solutions:** Validating gateways with E2E protection profiles, COM callouts and firewall rules, time-triggered gateway schedules, ISO 26262-6 Ann. D.
- **Why insufficient:** Point mechanisms exist; complete per-flow contracts plus overload and covert-channel analysis are rarely produced.
- **Automation feasibility:** High (flow-vs-contract matrix, unvalidated-flow flags, quota-vs-load checks).
- **AI applicability:** High (routing-table contract cross-checks, leakage-path enumeration; human approves boundary).
- **Human approval:** Required (safety architect approves gateway FFI argument per flow).
- **Evidence/sources:** AUTOSAR COM, E2E protection, OS/IoHwAb routing; ISO 26262-6 Ann. D; ISO/SAE 21434 (channels); W7-006; W7-013; W2-012.
- **Confidence:** Medium.
- **Unanswered questions:** What covert-channel bandwidth analysis, if any, do assessors require for gateway-shared state between QM and ASIL-D?
- **Lesson learnt:** Gateways are firewalls that nobody configured as firewalls — every unvalidated routed flow is a remote procedure call from your least-trusted partition into your most-trusted one.
- **AI-ready guideline:** IF gateway routes across ASILs THEN require per-flow validation plus quota and E2E continuity decision plus flow-matrix review WITH gateway FFI report GATE safety architect.

### W7-021 — Mode management across partitions (ECU modes, partition requests, inconsistent assumptions)

- **Domain:** Multi-partition / mode management
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** Partitions and SWCs assume inconsistent ECU/system modes (startup, run, sleep, degraded, limp-home): one partition requests sleep while an ASIL-D function assumes run, or degraded-mode behaviour is defined per-partition with no coherent global mode state.
- **Who:** System architects, BSW integrators, safety engineers, OS integrators.
- **Root cause:** No single owner of the global mode state machine; partition-local mode handling (AUTOSAR EcuM/BswM per partition, OS-Application mode requests) specified without a cross-partition mode contract.
- **Contributing factors:** Supplier SWCs with private mode assumptions; hypervisor guests managing own power/mode; legacy state machines carried into consolidated ECUs.
- **Technical limitations:** Requires coherent mode propagation with bounded latency, safe-mode fallback on disagreement, and ASIL-aware arbitration of conflicting partition mode requests (safety-relevant modes must win).
- **Tool limitations:** BswM/EcuM configuration tools validate per-partition rules, not cross-partition mode coherence or request-arbitration completeness.
- **Process limitations:** Mode tables reviewed per-SWC, never as a cross-partition truth table including degraded and fault modes.
- **Frequency/severity:** Common on multi-partition consolidated ECUs; severity up to ASIL-D (wrong-mode actuation or shutdown of safety function).
- **Time/cost impact:** Late mode-table reconciliation; re-validation of shutdown/sleep/wakeup sequences; extent unquantified.
- **Downstream effects:** Untested mode combinations reach field; FFI and safe-state arguments fail audit.
- **Existing solutions:** AUTOSAR EcuM/BswM mode arbitration, RTE mode ports/switch, hypervisor-coordinated power control, ISO 26262 safe-state derivation.
- **Why insufficient:** Mechanisms arbitrate locally; no enforced global mode invariant across partitions and guests.
- **Automation feasibility:** High (cross-partition mode-matrix consistency checks, unhandled-combination enumeration, conflicting-request simulation).
- **AI applicability:** Medium (mode-combination gap analysis; human defines safe fallback).
- **Human approval:** Required (system architect + safety manager sign mode matrix and arbitration rules).
- **Evidence/sources:** ISO 26262-4/6 (system/software architectural design, safe states); AUTOSAR SWS EcuM/BswM/RTE mode management; W7-018 (OS-Applications/partitions).
- **Confidence:** High.
- **Unanswered questions:** What fraction of field mode-related incidents traces to cross-partition disagreement versus single-partition logic errors?
- **Lesson learnt:** Every partition behaves correctly in its own assumed mode and the vehicle still fails — the bug lives in the mode combination nobody tabulated.
- **AI-ready guideline:** IF more than one partition can request ECU or functional modes THEN require a single cross-partition mode matrix with arbitration and safe fallback WITH matrix + conflict-simulation report GATE architect + safety manager.

### W7-022 — SWC to RTE to OS integration chain (contract vs execution phase, generation mismatch)

- **Domain:** Application/BSW integration / RTE contract
- **Lifecycle stage:** Design, implementation, integration, V&V
- **Problem:** Contract-phase RTE assumptions (ports, data types, runnable triggers, timing) diverge from execution-phase generated RTE and OS configuration: stale ARXML, mismatched generator versions, or runnable-to-task mappings edited post-generation break the integration silently.
- **Who:** RTE integrators, SWC developers, OS configurators, suppliers.
- **Root cause:** SWC contract, RTE generation input, and OS task configuration versioned and generated as separate steps with no atomic binding between them.
- **Contributing factors:** Multi-supplier ARXML exchange with divergent toolchains; manual post-generation tweaks; contract-phase testing against a simulated RTE that differs from the target build.
- **Technical limitations:** Requires byte-identical traceability from SWC description to generated RTE to OS schedule, including trigger periods, data-consistency semantics, and exclusive-area behaviour.
- **Tool limitations:** RTE generators diff poorly across versions; contract-vs-generated consistency checkers exist but are rarely run in CI across supplier drops (W2-012).
- **Process limitations:** No generation-fingerprint gate; integration testing proceeds against whatever RTE was last generated.
- **Frequency/severity:** Pervasive in multi-supplier Classic projects; severity up to ASIL-D (missed trigger, torn data, dead runnable).
- **Time/cost impact:** Integration-phase debug of faults injected at contract time; regeneration cascades that invalidate prior test evidence.
- **Downstream effects:** Test results voided by unrecorded regeneration; safety work products reference the wrong generated artefact.
- **Existing solutions:** AUTOSAR RTE contract/execution phases, generation-time consistency checks, W2-012 RTE contract discipline.
- **Why insufficient:** Checks are advisory and local; version skew across suppliers is a process gap, not a generator bug.
- **Automation feasibility:** High (ARXML-to-generated-RTE hash binding, contract-vs-execution diff in CI, trigger-map verification).
- **AI applicability:** Medium (skew diagnosis, stale-interface flagging across supplier drops).
- **Human approval:** Required (integrator signs generation fingerprint before integration test campaign).
- **Evidence/sources:** AUTOSAR SWS RTE, AUTOSAR methodology (contract vs execution); ISO 26262-6/8 (integration, consistency); W2-012.
- **Confidence:** High.
- **Unanswered questions:** How often does post-generation manual editing (vs generator version skew) cause the contract-execution divergence?
- **Lesson learnt:** Testing against the wrong RTE proves nothing except that the wrong RTE works — bind the contract, the generator, and the OS schedule or the integration evidence is fiction.
- **AI-ready guideline:** IF RTE generation inputs change THEN block integration testing until contract-vs-generated consistency check passes WITH generation fingerprint + diff report GATE integrator.

### W7-023 — COM and signal-service mapping under ASIL (grouping, atomicity, gateway timing)

- **Domain:** Communication / mixed-criticality integration
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** ASIL-relevant signals cross COM/SOME-IP/service boundaries with broken grouping, torn updates, or unbounded gateway latency: signal groups split across PDUs, end-to-end protection applied inconsistently, and SOME/IP service timing assumed rather than budgeted against Classic COM deadlines.
- **Who:** Communication designers, gateway engineers, safety engineers.
- **Root cause:** Signal-to-PDU and signal-to-service mapping done for bus-load efficiency, not for ASIL data-consistency and freshness requirements.
- **Contributing factors:** Mixed Classic/Adaptive stacks on one ECU; gateway routing between CAN/CAN-FD/Ethernet with unbudgeted queuing; E2E protection configured per-link instead of end-to-end (W2-002, G5-001).
- **Technical limitations:** Requires atomic signal-group transfer, configured E2E profiles with residual-error reasoning, and freshness/timeout budgets decomposed across COM, gateway, and service layers.
- **Tool limitations:** Bus-configuration tools optimise load and pack signals; cross-layer freshness-budget and atomicity verification is manual.
- **Process limitations:** No owner of the end-to-end signal path spanning COM, gateway, and SOME/IP services.
- **Frequency/severity:** Common on zonal/gateway ECUs; severity up to ASIL-D (stale or torn safety signal acted upon).
- **Time/cost impact:** Late PDU repacking; gateway retiming; re-validation of timeout and E2E fault handling.
- **Downstream effects:** Dependent-failure and cascading-failure arguments collapse; ADAS/chassis functions see phantom or frozen inputs.
- **Existing solutions:** AUTOSAR COM signal groups, E2E protection profiles, SOME/IP-SD with timeouts, gateway PDU routing with timing budgets, W2-002 middleware guidance.
- **Why insufficient:** Each layer protects itself; nobody verifies atomicity plus freshness plus E2E across the full mapped path.
- **Automation feasibility:** High (signal-path atomicity checks, freshness-budget summation, E2E coverage verification per ASIL signal).
- **AI applicability:** Medium (stale-signal path detection, repacking proposals preserving groups).
- **Human approval:** Required (communication architect + safety engineer sign signal-path budgets).
- **Evidence/sources:** AUTOSAR SWS COM/E2E/SOME-IP; ISO 26262-4/6 (communication, dependent failures); W2-002; G5-001; W7-013 (E2E protection).
- **Confidence:** High.
- **Unanswered questions:** What gateway queuing contribution dominates freshness violations on mixed CAN/Ethernet paths?
- **Lesson learnt:** The signal is correct on every bus trace and wrong at the application — grouping and freshness are end-to-end properties, and the gateway in the middle owns neither.
- **AI-ready guideline:** IF a signal carries an ASIL requirement THEN require mapped end-to-end path with atomic group, E2E profile, and freshness budget WITH signal-path dossier GATE communication architect + safety engineer.

### W7-024 — Variant and calibration binding in mixed-criticality builds (post-build variants, sprawl, wrong-variant flashing)

- **Domain:** Variants / calibration / production
- **Lifecycle stage:** Integration, production, service, OTA update
- **Problem:** Post-build variants and calibration sets multiply across ASILs without binding: calibration sprawl across SWCs, variant selectors resolved at flash or first-startup time, and wrong-variant or mixed-variant datasets flashed together, silently invalidating the homologated ASIL configuration.
- **Who:** Calibration engineers, variant managers, production/flashing engineers, safety engineers.
- **Root cause:** Variant and calibration binding treated as data logistics, not as part of the safety configuration; no machine-checked link between code variant, dataset variant, and the ASIL evidence that qualified the combination.
- **Contributing factors:** Per-market/per-powertrain dataset explosion; supplier-delivered partial datasets; service and OTA flows that update code without its matched calibration (W4 family, W6 bootloader/update).
- **Technical limitations:** Requires variant-aware safety case: allowed code-plus-data combinations enumerated, post-build selectors constrained, and incompatible combinations rejected at flash and at startup.
- **Tool limitations:** Variant tools manage code switches or calibration labels, rarely the cross-product binding with ASIL qualification status.
- **Process limitations:** Calibration released on its own cadence with no joint sign-off against the exact software it pairs with.
- **Frequency/severity:** Frequent on high-variant platforms; severity up to ASIL-D (unqualified code-plus-data combination in field).
- **Time/cost impact:** Dataset rework; reflash campaigns; re-qualification of unplanned combinations.
- **Downstream effects:** Homologation evidence voided; wrong-variant behaviour misdiagnosed as software defects.
- **Existing solutions:** AUTOSAR post-build loadable/selectable variants, calibration (XCP/ASAM) workflows, variant-coding checks at EOL, W4 variant discipline.
- **Why insufficient:** Binding is documented, not enforced; flash-time and startup-time checks cover identity, not ASIL-qualification compatibility.
- **Automation feasibility:** High (allowed-combination matrix, code-plus-data fingerprint match at flash and boot, sprawl metrics per SWC).
- **AI applicability:** Medium (unqualified-combination detection across release trains; dataset-diff risk flags).
- **Human approval:** Required (variant manager + safety manager approve allowed-combination matrix per release).
- **Evidence/sources:** AUTOSAR methodology (post-build variants); ISO 26262-8 (configuration management); W4 variant records; W6 bootloader/update records.
- **Confidence:** Medium.
- **Unanswered questions:** Which binding point (flash-time check vs startup-time check vs backend release gate) catches most wrong-variant escapes?
- **Lesson learnt:** The code was qualified and the data was qualified but the combination never was — variant safety lives in the pairing, and the pairing is what nobody signs.
- **AI-ready guideline:** IF a build supports post-build variants or separate calibration THEN require an allowed code-plus-data combination matrix enforced at flash and startup WITH matrix + fingerprint-match log GATE variant manager + safety manager.

### W7-025 — Application integration testing across ASILs (SiL/HiL coverage, fault injection, robustness)

- **Domain:** Integration testing / mixed-criticality V&V
- **Lifecycle stage:** Integration, verification, validation
- **Problem:** SiL/HiL campaigns verify each ASIL function in isolation but never the mixed-criticality whole: cross-ASIL interference absent from test scope, fault injection stops at unit level, and robustness (resource exhaustion, babbling QM sender, corrupted shared data) is untested at integration level.
- **Who:** Integration test engineers, safety V&V engineers, HiL rig owners.
- **Root cause:** Test plans structured per-SWC/per-ASIL, with no integration-level test objectives derived from the interference analysis and dependent-failure analysis.
- **Contributing factors:** Limited HiL rig time allocated to nominal scenarios; fault-injection harnesses incompatible with production RTE/OS builds; QM components excluded from safety test scope by definition.
- **Technical limitations:** Requires mixed-criticality integration suite: concurrent ASIL+QM load, injected memory/timing/communication faults, and pass criteria tied to FFI and safe-state entry, not just functional correctness.
- **Tool limitations:** SiL/HiL fault-injection and interference-generation tooling exists but is rig-specific and unqualified; coverage metrics count requirements, not interference paths.
- **Process limitations:** Fault-injection and robustness testing scheduled after functional sign-off, when failures can only be waived, not fixed.
- **Frequency/severity:** Pervasive; severity up to ASIL-D (interference-induced failure escapes to road).
- **Time/cost impact:** Rig-time contention; late-discovered interference forces architecture rework under SOP pressure.
- **Downstream effects:** Safety case claims integration testing that never exercised the claimed interference freedom; field robustness failures.
- **Existing solutions:** ISO 26262-4/6 integration and fault-injection testing, HiL-based robustness testing, W3 test methods, W7-006..W7-009 interference analysis as test input.
- **Why insufficient:** Interference analyses exist as documents and tests exist as rigs, but the trace from interference path to injected test case is routinely missing.
- **Automation feasibility:** High (interference-path-to-test-case trace matrix, automated fault-injection sequences, robustness regression suites).
- **AI applicability:** Medium (gap analysis between interference catalogue and executed cases; failure-clustering across runs).
- **Human approval:** Required (safety assessor confirms interference coverage before integration sign-off).
- **Evidence/sources:** ISO 26262-4/6 (integration, fault injection, robustness); W3 testing records; W7-006..W7-009; W2-004.
- **Confidence:** High.
- **Unanswered questions:** What minimum injected-fault coverage per interference channel is sufficient for ASIL-D integration sign-off?
- **Lesson learnt:** Green per-function tests plus an untested integration is how interference ships — the rig must be hostile, with QM tenants misbehaving on purpose, or the test proves only the sunny day.
- **AI-ready guideline:** IF mixed-criticality software reaches integration THEN require fault-injection and robustness cases traced to every identified interference path WITH trace matrix + injection results GATE safety assessor.

### W7-026 — BSW module integration under ASIL (mixed-ASIL stacking, scheduler, ownership)

- **Domain:** BSW stack integration / mixed-criticality
- **Lifecycle stage:** Architecture, design, integration, V&V
- **Problem:** BSW modules of differing ASILs (e.g. QM COM stack alongside ASIL-D NvM/Fee, ASIL-B Dem) are stacked into one ECU image sharing the BSW scheduler (SchM) exclusive areas, call contexts and memory sections, with no single owner for the integrated ASIL claim.
- **Who:** BSW integrators, safety architects, stack suppliers.
- **Root cause:** BSW selected per-function from multiple vendors; ASIL capability claimed per module, but interference properties of the stacked combination are never specified or verified.
- **Contributing factors:** SchM exclusive-area misuse and over-long critical sections; mixed-ASIL BSW sharing tasks/partitions; vendor integration manuals applied selectively.
- **Technical limitations:** Requires per-module ASIL claim + SchM/OsApplication isolation argument + verified exclusive-area discipline; ASIL decomposition does not apply cleanly across shared BSW scheduler state.
- **Tool limitations:** BSW generators compose modules without checking cross-module ASIL compatibility or exclusive-area conflicts.
- **Process limitations:** No named BSW integration owner; each vendor validates its module standalone and disclaims the stack.
- **Frequency/severity:** Common on consolidated ECUs; severity up to ASIL-D.
- **Time/cost impact:** Late SchM rework and re-validation of whole stack when interference is found during integration testing.
- **Downstream effects:** FFI argument fails; timing regressions from added guards; safety case left with per-module certificates and no stack claim.
- **Existing solutions:** AUTOSAR SchM exclusive areas, OS-Application access control, vendor ASIL certificates, ISO 26262-8 qualification of software components.
- **Why insufficient:** Mechanisms exist but stacked-combination verification and ownership are routinely missing; certificates do not compose.
- **Automation feasibility:** High (SchM conflict inventory, BSW ASIL-mixing matrix, over-long critical-section flags).
- **AI applicability:** Medium (stacking-risk review, exclusive-area misuse detection; human decides).
- **Human approval:** Required (BSW integrator + safety manager approve stacked ASIL argument).
- **Evidence/sources:** ISO 26262-4, ISO 26262-8 Cl. 12; AUTOSAR SchM/OS SWS; W2-005; W2-012; W6-017.
- **Confidence:** High.
- **Unanswered questions:** Which SchM misuse patterns dominate stacked-BSW integration escapes across vendors?
- **Lesson learnt:** BSW stacks fail at the seams, not inside modules — the certificate folder is thick and the integration argument is one page of hand-waving.
- **AI-ready guideline:** IF BSW image mixes ASILs THEN require stacked-combination FFI check + SchM exclusive-area audit WITH stacking matrix + audit report GATE BSW integrator + safety manager.

### W7-027 — BSW configuration consistency (ECUC parameters, multi-tool, post-build loadables)

- **Domain:** BSW configuration / integration
- **Lifecycle stage:** Design, configuration, integration, production
- **Problem:** ECUC parameter sets for the same ECU diverge across configuration tools and vendors (ARXML round-trips, generator-version skew, hand edits), and post-build loadable datasets are flashed without re-validation of consistency, timing, or ASIL relevance.
- **Who:** BSW configuration engineers, integrators, calibration/release teams.
- **Root cause:** No single ECUC source of truth; configuration treated as tool output rather than safety-related work product under ISO 26262-6/8.
- **Contributing factors:** Multi-vendor toolchains (generator-specific parameters); copy-paste projects across MCU derivatives; post-build variants edited at the hex/hex-diff level.
- **Technical limitations:** Requires ECUC consistency rules + generation-time checks + load-time compatibility verification (hardware, software, config-set version triplet); partial-update consistency is a distinct argument.
- **Tool limitations:** Config tools validate syntax per module, not cross-module semantic consistency or ASIL-relevant constraint violations.
- **Process limitations:** Configuration review is diff-by-eyeball; post-build datasets released under calibration process with weaker safety gates.
- **Frequency/severity:** Pervasive in multi-variant programmes; severity up to ASIL-D for safety-related parameters.
- **Time/cost impact:** Late config-debug cycles on HIL/vehicle; variant-matrix explosion multiplies re-validation cost.
- **Downstream effects:** Wrong thresholds, timeouts, or Dem/DTC settings ship to field; update path (cf. W6-021) inherits inconsistent baseline.
- **Existing solutions:** AUTOSAR ECUC/STMD validation, generator consistency checks, post-build loadable checksum/version checks.
- **Why insufficient:** Checks are per-tool and per-generation; cross-tool, cross-version, and post-build-loadable consistency is rarely closed-loop verified.
- **Automation feasibility:** High (ECUC cross-tool diff, constraint-rule engine, loadable-compatibility gate).
- **AI applicability:** High (consistency-rule checking, anomaly flagging on parameter drift; human approves).
- **Human approval:** Required (integrator + safety engineer sign config-set release).
- **Evidence/sources:** AUTOSAR ECUC/BSW SWS; ISO 26262-6, ISO 26262-8 Cl. 10 (documentation); W2-005; W6-021.
- **Confidence:** High.
- **Unanswered questions:** What fraction of field configuration escapes originate in post-build loadables versus pre-compile configuration?
- **Lesson learnt:** Configuration is code that nobody reviews — the projects that survive treat every ECUC set like a source release, with diffs, rules, and a signature.
- **AI-ready guideline:** IF ECUC set or post-build loadable is released THEN require machine-checked consistency + compatibility-triplet verification WITH check report + version manifest GATE integrator + safety engineer.

### W7-028 — RTE/BSW contract gaps (BswM/EcuM states, NvM job results, Dem discipline)

- **Domain:** RTE/BSW contracts / state and fault handling
- **Lifecycle stage:** Design, integration, V&V
- **Problem:** Implicit RTE/BSW contracts are assumed but not enforced: BswM/EcuM mode and state transitions race with application runnables, NvM job results (pending/busy/error) go unchecked by callers, and Dem/DTC enable, debounce and storage discipline varies per project, breaking ASIL fault-handling chains.
- **Who:** RTE integrators, BSW engineers, safety engineers, application developers.
- **Root cause:** AUTOSAR SWS specifies module behaviour but project-level mode, state, and error-handling contracts are left to integration convention rather than executable interface contracts.
- **Contributing factors:** EcuM/BswM rule tables edited late; NvM async jobs polled inconsistently; Dem event/DTC mapping owned by diagnostics team, not safety chain owner.
- **Technical limitations:** Requires explicit mode/state machine per ECU + per-call NvM result handling + Dem debouncing/storage rationale tied to fault-tolerant time interval and safe-state transitions.
- **Tool limitations:** RTE generators wire ports without checking state-dependent call legality or NvM/Dem error-path coverage.
- **Process limitations:** No owner for the end-to-end fault-handling chain from detection (Dem) through mode action (BswM) to safe state (EcuM).
- **Frequency/severity:** Common; severity up to ASIL-D where NvM/Dem/BswM carry safety mechanisms.
- **Time/cost impact:** Late state-machine debugging on target; fault-injection re-runs when error paths prove untested.
- **Downstream effects:** Lost NVM writes after reset, missed DTCs, wrong mode actions; safety mechanisms credited in analysis but absent in code.
- **Existing solutions:** AUTOSAR EcuM/BswM/NvM/Dem SWS, mode-management design patterns, fault-injection testing.
- **Why insufficient:** Module-level SWS compliance does not guarantee project-level contract closure; error paths stay untested by default.
- **Automation feasibility:** High (state-contract checks, unchecked-NvM-result flags, Dem-mapping completeness checks, fault-path coverage).
- **AI applicability:** Medium (contract extraction from SWS plus gap flagging; human confirms safety relevance).
- **Human approval:** Required (safety engineer + integrator approve mode/error contracts and fault-path evidence).
- **Evidence/sources:** AUTOSAR EcuM/BswM/NvM/Dem/RTE SWS; ISO 26262-4, ISO 26262-6; W2-005; W2-012; W6-017.
- **Confidence:** Medium.
- **Unanswered questions:** Which contract gap class (mode race, NvM result, Dem discipline) contributes most to systematic safety-mechanism failures?
- **Lesson learnt:** The happy path integrates itself; the error path never does — every painful integration traces back to an NvM result nobody checked or a mode nobody owned.
- **AI-ready guideline:** IF safety mechanism depends on BswM/EcuM/NvM/Dem THEN require explicit state/error contract + negative-path tests WITH contract table + test evidence GATE safety engineer + integrator.

### W7-029 — MCAL portability and verification burden (MCU change, re-verification, safety-manual assumptions)

- **Domain:** MCAL / hardware-software interface
- **Lifecycle stage:** Architecture, design, integration, change management
- **Problem:** MCU change or derivative step (same family, new stepping, second source) invalidates MCAL verification: drivers are assumed portable via AUTOSAR MCAL API, but register behaviour, errata workarounds, and MCU safety-manual assumptions differ, forcing unplanned driver re-verification.
- **Who:** MCAL engineers, hardware/safety architects, procurement-driven platform teams.
- **Root cause:** Portability assumed at API level while safety depends on register/errata/safety-manual level.
- **Contributing factors:** Generator-style configuration carried over unchecked; errata workarounds versioned per silicon, not per safety case; safety-manual assumptions (clock, voltage, lockstep setup) implemented in bootloader or app, not MCAL.
- **Technical limitations:** Requires per-MCU-derivative verification delta + errata-workaround traceability + safety-manual assumption register (assumption, owner, implementation, test).
- **Tool limitations:** MCAL configuration tools migrate parameters across derivatives without flagging safety-relevant semantic changes.
- **Process limitations:** MCU swap treated as procurement change, not safety-relevant change request; impact analysis scoped to compile success.
- **Frequency/severity:** Frequent on cost-optimised platforms; severity up to ASIL-D where drivers implement safety mechanisms.
- **Time/cost impact:** Unplanned re-verification of MCAL plus dependent BSW/bootloader layers; schedule slip scales with derivative distance.
- **Downstream effects:** Bootloader/flash path (W6-017/W6-021) and timing arguments silently invalidated; latent errata exposure.
- **Existing solutions:** AUTOSAR MCAL SWS, MCU safety manuals, errata sheets, ISO 26262-5 HSI specification.
- **Why insufficient:** Artefacts exist but the derivative-delta argument and assumption register are rarely maintained as living evidence.
- **Automation feasibility:** High (register/errata diff, config-migration flags, assumption-register completeness checks).
- **AI applicability:** Medium (derivative-impact drafting and assumption extraction; human verifies).
- **Human approval:** Required (hardware-safety architect + MCAL owner approve derivative delta).
- **Evidence/sources:** MCU safety manuals and errata; ISO 26262-5; AUTOSAR MCAL SWS; G1-020; G1-021; W6-017; W6-021.
- **Confidence:** High.
- **Unanswered questions:** What verification delta is sufficient for a same-family MCU stepping versus a second-source MCU?
- **Lesson learnt:** MCAL portability ends where the errata begins — plan the derivative delta like a new development or the silicon will plan it for you.
- **AI-ready guideline:** IF MCU derivative or stepping changes THEN require verification-delta analysis + errata/assumption-register update WITH delta report + regression evidence GATE hardware-safety architect + MCAL owner.

### W7-030 — MCU driver safety challenges (watchdog, GPT, ADC, DMA, HSI and lockstep/EDC interaction)

- **Domain:** MCAL drivers / safety mechanisms
- **Lifecycle stage:** Design, implementation, integration, V&V
- **Problem:** Safety-critical MCU drivers (watchdog, GPT, ADC, DMA, clock/reset, EDC/ECC and lockstep support) carry faults that defeat system safety mechanisms: misconfigured watchdog windows, GPT drift, ADC plausibility gaps, DMA bypassing MPU intent, and hardware-safety-interaction (HSI) assumptions about lockstep/EDC left unvalidated in software.
- **Who:** MCAL/driver engineers, safety architects, HSI owners.
- **Root cause:** Drivers treated as proven vendor code while their configuration is the safety mechanism; HSI (ISO 26262-5) assumptions about lockstep comparators, EDC/ECC reporting, and fault-reaction paths are not tested end-to-end.
- **Contributing factors:** Watchdog serviced in wrong context or with wrong window; DMA masters added without allow-list update (cf. W7-006); ADC redundancy diverse in name only; lockstep/EDC error injection never executed on target.
- **Technical limitations:** Requires per-driver safety rationale (failure modes, diagnostics, reaction time) + window/timing validation on target + DMA allow-list enforcement + EDC/lockstep error-injection evidence.
- **Tool limitations:** Driver generators produce initialisation code without checking window, timing, or redundancy semantics against the safety case.
- **Process limitations:** HSI owned jointly by hardware and software teams, which in practice means owned by neither; driver faults escape unit test into integration.
- **Frequency/severity:** Recurrent in audit findings; severity up to ASIL-D.
- **Time/cost impact:** Target-level debug and re-validation of watchdogs, timers, and DMA paths late in integration.
- **Downstream effects:** Watchdog credited but ineffective, stale sensor data undetected, DMA corruption bypassing partitions, lockstep faults unhandled.
- **Existing solutions:** MCU safety manuals, vendor MCAL safety packages, watchdog/GPT/ADC SWS, hardware error-injection facilities.
- **Why insufficient:** Safety packages provide building blocks; project-specific HSI validation and configuration-as-mechanism evidence remain incomplete.
- **Automation feasibility:** Medium (window/timing rule checks, DMA-master inventory, injection-test harness generation).
- **AI applicability:** Medium (driver-config review and injection-case proposal; human approves and witnesses target runs).
- **Human approval:** Required (safety architect + HSI owner witness target-level evidence).
- **Evidence/sources:** MCU safety manuals; ISO 26262-5; AUTOSAR MCAL SWS (Wdg, Gpt, Adc, DMA); G1-020; G1-021; W2-005.
- **Confidence:** Medium.
- **Unanswered questions:** Which driver class yields the highest systematic-fault escape rate after vendor safety-package adoption?
- **Lesson learnt:** A watchdog is only as honest as its window and its context — every dead programme had a watchdog that barked on schedule and guarded nothing.
- **AI-ready guideline:** IF driver implements or hosts a safety mechanism THEN require per-driver safety rationale + target-validated timing + error-injection evidence WITH rationale + test logs GATE safety architect + HSI owner.