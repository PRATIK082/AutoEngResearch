# G1 — Semiconductor, Silicon and Hardware Engineering (Automotive)

**Workstream:** G1 (Semiconductor / Silicon / Hardware Engineering)  
**Scope:** Problem discovery, root-cause analysis, evidence, unanswered questions for problems originating before software development (Task.md §3.B)  
**Date:** 2026-11-05  
**Status:** Verify (Tier 2) — findings cross-checked against multiple sources; numerical claims only where sourced.

---

## 1. Workstream Summary

Semiconductor and hardware engineering in automotive is characterised by:

- **Lifecycle mismatch**: vehicles are engineered for 15+ years of service, but many semiconductors reach End-of-Life (EOL) in <7 years; device obsolescence is structural, not incidental.
- **Automotive-grade qualification burden**: AEC-Q100/Q101/Q200 and mission-profile realities create cost, schedule, and availability gaps; advanced nodes are hard to qualify.
- **Supply-chain fragility**: multi-tier opacity beyond Tier-1, geopolitical concentration of fab capacity, mature-node underinvestment, and competition with AI/datacenter for advanced nodes and memory.
- **Chip shortage legacy**: 2021–2022 shortages removed ~15 million vehicles from production schedules (2021–2022) and ~$210B loss (2021); feature deletion and deleted safety features impacted quality.
- **Hardware/software co-design silos**: OEMs/Tier-1s historically separate hardware and software; MCAL/BSP/driver layers couple software to silicon revisions; hardware revisions break BSP patches.
- **Memory wall**: compute growth (ADAS/SDV/HPC) outpaces embedded flash scalability; external flash bandwidth limits XiP; DRAM safety/reliability concerns.
- **HIL hardware dependency**: validation depends on physical rigs, sensor/actuator fidelity, and hardware availability; chip/ECU availability gates software validation.
- **Talent shortage**: semiconductor design/process/test engineering capacity is constrained; aging workforce and perception gap vs. software employers.

---

## 2. Problem Catalogue

### G1-001 — Automotive semiconductor content growth outpaces design and manufacturing capacity

- **Domain:** Semiconductor architecture / chip design
- **Lifecycle stage:** Architecture, design, manufacturing
- **Problem:** Semiconductor content per vehicle grows exponentially (ADAS, electrification, connectivity, SDV), but chip design capacity, fab capacity, and qualification throughput lag; OEMs compete for engineering and fab resources.
- **Who experiences it:** OEMs, Tier-1s, semiconductor suppliers, ECU manufacturers.
- **Root cause:** Vehicle E/E complexity growth (100+ ECUs, zonal/HPC) vs. linear growth in semiconductor engineering/fab capacity; long design cycles (2–5 years) for automotive silicon.
- **Contributing factors:** Consolidation to central compute; rising memory/SoC content; safety/security requirements; multi-year qualification.
- **Technical limitations:** Advanced-node design cost; packaging/thermal limits; mixed-signal/RF integration.
- **Human skill limitations:** Shortage of chip designers (projected 23,000 shortfall by 2030 in US, per Keysight/SIA-cited analysis); aging workforce.
- **Tool limitations:** EDA tool complexity; version control not suited to chip design (large files, tool integration).
- **Process limitations:** Siloed hardware/software; upfront requirements lock-in; long tape-out cycles.
- **Frequency/severity:** Structural, worsening with SDV/ADAS.
- **Time/cost impact:** Design cycles 2–5 years; fab build 2+ years; supply-demand mismatch persists (BCG).
- **Downstream effects:** Supply shortages, feature deletion, delayed launches, cost overruns.
- **Existing solutions:** Chiplets, foundry partnerships, OEM in-house silicon (Tesla, etc.), design reuse.
- **Why insufficient:** Chiplet standards immature; OEM silicon competence nascent; fab investment slow.
- **Automation feasibility:** Medium (EDA automation, AI-assisted design).
- **AI applicability:** Medium (AI-enabled EDA, but certification/IP constraints).
- **Human approval:** Required for safety-critical silicon.
- **Evidence/sources:**
  - Infosys Automotive Industry Outlook 2025 (semiconductor market $68B 2022 → $155B 2032; three biggest challenges include semiconductor shortages). https://www.infosys.com/iki/documents/automotive-industry-outlook2025.pdf
  - Intel SDV solution brief (100+ ECUs, mile of cable). https://www.intel.com/content/dam/www/central-libraries/us/en/documents/2024-04/automotive-sdv-solution-brief.pdf
  - Keysight 2025 (projected 23,000 skilled chip designer shortage by 2030). https://www.keysight.com/blogs/en/tech/sim-des/why-semiconductor-design-teams-can-t-affor-disconnected-workflows
  - McKinsey 2022 (automotive chip demand growth; supply-demand mismatch persists). https://www.mckinsey.com/industries/advanced-electronics/our-insights/will-the-supply-demand-mismatch-persist-for-automotive-semiconductors
- **Confidence:** High.
- **Unanswered questions:** Quantified per-vehicle semiconductor cost trend; actual OEM in-house silicon adoption rate.

### G1-002 — Automotive SoCs compete with AI/datacenter for advanced nodes and memory

- **Domain:** Semiconductor supply chain / advanced nodes
- **Lifecycle stage:** Sourcing, manufacturing
- **Problem:** SDV/ADAS silicon needs advanced nodes and high-bandwidth memory, putting automakers in direct competition with AI companies for capacity; memory (DRAM/HBM) shortages emerge.
- **Root cause:** Cross-industry demand for leading-edge nodes and memory; automotive's relatively low volume and price sensitivity vs. AI premiums.
- **Contributing factors:** Centralised compute; AI-defined vehicles; memory-hungry perception/LLM workloads.
- **Technical limitations:** Automotive qualification at advanced nodes; thermal/power constraints in vehicle.
- **Human skill limitations:** Limited SoC architecture expertise in OEMs.
- **Tool limitations:** N/A (market-driven).
- **Process limitations:** Long-term demand forecasting uncertainty; node classification ("mature" vs "advanced") ambiguity.
- **Frequency/severity:** Emerging, expected to intensify (Fed note 2026).
- **Time/cost impact:** Fed (2026): next disruption may concentrate in advanced chips/memory, not mature MCUs; automakers may lose to AI for memory (Automotive News 2026 cited).
- **Downstream effects:** Production prioritisation, feature deletion, price increases.
- **Existing solutions:** Long-term supply agreements; OEM-foundry partnerships; chiplet reuse.
- **Why insufficient:** Agreements not guaranteed; AI demand still outbids.
- **Automation feasibility:** Low (market).
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - Federal Reserve note 2026 (next disruption: advanced chips/memory; automakers competing with AI for memory). https://www.federalreserve.gov/econres/notes/feds-notes/when-the-chips-went-down-motor-vehicle-production-and-the-2021-2022-semiconductor-shortages-20260928.html
  - BCG 2022 (cross-industry competition for leading-edge chips). https://www.bcg.com/ja-jp/publications/2022/tracking-next-phase-automotive-semiconductor-shortage
  - Congress CRS 2021 (automotive chips must meet higher quality standards; lower margins). https://www.congress.gov/crs-product/IF12000
- **Confidence:** High.
- **Unanswered questions:** Magnitude of automotive memory demand by 2030; whether OEM-foundry LTAs will hold under AI demand spikes.

### G1-003 — Mature-node fab underinvestment and capacity reallocation

- **Domain:** Semiconductor manufacturing
- **Lifecycle stage:** Manufacturing, sourcing
- **Problem:** Automotive relies heavily on mature nodes (MCUs, analog, power, sensors), but fab investment skews to advanced nodes; mature capacity is reallocated or aged, causing recurring shortages even outside crises.
- **Root cause:** Lower margins on mature nodes; foundries prioritise advanced-node customers; automotive is ~10% of semiconductor sales (SIA 2019).
- **Contributing factors:** Geopolitical policy; cost penalty of new vs. depreciated fabs (BCG); capacity concentration in China.
- **Technical limitations:** Mature node fabs are old; yield/quality drift; EOL of process lines.
- **Human skill limitations:** Process engineering expertise aging.
- **Tool limitations:** N/A.
- **Process limitations:** Demand forecasting poor; node classification disputes.
- **Frequency/severity:** Recurring structural (BCG: tight supply for analog/MEMS persists).
- **Time/cost impact:** BCG: imbalance persists at least 4 years; analog/MEMS tight supply due to cost penalty of new fabs.
- **Downstream effects:** Obsolescence, price spikes, feature deletion.
- **Existing solutions:** CHIPS Act investments; OEM-foundry partnerships; multi-sourcing.
- **Why insufficient:** Investments slow (2+ years to build fab); automotive share small.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - Congress CRS 2021 (automotive chips lower margins; fab build 2+ years; hesitation to invest in mature nodes). https://www.congress.gov/crs-product/IF12000
  - BCG 2022 (analog/MEMS tight supply; 50% of future capacity planned in mainland China). https://www.bcg.com/ja-jp/publications/2022/tracking-next-phase-automotive-semiconductor-shortage
  - WIN SOURCE 2026 (mature process lines enter EOL due to capacity reallocation). https://blog.win-source.net/electronics-expo/managing-automotive-chip-obsolescence-shifting-from-crisis-response-to-strategic-procurement
- **Confidence:** High.
- **Unanswered questions:** Whether CHIPS Act funds will measurably improve automotive mature-node capacity.

### G1-004 — Multi-tier supply chain opacity beyond Tier-1

- **Domain:** Semiconductor supply chain
- **Lifecycle stage:** Sourcing, manufacturing
- **Problem:** OEMs and Tier-1s lack visibility beyond first-tier suppliers where semiconductors reside; disruptions (fire, flood, geopolitical) propagate unseen; many OEMs did not even know which chips were in their vehicles during 2021–2022.
- **Root cause:** Historical outsourcing of ECU design to Tier-1s; semiconductor vendors sit at Tier-2/3; no shared BOM/traceability across tiers.
- **Contributing factors:** Proprietary data; lack of standardised component identifiers; contractual barriers.
- **Technical limitations:** No end-to-end semiconductor BOM standard; PLM/ERP gaps.
- **Human skill limitations:** Procurement and engineering coordination gaps.
- **Tool limitations:** PLM/PDM vs. engineering data integration gaps (W4-024).
- **Process limitations:** No multi-tier risk mapping; reactive crisis management.
- **Frequency/severity:** High during disruptions; structural.
- **Time/cost impact:** 2021: 2.3M vehicles lost in North America (AutoForecast Solutions); $210B loss in 2021 (feature-removal study).
- **Downstream effects:** Production stoppages, feature deletion, redesign, warranty.
- **Existing solutions:** OEM-foundry direct partnerships (GM-Wolfspeed 2021); multi-sourcing; inventory buffers.
- **Why insufficient:** Partnerships cover few parts; buffers costly; visibility still partial.
- **Automation feasibility:** Medium (supply-chain digital twins, BOM harmonisation).
- **AI applicability:** Medium (predictive disruption analytics).
- **Human approval:** Required.
- **Evidence/sources:**
  - Congress CRS 2021 (OEMs lacked visibility beyond first tier; partnerships with chip suppliers). https://www.congress.gov/crs-product/IF12000
  - ScienceDirect systemic disruption study 2022 (thematic analysis of shortage causes/effects). https://www.sciencedirect.com/science/article/pii/S030504832200127X
  - Reuters Nexperia 2025 (chokepoint at Dongguan; automakers failed to learn). https://www.reuters.com/business/autos-transportation/how-nexperia-chip-crisis-upended-auto-supply-chains-again-2025-11-24
- **Confidence:** High.
- **Unanswered questions:** Actual adoption of multi-tier semiconductor BOM visibility; cost-effectiveness of buffers.

### G1-005 — Chip shortages caused massive production loss and feature deletion

- **Domain:** Semiconductor supply chain
- **Lifecycle stage:** Production, release
- **Problem:** Shortages forced cancellation of ~15M vehicles (2021–2022) and ~$210B loss (2021); OEMs deleted features (heated seats, etc.) and prioritised high-margin models, impacting quality and customer satisfaction.
- **Root cause:** Demand/supply mismatch; just-in-time inventory; low semiconductor buffers (inventories fell from 40 days to <5 days).
- **Contributing factors:** Pandemic demand swings; fire at Renesas Naka (2021); logistics.
- **Technical limitations:** Mature-node capacity; long fab lead times.
- **Human skill limitations:** Demand forecasting weakness.
- **Tool limitations:** N/A.
- **Process limitations:** Feature-removal strategy trade-offs; prioritisation decisions.
- **Frequency/severity:** Acute 2021–2022; recurring risk (Nexperia 2025).
- **Time/cost impact:** AutoForecast Solutions: 2.3M vehicles lost in North America 2021; ScienceDirect: ~15M worldwide 2021–2022; ~$210B loss 2021.
- **Downstream effects:** Lost revenue, customer dissatisfaction, deleted safety features, warranty/rework, accelerated obsolescence.
- **Existing solutions:** Feature removal/retrofit; prioritisation; buffer stock; dual sourcing.
- **Why insufficient:** Feature deletion hurts brand/quality; buffers costly; retrofit logistics.
- **Automation feasibility:** Medium (demand forecasting, inventory optimisation).
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - ScienceDirect feature-removal study 2025 (15M vehicles cancelled 2021–2022; $210B loss 2021; inventory 40→<5 days). https://www.sciencedirect.com/science/article/abs/pii/S0305048324001919
  - Congress CRS 2021 (2.3M North America 2021). https://www.congress.gov/crs-product/IF12000
  - Fed note 2026 (supply gradually improved; high-exposed vehicles 0.5% lower production). https://www.federalreserve.gov/econres/notes/feds-notes/when-the-chips-went-down-motor-vehicle-production-and-the-2021-2022-semiconductor-shortages-20260928.html
- **Confidence:** High.
- **Unanswered questions:** Long-term quality impact of deleted features; effectiveness of feature-removal strategy.

### G1-006 — Geopolitical concentration of semiconductor manufacturing

- **Domain:** Semiconductor supply chain / geopolitics
- **Lifecycle stage:** Sourcing, manufacturing
- **Problem:** ~50% of future fab capacity planned in mainland China (BCG); concentration of packaging/assembly in Asia; export controls and geopolitical shocks (Nexperia 2025) disrupt supply; automotive exposed.
- **Root cause:** Cost-driven offshoring; policy (CHIPS Act, export controls); geopolitical tensions.
- **Contributing factors:** Single-source chokepoints; national security policies.
- **Technical limitations:** N/A (geopolitical).
- **Human skill limitations:** Limited geopolitical risk management in engineering orgs.
- **Tool limitations:** N/A.
- **Process limitations:** Sourcing decisions optimise cost not resilience; slow diversification.
- **Frequency/severity:** Structural, escalating.
- **Time/cost impact:** Nexperia 2025: factory in Dongguan became global choke point; Dutch government reversed control (Reuters).
- **Downstream effects:** Production stoppages; accelerated reshoring cost.
- **Existing solutions:** CHIPS Act; friend-shoring; OEM-foundry partnerships.
- **Why insufficient:** Reshoring slow/costly; automotive share of capacity small.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - BCG 2022 (50% of future fabrication capacity planned in mainland China). https://www.bcg.com/ja-jp/publications/2022/tracking-next-phase-automotive-semiconductor-shortage
  - Reuters 2025 (Nexperia chokepoint). https://www.reuters.com/business/autos-transportation/how-nexperia-chip-crisis-upended-auto-supply-chains-again-2025-11-24
- **Confidence:** High.
- **Unanswered questions:** Effectiveness of friend-shoring for automotive; cost premium.

### G1-007 — Vehicle service life (15+ years) vs. semiconductor EOL (<7 years) mismatch

- **Domain:** Semiconductor obsolescence
- **Lifecycle stage:** Production, maintenance, service
- **Problem:** Vehicles engineered for 15+ years service, but semiconductors reach EOL in <7 years; EOL notices increasing; service/spare parts become unavailable; software updates may be blocked by hardware obsolescence.
- **Root cause:** Semiconductor innovation cycles (7–10 years) vs. automotive longevity (15–20 years); foundry process migration.
- **Contributing factors:** Rising EOL notice frequency; capacity reallocation; geopolitical policy.
- **Technical limitations:** Last-time buy (LTB) inventory risk; redesign/requalification cost.
- **Human skill limitations:** Lifecycle management expertise scarce.
- **Tool limitations:** Component lifecycle monitoring tools immature; database enrichment needed.
- **Process limitations:** No standardised obsolescence risk management; reactive LTB/redesign.
- **Frequency/severity:** Structural, worsening (Automotive-IQ 2026).
- **Time/cost impact:** ECU contains 300+ unique components; redesign of affected ECU; requalification 6 months (WIN SOURCE 2026 case: NXP S32K144 replacement, 2 months inventory vs 6 months requalification).
- **Downstream effects:** Production delays, redesign cost, service unavailability, safety/compliance risk, early vehicle obsolescence.
- **Existing solutions:** LTB, redesign, aftermarket/authorised continuation (LTSA), multi-sourcing, predictive analytics.
- **Why insufficient:** LTB ties capital and risk of excess/shortage; redesign costly; continuation supply limited; multi-sourcing requalification burden.
- **Automation feasibility:** Medium (predictive obsolescence analytics, LTB optimisation).
- **AI applicability:** Medium (AI-driven predictive analytics proposed as next frontier).
- **Human approval:** Required.
- **Evidence/sources:**
  - Automotive-IQ 2026 (vehicles 15+ years, chips EOL <7 years; semiconductor content doubling). https://www.automotive-iq.com/industry-reports/articles/bridging-the-lifecycle-gap-managing-semiconductor-obsolescence-in-automotive-ecus
  - IJFMR 2025 (15-year service life vs 7–10 year semiconductor obsolescence; EOL notices rising). https://www.ijfmr.com/research-paper.php?id=55624
  - WIN SOURCE 2026 (NXP S32K144 case: 2 months inventory vs 6 months requalification). https://blog.win-source.net/electronics-expo/managing-automotive-chip-obsolescence-shifting-from-crisis-response-to-strategic-procurement
  - Tieto 2020 (manage hardware dependencies in long software lifecycles). https://www.tietoevry.com/globalassets/files/whitepapers/mastering_long_automotive_software_life_cycles_whitepaper.pdf
- **Confidence:** High.
- **Unanswered questions:** Cost of obsolescence per vehicle program; effectiveness of LTSAs; optimal LTB sizing.

### G1-008 — Obsolescence-driven ECU redesign and requalification cost

- **Domain:** ECU hardware / obsolescence
- **Lifecycle stage:** Maintenance, production
- **Problem:** When a key component is discontinued, ECU manufacturers must LTB, requalify an alternative, or redesign the ECU/module; each introduces cost, delay, risk, and potential safety re-assessment; pin/package/temperature-grade/AEC mismatches compound.
- **Root cause:** Tight coupling of hardware and software (drivers, MCAL, BSP); lack of drop-in equivalents; safety qualification of changes.
- **Contributing factors:** 300+ unique components per ECU; mixed-signal/thermal coupling; ISO 26262 change impact.
- **Technical limitations:** No universal drop-in replacement; package/pinout differences; driver/MCAL rework.
- **Human skill limitations:** Hardware/software co-design expertise scarce.
- **Tool limitations:** Limited automated change-impact analysis for hardware swaps.
- **Process limitations:** Requalification timelines (months); OEM approval gates.
- **Frequency/severity:** Recurring; structural.
- **Time/cost impact:** WIN SOURCE 2026: requalification ~6 months for an MCU swap; production gap risk.
- **Downstream effects:** Production stoppage, cost overruns, safety re-assessment, service unavailability.
- **Existing solutions:** Multi-sourcing, design for obsolescence, modular SiP, continuation supply.
- **Why insufficient:** Multi-sourcing requires upfront qualification of alternates; modular SiP still emerging.
- **Automation feasibility:** Medium (component lifecycle monitoring, alternates databases).
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - WIN SOURCE 2026; Automotive-IQ 2026; IJFMR 2025 (above).
  - Altium 2025 (AUTOSAR PCB: obsolescence risk, 10–15 year availability). https://resources.altium.com/p/autosar-compliance-and-automotive-pcb-design
- **Confidence:** Medium–High.
- **Unanswered questions:** Typical cost of ECU redesign due to obsolescence; frequency of obsolescence-driven redesigns.

### G1-009 — Last-time buy inventory risk and capital tie-up

- **Domain:** Semiconductor supply / finance
- **Lifecycle stage:** Production, service
- **Problem:** LTB requires large capital outlay and carries risk of excess/shortage; demand uncertainty over 15+ year service life; storage/aging of components; counterfeit risk in grey market.
- **Root cause:** Lifecycle mismatch; demand uncertainty; capital constraints.
- **Contributing factors:** Service parts unpredictability; geopolitical disruption.
- **Technical limitations:** Component aging, storage conditions, counterfeit mitigation.
- **Human skill limitations:** Demand forecasting and inventory optimisation.
- **Tool limitations:** Limited predictive demand tools.
- **Process limitations:** Finance vs. engineering trade-offs.
- **Frequency/severity:** Structural.
- **Time/cost impact:** Not quantified in sources (flagged).
- **Downstream effects:** Capital tie-up, write-offs, shortages, counterfeit infiltration.
- **Existing solutions:** Consignment, LTSA, aftermarket continuation, predictive analytics.
- **Why insufficient:** Consignment requires supplier willingness; analytics immature.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - IJFMR 2025; Automotive-IQ 2026.
- **Confidence:** Medium.
- **Unanswered questions:** Optimal LTB sizing methods; cost of excess/shortage.

### G1-010 — AEC-Q100 qualification burden and temperature-grade availability gaps

- **Domain:** Automotive-grade semiconductor qualification
- **Lifecycle stage:** Design, sourcing, manufacturing
- **Problem:** AEC-Q100 defines stress tests and temperature grades (0–3); qualification requires large sample sizes (2400 devices for ELFR vs 1500 commercial), long durations (1000h HTOL), and extensive documentation; not all functions available in all grades; grade availability gaps force design compromises.
- **Root cause:** Automotive safety/reliability requirements; harsh environment; long service life.
- **Contributing factors:** Junction vs. ambient temperature confusion; self-heating; grade 3 rarely usable due to -40°C cold start.
- **Technical limitations:** Advanced nodes harder to qualify; cost of qualification.
- **Human skill limitations:** Qualification engineering expertise; PPAP/DFMEA capability.
- **Tool limitations:** Qualification test infrastructure cost (~30% of characterization cost, per Infineon ATS 2024).
- **Process limitations:** AEC-Q100 is qualification not certification; customer-specific add-ons; PPAP/DFMEA often not provided for non-A-grade parts.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** Infineon ATS 2024: characterization hardware cost ~30% of total; qualification sample sizes larger; cold-start forces Grade 2 minimum for most cabin apps.
- **Downstream effects:** Limited component choice; cost; design constraints; over/under-qualification.
- **Existing solutions:** AEC-Q100/Q101/Q200; mission-profile-based assessment; supplier qualification reports.
- **Why insufficient:** AEC-Q100 temperature grades can overburden advanced nodes; not synonymous with "automotive grade" (Silicon Labs); customer-specific requirements beyond baseline.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - embien AEC-Q100 guide (grades, cold-start forces Grade 2, industrial grade never substitute). https://www.embien.com/technology-insights/aec-q100-automotive-ic-qualification-guide
  - Silicon Labs AN1421 (AEC-Q100 ≠ Automotive Grade; PPAP/DFMEA not provided for non-A-grade). https://www.silabs.com/documents/public/application-notes/an1421-aec-qualifications-vs-auto-grade.pdf
  - Infineon ATS 2024 (characterization hardware challenges; cost ~30%). https://www.computer.org/csdl/proceedings-article/ats/2024/10915414/254QXDDYfhS
  - ScienceDirect 2018 (AEC-Q100 overburdens advanced nodes; mission-profile alternative). https://www.sciencedirect.com/science/article/abs/pii/S0026271418308151
- **Confidence:** High.
- **Unanswered questions:** Actual cost of qualification per device; rate of grade-availability-driven redesigns.

### G1-011 — AEC-Q100 temperature-grade mismatch with real mission profiles

- **Domain:** Automotive-grade qualification
- **Lifecycle stage:** Design, qualification
- **Problem:** AEC-Q100 uses worst-case temperature grades that can overburden (over-qualify) advanced nodes and hinder adoption, while potentially underestimating write/leakage power (up to 17.9% write, 5.4% leakage) vs. real mission profiles; mission-profile-based assessment proposed but not standardised.
- **Root cause:** Qualification standard based on worst-case grades rather than application-specific profiles; process variation and aging.
- **Contributing factors:** Conservative OEM requirements; lack of agreed mission-profile methodology.
- **Technical limitations:** Mission-profile framework research-stage; not in AEC baseline.
- **Human skill limitations:** Qualification engineering judgement.
- **Tool limitations:** Simulation/thermal modelling gaps.
- **Process limitations:** Standards evolution slow.
- **Frequency/severity:** Affects advanced-node adoption.
- **Time/cost impact:** ScienceDirect 2018: up to 17.9% write power underestimation, 5.4% leakage; AEC-Q100 excessively hinders advanced node qualification.
- **Downstream effects:** Delayed adoption of advanced nodes; over-conservative designs; cost.
- **Existing solutions:** Mission-profile-based technology assessment (research); AEC-Q100 revisions.
- **Why insufficient:** Not standardised; OEM acceptance uncertain.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - ScienceDirect 2018 (mission profile vs AEC-Q100; power underestimation). https://www.sciencedirect.com/science/article/abs/pii/S0026271418308151
- **Confidence:** Medium–High.
- **Unanswered questions:** Whether mission-profile assessment will be adopted into AEC or OEM specs.

### G1-012 — Zero-defect (ppm) pressure and screening cost

- **Domain:** Automotive semiconductor quality
- **Lifecycle stage:** Manufacturing, qualification
- **Problem:** Automotive demands near-zero defects (ppm levels); screening (burn-in, HTOL, ELFR with 2400 devices) and test cost rise; as complexity grows, zero-defect becomes harder; failures in field are safety-critical and costly (recalls).
- **Root cause:** Safety-critical application; long service life; harsh environment.
- **Contributing factors:** Test escape; latent defects; aging.
- **Technical limitations:** Test coverage limits; fault grading; soft errors (cosmic rays, alpha particles) per JESD89.
- **Human skill limitations:** Test engineering expertise.
- **Tool limitations:** Test cell throughput; in-system test (Tessent IST) emerging.
- **Process limitations:** Cost vs. quality trade-offs.
- **Frequency/severity:** Pervasive; safety-critical.
- **Time/cost impact:** SemiEngineering 2025: as chips increase in complexity, harder to achieve zero-defect; new solutions (Tessent IST Nov 2024) for in-system deterministic test patterns.
- **Downstream effects:** Recalls, warranty, safety incidents.
- **Existing solutions:** AEC-Q100 stress tests; fault grading; in-system test; screening.
- **Why insufficient:** Complexity outpaces screening; test cost rising.
- **Automation feasibility:** Medium (in-system test, AI test optimisation).
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - U-Michigan 2024 (zero-defect harder as complexity grows; KLA/Mcity). https://news.engin.umich.edu/2024/10/u-michigans-lead-role-in-vital-automotive-semiconductor-efforts
  - SemiEngineering 2025 (IC testing; Tessent IST). https://semiengineering.com/challenges-in-using-sub-7nm-ics-in-automotive
  - AEC-Q100 Rev J1 (JESD89 soft errors reference). http://www.aecouncil.com/files/Documents/AEC%20Q100-Rev%20J1.pdf
- **Confidence:** Medium–High.
- **Unanswered questions:** Actual ppm defect rates by node; cost of screening per device.

### G1-013 — Advanced node (<7nm) reliability and automotive qualification challenges

- **Domain:** Semiconductor technology / advanced nodes
- **Lifecycle stage:** Design, qualification
- **Problem:** Sub-7nm ICs face reliability challenges (aging, variation, soft errors, thermal) in automotive; virtual hardware allows pre-silicon software dev but physical qualification is hard; automotive safety culture requires stringent testing.
- **Root cause:** Transistor scaling slowing; price/transistor rising; automotive environment (temperature, vibration, longevity).
- **Contributing factors:** Mixed-signal/RF integration; new materials.
- **Technical limitations:** Gate oxide, electromigration, NBTI/PBTI; soft-error rates.
- **Human skill limitations:** Advanced-node design expertise.
- **Tool limitations:** EDA for reliability sign-off.
- **Process limitations:** Qualification standards lag node scaling.
- **Frequency/severity:** Emerging.
- **Time/cost impact:** SemiEngineering 2025 (sub-7nm challenges; virtual hardware for software dev before silicon).
- **Downstream effects:** Delayed adoption of advanced nodes in safety-critical ECUs; cost.
- **Existing solutions:** Virtual prototyping; in-system test; mission-profile assessment.
- **Why insufficient:** Physical qualification still required; standards lag.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (AI-enabled EDA).
- **Human approval:** Required.
- **Evidence/sources:**
  - SemiEngineering 2025 (sub-7nm automotive challenges). https://semiengineering.com/challenges-in-using-sub-7nm-ics-in-automotive
  - imec ACP 2024 (transistor scaling slowing; price/transistor increasing; chiplets). https://www.semi.org/sites/semi.org/files/2024-11/03%20Bart%20Plackl%C3%A9.pdf
- **Confidence:** Medium.
- **Unanswered questions:** When will sub-5nm be mainstream in automotive safety ECUs?

### G1-014 — Chiplet standardization and automotive qualification gaps

- **Domain:** Semiconductor architecture / chiplets
- **Lifecycle stage:** Architecture, design, manufacturing
- **Problem:** Chiplets promise to address reticle limits and cost, but automotive chiplet ecosystem lacks standards (UCIe AUTO, packaging, AEC-Q for chiplets, die-to-die interfaces, thermal management); bringing up multi-chip systems is hard; "Lego-style" plug-and-play not yet real.
- **Root cause:** Chiplet standards immature; automotive safety/reliability requirements for multi-die packages; thermal/power challenges.
- **Contributing factors:** Cost targets; diverse package technologies.
- **Technical limitations:** Die-to-die interface standardization; thermal; testability; repair.
- **Human skill limitations:** Chiplet system architecture expertise.
- **Tool limitations:** System-level modelling tools; digital twin.
- **Process limitations:** Pre-competitive alignment needed (imec ACP).
- **Frequency/severity:** Emerging (2025–2030).
- **Time/cost impact:** imec ACP 2024: challenges in quality/reliability, architecture, affordability, standardization; WP4–WP17 work packages; 2025 building chiplet reference design.
- **Downstream effects:** Delayed HPC/ADAS compute scaling; cost.
- **Existing solutions:** imec ACP; Arm CSA; UCIe; 3D IC flows.
- **Why insufficient:** Standards not ratified for automotive; ecosystem immature.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - imec ACP (Plackle 2024). https://www.semi.org/sites/semi.org/files/2024-11/03%20Bart%20Plackl%C3%A9.pdf
  - Siemens/Fraunhofer 2024 (chiplet challenges for zones/HPC; standardization needed). https://blogs.sw.siemens.com/semiconductor-packaging/2024/09/11/user2user-2024-chiplets-for-future-automotive-application-fraunhofer
  - U-Michigan 2024 (chiplets democratize design; Mstar forum). https://news.engin.umich.edu/2024/10/u-michigans-lead-role-in-vital-automotive-semiconductor-efforts
- **Confidence:** Medium.
- **Unanswered questions:** Timeline for automotive-grade chiplet standards; cost parity with monolithic.

### G1-015 — Hardware/software co-design silos and late integration

- **Domain:** Hardware/software co-design
- **Lifecycle stage:** Architecture, design, integration
- **Problem:** OEMs historically separate hardware and software into silos; hardware-centric, siloed approach leads to software tightly coupled to discrete hardware, long cycles (2–5 years), little iteration; requirements defined upfront; integration becomes discovery.
- **Root cause:** Organisational structure; waterfall processes; lack of shared models.
- **Contributing factors:** Domain silos; supplier boundaries; tool fragmentation.
- **Technical limitations:** No unified HW/SW modelling; timing analysis unsolved (W1-010); virtual platforms needed.
- **Human skill limitations:** HW/SW co-design knowledge gaps (W5-010); communication gaps.
- **Tool limitations:** EDA not integrated with software toolchains; VCS not suited to chip design.
- **Process limitations:** Waterfall; late software validation.
- **Frequency/severity:** Pervasive; improving slowly.
- **Time/cost impact:** Synopsys 2025: 2–5 year cycles; Cadence 2014: obstacles for software validation in context of hardware.
- **Downstream effects:** Late defects, rework, recalls, slow feature delivery.
- **Existing solutions:** HW/SW co-design methodologies; virtual prototyping; digital twins; SiP; model-based design.
- **Why insufficient:** Organisational change slow; tool integration partial; talent gaps.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - Synopsys 2025 (HW/SW co-design challenges; siloed approach). https://www.synopsys.com/blogs/chip-design/silicon-software-automotive-value-chain.html
  - EDN 2025 (hardware/software silos; AI least hardware-agnostic). https://www.edn.com/architectural-opportunities-propel-software-defined-vehicles-forward
  - CMU Koopman 2010 (HW/SW codesign in automotive/aerospace). https://users.ece.cmu.edu/~koopman/pubs/abdallah10_aero_auto_codesign.pdf
  - Embedded Computing 2014 (Cadence: obstacles for software validation in context of hardware). https://embeddedcomputing.com/application/automotive/effective-hardware-software-co-design-for-automotive-systems
- **Confidence:** High.
- **Unanswered questions:** Adoption rate of HW/SW co-design in OEMs; measured cycle-time improvement.

### G1-016 — SoC vs. companion MCU split and safety-island complexity

- **Domain:** Automotive SoC/MCU architecture
- **Lifecycle stage:** Architecture, design
- **Problem:** High-performance SoCs lack built-in safety mechanisms and safety documentation, making real-time/safety behaviour hard to predict; companion MCUs (ASIL B–D) handle real-time/safety; "safety island" inside SoC increases complexity/cost/trade-offs; OS functions must be separated from real-time functions.
- **Root cause:** SoC designed for performance not safety; safety requires separate certified MCU/island.
- **Contributing factors:** Consolidation pressure; cost; mixed-criticality.
- **Technical limitations:** Safety island complexity; predictable failure behaviour; security (HSM) integration.
- **Human skill limitations:** Mixed-criticality system design.
- **Tool limitations:** Safety analysis tools.
- **Process limitations:** Architecture decisions locked early.
- **Frequency/severity:** Pervasive in central compute.
- **Time/cost impact:** Infineon 2025: using SoC without companion MCU requires safety island, increasing complexity/cost/trade-offs, limiting practicality.
- **Downstream effects:** Cost, complexity, validation burden, vendor lock-in.
- **Existing solutions:** Companion MCU; safety island; virtualisation; hypervisor.
- **Why insufficient:** Safety island increases complexity; hypervisor overhead; SOA portability limited (Tieto 2025).
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - Infineon 2025 (companion MCU blog). https://community.infineon.com/t5/Blogs/Accelerating-the-Shift-to-Software-Defined-Vehicles-The-Evolution-of-Automotive/ba-p/987091
  - W2-009 (hypervisor/mixed-criticality), W1-016 (zonal challenges).
- **Confidence:** Medium–High.
- **Unanswered questions:** Cost/complexity comparison of companion MCU vs. safety-island approaches at scale.

### G1-017 — Memory wall: compute growth vs. memory bandwidth/capacity/safety

- **Domain:** Memory architecture
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** Heterogeneous multi-core and accelerators (ADAS/SDV) outpace DRAM/SRAM bandwidth; memory becomes limiting factor; DRAM safety/reliability challenges (refresh, ECC, thermal) in safety-critical use; SRAM real-time vs. DRAM density trade-offs.
- **Root cause:** Compute scaling faster than memory; DRAM physics (refresh, row disturb, mechanical stress); safety requirements.
- **Contributing factors:** ADAS/AV data rates; centralised compute; OTA code storage.
- **Technical limitations:** DRAM read-disturb under mechanical stress (Aal 2025); soft errors (JESD89); real-time determinism.
- **Human skill limitations:** Memory system architecture expertise.
- **Tool limitations:** Memory performance modelling.
- **Process limitations:** Memory decisions late in design.
- **Frequency/severity:** Growing.
- **Time/cost impact:** Fraunhofer IESE MEMTONOMY: memory becomes limiting factor in safety-critical automotive; DRAM challenges for safety.
- **Downstream effects:** Performance bottlenecks, safety case gaps, cost.
- **Existing solutions:** Cache hierarchies; LPDDR; HBM; MRAM/RRAM research; memory co-packaging.
- **Why insufficient:** Cache cannot hold all memory; LPDDR bandwidth still limited; emerging NVM not mature.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - Fraunhofer IESE MEMTONOMY (DRAM challenges in safety-critical automotive). https://www.iese.fraunhofer.de/en/trend/autonomous-systems/memtonomy.html
  - Aal SEMICON 2025 (SRAM read disturb due to mechanical stress). https://www.semi.org/sites/semi.org/files/2025-11/Andreas_Aal.pdf
  - SemiEngineering 2026 (memory types in automotive; DRAM for compute, NAND for data, NOR for code). https://semiengineering.com/new-automotive-architectures-are-shaking-up-processors-and-memory-choices
- **Confidence:** Medium–High.
- **Unanswered questions:** DRAM soft-error rates in vehicles; optimal memory hierarchy for zonal compute.

### G1-018 — Embedded flash (eNVM) scalability and cost at advanced nodes

- **Domain:** Non-volatile memory
- **Lifecycle stage:** Design, manufacturing
- **Problem:** At advanced process nodes, automotive-qualified embedded non-volatile memory (eFlash) faces high cost (die area) and lack of scalability; more flash needed for growing code size (SDV/ADAS); external memory required.
- **Root cause:** Flash physics doesn't scale with logic; automotive qualification at advanced nodes.
- **Contributing factors:** Code size growth; OTA requirements; centralised compute.
- **Technical limitations:** eFlash area/cost; RRAM/FRAM/MRAM alternatives not mature for automotive.
- **Human skill limitations:** NVM technology expertise.
- **Tool limitations:** N/A.
- **Process limitations:** Technology selection lock-in.
- **Frequency/severity:** Growing.
- **Time/cost impact:** AllAboutCircuits 2023: at advanced nodes, eNVM costly due to die area and scalability; external memory required.
- **Downstream effects:** External memory dependency, bandwidth bottlenecks, cost.
- **Existing solutions:** External NOR/xSPI; LPDDR flash; RRAM/FRAM/MRAM research; eCT.
- **Why insufficient:** xSPI bandwidth too slow for XiP; emerging NVM not yet automotive-qualified at scale.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - AllAboutCircuits 2023 (LPDDR flash; eNVM cost/scalability). https://www.allaboutcircuits.com/industry-articles/lpddr-flash-enabling-automotive-e-e-architectures
  - EDN Asia 2023 (LPDDR flash for zonal). https://www.ednasia.com/lpddr-flash-targets-next-gen-automotive-systems
  - IEEE 2023 (28nm RRAM automotive microcontroller data memory). https://ieeexplore.ieee.org/iel7/10145914/10145815/10145951.pdf
  - EEJournal 2025 (MRAM/FRAM/PCM predictions; decade before supplanting Flash). https://www.eejournal.com/article/want-to-know-the-future-of-new-memories-mram-fram-pcm-tom-coughlin-and-jim-handy-make-predictions
- **Confidence:** Medium–High.
- **Unanswered questions:** Adoption timeline of RRAM/MRAM in automotive MCUs; cost parity.

### G1-019 — External flash bandwidth limitations for execute-in-place (XiP)

- **Domain:** Memory / storage
- **Lifecycle stage:** Architecture, design
- **Problem:** Zonal/centralised architectures use multi-core SoCs without embedded flash; external NOR/xSPI flash is too slow (xSPI peaks ~200 MHz) for real-time XiP; "shadow code" to DRAM adds cost/space; growing code size worsens.
- **Root cause:** xSPI bandwidth insufficient; DRAM needed for performance; advanced nodes lack eFlash.
- **Contributing factors:** Code size growth; real-time requirements; cost.
- **Technical limitations:** xSPI throughput; DRAM volatility; power.
- **Human skill limitations:** Memory system design.
- **Tool limitations:** N/A.
- **Process limitations:** Memory interface decisions locked early.
- **Frequency/severity:** Growing.
- **Time/cost impact:** Infineon SEMPER X1 (2023): xSPI NOR too slow, can't scale beyond 200 MHz; LPDDR4 interface offers scalability; LPDDR flash combines performance + non-volatility.
- **Downstream effects:** Cost, board space, performance, boot time.
- **Existing solutions:** LPDDR flash (SEMPER X1); eMMC/UFS; shadow code to DRAM.
- **Why insufficient:** LPDDR flash new; ecosystem/qualification immature; shadow code costly.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - Infineon SEMPER X1 (2023). https://www.allaboutcircuits.com/news/targeting-automotive-infineon-spins-industrys-first-lpddr-flash-memory
  - AllAboutCircuits 2023 (LPDDR flash). https://www.allaboutcircuits.com/industry-articles/lpddr-flash-enabling-automotive-e-e-architectures
  - EDN Asia 2023. https://www.ednasia.com/lpddr-flash-targets-next-gen-automotive-systems
- **Confidence:** Medium–High.
- **Unanswered questions:** Adoption of LPDDR flash; qualification status; cost vs. shadow-code approach.

### G1-020 — MCAL development complexity and verification burden

- **Domain:** MCAL / device drivers
- **Lifecycle stage:** Design, development, validation
- **Problem:** MCAL must map standardised software modules to specific MCU resources; complex interaction between hardware features and standardised requirements; non-standard complex drivers (CDD) must integrate; extensive configuration/dependency space makes verification/validation hard.
- **Root cause:** Standardised AUTOSAR MCAL specification vs. diverse hardware; resource sharing; configuration explosion.
- **Contributing factors:** Multicore; safety requirements; tool configuration (EB tresos).
- **Technical limitations:** MCAL safety strategy (ASIL D/B); CSP evidence; hardware safety manual alignment.
- **Human skill limitations:** AUTOSAR MCAL expertise (W2-019); driver development.
- **Tool limitations:** Configuration tools; verification.
- **Process limitations:** Verification late; config drift.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** SAE 2006 (MCAL on 32-bit): complex interaction between hardware features and standardised requirements is big challenge; verification/validation challenge due to extensive config/dependency space.
- **Downstream effects:** Integration failures; safety gaps; rework.
- **Existing solutions:** Safety-certified MCAL packages (TI: ASIL D/B); CSP; AUTOSAR MCAL specs.
- **Why insufficient:** Vendor-specific; tool coupling; CDD integration manual; safety release only final release assessed.
- **Automation feasibility:** Medium (config generation, consistency checks).
- **AI applicability:** Medium.
- **Human approval:** Required for safety config.
- **Evidence/sources:**
  - SAE 2006-01-1554 (MCAL on 32-bit challenges). https://legacy.sae.org/publications/technical-papers/content/2006-01-1554/
  - TI MCAL docs (safety strategy, CSP, CDD). https://software-dl.ti.com/C2000/docs/mcal/F29H85x/01.04.00/src/Overview_of_MCAL.html
  - W2-001, W2-012, W4-003.
- **Confidence:** High.
- **Unanswered questions:** MCAL verification effort per project; defect rates in MCAL config.

### G1-021 — MCAL vendor lock-in and toolchain coupling

- **Domain:** MCAL / toolchain
- **Lifecycle stage:** Design, configuration
- **Problem:** MCAL is delivered by MCU vendor and tied to vendor tools (EB tresos, etc.); porting to another MCU requires new MCAL and reconfiguration; configuration is tool-specific; lock-in.
- **Root cause:** MCAL is hardware-specific; vendor owns both silicon and MCAL; AUTOSAR standardises interfaces not implementations.
- **Contributing factors:** Tool licensing; safety certification tied to specific MCAL release.
- **Technical limitations:** No cross-vendor MCAL portability.
- **Human skill limitations:** Vendor-specific tool knowledge (W5-011).
- **Tool limitations:** Tool lock-in (W4-004).
- **Process limitations:** Vendor selection lock-in.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** Not quantified (flagged).
- **Downstream effects:** Migration cost; obsolescence risk; supplier dependency.
- **Existing solutions:** AUTOSAR standardised MCAL interfaces; open-source MCAL (limited).
- **Why insufficient:** Open-source MCAL immature (W4-025); certification burden.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - W2-005, W4-004, W4-025.
  - TI MCAL docs (tool-specific).
- **Confidence:** Medium–High.
- **Unanswered questions:** Cost of MCU migration including MCAL rework.

### G1-022 — BSP fragility across hardware revisions

- **Domain:** Board Support Package
- **Lifecycle stage:** Bring-up, integration, production
- **Problem:** Vendor BSPs are written for one eval board; any hardware revision (moved pin, new peripheral, power sequence) requires patches that may not apply cleanly; BSP fragility surfaces after bring-up during integration/production; toolchain validated only at bring-up.
- **Root cause:** BSP coupled to specific board revision; patch-based customisation; lack of reproducible/version-controlled toolchain.
- **Contributing factors:** Multiple hardware revisions; vendor release cycles; no CI validation against real device config.
- **Technical limitations:** BSP portability; driver abstractions.
- **Human skill limitations:** BSP engineering expertise.
- **Tool limitations:** Toolchain reproducibility; debug hardware dependency.
- **Process limitations:** Validation only at bring-up; no production-like CI.
- **Frequency/severity:** Pervasive in embedded Linux/QNX/AUTOSAR projects.
- **Time/cost impact:** Promwad 2025: toolchains break after bring-up; hardware revision pin mapping change broke BSP patch; 3 weeks later failure.
- **Downstream effects:** Integration delays; production transfer risk; quality.
- **Existing solutions:** Version-controlled toolchains; Yocto/Buildroot; CI/CD; modular BSP.
- **Why insufficient:** Automotive safety/tool qualification; hardware variants; HIL availability.
- **Automation feasibility:** Medium (CI validation against device matrix).
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - Promwad 2025 (BSP fragility after bring-up). https://promwad.com/news/top-tools-embedded-software-development-2025
  - Embitel (BSP dev challenges). https://www.embitel.com/board-support-package-bsp-development-services
  - W4-020 (CI/CD for safety-critical).
- **Confidence:** Medium–High.
- **Unanswered questions:** Rate of BSP-related integration failures; cost of hardware-revision BSP rework.

### G1-023 — Hardware bring-up late discovery and integration surprises

- **Domain:** Hardware bring-up
- **Lifecycle stage:** Bring-up, integration
- **Problem:** Hardware/software integration often happens late; bring-up reveals pinout, timing, power-sequence, memory-map issues; early software validation limited by hardware availability; virtual platforms help but fidelity gaps.
- **Root cause:** Sequential HW-then-SW development; limited early access to silicon; no virtual prototypes.
- **Contributing factors:** Silicon availability delays; toolchain setup; debug access.
- **Technical limitations:** Virtual platform fidelity (W3-004, W4-019); RTL-to-software co-simulation.
- **Human skill limitations:** Bring-up expertise; debug skills.
- **Tool limitations:** Debug hardware availability; trace.
- **Process limitations:** Late integration; no shift-left validation.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** AVIN case: BSP development accelerated bring-up and validation by 40% (vendor claim).
- **Downstream effects:** Schedule slip; rework; quality.
- **Existing solutions:** Virtual prototyping; FPGA prototyping; SiP; early BSP.
- **Why insufficient:** Virtual fidelity gaps; FPGA prototyping cost/time; hardware still needed for final validation.
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - AVIN case study (40% acceleration). https://www.avinsystems.com/case-studies/bsp-board-support-package-development/
  - S2C (FPGA prototyping for automotive ASIC). https://jp.s2cinc.com/solution-application-auto
  - W3-004, W4-019.
- **Confidence:** Medium.
- **Unanswered questions:** Actual bring-up duration and cost; effectiveness of virtual platforms.

### G1-024 — ECU hardware architecture: mixed-criticality consolidation, thermal, power, redundancy

- **Domain:** ECU hardware architecture
- **Lifecycle stage:** Architecture, design, validation
- **Problem:** Consolidating functions onto zonal/central HPC introduces mixed-criticality integration on shared compute, resource allocation/scheduling, redundancy/fault tolerance, thermal/power management, and single point of failure risks.
- **Root cause:** Consolidation of dozens of ECUs onto few HPCs; virtualisation; new failure modes.
- **Contributing factors:** Software complexity; thermal density; power budget (EV).
- **Technical limitations:** Virtualisation overhead; hypervisor safety; thermal dissipation; power states discouraged by ISO 26262.
- **Human skill limitations:** Systems/hardware architecture.
- **Tool limitations:** Thermal simulation; power analysis.
- **Process limitations:** Architecture decisions early and hard to change.
- **Frequency/severity:** Emerging, growing.
- **Time/cost impact:** Promwad 2025 (redundancy critical; heat/power; single processor failure risk); Intel 2023 (ISO 26262 discourages low-power states in safety ECUs).
- **Downstream effects:** New failure modes; cost; validation complexity; supplier renegotiation.
- **Existing solutions:** Virtualisation, hypervisors, zonal ECUs, SOA, redundancy.
- **Why insufficient:** Virtualisation overhead; SOA portability limited; standards evolving.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - Promwad 2025; Accelerat 2025; Frontiers 2025; ArXiv 2412.01840 (W1-016).
  - Intel 2023 (power management standard proposal; ISO 26262 discourages low-power states). https://www.intel.com/content/dam/www/central-libraries/us/en/documents/2023-12/vehicle-platform-power-management-standard-whitepaper.pdf
  - W1-016.
- **Confidence:** Medium–High.
- **Unanswered questions:** Failure rates of consolidated compute; cost of redundancy.

### G1-025 — Power management complexity (PMIC, cold crank, EMI, thermal)

- **Domain:** Power management / power electronics
- **Lifecycle stage:** Design, validation
- **Problem:** ECU power management must handle cold crank, start-stop, load dump, EMI (CISPR 25), thermal, multiple rails, and sequencing; mixed discrete regulators; power states constrained by ISO 26262; EV power budget tighter.
- **Root cause:** Harsh automotive electrical environment; rising compute power; safety constraints.
- **Contributing factors:** Centralised HPC power draw; EV efficiency; EMI from internal/external sources.
- **Technical limitations:** Quiescent current; EMI; thermal dissipation; cold-crank support.
- **Human skill limitations:** Power electronics design.
- **Tool limitations:** Power/thermal simulation.
- **Process limitations:** ECU-level vs. vehicle-level power management; no standard.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** ADI 2019/2023; NXP 2021 (power management complexity grows with system complexity; synchronisation of power-up/down sequences).
- **Downstream effects:** EMI failures, thermal issues, power consumption, validation cost.
- **Existing solutions:** Integrated PMICs (NXP BYLink, ADI, Maxim); SBCs; vehicle-level power management proposals.
- **Why insufficient:** No vehicle-level standard; ISO 26262 discourages low-power states; OEM-specific designs.
- **Automation feasibility:** Low–Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - NXP 2021 (missing link to safely power all ECUs). https://www.nxp.com/company/about-nxp/smarter-world-blog/BL-SAFELY-POWER-ALL-ECUS
  - ADI 2019 (high-voltage automotive power challenges). https://www.analog.com/en/resources/technical-articles/addressing-the-challenges-of-highvoltage-automotive-power-applications.html
  - ADI 2023 (off-battery PMIC challenges). https://www.analog.com/en/resources/technical-articles/choosing-off-battery-power-management-ic-for-automotive-electronics.html
  - Intel 2023 (vehicle platform power management standard proposal). https://www.intel.com/content/dam/www/central-libraries/us/en/documents/2023-12/vehicle-platform-power-management-standard-whitepaper.pdf
- **Confidence:** Medium–High.
- **Unanswered questions:** Adoption of vehicle-level power management standard; cost of PMIC integration.

### G1-026 — Power management standardization gap at vehicle level

- **Domain:** Power management
- **Lifecycle stage:** Architecture
- **Problem:** No standardised vehicle-level power management; each OEM designs E/E power differently; 100 ECUs cumulative ~316 Wh/mi; EV efficiency demands better; HPC power management needs dozens of cores/accelerators managed; static config files need updates for aftermarket.
- **Root cause:** OEM-specific architectures; lack of industry standard; safety constraints.
- **Contributing factors:** Distributed ECUs; zonal transition.
- **Technical limitations:** Heterogeneous buses; power-state safety concerns.
- **Human skill limitations:** Systems power engineering.
- **Tool limitations:** N/A.
- **Process limitations:** No governance body for power management standard.
- **Frequency/severity:** Growing.
- **Time/cost impact:** Intel 2023: ~316 Wh/mi cumulative; lack of standard; ISO 26262 discourages low-power states.
- **Downstream effects:** Energy inefficiency; range impact; complexity.
- **Existing solutions:** Intel proposal; OEM-specific.
- **Why insufficient:** Proposal not adopted; safety constraints.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (AI-driven power management).
- **Human approval:** Required.
- **Evidence/sources:**
  - Intel 2023 (above).
- **Confidence:** Medium.
- **Unanswered questions:** Whether a vehicle-level power management standard will emerge.

### G1-027 — Sensor and actuator interface fragmentation and proprietary SerDes

- **Domain:** Sensor/actuator interfaces
- **Lifecycle stage:** Architecture, integration
- **Problem:** No standardisation of sensor hardware interfaces; proprietary SerDes (GMSL, FPD-Link, APIX) dominate long-reach; MIPI CSI-2 for short reach; evolution to Ethernet-based sensors (1722) ongoing; I2C/SPI/GPIO tunnelling over Ethernet; interface churn across generations.
- **Root cause:** Vendor-specific high-speed links; bandwidth/latency needs; cost; no universal standard.
- **Contributing factors:** Camera/radar data rates; zonal architecture; SDV centralisation.
- **Technical limitations:** Bandwidth, latency, power; proprietary lock-in.
- **Human skill limitations:** Sensor integration expertise.
- **Tool limitations:** Interface test equipment.
- **Process limitations:** Interface definitions not standardised; OEM-specific.
- **Frequency/severity:** Pervasive; evolving.
- **Time/cost impact:** IEEE 2024 (Axer/Kunz): evolution of sensor interfaces; proprietary SerDes history; trend to Ethernet sensors.
- **Downstream effects:** Integration cost; vendor lock-in; validation burden; sensor replacement difficulty.
- **Existing solutions:** AUTOSAR sensor interfaces (Adaptive); IEEE 1722; MIPI; Ethernet-based sensors.
- **Why insufficient:** Proprietary links still dominate; AUTOSAR sensor interfaces exclude mechanical/electrical interface and raw data level.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - IEEE 2024 (Axer/Kunz advancements in automotive Ethernet). https://standards.ieee.org/wp-content/uploads/2024/11/104-philp-axer-advancements-automotive-ethernet.pdf
  - AUTOSAR EXP SensorInterfaces (R22-11/R20-11) (excludes mechanical/electrical and raw data level). https://www.autosar.org/fileadmin/standards/R22-11/AP/AUTOSAR_EXP_SensorInterfaces.pdf
  - Fierce Sensors (sensor integration pain; no standardisation). https://www.fiercesensors.com/sensors/trouble-sensor-interfaces
- **Confidence:** Medium–High.
- **Unanswered questions:** Timeline for Ethernet-based sensor dominance; cost of SerDes transition.

### G1-028 — Sensor integration pain and lack of standardised hardware interfaces

- **Domain:** Sensor/actuator integration
- **Lifecycle stage:** Integration, validation
- **Problem:** Integrating sensors into designs is the biggest challenge for >1/3 of engineers (Fierce survey); vendor provides only app notes/demo code; interfaces vary across vendors/parts; hardware interfaces not standardised; full-chain adaptation (HAL/BSP/MCAL/app) required.
- **Root cause:** No standardised sensor hardware interfaces; vendor-specific implementations; complexity.
- **Contributing factors:** Calibration, timing, data formats; environmental conditions.
- **Technical limitations:** Raw data level excluded from AUTOSAR sensor interfaces; calibration.
- **Human skill limitations:** Sensor integration expertise.
- **Tool limitations:** Test/validation tools.
- **Process limitations:** Late sensor integration; no standard.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** Fierce Sensors: >1/3 respondents report integration as biggest challenge, beating budget constraints.
- **Downstream effects:** Development cost, validation, sensor replacement, ADAS scenario coverage.
- **Existing solutions:** AUTOSAR sensor interfaces; OSI; ASAM; calibration standards.
- **Why insufficient:** Hardware interfaces excluded; calibration remains; vendor lock-in.
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - Fierce Sensors (above).
  - EEWORLD 2025 (sensor driver development across BSP/MCAL/ECU abstraction/app). https://en.eeworld.com.cn/news/qrs/eic707682.html
  - AUTOSAR EXP SensorInterfaces.
- **Confidence:** Medium.
- **Unanswered questions:** Cost of sensor integration; standardisation progress.

### G1-029 — ASIC/FPGA development: long cycles, tape-out risk, mixed-signal verification

- **Domain:** ASIC/FPGA development
- **Lifecycle stage:** Design, verification, manufacturing
- **Problem:** Automotive ASIC/FPGA development faces long cycles, multiple tape-outs risk, mixed-signal/analogue verification, ISO 26262 compliance, low volumes making foundry scheduling hard, and verification dominating (~70% of SoC dev).
- **Root cause:** Mixed-signal complexity; safety standards; low-volume automotive; foundry prioritisation.
- **Contributing factors:** Analogue interfaces (sensors/actuators); parasitic effects; thermal.
- **Technical limitations:** Mixed-signal modelling; FPGA prototyping fidelity; tape-out cost.
- **Human skill limitations:** Mixed-signal/ASIC design expertise; verification.
- **Tool limitations:** EDA for mixed-signal; FPGA prototyping tools.
- **Process limitations:** Verification late; re-spin cost.
- **Frequency/severity:** Pervasive for custom silicon.
- **Time/cost impact:** SAE 2014 (verification ~70% of SoC dev); S2C (multiple tape-outs risk; low-volume scheduling hard).
- **Downstream effects:** Cost, schedule, risk; ASIC obsolescence.
- **Existing solutions:** FPGA prototyping (FiL); model-based design; virtual platforms; eFPGA.
- **Why insufficient:** Prototyping cost/time; fidelity; foundry scheduling.
- **Automation feasibility:** Medium (AI-enabled EDA, virtual platforms).
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - SAE 2014-01-0240 (FPGA-based dev for automotive safety-critical SoC; verification ~70%). https://legacy.sae.org/publications/technical-papers/content/2014-01-0240/
  - MathWorks (FPGA/ASIC algorithm implementation challenges). https://www.mathworks.com/content/dam/mathworks/conference-or-academic-paper/meeting-functional-safety-standards-on-algorithm-implementation-for-fpga-and-asic-in-a-dynamic-automotive-environment.pdf
  - S2C (automotive ASIC prototyping). https://jp.s2cinc.com/solution-application-auto
  - Xilinx WP169 (processor obsolescence; FPGA soft cores). https://docs.amd.com/api/khub/documents/SRimXacG67qSXaRyh7tUXw/content
- **Confidence:** Medium–High.
- **Unanswered questions:** Cost of automotive ASIC tape-out; FPGA-to-ASIC conversion success rates.

### G1-030 — FPGA-to-ASIC conversion portability pitfalls

- **Domain:** ASIC/FPGA
- **Lifecycle stage:** Design, manufacturing
- **Problem:** Converting FPGA designs to ASICs (for cost/volume) has portability pitfalls: pinout/voltage mismatch, clocking (PLL/DLL) differences, boundary-scan, test considerations, tool flow differences; design portability not guaranteed.
- **Root cause:** FPGA vs. ASIC technology differences; tool/vendor lock-in; abstraction leaks.
- **Contributing factors:** Multi-vendor; timing closure; power.
- **Technical limitations:** Clock tree, I/O, test.
- **Human skill limitations:** Portability expertise.
- **Tool limitations:** Conversion tools.
- **Process limitations:** Late conversion decisions.
- **Frequency/severity:** Moderate.
- **Time/cost impact:** Microchip app note (portability pitfalls); Xilinx WP (soft cores avoid obsolescence).
- **Downstream effects:** Cost, schedule, risk.
- **Existing solutions:** Portable RTL; soft processor cores (MicroBlaze); design guidelines.
- **Why insufficient:** Guidelines not enforced; tool flows differ.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence/sources:**
  - Microchip app note (design portability for FPGA/ASIC conversion). https://www.microchip.com/content/dam/mchp/documents/OTH/ApplicationNotes/ApplicationNotes/doc4412.pdf
  - Xilinx WP169.
- **Confidence:** Medium.
- **Unanswered questions:** Rate of successful FPGA-to-ASIC conversions in automotive.

### G1-031 — Hardware revisions and compatibility / BSP patch breakage

- **Domain:** Hardware revisions / compatibility
- **Lifecycle stage:** Integration, production
- **Problem:** Hardware revisions (pin mapping, peripheral, power sequence) break BSP patches and toolchains; compatibility across revisions not guaranteed; software must track hardware revision; field hardware varies.
- **Root cause:** Hardware/software coupling; lack of revision-aware BSP; patch-based customisation.
- **Contributing factors:** Multiple hardware variants; vendor release cycles.
- **Technical limitations:** BSP portability; revision detection.
- **Human skill limitations:** Configuration management.
- **Tool limitations:** Version control; diff/merge.
- **Process limitations:** No revision-aware CI.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** Promwad 2025 (hardware revision pin mapping change broke BSP patch after 3 weeks).
- **Downstream effects:** Integration delays; production transfer risk; field service.
- **Existing solutions:** Revision-aware BSP; CI validation; modular hardware.
- **Why insufficient:** Automotive safety/tool qualification; hardware variants.
- **Automation feasibility:** Medium.
- **AI applicability:** Low–Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - Promwad 2025 (above).
  - W4-005 (config diff/merge).
- **Confidence:** Medium–High.
- **Unanswered questions:** Cost of hardware-revision software rework.

### G1-032 — HIL hardware dependencies: availability, fidelity, sensor/actuator interfaces

- **Domain:** HIL / hardware dependencies
- **Lifecycle stage:** Testing, validation
- **Problem:** HIL validation depends on physical rigs, sensor/actuator fidelity, and hardware availability; chip/ECU availability gates software validation; HIL rigs are scarce/expensive; sensor/actuator physical constraints and latency; fixture verification hard.
- **Root cause:** Physical hardware dependency; HIL cost/scalability; sensor/actuator fidelity.
- **Contributing factors:** Chip shortages; HIL scheduling; model fidelity.
- **Technical limitations:** Sensor/actuator latency; physical constraints; model simplification.
- **Human skill limitations:** HIL engineering expertise (W3-038).
- **Tool limitations:** HIL rig cost/scheduling (W3-010/016); W4-018.
- **Process limitations:** Late HIL availability; no shift-left.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** W3-010/011/013/014; W4-018.
- **Downstream effects:** Validation delays; coverage gaps; late defect discovery.
- **Existing solutions:** vECU/SIL; cloud HIL; model fidelity improvements.
- **Why insufficient:** vECU fidelity gaps (W3-004, W4-019); cloud HIL latency/cyber/compliance (W3-015).
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (intelligent HIL automation).
- **Human approval:** Required.
- **Evidence/sources:**
  - W3-010–W3-016, W4-018, W4-019.
  - NI Automotive Journal (HIL world challenges).
- **Confidence:** High.
- **Unanswered questions:** HIL utilisation rates; cost per HIL hour.

### G1-033 — Hardware/software co-configuration complexity (variant binding)

- **Domain:** Hardware/software co-configuration
- **Lifecycle stage:** Configuration, release
- **Problem:** Hardware and software configurations are interdependent but managed separately; variant binding (design/run-time) complex; version constraints (semver-like) in automotive context; configuration systems hardware-centric.
- **Root cause:** Increasing software functionality on hardware; config systems adapted from hardware-only; close connection not reflected in config systems.
- **Contributing factors:** Variant explosion; OTA/subscription features; retrofit.
- **Technical limitations:** No unified HW/SW config ontology; binding-time confusion.
- **Human skill limitations:** Configuration engineering.
- **Tool limitations:** Config tools (W4-003/005).
- **Process limitations:** No end-to-end config validation.
- **Frequency/severity:** Growing.
- **Time/cost impact:** Jost & Sinz 2024 (hardware/software co-configuration challenges; Mercedes-Benz).
- **Downstream effects:** Misconfiguration; invalid variants; release delays.
- **Existing solutions:** Ontologies for function distribution (research); config management tools.
- **Why insufficient:** Research-stage; tool-specific.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - Jost & Sinz 2024 (CEUR Vol-3812). https://ceur-ws.org/Vol-3812/paper2.pdf
  - W1-011, W2-011.
- **Confidence:** Medium.
- **Unanswered questions:** Rate of co-configuration errors in production.

### G1-034 — Semiconductor talent shortage (design, process, test)

- **Domain:** Semiconductor workforce
- **Lifecycle stage:** All
- **Problem:** Aging workforce, declining STEM enrolment, perception gap (chip firms rank lower than software employers on pay/flexibility/learning), leadership pipeline gaps; projected 23,000 chip designer shortage by 2030 (US); second/third-line leaders scarce.
- **Root cause:** Demographics; competition with software industry; education pipeline.
- **Contributing factors:** Geopolitical self-reliance pressures; demand growth.
- **Technical limitations:** N/A.
- **Human skill limitations:** Core issue.
- **Tool limitations:** N/A.
- **Process limitations:** Recruiting/retention practices.
- **Frequency/severity:** Structural.
- **Time/cost impact:** Sourceability 2025 (widening talent gap); Keysight 2025 (23,000 shortfall by 2030).
- **Downstream effects:** Capacity constraints; quality risk; slower innovation.
- **Existing solutions:** Reskilling; global CoEs; industry-academia partnerships.
- **Why insufficient:** Perception gap persists; training takes years.
- **Automation feasibility:** Low (human).
- **AI applicability:** Medium (AI-assisted design).
- **Human approval:** Required.
- **Evidence/sources:**
  - Sourceability 2025 (semiconductor talent crisis). https://sourceability.com/post/overcoming-semiconductor-talent-challenges
  - Keysight 2025 (23,000 shortfall). https://www.keysight.com/blogs/en/tech/sim-des/why-semiconductor-design-teams-can-t-afford-disconnected-workflows
- **Confidence:** Medium.
- **Unanswered questions:** Automotive-specific semiconductor talent gap quantification.

### G1-035 — Hardware-in-the-loop dependency on ECU/hardware availability

- **Domain:** HIL / hardware dependencies
- **Lifecycle stage:** Validation
- **Problem:** HIL and vehicle testing depend on physical ECU/hardware availability; chip/ECU shortages delay software validation; HIL rigs are shared scarce resources; late hardware arrival compresses validation.
- **Root cause:** Physical hardware dependency; supply chain; HIL scarcity.
- **Contributing factors:** Chip shortages; ECU prototype availability; HIL scheduling.
- **Technical limitations:** vECU fidelity gaps; model accuracy.
- **Human skill limitations:** HIL operations.
- **Tool limitations:** HIL rig management (W3-016).
- **Process limitations:** Late hardware integration; no shift-left.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** W3-010/011/016; W4-018.
- **Downstream effects:** Validation delays; coverage gaps; late defect discovery.
- **Existing solutions:** vECU/SIL; cloud HIL; early silicon.
- **Why insufficient:** Fidelity; compliance; latency.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence/sources:**
  - W3-010, W3-011, W3-016, W4-018, W4-019.
- **Confidence:** Medium–High.
- **Unanswered questions:** Quantified delay due to hardware availability.

---

## 3. Cross-Cutting Observations

1. **Lifecycle mismatch is structural**: 15+ year vehicle life vs. <7 year semiconductor EOL drives obsolescence, LTB, redesign, and service risk; not solvable by better forecasting alone.
2. **Automotive-grade qualification is both necessary and a barrier**: AEC-Q100 ensures reliability but overburdens advanced nodes and limits component choice; mission-profile assessment is promising but not standardised.
3. **Supply chain opacity is a root cause**: multi-tier visibility gaps turn local disruptions into global production stoppages; OEM-foundry partnerships help but cover few parts.
4. **Hardware/software silos persist**: organisational separation, tool fragmentation, and BSP fragility make hardware revisions break software; HW/SW co-design is the proposed cure but blocked by culture/skills/tools.
5. **Memory is becoming the bottleneck**: compute growth (ADAS/SDV/HPC) outpaces embedded flash and DRAM; external memory bandwidth and safety are unsolved; emerging NVM (RRAM/MRAM/FRAM) not yet automotive-mature.
6. **HIL hardware dependency gates validation**: chip/ECU availability and HIL scarcity delay software validation; vECU/SIL helps but fidelity gaps remain.
7. **Chip shortages shifted from acute crisis to structural risk**: mature-node underinvestment, geopolitical concentration, and AI competition mean shortages will recur in new forms (advanced nodes/memory).
8. **Talent is a binding constraint**: semiconductor design/process/test capacity is aging and shrinking relative to demand.

---

## 4. Evidence and Source Index (selected)

- Federal Reserve note 2026 — motor vehicle production and 2021–2022 semiconductor shortages (advanced chips/memory next). https://www.federalreserve.gov/econres/notes/feds-notes/when-the-chips-went-down-motor-vehicle-production-and-the-2021-2022-semiconductor-shortages-20260928.html
- Congress CRS 2021 — semiconductor shortage constrains vehicle production. https://www.congress.gov/crs-product/IF12000
- BCG 2022 — examining the automotive semiconductor shortage. https://www.bcg.com/ja-jp/publications/2022/tracking-next-phase-automotive-semiconductor-shortage
- ScienceDirect 2025 — feature removal strategy amid shortages. https://www.sciencedirect.com/science/article/abs/pii/S0305048324001919
- ScienceDirect 2022 — systemic disruption from COVID-induced shortage. https://www.sciencedirect.com/science/article/pii/S030504832200127X
- Reuters 2025 — Nexperia chip crisis. https://www.reuters.com/business/autos-transportation/how-nexperia-chip-crisis-upended-auto-supply-chains-again-2025-11-24
- McKinsey 2022 — will supply-demand mismatch persist. https://www.mckinsey.com/industries/advanced-electronics/our-insights/will-the-supply-demand-mismatch-persist-for-automotive-semiconductors
- Infosys 2025 — automotive industry outlook. https://www.infosys.com/iki/documents/automotive-industry-outlook2025.pdf
- Automotive-IQ 2026 — managing semiconductor obsolescence in ECUs. https://www.automotive-iq.com/industry-reports/articles/bridging-the-lifecycle-gap-managing-semiconductor-obsolescence-in-automotive-ecus
- IJFMR 2025 — effective management of semiconductor obsolescence. https://www.ijfmr.com/research-paper.php?id=55624
- WIN SOURCE 2026 — managing automotive chip obsolescence. https://blog.win-source.net/electronics-expo/managing-automotive-chip-obsolescence-shifting-from-crisis-response-to-strategic-procurement
- Tieto 2020 — mastering long automotive software life cycles. https://www.tietoevry.com/globalassets/files/whitepapers/mastering_long_automotive_software_life_cycles_whitepaper.pdf
- embien AEC-Q100 guide. https://www.embien.com/technology-insights/aec-q100-automotive-ic-qualification-guide
- Silicon Labs AN1421 — AEC qualification vs automotive grade. https://www.silabs.com/documents/public/application-notes/an1421-aec-qualifications-vs-auto-grade.pdf
- ScienceDirect 2018 — mission profile vs AEC-Q100. https://www.sciencedirect.com/science/article/abs/pii/S0026271418308151
- Infineon ATS 2024 — MCU characterization hardware challenges. https://www.computer.org/csdl/proceedings-article/ats/2024/10915414/254QXDDYfhS
- U-Michigan 2024 — vital automotive semiconductor efforts. https://news.engin.umich.edu/2024/10/u-michigans-lead-role-in-vital-automotive-semiconductor-efforts
- imec ACP 2024 (Plackle) — chiplets for automotive HPC. https://www.semi.org/sites/semi.org/files/2024-11/03%20Bart%20Plackl%C3%A9.pdf
- Siemens/Fraunhofer 2024 — chiplets for automotive. https://blogs.sw.siemens.com/semiconductor-packaging/2024/09/11/user2user-2024-chiplets-for-future-automotive-application-fraunhofer
- SemiEngineering 2025 — sub-7nm ICs in automotive. https://semiengineering.com/challenges-in-using-sub-7nm-ics-in-automotive
- Synopsys 2025 — silicon and software automotive value chain. https://www.synopsys.com/blogs/chip-design/silicon-software-automotive-value-chain.html
- EDN 2025 — architectural opportunities for SDVs. https://www.edn.com/architectural-opportunities-propel-software-defined-vehicles-forward
- CMU Koopman 2010 — HW/SW codesign. https://users.ece.cmu.edu/~koopman/pubs/abdallah10_aero_auto_codesign.pdf
- Embedded Computing 2014 — HW/SW co-design. https://embeddedcomputing.com/application/automotive/effective-hardware-software-co-design-for-automotive-systems
- Infineon 2025 — companion MCU for SDVs. https://community.infineon.com/t5/Blogs/Accelerating-the-Shift-to-Software-Defined-Vehicles-The-Evolution-of-Automotive/ba-p/987091
- Fraunhofer IESE MEMTONOMY — DRAM in safety-critical automotive. https://www.iese.fraunhofer.de/en/trend/autonomous-systems/memtonomy.html
- Aal SEMICON 2025 — mission critical. https://www.semi.org/sites/semi.org/files/2025-11/Andreas_Aal.pdf
- SemiEngineering 2026 — new automotive architectures shaking up processors/memory. https://semiengineering.com/new-automotive-architectures-are-shaking-up-processors-and-memory-choices
- AllAboutCircuits 2023 — LPDDR flash. https://www.allaboutcircuits.com/industry-articles/lpddr-flash-enabling-automotive-e-e-architectures
- Infineon SEMPER X1 (via AllAboutCircuits). https://www.allaboutcircuits.com/news/targeting-automotive-infineon-spins-industrys-first-lpddr-flash-memory
- EDN Asia 2023 — LPDDR flash. https://www.ednasia.com/lpddr-flash-targets-next-gen-automotive-systems
- IEEE 2023 — 28nm RRAM in automotive MCUs. https://ieeexplore.ieee.org/iel7/10145914/10145815/10145951.pdf
- EEJournal 2025 — MRAM/FRAM/PCM predictions. https://www.eejournal.com/article/want-to-know-the-future-of-new-memories-mram-fram-pcm-tom-coughlin-and-jim-handy-make-predictions
- SAE 2006-01-1554 — MCAL on 32-bit. https://legacy.sae.org/publications/technical-papers/content/2006-01-1554/
- TI MCAL docs. https://software-dl.ti.com/C2000/docs/mcal/F29H85x/01.04.00/src/Overview_of_MCAL.html
- Promwad 2025 — BSP/toolchain break after bring-up. https://promwad.com/news/top-tools-embedded-software-development-2025
- Embitel BSP services. https://www.embitel.com/board-support-package-bsp-development-services
- AVIN BSP case study. https://www.avinsystems.com/case-studies/bsp-board-support-package-development/
- S2C automotive prototyping. https://jp.s2cinc.com/solution-application-auto
- MathWorks FPGA/ASIC paper. https://www.mathworks.com/content/dam/mathworks/conference-or-academic-paper/meeting-functional-safety-standards-on-algorithm-implementation-for-fpga-and-asic-in-a-dynamic-automotive-environment.pdf
- SAE 2014-01-0240 — FPGA-based automotive safety-critical SoC. https://legacy.sae.org/publications/technical-papers/content/2014-01-0240/
- Microchip app note — FPGA/ASIC portability. https://www.microchip.com/content/dam/mchp/documents/OTH/ApplicationNotes/ApplicationNotes/doc4412.pdf
- Xilinx WP169 — processor obsolescence. https://docs.amd.com/api/khub/documents/SRimXacG67qSXaRyh7tUXw/content
- NXP 2021 — power all ECUs. https://www.nxp.com/company/about-nxp/smarter-world-blog/BL-SAFELY-POWER-ALL-ECUS
- ADI 2019/2023 — automotive power. https://www.analog.com/en/resources/technical-articles/addressing-the-challenges-of-highvoltage-automotive-power-applications.html
- Intel 2023 — vehicle platform power management. https://www.intel.com/content/dam/www/central-libraries/us/en/documents/2023-12/vehicle-platform-power-management-standard-whitepaper.pdf
- IEEE 2024 — Axer/Kunz automotive Ethernet. https://standards.ieee.org/wp-content/uploads/2024/11/104-philp-axer-advancements-automotive-ethernet.pdf
- AUTOSAR EXP SensorInterfaces (R22-11). https://www.autosar.org/fileadmin/standards/R22-11/AP/AUTOSAR_EXP_SensorInterfaces.pdf
- Fierce Sensors — trouble with sensor interfaces. https://www.fiercesensors.com/sensors/trouble-sensor-interfaces
- EEWORLD 2025 — sensor driver development. https://en.eeworld.com.cn/news/qrs/eic707682.html
- Jost & Sinz 2024 — hardware-software co-configuration (CEUR Vol-3812). https://ceur-ws.org/Vol-3812/paper2.pdf
- Sourceability 2025 — semiconductor talent. https://sourceability.com/post/overcoming-semiconductor-talent-challenges
- Keysight 2025 — disconnected workflows / talent. https://www.keysight.com/blogs/en/tech/sim-des/why-semiconductor-design-teams-can-t-afford-disconnected-workflows
- Altium 2025 — AUTOSAR PCB design. https://resources.altium.com/p/autosar-compliance-and-automotive-pcb-design
- Altium 2025 — automotive standards for electronic components. https://resources.altium.com/p/automotive-standards-electronic-components
- NI Automotive Journal (HIL challenges).
- JEDEC 2026 — automotive memory transitions (Cichosz/GM). https://www.jedec.org/sites/default/files/1530-Karl%20Cichosz-2026-09-08-v1.pdf
- W1/W2/W3/W4/W5 reports (cross-references).

---

## 5. Unanswered Research Questions (consolidated)

1. What is the automotive-specific cost-of-change multiplier for hardware (silicon) vs. software?
2. What fraction of integration defects trace to hardware revisions / BSP breakage vs. software?
3. What is the real adoption rate of HW/SW co-design and virtual prototyping in automotive OEMs?
4. What is the cost and schedule impact of obsolescence-driven ECU redesigns?
5. What is the optimal last-time-buy sizing given 15+ year service life and demand uncertainty?
6. Will mission-profile-based qualification replace or supplement AEC-Q100?
7. When will RRAM/MRAM/FRAM reach automotive maturity for MCU code storage?
8. What is the adoption timeline for LPDDR flash in zonal architectures?
9. What is the cost-effectiveness of chiplets vs. monolithic SoCs in automotive?
10. What is the automotive-specific semiconductor talent gap and its cost?
11. What is the actual HIL utilisation and cost per hour?
12. What fraction of compliance/qualification effort is genuine safety work vs. avoidable overhead?
13. How often do timing violations surface from memory/interconnect in production?
14. What is the effectiveness of OEM-foundry direct partnerships vs. multi-sourcing?

---

## 6. Limitations of This Workstream

- Numerical figures (15M vehicles, $210B, 23,000 designer shortfall, 316 Wh/mi, 17.9%/5.4% power underestimation, 30% characterization hardware cost, 40% BSP acceleration, 70% verification share) are from secondary/cross-industry or vendor sources and are not independently verified automotive-specific data.
- No direct access to OEM proprietary data, fab capacity plans, or qualification cost breakdowns; findings rely on published studies, vendor blogs, whitepapers, and open papers.
- Tool/vendor-specific claims (TI MCAL, NXP BYLink, Infineon SEMPER X1, AVIN 40%) reflect vendor positioning; treat as indicative.
- Some sources are vendor marketing; contradictory evidence not fully resolved.
- Tier 2 (Verify) depth; Tier 3 (Auditor) would require full pagination of all sources and additional independent verification.
- Coverage of analog/mixed-signal, MEMS, power semiconductors, and sensors is partial; deeper dives needed for each.

---

*End of G1 report.*