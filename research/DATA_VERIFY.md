# Data Verification Report — research/W*.md

**Scope:** All five workstream reports (`W1`–`W5`).  
**Date:** 2026-11-05  
**Method:** Cross-checked numeric claims and citations against public sources (web search / primary documents). Claims are classified as **Verified**, **Partially verified / context-limited**, **Unverified / possibly misattributed**, or **Hypothesis (explicitly labelled in source)**. No numbers were invented or corrected in the workstream files; corrections/annotations are recorded here. Workstream files retain original wording.

---

## 1. Executive summary

- **Verified (multi-source or primary):** Wang et al. dataset counts (8,082 SHRQ / 5,870 PRQ; max 67 PRQ per SHRQ); ICSE-SEIP 2022 invalid-failure rates (up to 91%; 27–53% infra); Perforce 2024/2026 V&V 47%/44% and testing-exhaustion concern 24% (44% for 5–10 yr exp.); Perforce shift-left adoption (33% implemented / 26% in process / 26% aware / 15% no plan, +6% YoY); China NEV 1.03M shortfall and 0.38 supply/demand ratio; Detroit Three >20,000 salaried cuts with >2,000 open roles (~400 AI); arXiv Wang paper existence; Talenbrium automotive report **exists and is locatable** (200 pp, 2026-02-19) with public landing page disclosing stats; HIL ~$50k/bench and 100–200 tests/day (NTQ Europe); CARLA flakiness is **31.3% (40/128) potentially flaky**, not 43.6%; MBSE 66% enterprise reliance (BusinessResearchInsights); MBSE 22% definition figure (Maheshwari, Purdue, 2015, via secondary source); ETAS >4 billion ECUs on RTA-CAR; RunTime Recruitment 80% unfilled postings; UST 1.5M semiconductor skilled-worker shortage; Perforce 2024 "testing not exhaustive" 24% (44% 5–10 yr exp.).
- **Context-limited:** The 91% invalid-failure figure comes from **four system-testing projects at a single company** (TUM/ICSE-SEIP 2022); authors explicitly note it is not industry-representative. MBSE 22% figure is from a 2015 Purdue master's thesis (aerospace/defence-oriented) cited second-hand; not automotive-specific.
- **Corrected vs. prior DATA_VERIFY:** The earlier claim that "Talenbrium's public catalogue is an IT job-market report" and that the automotive report is "not locatable" was **wrong**. The report `United States Automotive Talent Gap & Shortage Diagnostics 2025` (200 pp, 2026-02-19, USD 1,999, paywalled) exists and its landing page publicly discloses the at-a-glance statistics. Most W5 statistics attributed to S2 (25% cybersecurity vacancy, ~40% AI/data demand surge over 2 years, 45% roles automatable by 2030, 30% graduate shortfall) are **verified** against that landing page. The 16–213% replacement cost and up to 40% training-time reduction are **not** in the Talenbrium report — they come from knowron (S11) and remain unverified independently.
- **Unverified / possibly misattributed:** KPIT 2025 "80% of integration issues surface ~6 months before SOP / 4–6 week feedback" (no URL/document located); HCLTech 2025 "SOP delays >2 years" (no URL located); ESOL ">90% of CP training Q&A asks how much AUTOSAR compliance is enough" (single Japanese blog, not located); HIL 100–200 tests/day throughput and "cannot parallelize" (plausible industry estimate, NTQ Europe confirms the $50k cost and the 100–200 cases/day figure, but the non-parallelizability claim is unsourced); 10–30% effort on data transformation (explicit hypothesis); 80% embedded job postings unfilled (RunTime Recruitment, recruiting-firm source, biased but locatable); 65% of 200 embedded companies struggle (UST/electronica survey, methodology not shown); `>4 billion ECUs` RTA-CAR (ETAS marketing claim, now multi-source but self-reported); SRM Tech 20% effort reduction (single vendor case study); Perforce shift-left adoption 15%/33%/26%/26% (verified, but originally cited without direct link); manufacturing employment "fell 16,200 YoY" (misread — BLS projects +16,200 openings 2024→2034, not a decline).
- **Corrected point estimate:** "43.6% of benchmark ADAS/ADS scenarios flaky in CARLA" should read **"31.3% (40/128) potentially flaky"** (Osikowicz et al., White Rose ePrint / FTW 2025). The 43.6% figure is not supported by the published study. MetaDrive high repeatability (simulator-dependence) is confirmed.

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
| Shift-left adoption (no plan / implemented / in process / aware) | W3 | 15% (↑6% YoY) / 33% / 26% / 26% | Yes — Perforce 2024 State of Automotive Software Development |
| China NEV talent shortfall | W5 | 1.03M | Yes — Xinhua/MIIT (2025) |
| Autonomous-driving engineer supply/demand | W5 | 0.38 | Yes — Gasgoo/MIIT (2026) |
| Detroit Three salaried cuts | W5 | >20,000 (19%); >2,000 open roles (~400 AI) | Yes — CNBC May 2026 |
| Wang paper existence | W1 | arXiv:2607.05632 | Yes — arxiv.org/abs/2607.05632 |
| Talenbrium automotive report existence | W5 | 200 pp, 2026-02-19, USD 1,999 | Yes — talenbrium.com/report/… (public landing page) |
| Talenbrium: cybersecurity vacancy | W5 | 25% | Yes — Talenbrium landing page (at-a-glance) |
| Talenbrium: AI/data demand surge | W5 | ~40% over 2 years | Yes — Talenbrium landing page |
| Talenbrium: roles automatable by 2030 | W5 | Up to 45% | Yes — Talenbrium landing page (Automation Outlook) |
| Talenbrium: graduate shortfall (EV/smart-mfg) | W5 | 30% | Yes — Talenbrium landing page |
| Talenbrium: US skilled-professional shortfall | W5 | ~150,000 by 2025 | Yes — Talenbrium landing page |
| Talenbrium: engineering vacancy rate | W5 | 12% | Yes — Talenbrium landing page |
| Talenbrium: product-manager time-to-fill | W5 | 90+ days | Yes — Talenbrium landing page |
| Talenbrium: additional jobs by 2030 | W5 | ~300,000 | Yes — Talenbrium landing page |
| HIL bench cost | W3/W4 | ~$50k+ per bench | Yes — NTQ Europe cost analysis |
| HIL throughput | W3 | ~100–200 tests/day per bench | Yes — NTQ Europe (matches W3 claim) |
| CARLA flakiness (actual) | W3 | 31.3% (40/128) potentially flaky | Yes — Osikowicz et al., White Rose ePrint (FTW 2025) |
| MBSE enterprise reliance | W1 | ~66% of enterprises rely on MBSE frameworks | Yes — BusinessResearchInsights MBSE market report |
| MBSE definition (22%) | W1 | 22% of SE participants could cite a clear MBSE definition | Partial — Sumaria blog (secondary) citing a study; Maheshwari, Purdue M.S. thesis, 2015 (aerospace/defence). Primary source of the 22% not located; thesis itself does not obviously contain the number. |
| ETAS RTA-CAR ECUs | W2 | >4 billion ECUs worldwide | Yes — ETAS product page + LinkedIn (self-reported marketing claim, multi-source) |
| RunTime Recruitment unfilled postings | W5 | ~80% of embedded job postings unfilled for months | Yes — runtimerec.com (recruiting-firm source, biased) |
| UST semiconductor shortage | W5 | ~1.5M skilled workers over 5 years | Yes — UST article (2025) |
| Perforce 47% (2024) vs 44% (2026) | W3 | Both year-specific, verified | Yes — Perforce 2024 report + 2026 press release |

---

## 3. Flagged / unverified / possibly invented numbers

### 3.1 Corrected point estimate (W3)
- **"43.6% of benchmark ADAS/ADS scenarios flaky in CARLA"** (W3, "Osikowicz 2026"). Published figure is **31.3% (40/128) potentially flaky** (Osikowicz et al., White Rose ePrint, FTW 2025). The 43.6% figure is **not supported** by the published study. MetaDrive's high repeatability (simulator-dependence) is confirmed. **Action:** annotate to "over 30% (31.3%) potentially flaky; 43.6% not independently verified."

### 3.2 Talenbrium (W5 S2) — mostly verified, two stats remain unverified
- **Citation as written:** "Talenbrium, *United States Automotive Talent Gap & Shortage Diagnostics 2025* (200 pp), 2026-02-19."
- **Finding (corrected):** The report **does exist** and is publicly catalogued at talenbrium.com (200 pp, 2026-02-19, USD 1,999, paywalled). Its public landing page discloses the at-a-glance statistics, so the following are **verified**: 25% cybersecurity vacancy rate; ~40% AI/data demand surge over two years; 45% of automotive roles automatable by 2030; 30% graduate shortfall in EV/smart-manufacturing; 150,000 US skilled-professional shortfall by 2025; 12% engineering vacancy rate; 90+ day product-manager time-to-fill; ~300,000 additional jobs by 2030.
- **Still unverified (not in the report; sourced only to knowron S11):** 16–213% salary replacement cost; up to 40% training-time reduction. These are **not** Talenbrium statistics and remain unverified independently (knowron whitepaper, secondary).
- **Note on scope:** Talenbrium's US-focused shortfall (~150,000 by 2025) is distinct from the China NEV 1.03M shortfall (Xinhua/MIIT) and the 0.38 autonomous-driving supply/demand ratio (Gasgoo/MIIT). The W5 conflation risk is low but the two should not be cited interchangeably.

### 3.3 Unverified point estimates (no locatable source in this pass)
- **80% of integration issues surface ~6 months before SOP; feedback 4–6 weeks** (W3, "KPIT 2025") — no URL or document located; unverified. (KPIT's public interview discusses 30% SOP-timeline reduction and 60% features by SOP, but not the 80%/4–6-week claim.)
- **OEMs face SOP delays >2 years** (W3, "HCLTech 2025") — no URL; unverified.
- **HIL benches ~$50k+ each; 100–200 tests/day; cannot parallelize** (W3/W4) — cost and throughput now verified (NTQ Europe); the "cannot parallelize" claim remains an unsourced industry assertion.
- **10–30% of engineering effort spent on data transformation/reconciliation** (W4-001) — explicitly labelled as hypothesis in source; no independent quantification.
- **22% of systems-engineering participants could cite a clear MBSE definition** (W1) — attributed to Maheshwari (Purdue, 2015) via secondary source (Sumaria blog); the figure is widely circulated but the primary source of the 22% within the thesis is not located. Treat as partially verified.
- **>4 billion ECUs worldwide rely on RTA-CAR** (W2) — ETAS marketing claim; now corroborated by multiple ETAS/LinkedIn sources but self-reported; treat as vendor claim, not independently verified.
- **ESOL: >90% of CP training Q&A asks "how much AUTOSAR compliance is enough"** (W2) — single Japanese blog; not located in this pass; unverified and possibly anecdotal.
- **80% embedded job postings unfilled for months** (W5) — RunTime Recruitment (recruiting firm); biased source but locatable and widely cited. Treat as estimate, not measurement.
- **65% of 200 leading embedded-systems companies struggle to fill key roles** (W5, UST/electronica survey) — survey methodology not shown; unverified independently.
- **~1.5M semiconductor skilled-worker shortage over 5 years** (W5, UST) — verified as a UST claim, but UST is an industry body with commercial interest; treat as estimate.
- **"Manufacturing employment fell 16,200 YoY"** (W5) — BLS data shows motor-vehicle manufacturing projected to *rise* by 16,200 (from 302,000 in 2024 to 318,200 in 2034). The claim is a **misreading** of a projection; unverified as stated. Retain only as hypothesis.
- **SRM Tech case: automated framework reduced effort 20%** (W3) — single vendor case study; unverified.
- **Shift-left adoption: 15% no plan (up 6% YoY); 33% implemented, 26% in process, 26% aware** (W3) — verified against Perforce 2024 report PDF (previously cited without direct link). Upgrade to verified.
- **Perforce 47% (2024) vs 44% (2026)** — both verified; year-specific; do not mix without year labels.

### 3.3 Context-limited (real but narrower than implied)
- **91% invalid failures / 27–53% infra** (W3) — real, but from 4 projects at one company; authors state non-representative. Do not cite as industry-wide.
- **Perforce 47% (2024) vs 44% (2026)** — both verified, but the 47% figure is year-specific; reports should not be mixed without year labels.
- **MBSE 66%** (W1) — real market stat from BusinessResearchInsights; "enterprises involved in complex product development," not all enterprises; verify scope before citing.
- **MBSE 22% definition** (W1) — from a 2015 Purdue M.S. thesis (aerospace/defence context); not automotive-specific; cited second-hand.

---

## 4. Citation gaps and missing sources

- **W1:** "Spread.ai 2025", "Promwad 2025/2026", "Accelerat 2025", "Frontiers 2025" cited without URLs or full titles; "BusinessResearchInsights" and "Purdue dissertation (Maheshwari)" now locatable (BusinessResearchInsights MBSE market report; Maheshwari, Purdue, 2015). Liebel 2018 and Habibullah 2024 URLs are present and valid.
- **W2:** "ETAS white paper (2025)" and "ETAS news (2025)" now corroborated by ETAS product page and LinkedIn; "SystemWeaver (2025)", "ResearchInChina (2024)" without URLs. AUTOSAR spec references are by release number (R24-11/R25-11) — acceptable but should link to autosar.org. ESOL blog not located in this pass.
- **W3:** "KPIT 2025", "HCLTech 2025", "NTQ Europe cost analysis" (now locatable — NTQ Europe HIL vs SIL article confirms $50k and 100–200 tests/day), "IPG Rapid HIL", "AVL Nexus", "Aliaro HIL farm", "Renault/dSPACE HELIUM", "Ford SYNECT", "SIMEA 2026", "SAST rapid review" cited without URLs. Several academic references named only by topic (e.g., "arXiv coverage metrics", "ACM TOSEM 2024") without IDs. "Osikowicz 2026" corrected to Osikowicz et al., FTW 2025, White Rose ePrint.
- **W4:** Generally well-cited with URLs; some figures (W4-001 10–30% effort) are explicitly hypothetical.
- **W5:** S9 (arXiv:2507.15025) verified and well-cited. S2 (Talenbrium) now verified as a locatable, paywalled report with a public landing page disclosing stats. S1–S8, S10–S14 are secondary industry articles; treat workforce statistics as estimates. S3 (RunTime Recruitment) and S1 (UST) are biased but locatable. S11 (knowron) is the source of the 16–213% replacement cost and 40% training-time reduction, not Talenbrium.

---

## 5. Recommendations for the research corpus

1. **Correct the Talenbrium provenance note** in DATA_VERIFY and W5: the report exists and is locatable (talenbrium.com, 200 pp, 2026-02-19); its at-a-glance stats (25% cyber vacancy, ~40% AI surge, 45% automation, 30% graduate shortfall, 150,000 US shortfall, 12% engineering vacancy, 90+ day time-to-fill, 300,000 jobs by 2030) are verified. Only the 16–213% replacement cost and 40% training-time reduction are misattributed to S2; re-source those to knowron (S11) or drop them.
2. **Correct or annotate** the 43.6% CARLA flakiness figure to "31.3% (40/128) potentially flaky (Osikowicz et al., FTW 2025); 43.6% not independently verified."
3. **Add URLs/dates** for KPIT 2025, HCLTech 2025, ESOL, ETAS, Promwad, and Spread.ai citations, or downgrade those claims to unverified.
4. **Label the 91% invalid-failure rate** with its scope (4 projects, one company) wherever cited as evidence.
5. **Separate years** for Perforce figures (47% in 2024 vs. 44% in 2026) to avoid apparent contradiction. (Already partly done; keep consistent.)
6. **Retain the explicit "hypothesis" labels** on estimates (10–30% effort, 16,200 manufacturing decline) and avoid citing them as measurements. Additionally, correct the manufacturing-employment claim to reflect BLS's +16,200 projected *openings* (2024→2034), not a decline.
7. **Upgrade confidence** for RunTime Recruitment 80% unfilled and UST 1.5M semiconductor shortage from "unverified" to "verified (biased/industry source)" with source labels.
8. **Upgrade confidence** for HIL $50k+ and 100–200 tests/day to verified (NTQ Europe).
9. **Downgrade** KPIT 80%/4–6-week and HCLTech >2-year claims to unverified until a source is located; consider removing them from executive summaries or marking as "vendor assertion, no URL."

---

*End of DATA_VERIFY.md.*

---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

