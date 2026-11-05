# Data Verification Report — research/W*.md

**Scope:** All five workstream reports (`W1`–`W5`).  
**Date:** 2026-11-05  
**Method:** Cross-checked numeric claims and citations against public sources (web search / primary documents). Claims are classified as **Verified**, **Partially verified / context-limited**, **Unverified / possibly misattributed**, or **Hypothesis (explicitly labelled in source)**. No numbers were invented or corrected; original wording is preserved in the workstream files.

---

## 1. Executive summary

- **Verified (multi-source or primary):** Wang et al. dataset counts (8,082 SHRQ / 5,870 PRQ; max 67 PRQ per SHRQ); ICSE-SEIP 2022 invalid-failure rates (up to 91%; 27–53% infra); Perforce 2024 V&V 47% / 24% testing-exhaustion concern; China NEV 1.03M shortfall and 0.38 supply/demand ratio; Detroit Three >20,000 salaried cuts with >2,000 open roles (~400 AI); arXiv Wang paper existence.
- **Context-limited:** The 91% invalid-failure figure comes from **four system-testing projects at a single company** (TUM/ICSE-SEIP 2022); authors explicitly note it is not industry-representative.
- **Unverified / possibly misattributed:** W5's "Talenbrium, *United States Automotive Talent Gap & Shortage Diagnostics 2025* (200 pp)" citation does not match a publicly locatable automotive report (Talenbrium's public catalogue is an **IT** job-market report). Statistics attributed to it (25% cybersecurity vacancy, 40% AI demand surge, 45% automation by 2030, 30% graduate shortfall, 16–213% replacement cost, 40% training-time reduction) are **not independently verified** in this pass. Some (e.g., 1.03M China shortfall, 0.38 ratio, Detroit cuts) are verified via other sources (Xinhua/MIIT, CNBC).
- **Unverified point estimates:** 43.6% flaky CARLA scenarios (actual published figure is "over 30% potentially flaky"); 80% integration issues ~6 months before SOP / 4–6 week feedback (KPIT 2025, no URL); SOP delays >2 years (HCLTech 2025, no URL); HIL ~$50k+/bench and 100–200 tests/day; 10–30% effort on data transformation (explicit hypothesis); 22% MBSE-definition figure and 66% MBSE-adoption market stat.
- **Missing sources:** Many W1/W2/W3 claims cite vendor/blog names without URLs or dates (Spread.ai, Promwad, KPIT, HCLTech, ESOL, ETAS white paper). W4/W5 are comparatively well-cited.

---

## 2. Verified statistics (with sources)

| Claim | Workstream | Value | Source verified? |
|---|---|---|---|
| SHRQ/PRQ dataset scale | W1 | 8,082 SHRQ / 5,870 PRQ | Yes — arXiv:2607.05632 (Infineon) |
| Max PRQs per SHRQ | W1 | 67 | Yes — arXiv:2607.05632 (table shows Max 67) |
| Invalid failed test executions | W3 | Up to 91% | Yes — Jordan et al., ICSE-SEIP 2022 (4 projects, single company) |
| Infra-linked failures | W3 | 27–53% | Yes — same source |
| V&V most time-consuming | W3 | 47% (2024); 44% (2026) | Yes — Perforce 2024 (eenewseurope) and 2026 press release |
| "Testing not exhaustive" top concern | W3 | 24% (44% for 5–10 yr exp.) | Yes — Perforce 2024 |
| China NEV talent shortfall | W5 | 1.03M | Yes — Xinhua/MIIT (2025) |
| Autonomous-driving engineer supply/demand | W5 | 0.38 | Yes — Gasgoo/MIIT (2026) |
| Detroit Three salaried cuts | W5 | >20,000 (19%); >2,000 open roles (~400 AI) | Yes — CNBC May 2026 |
| Wang paper existence | W1 | arXiv:2607.05632 | Yes — arxiv.org/abs/2607.05632 |

---

## 3. Flagged / unverified / possibly invented numbers

### 3.1 Likely misattributed citation (W5 S2)
- **Citation as written:** "Talenbrium, *United States Automotive Talent Gap & Shortage Diagnostics 2025* (200 pp), 2026-02-19."
- **Finding:** No public record of an automotive-specific Talenbrium report with that title. Talenbrium's public catalogue contains a *US IT Job Market* report (250 pp). The "200 pp" automotive report is not locatable.
- **Impact:** Statistics attributed solely to S2 are unverified: 25% cybersecurity vacancy rate; ~40% AI/data demand surge over two years; 45% of automotive roles automatable by 2030; 30% graduate shortfall in EV/smart-manufacturing; 16–213% salary replacement cost; up to 40% training-time reduction. (The 40% training-time reduction is also claimed via knowron S11, separately.)
- **Note:** Some S2-attributed facts (1.03M China shortfall, 0.38 ratio, Detroit cuts, salary ranges) are corroborated by upperlev.com (CNBC/Gasgoo/BLS/Glassdoor) — but the *Talenbrium* provenance itself is unverified.

### 3.2 Unverified point estimates (no locatable source in this pass)
- **43.6% of benchmark ADAS/ADS scenarios flaky in CARLA** (W3, "Osikowicz 2026"). Published comparable figure is "over 30% potentially flaky" (White Rose ePrint, Osikowicz et al.). The 43.6% figure is **not confirmed**; treat as unverified. Also, MetaDrive is reported highly repeatable, so simulator-dependence is real but the exact percentage is uncertain.
- **80% of integration issues surface ~6 months before SOP; feedback 4–6 weeks** (W3, "KPIT 2025") — no URL or document located; unverified.
- **OEMs face SOP delays >2 years** (W3, "HCLTech 2025") — no URL; unverified.
- **HIL benches ~$50k+ each; 100–200 tests/day; cannot parallelize** (W3/W4) — plausible industry estimate; no specific source located; should be cited to a vendor/analyst source.
- **10–30% of engineering effort spent on data transformation/reconciliation** (W4-001) — explicitly labelled as hypothesis in the source; no independent quantification.
- **22% of systems-engineering participants could cite a clear MBSE definition** (W1) — attributed to a Purdue dissertation (Maheshwari) with no URL; unverified.
- **66% of enterprises rely on MBSE frameworks** (W1) — market stat from BusinessResearchInsights; explicitly flagged "verify" in source; unverified.
- **>4 billion ECUs worldwide rely on RTA-CAR** (W2) — ETAS marketing claim; unverified independently.
- **ESOL: >90% of CP training Q&A asks "how much AUTOSAR compliance is enough"** (W2) — single Japanese blog; unverified and possibly anecdotal.
- **80% embedded job postings unfilled for months** (W5) — from a recruiting firm (RunTime Recruitment); plausible but biased source; unverified independently.
- **65% of 200 leading embedded-systems companies struggle to fill key roles** (W5, UST/electronica survey) — survey methodology not shown; unverified.
- **~1.5M semiconductor skilled-worker shortage over 5 years** (W5, UST) — unverified in this pass.
- **"Manufacturing employment fell 16,200 YoY"** (W5) — BLS data shows motor vehicle manufacturing projections of 16,200 *openings*, not a decline; the claim may be a misreading. Unverified as stated.
- **SRM Tech case: automated framework reduced effort 20%** (W3) — single vendor case study; unverified.
- **Shift-left adoption: 15% no plan (up 6% YoY); 33% implemented, 26% in process, 26% aware** (W3) — attributed to Perforce 2024 without direct source link; plausible but not located in this pass.

### 3.3 Context-limited (real but narrower than implied)
- **91% invalid failures / 27–53% infra** (W3) — real, but from 4 projects at one company; authors state non-representative. Do not cite as industry-wide.
- **Perforce 47% (2024) vs. 44% (2026)** — both verified, but the 47% figure is year-specific; reports should not be mixed without year labels.

---

## 4. Citation gaps and missing sources

- **W1:** "Spread.ai 2025", "Promwad 2025/2026", "Accelerat 2025", "Frontiers 2025" cited without URLs or full titles; "BusinessResearchInsights" and "Purdue dissertation (Maheshwari)" without links. Liebel 2018 and Habibullah 2024 URLs are present and valid.
- **W2:** "ETAS white paper (2025)" and "ETAS news (2025)" without URLs; "SystemWeaver (2025)", "ResearchInChina (2024)" without URLs. AUTOSAR spec references are by release number (R24-11/R25-11) — acceptable but should link to autosar.org.
- **W3:** "KPIT 2025", "HCLTech 2025", "NTQ Europe cost analysis", "IPG Rapid HIL", "AVL Nexus", "Aliaro HIL farm", "Renault/dSPACE HELIUM", "Ford SYNECT", "SIMEA 2026", "SAST rapid review" cited without URLs. Several academic references named only by topic (e.g., "arXiv coverage metrics", "ACM TOSEM 2024") without IDs.
- **W4:** Generally well-cited with URLs; some figures (W4-001 10–30% effort) are explicitly hypothetical.
- **W5:** S9 (arXiv:2507.15025) verified and well-cited. S1–S8, S10–S14 are secondary industry articles; treat workforce statistics as estimates. S2 citation is the most problematic (see §3.1).

---

## 5. Recommendations for the research corpus

1. **Downgrade confidence** for all W5 statistics sourced only to S2 (Talenbrium) until an automotive-specific report is located or the stats are re-sourced to upperlev.com / CNBC / Gasgoo / BLS.
2. **Correct or annotate** the 43.6% CARLA flakiness figure to "over 30% potentially flaky (Osikowicz et al.); 43.6% not independently verified."
3. **Add URLs/dates** for KPIT 2025, HCLTech 2025, ESOL, ETAS, Promwad, and Spread.ai citations.
4. **Label the 91% invalid-failure rate** with its scope (4 projects, one company) wherever cited as evidence.
5. **Separate years** for Perforce figures (47% in 2024 vs. 44% in 2026) to avoid apparent contradiction.
6. **Retain the explicit "hypothesis" labels** on estimates (10–30% effort, 16,200 manufacturing decline) and avoid citing them as measurements.

---

*End of DATA_VERIFY.md.*