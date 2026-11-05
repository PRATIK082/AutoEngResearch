# Cross-Check Report — Workstreams W1–W5

**Scope:** All files `research/W*.md` (W1 requirements/system engineering, W2 AUTOSAR/middleware, W3 testing/validation, W4 toolchain/standards, W5 skills/AI).  
**Date:** 2026-11-05  
**Purpose:** Identify contradictions, overlaps, gaps, and inconsistencies across workstreams, and recommend consolidation actions.

---

## 1. Contradictions

Direct conflicts or mutually incompatible claims between workstreams.

| # | Claim A (source) | Claim B (source) | Nature of conflict |
|---|---|---|---|
| C1 | W1: "MBSE adoption is slow" (W1-009) and "only 22% of systems-engineering participants could cite a clear MBSE definition." | W1: "66% of enterprises rely on MBSE frameworks (market stat; verify)." | Internal tension within W1: 66% reliance vs. 22% definitional clarity are not reconciled; the 66% figure is flagged unverified. If both were true, "reliance" would be mostly nominal. |
| C2 | W2: "Adaptive Platform has not gained traction as hoped; Classic still strong; no plans to replace" (cross-cutting §3). | W1: "Transition to zonal/centralised HPC introduces new engineering challenges" (W1-016); W5: "SDV/centralized computing skill gap is acute" (W5-024). | W2 frames Adaptive as stalled, while W1/W5 treat zonal/centralised compute as an active, skill-constrained transition. Scope distinction (Adaptive standard vs. zonal architecture generally) is not stated; readers cannot tell whether the transition is happening on Adaptive or on non-AUTOSAR stacks. |
| C3 | W5: "45% of current automotive roles could be automated by 2030" (W5-021). | W5: "AI not yet reliable for safety-critical code; certification unresolved" (W5-017, W5-019) and "AI applicability Low for safety case/certification." | If 45% of roles are automatable but safety-critical engineering cannot be automated, the 45% must exclude safety-critical roles — but W5 does not state this scope. The figure reads as over-reaching. |
| C4 | W2: "Only Tesla (as of NHTSA 2020 report) performing OTA of safety-critical ECUs; most OEMs OTA infotainment only." | W4: treats OTA/R156/SUMS as a current, high-priority compliance problem for all connected vehicles (W4-016); W2-010 frames FOTA as a live engineering problem. | The 2020 NHTSA data is presented as a current anchor without staleness caveat, while other workstreams assume broad OTA capability. Either OTA maturity has advanced (making the Tesla claim stale) or it has not (making W4's urgency overstated). |
| C5 | W1: requirements-driven rework is "35–45% of total rework cost" (W1-012, cross-industry Capers Jones). | W3: "testing accounts for up to ~50% of total software development cost" (Boehm/Basili 2001). | Different denominators (rework cost vs. development cost) and different bases (cross-industry vs. ISO 26262 context); not strictly contradictory, but the reports do not reconcile them, inviting conflation into a single "cost of quality" narrative. |
| C6 | W1: MBSE is "the proposed cure but is itself blocked" (cross-cutting §4). | W1-009: "66% of enterprises rely on MBSE frameworks." | Same tension as C1: if two-thirds already rely on MBSE, the "blocked adoption" framing needs qualification (partial/ad-hoc use vs. genuine MBSE). |
| C7 | W3: "80% of integration issues surface ~6 months before SOP" (KPIT 2025). | W1: "Late defect discovery" cited as a primary cost driver; W3-031: "not testing early enough is 2nd top testing concern (24%)." | If 80% of integration issues are found 6 months pre-SOP, discovery is not obviously "late" in calendar terms; the reports do not define what "late" means (relative to fix cost, not schedule). Minor but worth reconciling. |

---

## 2. Overlaps

Substantial duplication of the same root problem across workstreams, recorded under different IDs with different field schemas.

### 2.1 ARXML / configuration (recorded 3+ times)
- W1-013 (ARXML tool interoperability and configuration drift)
- W2-001 (ARXML configuration complexity), W2-011 (variant management), W2-012 (RTE contract), W2-015 (ARXML VCS friction)
- W4-002 (ARXML version/dialect fragmentation), W4-003 (manual ARXML authoring), W4-005 (config diff/merge and variants)

All describe the same underlying problem (verbose, version-fragile, non-canonical XML as the lingua franca). Root-cause attributions differ slightly (W1 emphasises tooling, W2 emphasises "configuration instead of implementation," W4 emphasises schema/version drift), but the evidence bases overlap (autosar.io, serialization rules, Audi BUSNET).

### 2.2 Traceability (recorded 4+ times)
- W1-002 (unconnected abstraction levels), W1-003 (traceability burden and link quality), W1-004 (ML traceability gaps)
- W3-023 (horizontal MIL–SIL–HIL traceability)
- W4-007 (end-to-end traceability gaps), W4-029 (coverage evidence management)

All describe manual, brittle, siloed trace links. W1 focuses on requirements↔design, W3 on test-level horizontal traceability, W4 on compliance evidence. These are facets of one problem, not three independent problems.

### 2.3 Toolchain fragmentation (recorded 5+ times)
- W1 §1 bullet 3 / W1-013
- W2-005 (vendor lock-in and BSW toolchain interoperability)
- W3-036 (test tool interoperability and fragmented toolchain)
- W4-001 (fragmented heterogeneous toolchain), W4-023 (supplier/OEM toolchain mismatch), W4-028 (glue work)
- W5-011 (toolchain knowledge gaps), W5-014 (communication gaps)

### 2.4 Skills / workforce gaps (recorded across all 5 workstreams)
- W1-015 (system engineering skill and workforce gaps)
- W2-019 (skill/knowledge gaps in AUTOSAR methodology)
- W3-038 (knowledge and skill gaps in testing/validation)
- W4-022 (tool onboarding, training time, single-person expertise)
- W5-001 … W5-025 (25 dedicated problems)

W5 is the deep dive; W1/W2/W3/W4 each re-record the same gap at shallower depth with different IDs. No cross-reference is made between, e.g., W1-015 and W5-007.

### 2.5 Compliance overhead (recorded 4+ times)
- W1-014 (compliance overhead and standards fragmentation)
- W3-025 (compliance documentation burden for testing)
- W4-011 (ISO 26262 overhead), W4-013 (ASPICE assessment overhead), W4-014 (ISO/SAE 21434), W4-015 (R155), W4-016 (R156/ISO 24089), W4-026 (standards evolution and regional fragmentation)

W4 splits compliance into per-standard problems; W1 and W3 treat it as cross-cutting. The per-standard split in W4 is not reconciled with W1-014's "genuine safety work vs. avoidable overhead" framing.

### 2.6 HIL cost / scarcity (recorded 2 times)
- W3-010 (HIL cost and scalability), W3-016 (HIL environment management)
- W4-018 (HIL test bench scarcity and scheduling bottlenecks)

### 2.7 vECU fidelity (recorded 2 times)
- W3-004, W3-007
- W4-019

### 2.8 Knowledge silos / turnover (recorded 3+ times)
- W5-012, W5-023
- W4-022
- W1-015, W2-019

### 2.9 Timing analysis (recorded 3 times)
- W1-010 (timing analysis unsolved for AUTOSAR), W1-018 (network architecture and timing)
- W2-006 (multicore distribution), W2-013 (Classic OS scheduling limits)
- W3-005 (temporal synchronization), W3-013 (HIL sensor/actuator latency)

### 2.10 Variant management (recorded 3 times)
- W1-011
- W2-011
- W4-005

### 2.11 ML/AI requirements gaps (recorded 2 times)
- W1-004 (ML/AI-specific requirements and traceability gaps)
- W5-005, W5-006, W5-019 (skills and certification gaps)

### 2.12 Zonal/centralised architecture (recorded 3 times)
- W1-016
- W2-009, W2-016, W2-018
- W5-024

### 2.13 OTA (recorded 3 times)
- W2-010
- W4-016
- W5-024 (partial)

### 2.14 Interface governance (recorded 2 times)
- W1-007 (unstable interfaces)
- W2-002 (Classic/Adaptive interop and gateway translation)

**Impact of overlaps:** The same root cause is counted multiple times across workstreams, inflating the apparent problem count (18 + 20 + 38 + 30 + 25 = 131 records, but likely ~60–70 unique problems). Evidence bases are duplicated rather than cross-referenced. No workstream acknowledges the others' treatment of a shared problem except via generic "cross-workstream dependencies referenced but not deeply analyzed" disclaimers (W2 §6, W3 §10).

---

## 3. Gaps

Topics not covered, or covered only superficially, across all workstreams.

### 3.1 Missing topic areas
- **Hardware/semiconductor lifecycle and supply chain** — only W5-010 touches HW/SW co-design; chip shortage, silicon revision management, and foundry dependencies are not treated as software-engineering problems.
- **Cloud/backend and connected-services software** — telematics, V2X, backend services, fleet learning, and data pipelines are essentially absent (W5-004 mentions connectivity only as a skill gap).
- **Energy, power, and thermal software concerns** — mentioned in W1-016 as a zonal-architecture challenge but never given a dedicated problem record; no thermal-aware scheduling, power management, or energy-budget software problem.
- **Open-source software governance and licensing in automotive** — W4-025 covers open-source *tooling* only; use of OSS components in production ECUs, license compliance, and SBOM management are not covered.
- **Software architecture quality, modularity, and technical-debt metrics** — no workstream records modularity, coupling, or technical-debt measurement as a problem.
- **Multi-language and globalization of software** — absent.
- **Developer experience / ergonomics of engineering tools** — partially in W4-006 (requirements tool UX) but not generalized.
- **Cost/benefit and ROI quantification** — every workstream marks cost impact as "unquantified"; no workstream proposes a measurement framework.

### 3.2 Missing cross-cutting concerns
- **No consolidated problem taxonomy or master index** — IDs are per-workstream (W1-001 … W5-025), with no global ID, no deduplication, and no dependency links between problems.
- **No cross-workstream dependency map** — problems are not linked (e.g., W1-001 ambiguity → W3-024 test spec quality → W4-007 traceability). Each workstream lists "unanswered questions" independently; several are duplicates.
- **No reconciliation of quantitative anchors** — the same population is measured by different surveys (Perforce ~600 professionals vs. electronica 200 companies) with no attempt to triangulate.
- **No treatment of the "Task.md" master prompt scope** — workstreams reference `Task.md` but its full scope (e.g., root-cause analysis template §11) is not verified as covered.

### 3.3 Missing evidence on key claims
- W2's "Tesla only OEM doing safety-critical OTA (NHTSA 2020)" is 6 years stale and presented as current.
- W1's "66% MBSE reliance" and "22% definitional clarity" are not reconciled.
- W3's "15% no plan to implement shift-left (up 6% YoY)" is ambiguous (percentage points vs. percent).
- W5's "80% of embedded job postings unfilled" comes from a recruiting firm with a business interest (acknowledged but not corrected).

---

## 4. Inconsistencies

### 4.1 Schema and format inconsistencies
- **Field-name inconsistency:** W1 uses "Problem"; W2 uses "Problem"; W3 uses "Problem"; W4 uses "Problem description"; W5 uses "Description." W3 adds "Scenario"; W4 adds "Actual engineering scenario"; W5 adds "Actual scenario." W2 adds "Scenario" in some records. This prevents machine comparison.
- **Evidence-tier labeling inconsistency:** W1 and W3 declare "Verify (Tier 2)"; W4 declares "Research report (problem discovery)"; W5 declares no tier. The master prompt's tier system (Scout/Verify/Auditor) is applied inconsistently.
- **Confidence scale inconsistency:** all use High/Medium/Low, but W4 adds "Low-Medium" and "Medium-High" as intermediate values; W1 uses "Medium–High" with an en-dash. Minor but affects filtering.
- **Problem-count imbalance:** W3 records 38 problems, W4 30, W2 20, W5 25, W1 18. Granularity differs: W1 groups at a higher abstraction level; W3 splits by test level. Counts are not comparable as a measure of problem density.

### 4.2 Numerical and sourcing inconsistencies
- **Stale data presented as current:** W2's NHTSA 2020 OTA claim (C4); W1's Capers Jones rework figure is cross-industry and dated.
- **Overlapping cost claims with different bases:** W3 "testing ~50% of dev cost" vs. W1 "requirements-driven rework 35–45% of total rework cost" vs. W4 "10–30% of engineering effort on data transformation (hypothesis)." These could double-count; no reconciliation.
- **Survey population mismatch:** Perforce ~600 professionals (W3) vs. electronica 200 companies (W5-001) vs. S9 small industry survey n=9 (W5). Treated as complementary without triangulation.
- **Market stats flagged but not resolved:** W1 flags "66% MBSE reliance" as "verify"; W5 flags commercial report stats. Neither follow-up is recorded.

### 4.3 Internal inconsistencies within workstreams
- W2-004 and W2-018 phrase Adaptive's ASIL target as "ASIL-B (target D)" — unclear whether "target D" is a typo or a statement of aspiration.
- W3-019 cites 43.6% flaky scenarios in CARLA; W3-003 cites model instability breaking CI. Both relate to nondeterminism but are recorded separately without cross-reference.
- W5-013 estimates "12–24 months to independently own an ASIL B component" as inferred; W5-003 says safety process knowledge is more available than analytical depth. These two claims about onboarding time and skill scarcity are not linked.

### 4.4 Cross-reference gaps
- No workstream cites another workstream's problem IDs. E.g., W3-038 (skill gaps in testing) does not reference W5-009 (testing/validation expertise gaps) despite near-identical content.
- W4-030 (AI-assisted engineering tooling) and W5-016/W5-017/W5-018/W5-025 (AI adoption barriers) cover the same ground with different framing; neither references the other.

---

## 5. Recommendations

### 5.1 Consolidate and deduplicate
1. **Create a master problem index** with global IDs (e.g., `P-001`) mapping to per-workstream IDs, so the 131 records collapse to ~60–70 unique problems. Mark each as "primary owner" workstream.
2. **Merge the overlapping problem families** (ARXML/config, traceability, toolchain fragmentation, skills, compliance, HIL, vECU, knowledge silos, timing, variants, ML-RE, zonal architecture, OTA) into single canonical records with cross-workstream evidence appended.
3. **Harmonize the problem-record schema** — adopt one field set (Domain, Lifecycle stage, Problem, Scenario, Who, Root cause, Contributing factors, Technical limits, Skill limits, Tool limits, Process limits, Frequency/severity, Time/cost impact, Downstream effects, Existing solutions, Why insufficient, Improvement areas, Automation feasibility, AI applicability, Human approval, Evidence, Confidence, Unanswered) and rename consistently.

### 5.2 Reconcile quantities and staleness
4. **Add a "data currency" column** to every quantitative claim (source date, survey population, confidence). Flag stale anchors (NHTSA 2020 OTA) for refresh or removal.
5. **Triangulate survey figures** (Perforce, electronica, S9 n=9) before using them in prioritization; where triangulation fails, downgrade confidence.
6. **Reconcile the cost claims** (50% dev cost for testing, 35–45% rework cost for requirements, 10–30% effort for data transformation) by stating denominators explicitly and noting overlap.

### 5.3 Close coverage gaps
7. **Add workstreams or sections for:** hardware/semiconductor lifecycle, cloud/backend and connected services, energy/power/thermal software, OSS governance and SBOM, software architecture quality and technical-debt metrics, developer experience.
8. **Add a cross-workstream dependency map** linking problems (e.g., requirement ambiguity → test spec quality → traceability) and deduplicating unanswered questions.
9. **Verify Task.md coverage** — confirm every § of the master prompt (including §11 root-cause template) is addressed by at least one workstream.

### 5.4 Improve evidence discipline
10. **Apply the tier system consistently** (Scout/Verify/Auditor) and record which tier each workstream achieved.
11. **Resolve the MBSE tension** (66% reliance vs. 22% definitional clarity) with a clearer definition of "reliance" (nominal adoption vs. genuine MBSE practice).
12. **Resolve the Adaptive-vs-zonal tension** by stating which zonal/centralised compute platforms are AUTOSAR Adaptive vs. non-AUTOSAR (Linux/QNX/Android).

### 5.5 Prioritize high-leverage root causes
13. **Treat the following as the core, cross-cutting root causes** (each appears in ≥3 workstreams): toolchain fragmentation and proprietary formats; ARXML/configuration complexity; compliance administration overhead; timing analysis unsolved; scenario completeness unknown for ADAS/ADS; skills and workforce gaps; knowledge silos and turnover. Direct improvement investment here rather than at the symptom level.
14. **Define measurable KPIs** for the gaps currently marked "unquantified": trace link density and correctness, model fidelity thresholds per ASIL, scenario coverage criteria, HIL utilization and queue lengths, onboarding duration by role, knowledge-loss cost.

### 5.6 Governance
15. **Establish an AI-in-engineering governance framework** (W5-025, W4-030) before further AI tooling adoption: permitted tasks, verification gates, provenance, IP handling, audit trails, and human-approval requirements.
16. **Promote open standards and open-source reference toolchains** (W4-004, W4-025, W4-027) to reduce vendor lock-in and lower barriers for SMEs/startups, with conformance test suites to make openness auditable.

---

## 6. Short Summary

The five workstreams are individually thorough but **not integrated**: 131 problem records reduce to roughly 60–70 unique problems once overlaps are removed; the same root causes (toolchain fragmentation, ARXML complexity, compliance overhead, timing, scenario completeness, skills, knowledge silos) are recorded 2–5 times each under different IDs and schemas. Several quantitative anchors are stale (NHTSA 2020 OTA), unverified (66% MBSE reliance), or ambiguous (shift-left "up 6%"). Key topic gaps include hardware/semiconductor lifecycle, cloud/backend software, energy/thermal software, OSS governance, and architecture-quality metrics. Recommendations: build a master problem index with global IDs, harmonize the record schema, deduplicate overlapping families, reconcile quantities and staleness, close the topic gaps, and prioritize investment in the seven cross-cutting root causes.

*End of cross-check report.*

---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

