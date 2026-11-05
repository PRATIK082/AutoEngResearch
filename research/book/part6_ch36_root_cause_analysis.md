# Part VI — Consolidated Problem Discovery

## Chapter 36 — Root-Cause Analysis

**Book:** *The State of Automotive Software Engineering*  
**Part:** VI — Consolidated Problem Discovery  
**Chapter:** 36 — Root-Cause Analysis  
**Sources:** Task.md §11 (Root-Cause Analysis); KNOWLEDGE_INDEX §5 (cross-cutting themes); Part III Chapters 14–22; Part IV Chapter 27

---

## 36.0 Chapter purpose

Part III catalogued *what* problems exist. This chapter explains *why* they exist. It applies structured root-cause methods (5 Whys, fishbone, FMEA, FTA, systems thinking, socio-technical analysis) to the problem records and distills the **root causes that recur across domains**. It is the input to Chapter 37 (cross-domain dependency mapping) and Chapter 38 (unsolved/underserved problems).

**Key principle (Task.md §11):** do not stop at surface problems. A symptom such as "integration is late" must be traced to its root (unclear system-level requirements ownership, uncontrolled interfaces, late validation), and from there to the structural conditions that produce it (organisational boundaries, tool fragmentation, skill scarcity).

---

## 36.1 Method and evidence tiers

- **Scout (Tier 1):** quick positive lookup; provisional.
- **Verify (Tier 2, default):** task-directed graph evidence, relevant trace directions, exact snippets for material claims, relevant pagination.
- **Auditor (Tier 3):** bounded-scope full verification with current generation, complete relevant pagination, both call directions and broader relationships.

This chapter is compiled from the Verify-tier evidence in the source reports (W1–W5, G1–G4) and the cross-cutting themes in KNOWLEDGE_INDEX. No new numerical values are introduced; estimates inherited from sources are flagged.

**Methods applied:**
- **5 Whys** — iterative "why" to reach a controllable root.
- **Fishbone (Ishikawa)** — group contributing factors by category: People, Process, Technology, Environment, Organisation.
- **FMEA** — failure modes, effects, causes, severity/occurrence/detection.
- **FTA** — fault trees for top-level failures (e.g., "late SOP", "field defect escape").
- **Systems thinking** — feedback loops, delays, accumulation.
- **Socio-technical analysis** — interactions between social/organisational and technical systems.

---

## 36.2 The root-cause taxonomy

Across the ~258 problem records, root causes cluster into **eight families**. Each family is a *structural* condition, not a one-off failure.

| # | Root-cause family | Representative problem IDs |
|---|---|---|
| RC-1 | **Fragmentation and non-composition** (tools, formats, data, organisations) | W4-001, W4-002, W4-005, W4-023, W4-024, W4-028, W1-007, W1-013 |
| RC-2 | **Late discovery / late integration** (problems found at the end) | W1-006, W1-015, W5-008, G1-015, G1-023, W2-012 |
| RC-3 | **Unclear ownership and governance** (requirements, interfaces, architecture) | W1-006, W1-007, W1-008, W1-017, W4-015 |
| RC-4 | **Skill and knowledge scarcity** (structural, not cyclical) | W5-001…W5-005, W5-012, W5-022, W5-023, G1-034, G3-026 |
| RC-5 | **Safety/compliance as manual, heavy process** | W4-011, W4-012, W4-013, W4-015, W4-016, W1-014, W3-025, W3-034 |
| RC-6 | **Timing and real-time unpredictability** | W1-010, W1-018, W2-004, W2-006, W2-007, W2-008, W2-013, W2-014 |
| RC-7 | **Model and simulation fidelity gaps** | W3-001, W3-004, W3-007, W3-012, W3-019, W3-035, W4-019 |
| RC-8 | **AI/certification trust gap** | W4-030, W5-016…W5-020, W5-025, W5-019 |

---

## 36.3 Root-cause chains (5 Whys) for the top failures

### 36.3.1 Late integration surprise

1. **Why is integration late?** Because defects surface only when components are combined (W1-006, W5-008).
2. **Why only at combination?** Because component-level verification passes against incomplete/assumed interfaces (W1-007, W2-012).
3. **Why incomplete interfaces?** Because requirements handed to suppliers are high-level and ambiguous (W1-001, W1-002, W1-006).
4. **Why high-level/ambiguous?** Because system-level requirements ownership is unclear between OEM and suppliers (W1-006, W1-008).
5. **Why unclear ownership?** Because organisational boundaries are contract-driven, not engineering-driven; no single owner of end-to-end system behaviour (W5-014, W4-023).

**Root cause:** system-level requirements ownership is undefined across organisational boundaries; integration is scheduled as a phase rather than continuously validated. → RC-3, RC-2.

### 36.3.2 HIL bottleneck and validation cost

1. **Why is validation the dominant cost (up to ~50%)?** Because HIL benches are scarce, expensive, and slow (W3-010, W4-018).
2. **Why scarce/expensive?** Because each bench is capital-intensive, non-virtualisable, and variant-specific (W3-010, W3-011).
3. **Why variant-specific?** Because variant explosion (hardware × market × software × calibration) multiplies bench configurations (W1-011, W2-011, W3-011).
4. **Why variant explosion unmanaged?** Because variant dimensions are managed independently with implicit binding time (W1-011, W2-011, G1-033).
5. **Why implicit binding time?** Because no governed configuration model exists; management is by spreadsheets/scripts (W4-028, W4-005).

**Root cause:** variant complexity is managed implicitly and manually, multiplying HIL configurations; no governed, machine-readable configuration model. → RC-1, RC-2.

### 36.3.3 Requirements-driven rework

1. **Why 35–45% of rework cost from requirements?** Because late changes and ambiguity cascade (W1-012).
2. **Why late changes?** Because requirements are authored in natural language without quality enforcement (W1-001, W5-006).
3. **Why no quality enforcement?** Because ALM tools store artifacts but do not enforce semantic consistency (W4-006, W4-007).
4. **Why not enforced?** Because requirements quality is treated as a writing skill, not an engineering property; tools are legacy-heavy (W4-006).
5. **Why legacy tools?** Because procurement is compliance-driven and switching costs are high (W4-006, W4-027, W4-004).

**Root cause:** requirements are treated as documents, not as governed engineering data; tools store but do not validate. → RC-1, RC-5.

### 36.3.4 AUTOSAR configuration complexity

1. **Why is AUTOSAR configuration a burden?** Because ARXML is verbose, not canonical, and merges badly (W2-001, W4-003, W4-005).
2. **Why not canonical?** Because the standard defines structure but not a canonical serialization or diff/merge (W4-002, W2-015).
3. **Why standard gaps?** Because AUTOSAR standardises interfaces/methodology, not tooling (W2-005, W4-004).
4. **Why tooling not standardised?** Because vendors differentiate on configuration tools; lock-in is the business model (W4-004, W4-027).
5. **Why accepted?** Because compliance-driven procurement and sunk cost entrench incumbent tools (W4-006, W4-027).

**Root cause:** the standard stops at the interface; implementation and tooling are vendor-controlled, and the exchange format is not canonical. → RC-1, RC-5.

### 36.3.5 Scenario coverage unmeasurability (ADAS/SOTIF)

1. **Why can't we prove ADAS safety?** Because the scenario space is unbounded and coverage is unmeasurable (W3-018, W4-017).
2. **Why unbounded?** Because ODD is specified informally and real-world variation is infinite (G2-001, G2-031).
3. **Why informal ODD?** Because requirements engineering for ML/AI (data, ODD, monitors) is not standardised (W1-004).
4. **Why not standardised?** Because ML safety (ISO/PAS 8800, SOTIF) is still evolving and tooling is immature (W4-017, W1-014).
5. **Why immature tooling?** Because the problem (unknown unsafe scenarios) is not computationally tractable with current methods (W4-017, W3-018).

**Root cause:** SOTIF requires enumerating unknown unsafe scenarios in an unbounded space; no completeness metric or scalable method exists. → RC-7, RC-5.

### 36.3.6 Talent shortage

1. **Why is there a talent shortage?** Because demand for embedded/AUTOSAR/safety/cyber/AI skills exceeds supply (W5-001…W5-005).
2. **Why demand exceeds supply?** Because SDV/centralised/AI workloads multiply skill requirements faster than education produces them (W5-021, W5-022, W5-024).
3. **Why education lags?** Because university curricula emphasise general-purpose computing; embedded/safety content is sparse (W5-022).
4. **Why sparse?** Because faculty and labs lack automotive depth; industry-academia collaboration is weak (W5-022).
5. **Why weak collaboration?** Because IP, confidentiality, and long product cycles make academic engagement costly (W5-016, W5-018).

**Root cause:** education and training pipelines are misaligned with industry needs by years; the gap is structural and self-reinforcing. → RC-4.

### 36.3.7 AI adoption barriers

1. **Why isn't AI adopted in safety-critical engineering?** Because AI outputs cannot be certified (W5-019, W4-030).
2. **Why not certifiable?** Because LLMs hallucinate and are probabilistic, while safety requires deterministic evidence (W5-017, W5-019).
3. **Why hallucination?** Because general-purpose LLMs lack automotive-specific training data and grounding (W5-018).
4. **Why no data?** Because engineering data is confidential/IP-protected and cannot be shared for training (W5-016).
5. **Why can't it be used internally?** Because no governance framework, provenance tracking, or verification gates exist (W5-020, W5-025).

**Root cause:** AI adoption is blocked by a trust/governance gap, not by capability alone; data confidentiality and hallucination are structural. → RC-8.

---

## 36.4 Fishbone (Ishikawa) — top-level failure: "integration as discovery"

**People:** integration expertise scarce (W5-008); system engineering skills gap (W5-007, W1-015); communication gaps (W5-014); single-person expertise (W5-012).

**Process:** integration scheduled as a phase, not continuous (W1-006); no interface governance (W1-007); no architecture consistency enforcement (W1-008, W1-017); compliance organised per-project (W1-014, W4-013).

**Technology:** tools fragmented (W4-001); ARXML not canonical (W4-002, W4-005); RTE contract validation weak (W2-012); timing tools don't compose (W1-010); vECU fidelity gaps (W4-019).

**Environment:** supplier/OEM toolchain mismatch (W4-023); multi-tier supply opacity (G1-004); semiconductor obsolescence (G1-007); zonal migration (W1-016).

**Organisation:** unclear system-requirements ownership (W1-006); contract-driven communication (W5-014); supplier boundaries (W4-023, G1-004).

---

## 36.5 FMEA — selected failure modes

| Failure mode | Effect | Cause (problem IDs) | S (1-10) | O | D | Notes |
|---|---|---|---|---|---|---|
| Late integration surprise | Program delay, cost overrun | W1-006, W1-007, W5-008 | 9 | 7 | 6 | Chronic; detection late |
| HIL bench scarcity | Test queue, schedule slip | W3-010, W4-018, W3-011 | 8 | 8 | 4 | Capacity, not methodology |
| Requirements ambiguity escape | Rework 35–45% | W1-001, W1-005, W5-006 | 7 | 8 | 5 | Cost largely invisible |
| ARXML drift across tools | Integration defects | W1-013, W4-002, W4-005, W2-015 | 8 | 7 | 6 | No canonical format |
| Timing violation at integration | Safety risk, redesign | W1-010, W2-004, W2-006 | 9 | 5 | 7 | Analysed, not designed |
| vECU fidelity gap | False confidence in SIL | W3-004, W4-019, W3-007 | 7 | 6 | 7 | No acceptance criteria |
| Scenario coverage gap (ADAS) | Safety case weak | W3-018, W4-017, G2-031 | 10 | 9 | 9 | Unmeasurable |
| Invalid test failures | Triage waste (up to 91%) | W3-027, W3-003 | 6 | 8 | 4 | Infrastructure reliability |
| AI hallucination in engineering | Wrong artefact, audit fail | W5-017, W5-019, W4-030 | 8 | 6 | 8 | No governance |
| Obsolescence-driven redesign | Cost, requalification | G1-007, G1-008, G1-009 | 8 | 7 | 5 | Structural, decades |
| OTA mis-targeting / bricking | Field failure, recall | G3-019, G3-001, W2-010 | 10 | 4 | 7 | High impact, low frequency |
| Knowledge loss on turnover | Productivity collapse | W5-012, W5-023 | 7 | 7 | 8 | Bus factor |

Severity/Occurrence/Detection are qualitative (1–10) judgments from the research record, not measured data.

---

## 36.6 Fault tree — top event: "field defect escape"

```
Field defect escape
├── OR: Defect not detected in V&V
│   ├── AND: Incomplete test coverage (W3-026) + time pressure (W3-026)
│   ├── OR: Model fidelity gap (W3-001, W3-012) → false pass
│   ├── OR: Scenario coverage gap (W3-018, W4-017) → untested scenario
│   ├── OR: Invalid/flaky infrastructure (W3-027, W3-003) → masked failure
│   └── OR: Horizontal traceability gap (W3-023) → unlinked XiL evidence
├── OR: Defect introduced late
│   ├── OR: Late requirement change (W1-005) → rework
│   ├── OR: Interface change unnotified (W1-007) → mismatch
│   └── OR: Variant binding error (W1-011, W2-011, G1-033)
├── OR: Defect not traceable to source
│   └── OR: Traceability gaps (W4-007, W1-003, W3-023)
└── OR: Field conditions not represented in tests
    ├── OR: Environmental/EMC/durability (W3-021)
    ├── OR: Real-world data gaps (W3-022, G3-015)
    └── OR: Connectivity/OTA edge cases (G3-002, G3-016)
```

---

## 36.7 Systems thinking — feedback loops

1. **Skill scarcity → more manual work → longer cycles → less time for training → skill scarcity.** (W5-001…, W5-013, W4-022). Reinforcing loop.
2. **Tool fragmentation → glue work → hidden dependencies → harder to change tools → fragmentation.** (W4-001, W4-028). Reinforcing loop.
3. **Variant explosion → more HIL configs → less HIL time per variant → thinner coverage → more field defects → more variants (patches).** (W1-011, W3-010, W3-026). Reinforcing loop.
4. **Compliance overhead → less engineering time → later defects → more compliance re-work.** (W4-011, W4-012, W1-012). Reinforcing loop.
5. **AI adoption blocked by data confidentiality → no automotive datasets → weak AI → no value → no investment in data governance → confidentiality persists.** (W5-016, W5-018, W5-025). Reinforcing loop.
6. **Obsolescence → last-time buys → frozen baselines → constrained software evolution → more legacy maintenance → less modernisation.** (G1-007…G1-009). Reinforcing loop.

**Balancing loops (partial):**
- Shift-left ambition (W3-031) balances late discovery, but adoption is only ~33%.
- vECU/SiL (W3-004, W4-019) balances HIL scarcity, but fidelity gaps limit trust.
- MBSE (W1-009) balances architecture inconsistency, but adoption is blocked.

---

## 36.8 Socio-technical root causes

The research record repeatedly shows that technical problems are coupled to organisational ones:

- **Organisational boundaries create technical debt.** OEM/supplier handoff (W1-006, W1-002) turns requirements into assumptions; toolchain choices per organisation (W4-023, W4-001) make data exchange manual; compliance spans organisations that cannot observe each other (W4-015).
- **Incentives misalign.** Suppliers optimise local delivery; OEMs need global consistency. Compensation disadvantage (W5-015) drives talent to tech rivals. Vendor lock-in is economically rational for vendors (W4-004, W4-027).
- **Knowledge is tacit and concentrated.** Single-person expertise (W5-012) means the organisation's capability is not its documented process. Turnover (W5-023) causes disproportionate loss.
- **Standards are both protection and overhead.** The boundary between genuine safety work and avoidable overhead is organisation-specific and poorly measured (W1-014, CQ-026). Compliance is performed by organisations that cannot fully observe their supply chains (W4-015).

---

## 36.9 Problem-to-root-cause matrix (selected)

| Problem ID | Title (short) | Primary root-cause family | Contributing families |
|---|---|---|---|
| W1-001 | Ambiguous NL requirements | RC-1 (no semantic enforcement) | RC-3, RC-4 |
| W1-006 | Incomplete specs to suppliers | RC-3 (ownership) | RC-2, RC-4 |
| W1-010 | Timing analysis unsolved | RC-6 | RC-1, RC-4 |
| W2-001 | ARXML config complexity | RC-1 (non-canonical) | RC-5 |
| W2-002 | Classic vs Adaptive interop | RC-1 (two platforms) | RC-6 |
| W2-004 | FFI on Adaptive unsolved | RC-6 | RC-7, RC-5 |
| W3-010 | HIL cost/scalability | RC-1 (variant × bench) | RC-2 |
| W3-018 | ADAS scenario coverage unknown | RC-7 (fidelity) | RC-5 |
| W3-027 | Invalid test failures (up to 91%) | RC-1 (infra reliability) | RC-2 |
| W4-001 | Toolchain fragmentation | RC-1 | RC-3, RC-4 |
| W4-004 | AUTOSAR tool lock-in | RC-1 (vendor control) | RC-5 |
| W4-007 | End-to-end traceability gaps | RC-1 (semantic gaps) | RC-3, RC-5 |
| W4-008 | Static analysis false positives | RC-1 (tool precision) | RC-5 |
| W4-011 | ISO 26262 overhead | RC-5 | RC-3 |
| W4-017 | SOTIF doesn't scale | RC-7, RC-5 | RC-6 |
| W4-018 | HIL bench scarcity | RC-1 (capital) | RC-2 |
| W4-030 | AI tooling trust | RC-8 | RC-5 |
| W5-001 | Embedded talent shortage | RC-4 | RC-8 (AI) |
| W5-012 | Knowledge silos/turnover | RC-4 (tacit) | RC-1 |
| W5-016 | AI adoption barriers (IP/confidentiality) | RC-8 | RC-3 |
| W5-017 | LLM hallucination | RC-8 | RC-7 |
| G1-007 | Service life vs semiconductor EOL | RC-1 (obsolescence) | RC-5 |
| G1-020 | MCAL complexity | RC-1 (vendor lock-in) | RC-4 |
| G2-001 | ADAS perception/ML lifecycle | RC-7 | RC-4, RC-5 |
| G2-027 | Mixed criticality/FFI | RC-6 | RC-5 |
| G3-001 | OTA campaign orchestration | RC-1 (heterogeneous fleet) | RC-3, RC-5 |
| G3-003 | Data volume overwhelms pipelines | RC-1 (legacy batch) | RC-4 |
| G3-009 | Cloud cybersecurity attack surface | RC-1 (surface expansion) | RC-4, RC-5 |
| G4-001 | SoC estimation uncertainty | RC-7 (model fidelity) | RC-5 |
| G4-004 | Thermal runaway detection latency | RC-6, RC-7 | RC-5 |

---

## 36.10 Cross-cutting root causes (consolidated)

These are the **structural** conditions that produce the symptoms across all domains:

1. **Tools do not compose.** Fragmentation and proprietary formats (RC-1) are a root cause, not a symptom. No end-to-end digital thread, no shared semantic model, no automated safety evidence exists (Part IV, Chapter 27).
2. **Integration is scheduled, not continuous.** Late discovery (RC-2) is the single most cited root cause of program delay and cost (W1-006, W5-008).
3. **Ownership is unclear across boundaries.** Requirements, interfaces, and architecture lack a single owner spanning OEM and suppliers (RC-3).
4. **Talent is structurally scarce.** The shortage is not cyclical; education and compensation lag by years (RC-4).
5. **Compliance is manual and heavy.** Safety/cybersecurity evidence is generated by hand; the genuine-safety vs avoidable-overhead boundary is unmeasured (RC-5).
6. **Timing is analysed, not designed.** Real-time predictability is treated as a check, not a constraint (RC-6).
7. **Fidelity has no standard.** Model/vECU credibility is unmeasured and unstandardised (RC-7).
8. **AI cannot be certified.** Hallucination, data confidentiality, and absence of governance block adoption (RC-8).

---

## 36.11 Why existing solutions have not fixed the root causes (bridge to Part IV)

Part IV (Chapters 23–27) shows that existing tools address *symptoms* within their domains but not the *structural* root causes:

- Commercial tools are strong in isolation, weak in combination (RC-1) (Chapter 23, 27.3.1).
- Open source is strong in infrastructure, weak in safety-critical authoring (RC-5, RC-1) (Chapter 24).
- Automation is strong in mechanical tasks, weak in semantic/safety tasks (RC-1, RC-3, RC-5) (Chapter 25).
- AI is strong in assistive tasks, blocked by RC-8 (Chapter 26).
- Standards are strong in intent, heavy in overhead (RC-5) (Chapter 18, 22).

The common thread: **the gaps are structural (organisational, economic, standardisation, skills), not incremental.** No single tool or vendor can resolve them (Chapter 27.2).

---

## 36.12 Evidence limitations

- Root-cause attributions are synthesised from problem records and cross-cutting themes; they are research hypotheses where direct measurement is absent.
- FMEA severity/occurrence/detection scores are qualitative judgements, not measured data.
- Fault-tree and systems-thinking models are analytical constructs, not validated against program data.
- No OEM-internal post-mortem data was available; attributions rely on public sources and practitioner reports.

---

## 36.13 Cross-references

- **Chapter 35 (Catalogue):** the full problem ID list.
- **Chapter 37 (Cross-Domain Dependency):** how root causes propagate across domains.
- **Chapter 38 (Unsolved/Underserved):** problems with insufficient existing solutions.
- **Part IV (Chapters 23–27):** existing solutions and their limitations.
- **Part III (Chapters 14–22):** problem investigation by theme.

---

*End of Chapter 36.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

