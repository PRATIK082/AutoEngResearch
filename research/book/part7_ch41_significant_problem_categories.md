# Part VII — Research Conclusions

## Chapter 41 — The Most Significant Industry-Wide Problem Categories

### Summary

Across the research corpus (workstreams W1–W5 and G1–G4, 258 problem records, cross-checked in `CROSS_CHECK.md` and consolidated in `KNOWLEDGE_INDEX.md`), the same root causes recur in multiple workstreams under different IDs. The cross-check reduces the 131 workstream records to roughly 60–70 unique problems and identifies **seven cross-cutting root causes that appear in three or more workstreams** (CROSS_CHECK §5.5). This chapter names and ranks the **ten most significant industry-wide problem categories** — categories that are (a) reported in at least three distinct domains/workstreams, (b) structural rather than incidental, and (c) not solvable by any single tool or vendor. These categories are the primary input to Chapters 42–45 and, ultimately, to any future solution or investment decision.

The ten categories, in priority order, are:

1. Toolchain fragmentation, proprietary formats, and semantic non-composition (W4-001, W4-002, W4-004, W4-023, W4-024, W4-027, W4-028, W1-013, W2-005, W2-015, W3-036).
2. ARXML and configuration-data complexity (W2-001, W2-011, W2-012, W2-015, W4-002, W4-003, W4-005, W1-011, W1-013, G1-020, G1-021, G1-033, W3-011).
3. Requirements ambiguity and weak end-to-end traceability (W1-001 … W1-007, W4-006, W4-007, W3-023, W4-029, W1-012, W1-004).
4. Timing analysis and real-time predictability (unsolved) (W1-010, W1-018, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014, W2-017, G1-016, G1-017, G2-002).
5. ADAS/ADS scenario completeness and SOTIF (W3-017, W3-018, W3-019, W3-020, W4-017, G2-001, G2-031, W1-004, W3-029, W5-019, W3-022, W3-013).
6. Validation infrastructure scarcity and XiL fidelity (W3-001 … W3-016, W3-033, W3-035, W4-018, W4-019, W3-007, G1-032, G1-035, G4-031).
7. Compliance and standards administration overhead (W1-014, W3-025, W4-008, W4-009, W4-010, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-026, W4-029).
8. Semiconductor lifecycle, obsolescence, and supply chain (G1-001 … G1-009, G1-010 … G1-014, G1-018, G1-019, G1-034, G1-004, G1-007, G1-008).
9. Skills and workforce gaps (W5-001 … W5-011, W5-013, W5-015, W5-021 … W5-024, W1-015, W2-019, W3-038, W4-022, G1-034, G3-026).
10. Knowledge silos, single-person expertise, and turnover (W5-012, W5-014, W5-023, W4-022, W1-015, W2-019, W3-038).

---

### 41.1 Method and selection criteria

Each category satisfies all three of:

1. **Multi-workstream recurrence.** The underlying problem is recorded in at least three of W1–W5/G1–G4 under different IDs (CROSS_CHECK §2 lists the families).
2. **Structural, not incidental.** The problem persists across OEMs, Tier-1s, and suppliers and is tied to industry structure (standards, supply chain, architecture) rather than to a single program's execution.
3. **Not solvable by one tool or vendor.** Part IV (Chapter 27) shows that no existing solution category closes it.

Ranking reflects (a) recurrence count, (b) downstream blast radius, (c) evidence of cost/time impact, and (d) whether the problem worsens under SDV/zonal/centralised and AI adoption (Task.md §12).

---

### 41.2 Category 1 — Toolchain fragmentation, proprietary formats, and semantic non-composition

**Problem IDs:** W4-001, W4-002, W4-004, W4-023, W4-024, W4-027, W4-028, W1-013, W2-005, W2-015, W3-036, W4-005, W4-003.

**Why it is industry-wide.** The engineering toolchain is assembled from dozens of specialised tools that exchange data via file-based, often lossy transformations. No neutral, versioned, machine-readable engineering data backbone exists; vendor lock-in and proprietary project formats make switching painful (W4-001, W4-004, W4-027). ARXML is the nominal lingua franca but is not canonical (W4-002, W2-015), so tools do not understand each other's data. PLM does not understand AUTOSAR semantics (W4-024). The result is manual "glue work" (W4-028) estimated at 10–30% of engineering effort (hypothesis, W4-001) and supplier/OEM toolchain mismatch (W4-023). This is the root cause behind traceability gaps, configuration drift, and integration-as-discovery.

**Impact.** Every other category is amplified by it: requirements tools cannot enforce quality (W4-006), static analysis cannot reconcile evidence (W4-029), and AI cannot be grounded (W5-016, W5-018). Cross-check identifies toolchain fragmentation as a root cause, not a symptom (CROSS_CHECK §5.5 item 1).

**Evidence limitations.** Cost of glue work is a hypothesis (W4-001); licence costs are not public (W4-027).

---

### 41.3 Category 2 — ARXML and configuration-data complexity

**Problem IDs:** W2-001, W2-011, W2-012, W2-015, W4-002, W4-003, W4-005, W1-011, W1-013, G1-020, G1-021, G1-033, W3-011, W4-004.

**Why it is industry-wide.** AUTOSAR's engineering data is verbose, version-fragile, non-canonical XML. Configuration instead of implementation shifts effort into XML; manual authoring is error-prone; round-trip and VCS friction are severe; variant management across binding times is confusing (W2-001, W2-011, W2-012, W2-015, W4-002, W4-003, W4-005). Configuration drift between system and ECU extracts is common (W1-013). MCAL configuration adds a vendor-locked, tool-specific layer (G1-020, G1-021). HIL reconfiguration weeks are driven by variant explosion (W3-011). No workstream quantifies the end-to-end cost (CQ-013).

**Impact.** ARXML friction is the single most frequently recorded problem family (CROSS_CHECK §2.1: 8+ records across W1/W2/W4). It blocks automation, traceability, and AI adoption.

---

### 41.4 Category 3 — Requirements ambiguity and weak end-to-end traceability

**Problem IDs:** W1-001, W1-002, W1-003, W1-004, W1-005, W1-006, W1-007, W4-006, W4-007, W3-023, W4-029, W1-012, W1-017, W5-006.

**Why it is industry-wide.** Requirements are written in natural language, are ambiguous and underspecified, and are handed to suppliers incomplete; abstraction levels across OEM/Tier-1/Tier-2 are unconnected; traceability links are created late or not at all; ML/AI-specific requirements (data, ODD, monitors) are not covered by classic RE (W1-001, W1-002, W1-003, W1-004, W1-006). Traceability breaks horizontally across MIL/SIL/HIL (W3-023) and at compliance evidence (W4-007, W4-029). Late requirement changes drive 35–45% of rework (cross-industry, W1-012) and integration becomes discovery (W1-006). Requirements tools are legacy-heavy (W4-006).

**Impact.** Requirements-driven rework is one of the largest cost drivers (W1-012); scenario-coverage gaps (W3-018) trace partly to requirement ambiguity (CQ-001).

---

### 41.5 Category 4 — Timing analysis and real-time predictability (unsolved)

**Problem IDs:** W1-010, W1-018, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014, W2-017, G1-016, G1-017, G2-002.

**Why it is industry-wide.** End-to-end timing budgets are computed manually and rarely validated against runtime; Classic assumes static priority scheduling; Adaptive introduces dynamic memory and service-discovery jitter; network timing (CAN/Ethernet/SOME/IP/DDS) is analysed with tools that do not compose (W1-010, W1-018, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014). Freedom from Interference on Adaptive is not certifiable to ASIL-D (W2-004). Consolidation (zonal/centralised) increases multicore pressure while timing analysis remains unsolved (CQ-009). Memory bandwidth is a limiting factor (G1-017).

**Impact.** Timing is safety-relevant and underpins the zonal/centralised transition; if it cannot be analysed, consolidation is risky (W1 cross-cutting theme 5).

---

### 41.6 Category 5 — ADAS/ADS scenario completeness and SOTIF

**Problem IDs:** W3-017, W3-018, W3-019, W3-020, W4-017, G2-001, G2-031, W1-004, W3-029, W5-019, W3-022, W3-013.

**Why it is industry-wide.** ADAS/ADS validation depends on scenario coverage that is unmeasurable: Euro NCAP "can barely scratch the surface"; SOTIF requires handling unknown unsafe scenarios that do not scale; simulation is nondeterministic (43.6% flaky in CARLA, unverified; published "over 30%"); scenario reuse is unsafe; ML nondeterminism compounds the problem; ML safety functions at ASIL C/D are not certifiable (W3-018, W3-019, W3-020, W4-017, W5-019). ADAS is the dominant new technical risk surface and the largest single cost driver.

**Impact.** This is the problem that most directly limits autonomous-driving deployment and that no existing tool (OpenSCENARIO, OpenDRIVE) resolves (Chapter 27 matrix).

---

### 41.7 Category 6 — Validation infrastructure scarcity and XiL fidelity

**Problem IDs:** W3-001, W3-002, W3-003, W3-004, W3-005, W3-006, W3-007, W3-008, W3-009, W3-010, W3-011, W3-012, W3-013, W3-014, W3-015, W3-016, W3-033, W3-035, W4-018, W4-019, G1-032, G1-035, G4-031.

**Why it is industry-wide.** Testing accounts for up to ~50% of development cost (cross-industry, Boehm/Basili 2001); invalid test failures reach up to 91% (single company, 4 projects); HIL benches cost ~$50k+ and are scarce; vECUs promise shift-left but fidelity and integration gaps limit trust; no standard acceptance criteria for vECU evidence exist (W3-010, W3-027, W4-018, W4-019, CQ-034). Shift-left adoption is only 33% (W3-031). Cloud HIL has latency/cyber/compliance barriers (W3-015).

**Impact.** Testing is the dominant cost/schedule driver (W3 cross-cutting theme 8); HIL scarcity is a capital bottleneck.

---

### 41.8 Category 7 — Compliance and standards administration overhead

**Problem IDs:** W1-014, W3-025, W4-008, W4-009, W4-010, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-026, W4-029.

**Why it is industry-wide.** Compliance regimes (ISO 26262, ASPICE, ISO/SAE 21434, UNECE R155/R156, SOTIF, MISRA/CERT) impose documentation, traceability, and process evidence demands that are largely manual. The boundary between genuine safety work and avoidable overhead is "organisation-specific and poorly measured" (W1-014, CQ-026). Safety-case maintenance cost is unquantified (W4-012). Compliance documentation consumes 41% of time in one survey (W3-025). Standards evolve and fragment regionally (W4-026). Static analysis false positives and undecidable rules add manual deviation effort (W4-008, W4-010).

**Impact.** Compliance is both genuine safety work and avoidable overhead; without measurement, improvement is blind.

---

### 41.9 Category 8 — Semiconductor lifecycle, obsolescence, and supply chain

**Problem IDs:** G1-001, G1-002, G1-003, G1-004, G1-005, G1-006, G1-007, G1-008, G1-009, G1-010, G1-011, G1-012, G1-013, G1-014, G1-018, G1-019, G1-034.

**Why it is industry-wide.** Vehicle service life (15+ years) exceeds semiconductor EOL (<7 years), forcing last-time buys, ECU redesign, and requalification (G1-007, G1-008, G1-009). Supply-chain opacity beyond Tier-1 caused 15M vehicles cancelled and $210B loss (G1-004, G1-005). Mature-node underinvestment and geopolitical concentration are structural (G1-003, G1-006). Qualification burden (AEC-Q100) and advanced-node reliability challenge adoption (G1-010, G1-011, G1-013). Memory wall and embedded-flash scalability limit feature growth (G1-017, G1-018, G1-019). Chip shortages recur (Nexperia 2025).

**Impact.** Obsolescence propagates for decades and freezes hardware baselines that constrain software evolution.

---

### 41.10 Category 9 — Skills and workforce gaps

**Problem IDs:** W5-001, W5-002, W5-003, W5-004, W5-005, W5-006, W5-007, W5-008, W5-009, W5-010, W5-011, W5-013, W5-015, W5-021, W5-022, W5-023, W5-024, W1-015, W2-019, W3-038, W4-022, G1-034, G3-026.

**Why it is industry-wide.** Shortages are structural across embedded, AUTOSAR, safety, cybersecurity, AI/ML, systems, integration, testing, HW/SW co-design, and SDV skills (W5-001 … W5-011, W5-024). Education pipeline mismatch (W5-022), aging workforce (W5-023), compensation disadvantage (W5-015), and onboarding cost (W5-013) compound it. Semiconductor design talent is also scarce (G1-034). Cloud/data skills are scarce (G3-026). Statistics are single-source and must be treated as indicative (DATA_VERIFY §3.1).

**Impact.** The workforce is the binding constraint on every transformation (Chapter 34).

---

### 41.11 Category 10 — Knowledge silos, single-person expertise, and turnover

**Problem IDs:** W5-012, W5-014, W5-023, W4-022, W1-015, W2-019, W3-038.

**Why it is industry-wide.** Critical engineering knowledge resides in individuals; turnover causes loss; tacit knowledge (rationale for safety decisions, integration quirks) is rarely documented; replacement cost ranges 16–213% of annual salary (W5-012). AI knowledge capture can reduce training time by up to 40% (W5-012) but depends on document quality, which is poor. Single-person expertise creates bus-factor risk across AUTOSAR config, CANoe, and HIL (CQ-037).

**Impact.** Knowledge loss is a hidden cost that no tool currently captures at scale.

---

### 41.12 Cross-cutting root causes (the seven)

CROSS_CHECK §5.5 names seven core root causes appearing in ≥3 workstreams. They map to the ten categories above:

| # | Core root cause | Categories |
|---|---|---|
| 1 | Toolchain fragmentation and proprietary formats | 1 |
| 2 | ARXML/configuration complexity | 2 |
| 3 | Compliance administration overhead | 7 |
| 4 | Timing analysis unsolved | 4 |
| 5 | Scenario completeness unknown for ADAS/ADS | 5 |
| 6 | Skills and workforce gaps | 9 |
| 7 | Knowledge silos and turnover | 10 |

Categories 3 (requirements/traceability), 6 (validation infrastructure), and 8 (semiconductor lifecycle) are cross-cutting amplifiers that interact with all seven.

---

### 41.13 What this chapter does not claim

- It does not quantify the relative cost of each category; most figures are qualitative or single-source (CQ-071).
- It does not assert that these are the only significant problems; hidden/emerging problems are treated in Chapter 44.
- It does not propose solutions; that is out of scope (Task.md §17).

---

### 41.14 Cross-references

- **Chapter 14–22 (Part III):** problem investigation by domain.
- **Chapter 27 (Part IV):** why existing solutions do not close these categories.
- **Chapter 35–40 (Part VI):** consolidated catalogue, root-cause analysis, dependency mapping, unsolved problems, emerging challenges, consolidated findings (referenced; Part VI chapters to be written).
- **Chapter 42 (this Part):** areas with insufficient existing solutions.
- **Chapter 43 (this Part):** technology and process gaps.
- **Chapter 44 (this Part):** opportunities for further investigation.
- **Chapter 45 (this Part):** open research questions.

---

### Problem IDs referenced in this chapter

W1-001, W1-002, W1-003, W1-004, W1-005, W1-006, W1-007, W1-010, W1-011, W1-012, W1-013, W1-017, W1-018; W2-001, W2-004, W2-005, W2-006, W2-007, W2-008, W2-011, W2-012, W2-013, W2-014, W2-015, W2-017; W3-001 … W3-016, W3-017, W3-018, W3-019, W3-020, W3-022, W3-023, W3-027, W3-029, W3-031, W3-033, W3-035, W3-036; W4-001, W4-002, W4-003, W4-004, W4-005, W4-006, W4-007, W4-008, W4-009, W4-010, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-017, W4-018, W4-019, W4-023, W4-024, W4-026, W4-027, W4-028, W4-029; W5-001 … W5-015, W5-019, W5-021, W5-022, W5-023, W5-024; G1-001 … G1-009, G1-010 … G1-014, G1-016, G1-017, G1-018, G1-019, G1-020, G1-021, G1-032, G1-033, G1-034, G1-035; G2-001, G2-002, G2-031; G4-031; CQ-009, CQ-013, CQ-026, CQ-034, CQ-037, CQ-071.

---

### 41.15 Sources

- `research/CROSS_CHECK.md` (§2 overlaps, §5.5 root causes, §5.3 gaps).
- `research/KNOWLEDGE_INDEX.md` (domain index, cross-cutting themes).
- `research/W1_requirements_system.md` … `research/W5_skills_ai.md`.
- `research/G1_semiconductor_hardware.md` … `research/G4_energy_thermal.md`.
- `research/book/part4_ch27_limitations_existing_solutions.md`.
- `research/CROSS_QUESTIONS.md` (CQ-009, CQ-013, CQ-026, CQ-034, CQ-037, CQ-071).
- `Task.md` §4, §8, §11, §12, §16.

---

*End of Chapter 41.*