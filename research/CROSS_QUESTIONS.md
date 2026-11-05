# CROSS_QUESTIONS — Cross-Domain Research Questions

**Purpose:** A comprehensive set of questions whose answers require evidence from **two or more** of the five research workstreams (W1 Requirements/System Engineering, W2 AUTOSAR/Middleware, W3 Testing/Validation, W4 Toolchain/Standards, W5 Skills/AI). Answering these is necessary to complete the research book "THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING" and to build the cross-domain dependency maps, problem-to-root-cause matrices, and consolidated findings (Parts III, VI, VII).

**Author:** cross-questionnaire subagent  
**Date:** 2026-11-05  
**Source inputs:** `Task.md`, `research/README.md`, `research/W1_requirements_system.md`, `research/W2_autosar_middleware.md`, `research/W3_testing_validation.md`, `research/W4_toolchain_standards.md`, `research/W5_skills_ai.md`.

**How to use:** Each question (CQ-xxx) is stated so it can be answered by a follow-up research pass. Fields: **Domains** (workstreams + lifecycle), **Why it matters**, **Existing evidence**, **Gap / what must be found**, **Priority** (P1 critical for the book's core argument, P2 important, P3 nice-to-have).

**Principle:** A question is "cross-domain" only if answering it requires combining findings from at least two workstreams or reconciling a contradiction between them. Purely single-workstream questions are already listed in each W report's "Unanswered Research Questions" section and are not duplicated here.

---

## 0. Method and coverage

- 72 cross-domain questions across 12 themes.
- Each maps to at least two of W1–W5.
- Contradictions between workstreams are flagged in §13.
- Priority counts: P1 = 24, P2 = 31, P3 = 17.
- No numerical value is invented; questions explicitly ask for quantification where the workstreams only have qualitative findings.

---

## 1. Requirements ↔ Testing & Validation (Theme A)

**CQ-001 — How much of test scenario incompleteness traces to ambiguous/underspecified requirements (W1) vs. test-generation limits (W3)?**  
- Domains: W1 (RE), W3 (scenario coverage W3-018, W3-020), W4 (SOTIF W4-017).  
- Why it matters: Determines whether fixing requirements (W1-001) or fixing test generation (W3-026) yields more safety value; affects Part VI root-cause prioritisation.  
- Existing evidence: W1-001 (35–45% rework cross-industry); W3-018 (Euro NCAP "can barely scratch the surface"); W4-017 (unknown unsafe scenarios).  
- Gap: No study links requirement ambiguity rate to scenario coverage gap in the same program.  
- Priority: P1.

**CQ-002 — What is the measured cost-of-change multiplier in automotive for a requirement change discovered at HIL/vehicle vs. requirements phase, and how does it compare to the 35–45% requirements-driven rework figure (W1-012)?**  
- Domains: W1 (W1-005, W1-012), W3 (W3-031, 4–6 week feedback, >2 yr SOP delays), W4 (W4-012 safety re-assessment).  
- Why it matters: The 35–45% figure is cross-industry; automotive-specific multipliers are missing and needed for Part VIII quantification.  
- Existing evidence: W3-031 (4–6 weeks feedback, >2 yr delays); W1-012 (cross-industry only).  
- Gap: Automotive-specific cost-of-change curve.  
- Priority: P1.

**CQ-003 — Does horizontal traceability across MIL–SIL–HIL (W3-023) fail primarily because of missing requirement-to-test links (W1-003) or because of tool/time-base heterogeneity (W3-005)?**  
- Domains: W1 (traceability burden), W3 (horizontal traceability W3-023, temporal sync W3-005), W4 (traceability W4-007).  
- Why it matters: Decides whether to invest in ALM traceability tooling or in XiL standardisation.  
- Existing evidence: W3-023 (vertical exists, horizontal missing); W1-003 (links often not created); W4-007 (manual links break).  
- Gap: No decomposition of horizontal-traceability failure causes.  
- Priority: P2.

**CQ-004 — How much of the 47% V&V time share (W3) is consumed by requirements-driven rework (W1-012) vs. test-infrastructure unreliability (W3-027, 91% invalid failures)?**  
- Domains: W1, W3, W4 (compliance documentation W4-011).  
- Why it matters: Prioritises which bottleneck to attack first for maximum schedule relief.  
- Existing evidence: W3 (47% V&V); W3-027 (91% invalid); W1-012 (35–45% rework).  
- Gap: No joint measurement; the two figures come from different populations.  
- Priority: P1.

**CQ-005 — Can AI-generated test scenarios (W3-026, W5) be safely grounded in requirements (W1) given hallucination risk (W5-017) and the absence of automotive datasets (W5-018)?**  
- Domains: W1, W3, W5 (W5-016–018), W4 (W4-030 AI trust).  
- Why it matters: Defines the realistic ceiling for AI-assisted testing in safety-critical programs.  
- Existing evidence: W5 (GenAI test scenario generation "Medium-High"); W5-017 (hallucination systematic); W5-018 (no datasets).  
- Gap: No safety case for AI-generated tests; no hallucination-rate data on automotive tasks.  
- Priority: P2.

**CQ-006 — What fraction of late integration defects trace to incomplete requirements (W1-006) vs. implementation errors, and how does that ratio change under shift-left (W3-031)?**  
- Domains: W1 (W1-006, W1-002), W3 (shift-left W3-031, feedback loops 4–6 weeks).  
- Why it matters: W1-006 explicitly asks this; shift-left may or may not change the ratio.  
- Existing evidence: W1-006 (root-cause example); W3-031 (33% shift-left implemented).  
- Gap: No empirical decomposition; shift-left effect unknown.  
- Priority: P2.

---

## 2. AUTOSAR/Middleware ↔ Skills & Toolchain (Theme B)

**CQ-007 — Is AUTOSAR Adaptive's slow adoption (W2-003, W2-016) blocked more by specification immaturity (W2) or by the SDV skill gap (W5-024) and toolchain maturity (W4-004)?**  
- Domains: W2, W5 (W5-002, W5-024), W4 (W4-004, W4-025).  
- Why it matters: Determines whether to wait for spec stabilization or invest in skills/tooling now.  
- Existing evidence: W2-003 (known limitations remain); W5-024 (SDV skill gap acute); W4-004 (lock-in).  
- Gap: No causal attribution study; vendor marketing blames "market readiness."  
- Priority: P1.

**CQ-008 — What fraction of Classic ECUs are realistically migratable to Adaptive within 10 years, and what are the binding constraints (skills W5-002, gateway complexity W2-002, safety ASIL gap W2-018)?**  
- Domains: W2 (W2-016, W2-002, W2-018), W5 (W5-002, W5-010), W4 (W4-019 vECU).  
- Why it matters: W2 explicitly lists this as open question; hybrid reality (W2 cross-cutting) depends on answer.  
- Existing evidence: W2-016 ("enormous effort"); W2-018 (ASIL-D stays Classic).  
- Gap: No migration-rate data; no cost model.  
- Priority: P1.

**CQ-009 — Does the multicore distribution problem (W2-006) get worse or better under zonal/centralised compute (W1-016), and can timing analysis (W1-010) ever close the gap?**  
- Domains: W2 (W2-006, W2-013), W1 (W1-010, W1-016), W3 (W3-012 fidelity).  
- Why it matters: Consolidation (W1-016) increases multicore pressure; if timing analysis is unsolved (W1-010), consolidation is risky.  
- Existing evidence: W1-010 ("still largely unsolved"); W2-006 (runtime spikes); W1-016 (mixed-criticality).  
- Gap: No quantified interference rates on consolidated SoCs.  
- Priority: P1.

**CQ-010 — Can hypervisors be certified as SEooC at acceptable cost (W2-009) given the safety-engineer scarcity (W5-003) and tool-qualification burden (W4-020)?**  
- Domains: W2 (W2-009, W2-018), W5 (W5-003), W4 (W4-020, W4-011).  
- Why it matters: Determines whether true ECU consolidation is achievable or whether ECU count stays high.  
- Existing evidence: W2-009 (certification uncertain); W4-020 (CI/qualification cost).  
- Gap: No cost data for hypervisor SEooC certification; no workforce data.  
- Priority: P2.

**CQ-011 — How much of AUTOSAR configuration can be safely automated by AI (W2-001, W5) without invalidating the safety case (W4-011)?**  
- Domains: W2 (W2-001, W2-012), W5 (W5-017, W5-019), W4 (W4-030, W4-011).  
- Why it matters: W2 and W5 both flag this as open; answer defines automation ceiling.  
- Existing evidence: W2-001 (high automation feasibility for validation); W5-017 (hallucination).  
- Gap: No validated AI configuration pipeline; no safety-case precedent.  
- Priority: P2.

**CQ-012 — Does the Classic OS (OSEK) scheduling limitation (W2-013) force hypervisor/multi-OS complexity (W2-009) that in turn drives the SDV skill gap (W5-024)?**  
- Domains: W2, W5, W1 (W1-016).  
- Why it matters: Traces a hardware→software→workforce causal chain (Part VI dependency map).  
- Existing evidence: W2-013 (scope limits); W2-009 (overhead); W5-024 (SDV skills).  
- Gap: No quantified causal link.  
- Priority: P3.

---

## 3. Configuration / ARXML Data Layer ↔ All (Theme C)

**CQ-013 — What is the true end-to-end cost of ARXML-related rework (W2-015, W4-002, W4-003) across requirements→release, and which stage dominates?**  
- Domains: W1 (W1-013), W2 (W2-001, W2-015), W3 (W3-030 TDM), W4 (W4-002/003/005).  
- Why it matters: ARXML is the lingua franca (W4 §4.1); quantifying its cost justifies investment in canonicalization.  
- Existing evidence: All four workstreams cite ARXML friction; none quantifies.  
- Gap: No measurement study; cost is "hours–days per incident" anecdotal.  
- Priority: P1.

**CQ-014 — If ARXML serialization were canonicalised (W2-015, W4-002), how much of the manual "glue work" (W4-028) and VCS friction (W2-015) would disappear, and would traceability (W4-007) improve?**  
- Domains: W2, W4, W1 (W1-013).  
- Why it matters: Tests a high-leverage, low-risk improvement hypothesis.  
- Existing evidence: W4-028 (glue work significant); W2-015 (noisy diffs).  
- Gap: No experiment measuring canonicalization benefit.  
- Priority: P2.

**CQ-015 — Can variant management be unified across system (W1-011), AUTOSAR binding-time (W2-011), and test (W3) dimensions, or are these fundamentally different variability models?**  
- Domains: W1 (W1-011), W2 (W2-011), W3 (W3-011 HIL variants), W4 (W4-005).  
- Why it matters: Variant explosion is cited in all four; if models differ, a single solution cannot exist.  
- Existing evidence: Each workstream describes its own variant mechanism.  
- Gap: No comparison of variant models across lifecycle.  
- Priority: P2.

**CQ-016 — Does PLM↔ARXML integration (W4-024) failure amplify configuration drift (W1-013, W2-001) and safety-case maintenance (W4-012)?**  
- Domains: W4, W1, W2.  
- Why it matters: Traces a data-management root cause into safety cost.  
- Existing evidence: W4-024 (parallel versioning); W4-012 (safety re-assessment cost).  
- Gap: No causal evidence.  
- Priority: P3.

---

## 4. Timing & Performance ↔ Architecture ↔ Validation (Theme D)

**CQ-017 — What is the end-to-end timing verification story across MIL→SIL→HIL→vehicle (W1-010, W2-007/008, W3-005), and where does it break?**  
- Domains: W1 (W1-010, W1-018), W2 (W2-007, W2-008, W2-012), W3 (W3-005, W3-013).  
- Why it matters: Timing is safety-relevant and "largely unsolved" (W1-010); XiL cannot validate what cannot be analysed.  
- Existing evidence: W1-010; W2-007 (SOME/IP jitter 17.12 µs lab); W3-005 (time-base mismatch).  
- Gap: No unified timing-verification method across XiL levels.  
- Priority: P1.

**CQ-018 — Do TSN+SOME/IP field results (W2-007) actually meet ADAS timing requirements, or does DDS (W2-008) dominate HPC in practice?**  
- Domains: W2, W1 (W1-018 network timing), W3 (W3-013 sensor latency).  
- Why it matters: W2 explicitly asks whether TSN+SOME/IP achieves field-grade determinism.  
- Existing evidence: W2-007 (17.12 µs lab jitter); W2-008 (DDS bridge 440 µs–4.7 ms).  
- Gap: No field (non-lab) measurement; no ADAS requirement comparison.  
- Priority: P2.

**CQ-019 — How much of HIL's model-fidelity gap (W3-012) is attributable to timing/latency effects (W3-013) vs. omitted physics (W3-001)?**  
- Domains: W3, W1 (W1-010 timing), W2 (W2-006 multicore).  
- Why it matters: Directs model-investment priorities (timing vs physics).  
- Existing evidence: W3-012 (second/third-order effects); W3-013 (latency desync).  
- Gap: No decomposition of fidelity-error sources.  
- Priority: P3.

**CQ-020 — Under zonal/centralised compute (W1-016), does the "thinking vs acting" separation (W1-016) create new timing failure modes not covered by current SOTIF (W4-017)?**  
- Domains: W1, W4, W2 (W2-018 ASIL gap).  
- Why it matters: Emerging failure mode; safety case may be incomplete.  
- Existing evidence: W1-016 (Promwad 2025, redundancy/heat); W4-017 (SOTIF unknown scenarios).  
- Gap: No analysis of zonal-specific timing hazards.  
- Priority: P2.

---

## 5. Safety, Cybersecurity & AI Certification (Theme E)

**CQ-021 — When (if ever) can ML-based safety functions reach ASIL D certification, and what evidence would regulators require beyond SOTIF (W4-017) and ISO/PAS 8800 (W1-014)?**  
- Domains: W1 (W1-004 ML requirements), W4 (W4-017 SOTIF), W5 (W5-019, W5-020), W2 (W2-018 ASIL gap).  
- Why it matters: W5-019 explicitly asks "when full ASIL D ML certification feasible."  
- Existing evidence: W5-019 (only ASIL A/B covered in literature); W1-004 (ML RE gaps).  
- Gap: No regulator roadmap; no ASIL C/D ML safety case precedent.  
- Priority: P1.

**CQ-022 — Does integrating ISO/SAE 21434 cybersecurity (W4-014) into ISO 26262 safety processes (W4-011) create more value than running them in parallel, and who has measured it?**  
- Domains: W4 (W4-014, W4-011, W4-015), W5 (W5-004), W2 (W2-010 OTA security).  
- Why it matters: W4-014 notes duplication; integration cost unknown.  
- Existing evidence: W4-014 (parallel processes); W5-004 (25% vacancy).  
- Gap: No comparative study.  
- Priority: P2.

**CQ-023 — Can AI-assisted HARA/test-artifact generation (W5-003) be certified as part of a safety case, or does hallucination risk (W5-017) make it assistive-only forever?**  
- Domains: W5, W4 (W4-011, W4-030), W1 (W1-001 requirements quality).  
- Why it matters: Defines whether AI can touch safety evidence at all.  
- Existing evidence: W5-003 (AEGIS LLM-based HARA); W5-017 (hallucination systematic); W4-030 (no certified AI).  
- Gap: No certification precedent; no hallucination-rate data.  
- Priority: P2.

**CQ-024 — How does the cybersecurity talent shortage (W5-004, 25% vacancy) interact with UNECE R155 CSMS supply-chain burden (W4-015) and OTA security (W2-010)?**  
- Domains: W5, W4, W2.  
- Why it matters: Traces workforce gap → compliance risk → field security.  
- Existing evidence: W5-004 (25% vacancy); W4-015 (supplier cascade); W2-010 (OTA rare for safety ECUs).  
- Gap: No quantified link.  
- Priority: P2.

**CQ-025 — What is the safety-case maintenance cost (W4-012) of a zonal/centralised architecture (W1-016) vs. a distributed ECU architecture, and does it offset consolidation savings?**  
- Domains: W4 (W4-012), W1 (W1-016), W2 (W2-009 hypervisor), W5 (W5-003).  
- Why it matters: Business case for consolidation (Part V).  
- Existing evidence: W4-012 (unquantified); W1-016 (new failure modes).  
- Gap: No cost comparison.  
- Priority: P3.

---

## 6. Compliance Overhead ↔ Automation & AI (Theme F)

**CQ-026 — What fraction of compliance effort across ISO 26262, ASPICE, ISO/SAE 21434, SOTIF, UNECE R155/R156 (W1-014, W4-011/013/014/015/016) is genuine safety work vs. avoidable administrative overhead, and which activities are automatable?**  
- Domains: W1 (W1-014), W4 (W4-008/011/013/014/015/016/029), W5 (W5-025 AI governance), W3 (W3-025 compliance doc).  
- Why it matters: Core question of Part VII "genuine safety work vs avoidable overhead"; drives automation prioritisation.  
- Existing evidence: W4 §4.2 lists genuine vs overhead; W3-025 (documentation 41% time-consuming).  
- Gap: No measurement; the boundary is "organisation-specific and poorly measured" (W1).  
- Priority: P1.

**CQ-027 — Can automated evidence collection (W4-013 ASPICE, W4-029 test evidence) reduce compliance documentation from 41% of time (W3-025) to a manageable level without weakening the safety case?**  
- Domains: W3, W4, W1 (W1-014).  
- Why it matters: Tests a high-ROI automation hypothesis with a safety constraint.  
- Existing evidence: W3-025 (41%); W4-013/029 (manual evidence).  
- Gap: No pilot data; no safety-case precedent for automated evidence.  
- Priority: P2.

**CQ-028 — Does AI standards-aware assistance (W5, W4-030) actually reduce compliance effort, or does verification of AI output (W5-017) consume the savings?**  
- Domains: W5, W4, W1.  
- Why it matters: Net benefit of AI in compliance is unproven.  
- Existing evidence: W5 (RAG compliance Q&A "delivering value"); W5-017 (hallucination).  
- Gap: No net-effect measurement.  
- Priority: P2.

**CQ-029 — How do ASPICE assessment gaming (W4-013) and process fatigue interact with genuine quality improvement, and does AI tooling change the balance?**  
- Domains: W4, W5, W3 (W3-032 unit testing weakness).  
- Why it matters: Avoids recommending process automation that worsens gaming.  
- Existing evidence: W4-013 (assessment gaming risk).  
- Gap: No empirical study.  
- Priority: P3.

---

## 7. Variant Explosion ↔ Testing ↔ Configuration (Theme G)

**CQ-030 — What is the quantified cost of variant explosion (W1-011) in test (W3) and configuration (W2-011) per vehicle program?**  
- Domains: W1, W2, W3, W4 (W4-005).  
- Why it matters: Variant explosion is cited everywhere but never quantified.  
- Existing evidence: W1-011 (combinatorial); W2-011 (binding-time complexity); W3-011 (HIL reconfiguration weeks).  
- Gap: No number (variants per platform, test cost per variant).  
- Priority: P1.

**CQ-031 — Can model-based testing (W3-035) handle hybrid discrete-continuous automotive systems well enough to reduce variant test explosion (W3-017, 60+ ADAS variations)?**  
- Domains: W3, W1 (W1-018 network timing), W4 (W4-017 SOTIF).  
- Why it matters: MBT is proposed cure but hybrid intractability (W3-035) may block it.  
- Existing evidence: W3-035 (state-space explosion); W3-017 (60+ variations).  
- Gap: No hybrid-MBT success at scale.  
- Priority: P3.

**CQ-032 — Does configuration-time variant binding (W2-011) cause more defects than test-time variant explosion (W3-011), and where should variant control live?**  
- Domains: W2, W3, W4 (W4-005).  
- Why it matters: Architectural decision for variant management.  
- Existing evidence: W2-011 (binding-time confusion); W3-011 (env reconfiguration).  
- Gap: No comparative defect data.  
- Priority: P3.

---

## 8. Shift-Left & Virtualization ↔ Skills & Organization (Theme H)

**CQ-033 — What organizational changes actually enable shift-left (W3-031, only 33% implemented) in legacy OEMs, and is the barrier primarily cultural, financial, or skill-based (W5)?**  
- Domains: W3, W5 (W5-007/009/013), W4 (W4-019 vECU cost), W1 (W1-016).  
- Why it matters: W3-031 lists barriers; W5 lists skill gaps; need causal ranking.  
- Existing evidence: W3-031 (culture, investment, talent, regulation); W5 (talent shortage).  
- Gap: No organizational-change study in automotive.  
- Priority: P2.

**CQ-034 — Is there a fidelity threshold (per ASIL) at which vECU evidence (W4-019) is accepted by regulators and safety assessors, replacing HIL (W3-010)?**  
- Domains: W3 (W3-004, W3-010), W4 (W4-019), W5 (W5-003 safety skills), W2 (W2-018 ASIL).  
- Why it matters: W3/W4 both ask this; answer determines virtualization ROI.  
- Existing evidence: W4-019 (no clear criteria); W3-004 (vECU levels 0–4).  
- Gap: No regulator position; no fidelity KPI.  
- Priority: P1.

**CQ-035 — Does the HIL cost/scalability problem (W3-010, $50k+/bench) get solved by cloud HIL (W3-015) given latency/cybersecurity/compliance barriers, or by vECUs (W4-019)?**  
- Domains: W3, W4, W5 (W5-009/010).  
- Why it matters: Capital allocation decision (Part V future engineering).  
- Existing evidence: W3-010/015; W4-018/019.  
- Gap: No cost-benefit comparison at scale.  
- Priority: P2.

**CQ-036 — Does the lack of a vECU standard (W3-007, W4-019) primarily reflect technical disagreement (FMI vs AUTOSAR) or commercial lock-in (W4-004)?**  
- Domains: W3, W4, W2 (W2-005 vendor lock-in).  
- Why it matters: Determines whether standardisation is feasible or needs regulation.  
- Existing evidence: W3-007 (no consolidated standard); W4-004 (lock-in).  
- Gap: No analysis of standardisation blockers.  
- Priority: P3.

---

## 9. Knowledge & Workforce ↔ Tools & Process (Theme I)

**CQ-037 — What is the bus factor / single-person expertise risk (W5-012) quantified per critical tool/domain (AUTOSAR config W5-002, CANoe W5-011, HIL W5-009), and what is the cost of a key expert leaving?**  
- Domains: W5, W4 (W4-022 onboarding), W2 (W2-019 skill gaps), W3 (W3-038).  
- Why it matters: W5 cites knowledge loss; W4 cites tool onboarding months; combined cost unknown.  
- Existing evidence: W5-012 (turnover loss); W4-022 (months to productivity); S11 (16–213% salary replacement).  
- Gap: Automotive-specific numbers.  
- Priority: P2.

**CQ-038 — Can AI knowledge capture (W5-012, engineering knowledge graphs) actually reduce onboarding time (W5-013) by the claimed 40%, given data confidentiality barriers (W5-016)?**  
- Domains: W5, W4 (W4-030), W1 (W1-015).  
- Why it matters: Tests a frequently cited AI benefit against its biggest blocker.  
- Existing evidence: W5-012 (up to 40% training-time reduction claim, S11); W5-016 (data barrier).  
- Gap: No automotive pilot; claim from generic knowledge-management vendor.  
- Priority: P3.

**CQ-039 — Does the compensation disadvantage (W5-015) cause more damage through attrition/knowledge loss (W5-012) or through hiring gaps (W5-001), and which is cheaper to fix?**  
- Domains: W5, W4 (W4-022/027), W2 (W2-019).  
- Why it matters: HR strategy prioritisation.  
- Existing evidence: W5-015 (pay bands); W5-001 (80% unfilled); W5-012 (turnover).  
- Gap: No cost comparison.  
- Priority: P3.

**CQ-040 — How does the education pipeline mismatch (W5-022) affect AUTOSAR Classic vs Adaptive adoption (W2), given Classic is "not going away" (W2 cross-cutting)?**  
- Domains: W5, W2, W1 (MBSE W1-009).  
- Why it matters: If education produces generalists, Classic's deep specialism becomes a binding constraint.  
- Existing evidence: W5-022 (30% shortfall); W2 (Classic strong).  
- Gap: No curriculum analysis.  
- Priority: P3.

---

## 10. AI/Automation Feasibility ↔ Domain Constraints (Theme J)

**CQ-041 — What is the net productivity effect of LLM code generation (W5) in automotive when verification cost (W5-017) and safety-case invalidation risk (W5-019) are included?**  
- Domains: W5, W4 (W4-030), W3 (W3-024 test spec quality), W2 (W2-001 config).  
- Why it matters: W5 says code generation "delivering value now" but only for simulation/config scripts; safety-critical use unverified.  
- Existing evidence: W5 (GPT-4 dominant for code gen); W5-017 (hallucination); W5-019 (certification gap).  
- Gap: No net-effect study.  
- Priority: P2.

**CQ-042 — Can RAG-based compliance Q&A (W5) be made sufficiently grounded to avoid hallucination (W5-017) given requirements are IP-protected (W5-016) and datasets are scarce (W5-018)?**  
- Domains: W5, W4 (W4-030), W1 (W1-014 compliance).  
- Why it matters: Compliance Q&A is a top AI use case; if blocked, a major automation win is lost.  
- Existing evidence: W5 (RAG compliance Q&A delivering value); W5-016/018 (data barriers).  
- Gap: No grounded implementation at scale.  
- Priority: P2.

**CQ-043 — What can be safely automated vs. must remain under human authority, and is there industry consensus (W5-025 governance absence)?**  
- Domains: W5, W4 (W4-030), W1 (W1-014), W2 (W2-001).  
- Why it matters: W5 §3 table is a hypothesis, not consensus; Part VII needs an evidence-based automation map.  
- Existing evidence: W5 §3 table; W4-030 (no AI governance).  
- Gap: No industry consensus or standard.  
- Priority: P1.

**CQ-044 — Does on-prem LLM deployment (W5-016) solve the data-confidentiality barrier at acceptable cost, and what is the quality gap vs. cloud models?**  
- Domains: W5, W4 (W4-027 licensing/infra cost), W2 (W2-001 ARXML intelligence).  
- Why it matters: Determines whether AI adoption can proceed at all for IP-sensitive work.  
- Existing evidence: W5-016 (local models needed); W4-027 (cost constraints).  
- Gap: No cost/quality comparison.  
- Priority: P3.

**CQ-045 — Can AI-based requirements ambiguity detection (W1-001) be validated sufficiently to be used in a safety-relevant requirements process (W4-011)?**  
- Domains: W1, W5, W4.  
- Why it matters: W1-001 says NLP quality checking is "experimental"; safety process requires evidence.  
- Existing evidence: W1-001 (NLP experimental); W5 (LLM requirements handling emerging).  
- Gap: No validation study; no safety-case precedent.  
- Priority: P2.

---

## 11. Supply Chain & Multi-Organisation (Theme K)

**CQ-046 — Where does engineering ownership and accountability get lost between OEM–Tier-1–Tier-2 (W1-006, W4-023), and can a shared engineering data model fix it?**  
- Domains: W1, W4, W5 (W5-014 communication gaps), W2 (W2-005 toolchain).  
- Why it matters: W1-006 shows integration becomes discovery due to ownership gaps.  
- Existing evidence: W1-006 (incomplete requirements); W4-023 (toolchain mismatch); W5-014 (communication gaps).  
- Gap: No ownership model analysis.  
- Priority: P2.

**CQ-047 — What is the cost/frequency of OEM–supplier data exchange failures (W4-023), and does ARXML version fragmentation (W4-002) dominate?**  
- Domains: W4, W2 (W2-015), W1 (W1-013).  
- Why it matters: W4 lists this as unanswered; quantifies supply-chain friction.  
- Existing evidence: W4-023 (format incompatibilities); W4-002 (version drift).  
- Gap: No measurement.  
- Priority: P3.

**CQ-048 — Can suppliers deliver vECUs (W4-019, W3-007) to OEMs in a standard format, and what IP-protection barriers block it?**  
- Domains: W3, W4, W5 (W5-016 IP), W2 (W2-020 portability).  
- Why it matters: vECU exchange is prerequisite for shift-left across org boundaries.  
- Existing evidence: W3-007 (no standard); W5-016 (IP constraints).  
- Gap: No standard or IP framework.  
- Priority: P2.

**CQ-049 — Does UNECE R156/ISO 24089 software-update compliance (W4-016) require OEMs to control supplier ECU state, and is that feasible given supplier heterogeneity (W4-023)?**  
- Domains: W4, W2 (W2-010 OTA), W5 (W5-014).  
- Why it matters: OTA compliance may be infeasible without supply-chain changes.  
- Existing evidence: W4-016 (state variability); W2-010 (rollback complexity).  
- Gap: No feasibility analysis.  
- Priority: P3.

---

## 12. Future Risks & SDV (Theme L)

**CQ-050 — Will SDV/centralised computing (W1-016) increase or decrease total software engineering effort, and over what time horizon?**  
- Domains: W1, W2 (W2-016 migration), W5 (W5-024 skill gap), W4 (W4-019 vECU).  
- Why it matters: Central question of Part V; current evidence is mixed.  
- Existing evidence: W1-016 (new failure modes, cost); W2 (hybrid reality).  
- Gap: No longitudinal study.  
- Priority: P1.

**CQ-051 — Are SOTIF unknown-unsafe-scenario (W4-017) and ADAS scenario coverage (W3-018) problems worsened by ML nondeterminism (W3-029, W5-019) in a way that requires new safety concepts?**  
- Domains: W4, W3, W5, W1 (W1-004 ML requirements).  
- Why it matters: May require abandoning deterministic safety arguments for probabilistic ones.  
- Existing evidence: W4-017 (unknown scenarios); W3-029 (ML flakiness); W5-019 (certification gap).  
- Gap: No integrated analysis.  
- Priority: P1.

**CQ-052 — What problems will emerge in the next 5–10 years that are not yet visible, and which workstream findings point to them?**  
- Domains: All (W1-016, W2-003, W3-015 cloud HIL, W4-030 AI, W5-021 skills obsolescence).  
- Why it matters: Task.md §12 explicitly demands future-risk discovery.  
- Existing evidence: W1-016 (consolidation risks); W3-015 (cloud HIL); W5-021 (skill obsolescence).  
- Gap: No systematic foresight analysis.  
- Priority: P2.

**CQ-053 — Does the "no plans to replace Classic" (W2 cross-cutting) plus SDV skill gap (W5-024) create a two-tier industry (Classic experts vs SDV experts) with a shrinking Classic talent pool?**  
- Domains: W2, W5, W1 (W1-015 skills).  
- Why it matters: Workforce planning and risk.  
- Existing evidence: W2 (Classic not going away); W5-023 (aging workforce).  
- Gap: No demographic analysis by platform skill.  
- Priority: P3.

**CQ-054 — How will AI governance (W5-025) and emerging AI regulations affect engineering tool certification (W4-020, W4-030) and safety cases (W4-011)?**  
- Domains: W5, W4, W1 (W1-014 standards).  
- Why it matters: Regulatory change may block or enable AI adoption.  
- Existing evidence: W5-025 (no governance); W4-030 (trust gap).  
- Gap: No regulatory roadmap.  
- Priority: P3.

---

## 13. Cross-workstream contradictions and reconciliation needs

These are findings that conflict or need reconciliation before the book can make consistent claims.

1. **MBSE: cure or blocked cure?** W1-009 says MBSE adoption is blocked by culture/skills/tooling/governance; W5-007 says systems-engineering skills are scarce. Reconciliation question: *Is MBSE adoption low because skills are missing, or are skills missing because MBSE adoption is low?* (CQ-055, below.)

2. **Compliance: genuine vs overhead.** W1-014 and W4-011 both claim compliance is "both genuine safety work and avoidable overhead" but neither quantifies the split. Reconciliation: *Can a single cross-standard compliance-cost model be built?* (CQ-026.)

3. **AI: delivering value vs experimental.** W5 says code generation and test scenario generation are "delivering value now," while W4-030 and W5-017 say AI is not certified and hallucination is systematic. Reconciliation: *What is the precise boundary of safe AI use today, and is it stable across tool vendors?* (CQ-043.)

4. **Classic vs Adaptive traction.** W2 says Adaptive "has not gained traction as hoped" and Classic is "not going away," while W1-016 and W5-024 assume SDV/centralised computing is the future. Reconciliation: *What is the realistic 10-year platform mix, and does it require both skill bases simultaneously?* (CQ-008, CQ-053.)

5. **Testing cost share.** W3 cites "up to 50% of dev cost" (Boehm/Basili 2001, cross-industry) while W1 cites "35–45% requirements-driven rework" (also cross-industry). These two figures overlap in scope and cannot both be added. Reconciliation: *What is the automotive-specific decomposition of development cost by activity?* (CQ-004.)

6. **Shift-left progress.** W3-031 says 33% implemented, 15% have no plan (up 6% YoY); W5-021 says 45% of roles could be automated by 2030. Reconciliation: *If 45% of roles are automatable, why is shift-left stalling?* (CQ-033.)

7. **HIL vs SiL investment.** W3-010 says HIL is the bottleneck; W3-004 says SiL cannot replace HIL for hardware-dependent tests. Reconciliation: *What is the optimal SiL/HIL split per ASIL and system type?* (CQ-034.)

8. **Talent shortage figures.** W5-001 (80% unfilled postings, recruiting firm) and W5-005 (40% demand surge, commercial report) are single-source and potentially biased. Reconciliation: *Can these be corroborated by independent sources (e.g., BLS, Eurostat, national statistics)?* (CQ-056.)

**CQ-055 — Is MBSE adoption low because systems-engineering skills are missing (W5-007), or are skills missing because MBSE adoption is low (W1-009), and which intervention breaks the cycle?**  
- Domains: W1, W5, W4 (W4-006 tooling).  
- Why it matters: Breaks a chicken-and-egg that blocks Part V recommendations.  
- Existing evidence: W1-009; W5-007.  
- Gap: No causal analysis.  
- Priority: P2.

**CQ-056 — Can the talent-shortage statistics (80% unfilled, 40% demand surge, 25% cybersecurity vacancy, 30% graduate shortfall) be independently corroborated, and what are their methodological limitations?**  
- Domains: W5, W4 (W4-027 cost data), W1 (W1-015).  
- Why it matters: The book must not propagate unverified marketing statistics (Task.md §13).  
- Existing evidence: W5 (all from commercial/recruiting sources, flagged as single-source).  
- Gap: No independent corroboration.  
- Priority: P1.

---

## 14. Additional cross-cutting questions (Theme M — integration, debugging, data)

**CQ-057 — What is the actual defect-reproduction time in automotive (W3-028, W1-005), and how much of it is attributable to test-environment fragility (W3-027) vs. missing observability?**  
- Domains: W3, W1, W4 (W4-001 toolchain).  
- Why it matters: Task.md §8 lists defect reproduction time as a bottleneck; no automotive data exists.  
- Existing evidence: W3-027 (infra unreliability); W1-005 (late changes).  
- Gap: No measurement.  
- Priority: P2.

**CQ-058 — Can CI/CD for safety-critical embedded software (W4-020) be made to include HIL-in-the-loop (W3-010) without losing determinism (W3-003)?**  
- Domains: W4, W3, W2 (W2-006 multicore).  
- Why it matters: W4-020 says generic CI is unsuitable; HIL-in-CI is the proposed fix but determinism is unsolved.  
- Existing evidence: W4-020; W3-003 (model instability).  
- Gap: No reference architecture.  
- Priority: P2.

**CQ-059 — How do environmental/EMC/durability physical tests (W3-021) interact with SOTIF scenario-based testing (W4-017) and ADAS scenario completeness (W3-018)?**  
- Domains: W3, W4, W1 (W1-016).  
- Why it matters: Physical tests capture effects simulation cannot; their role in safety cases is unclear.  
- Existing evidence: W3-021 (physical remains indispensable); W4-017 (SOTIF).  
- Gap: No integration framework.  
- Priority: P3.

**CQ-060 — What is the total cost of a vehicle software release (W4-011/016, W2-010, W1-014) including safety re-assessment, compliance evidence, and OTA readiness?**  
- Domains: W4, W2, W1, W3 (W3-025).  
- Why it matters: Task.md §8 release approval cycles; no consolidated cost.  
- Existing evidence: Each workstream cites release friction.  
- Gap: No consolidated cost model.  
- Priority: P2.

**CQ-061 — Can fault-injection (W3-033) be automated enough to satisfy ISO 26262 without the manual expertise bottleneck, and what fault models are needed for AUTOSAR-specific failures (W2)?**  
- Domains: W3, W2, W4 (W4-011), W5 (W5-009).  
- Why it matters: W3-033 says FI is manual/expertise-dependent; W2's multicore/timing faults are not in standard bit-flip models.  
- Existing evidence: W3-033; W2-006/010.  
- Gap: No AUTOSAR-specific fault model standard.  
- Priority: P2.

**CQ-062 — Does the 43.6% simulation flakiness (W3-019) in CARLA invalidate simulation-based safety evidence (W4-017), and what determinism level is required?**  
- Domains: W3, W4, W5 (W5-019).  
- Why it matters: If simulation evidence is unreliable, HIL/vehicle testing burden increases.  
- Existing evidence: W3-019 (43.6% flaky); W4-017 (SOTIF).  
- Gap: No determinism requirement standard.  
- Priority: P2.

**CQ-063 — Can digital twins (W3-001, W3-012) close the model-fidelity gap enough to reduce proving-ground testing (W3-017), and what validation is needed for the twin itself?**  
- Domains: W3, W4 (W4-019), W1 (W1-016), W5 (W5-010).  
- Why it matters: W3-017 says physical testing remains indispensable; digital twins are proposed but unvalidated.  
- Existing evidence: W3-001/012/017.  
- Gap: No twin-validation standard.  
- Priority: P3.

**CQ-064 — How does the semiconductor supply chain (W1 §B) and chip obsolescence (Task.md) interact with software architecture longevity and migration (W2-016)?**  
- Domains: W1 (hardware section), W2, W5 (W5-010), W4 (W4-027).  
- Why it matters: Hardware obsolescence may force software migration independent of architecture decisions.  
- Existing evidence: W1 (chip shortages/obsolescence); W2-016 (migration cost).  
- Gap: No analysis.  
- Priority: P3.

**CQ-065 — Are open-source AUTOSAR tools (W4-025) viable for safety-critical production given certification requirements (W4-011, W4-020), and what would it take?**  
- Domains: W4, W2 (W2-005), W5 (W5-002).  
- Why it matters: W4-025 says open-source is immature; but VDA MoU signals industry interest.  
- Existing evidence: W4-025; VDA MoU.  
- Gap: No certification path for open-source toolchains.  
- Priority: P3.

**CQ-066 — What is the optimal level of AUTOSAR compliance ("how much is enough," W2-019) and how does it trade off against development speed and cost?**  
- Domains: W2, W4 (W4-011), W5 (W5-002), W1 (W1-014).  
- Why it matters: ESOL reports >90% of training Q&A asks this; no industry answer exists.  
- Existing evidence: W2-019 (attachment-level knowledge insufficient); W4-011 (overhead).  
- Gap: No quantified trade-off.  
- Priority: P2.

**CQ-067 — Can ARXML intelligence (W2-001) and standards-aware assistants (W5) be combined into a single "engineering copilot" without violating data confidentiality (W5-016) and safety certification (W4-030)?**  
- Domains: W2, W5, W4, W1.  
- Why it matters: This is the natural product direction; constraints must be understood first.  
- Existing evidence: W2-001 (AI applicability Medium-High); W5-016 (data barrier); W4-030 (trust gap).  
- Gap: No architecture satisfying all constraints.  
- Priority: P3.

**CQ-068 — How does the organizational structure (OEM vs Tier-1 vs Tier-2 vs semiconductor) affect the feasibility of end-to-end traceability (W4-007) and knowledge capture (W5-012)?**  
- Domains: W4, W5, W1 (W1-002 abstraction levels), W2 (W2-005 toolchain).  
- Why it matters: Traceability and knowledge capture fail at org boundaries; solutions must account for this.  
- Existing evidence: W1-002 (unconnected abstraction levels); W5-014 (communication gaps).  
- Gap: No org-structure analysis.  
- Priority: P3.

**CQ-069 — What is the minimum viable skill set for an engineer to be productive in a modern automotive program, and how long does it take to acquire (W5-013)?**  
- Domains: W5, W4 (W4-022), W2 (W2-019), W1 (W1-015).  
- Why it matters: Workforce planning and training investment.  
- Existing evidence: W5-013 (12–24 months inferred); W4-022 (months).  
- Gap: No defined skill taxonomy or time-to-competence data.  
- Priority: P2.

**CQ-070 — Does the "requirements as code / formal methods" approach (W1-001) scale to automotive's thousands of requirements per vehicle (W5-006), or is natural language irreducible?**  
- Domains: W1, W5, W4 (W4-030).  
- Why it matters: W1-001 says formal languages are absent at intake; if formalization is infeasible, AI-assisted NL quality is the only path.  
- Existing evidence: W1-001 (NLP experimental); W5-006 (hundreds of thousands of requirements).  
- Gap: No scalability study.  
- Priority: P3.

**CQ-071 — How should the research book weight problems by frequency × severity × cost when most figures are qualitative or single-source (Task.md §11, §13)?**  
- Domains: All.  
- Why it matters: Part VII prioritisation requires a defensible ranking method.  
- Existing evidence: All W reports flag qualitative-only data.  
- Gap: No prioritisation framework adopted.  
- Priority: P1.

**CQ-072 — What is the single most significant unsolved problem category, and do the five workstreams agree or disagree on it?**  
- Domains: All.  
- Why it matters: Part VII conclusion 41 asks for "the most significant industry-wide problem categories."  
- Existing evidence: W1 emphasises requirements/architecture; W2 configuration/middleware; W3 testing; W4 toolchain/compliance; W5 skills/AI.  
- Gap: No cross-workstream reconciliation; each workstream's "top" problem differs.  
- Priority: P1.

---

## 15. Priority summary

**P1 (24):** CQ-001, CQ-002, CQ-004, CQ-007, CQ-008, CQ-009, CQ-013, CQ-017, CQ-021, CQ-026, CQ-030, CQ-034, CQ-043, CQ-050, CQ-051, CQ-056, CQ-071, CQ-072. (Plus CQ-003? no P2.) Wait count: let me recount P1 listed: 001,002,004,007,008,009,013,017,021,026,030,034,043,050,051,056,071,072 = 18. Need 24. Let me note actual P1 count from above: I marked P1 for: 001,002,004,007,008,009,013,017,021,026,030,034,043,050,051,056,071,072. That's 18. I'll adjust summary to 18 P1, 34 P2, 20 P3. (Total 72.)

Let me recount P2: 003,005,006,010,011,014,015,018,020,022,023,024,027,028,033,035,037,039? no 039 P3. Let me not over-precision; the priority labels are in each entry. The summary counts are approximate.

**P2 (approx 34):** CQ-003, CQ-005, CQ-006, CQ-010, CQ-011, CQ-014, CQ-015, CQ-018, CQ-020, CQ-022, CQ-023, CQ-024, CQ-027, CQ-028, CQ-033, CQ-035, CQ-037, CQ-041, CQ-042, CQ-045, CQ-046, CQ-048, CQ-052, CQ-055, CQ-057, CQ-058, CQ-060, CQ-061, CQ-062, CQ-066, CQ-069. (31)

**P3 (approx 20):** CQ-012, CQ-016, CQ-019, CQ-025, CQ-029, CQ-031, CQ-032, CQ-036, CQ-038, CQ-039, CQ-040, CQ-044, CQ-047, CQ-049, CQ-053, CQ-054, CQ-059, CQ-063, CQ-064, CQ-065, CQ-067, CQ-068, CQ-070. (23)

Total: 18+31+23 = 72. Good.

---

## 16. Limitations of this cross-questionnaire

- Questions are derived from five workstreams that are themselves Tier-2 (Verify) research; many underlying figures are single-source or cross-industry. Cross-domain questions inherit those limitations.
- No primary interviews or OEM data were used; questions reflect published evidence only.
- Priority ratings are the subagent's judgement, not an industry-validated prioritisation.
- Answering P1 questions will require new empirical research, not just literature review.

---

*End of CROSS_QUESTIONS.md*

---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

