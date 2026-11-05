# Part VI — Consolidated Problem Discovery

## Chapter 37 — Cross-Domain Dependency Mapping

**Book:** *The State of Automotive Software Engineering*  
**Part:** VI — Consolidated Problem Discovery  
**Chapter:** 37 — Cross-Domain Dependency Mapping  
**Sources:** Task.md §15 (artefacts 1–12); KNOWLEDGE_INDEX; Part III Chapters 14–22; Part VI Chapters 35–36

---

## 37.0 Chapter purpose

Problems in automotive software engineering do not occur in isolation. A semiconductor obsolescence decision (G1-007) propagates into ECU redesign (G1-008), which propagates into BSW/MCAL re-qualification (G1-020, W2-005), which propagates into integration (W1-006), testing (W3-010), and release (W4-016). This chapter maps those dependencies so that the *blast radius* of each problem is visible. It delivers the dependency maps required by Task.md §15 (artefacts 1–8 and 11).

---

## 37.1 Map 1 — Semiconductor → Software → Vehicle dependency chain

```
Silicon / process (G1-001…G1-014)
   │
   ├── SoC/MCU architecture (G1-016, G1-024)
   │      ├── Memory wall (G1-017, G1-018, G1-019)
   │      ├── Power management (G1-025, G1-026)
   │      └── Safety island / mixed criticality (G1-016, G2-027)
   │
   ├── MCAL / BSP (G1-020, G1-021, G1-022, G1-031, G1-033)
   │      └── AUTOSAR BSW config (W2-001, W2-005, W4-004)
   │             └── SWC development (W2-012) → RTE generation → integration (W1-006)
   │
   ├── Sensor/actuator interfaces (G1-027, G1-028)
   │      └── ADAS perception (G2-001) → scenario validation (G2-031, W3-018)
   │
   ├── Obsolescence (G1-007, G1-008, G1-009)
   │      └── ECU redesign → requalification (W4-011, W4-012) → release delay
   │
   └── Hardware bring-up (G1-015, G1-023)
          └── Late integration discovery (W1-006, W5-008) → HIL (W3-010, G1-032, G1-035)
```

**Propagation rule:** a change at the silicon layer (node, memory, obsolescence) cascades through MCAL/BSP, BSW, SWC, integration, HIL, and release. There is no automated impact analysis across these layers (W4-007, W1-017).

---

## 37.2 Map 2 — OEM / Tier-1 / Tier-2 engineering interaction

```
OEM (system requirements, architecture, homologation)
   │
   ├── Tier-1 (ECU, BSW, integration, HIL, validation)
   │      ├── Tier-2 (components, sensors, actuators, MCAL)
   │      │      └── Semiconductor suppliers (silicon, MCAL, BSP)
   │      └── Tool vendors (config, test, HIL, ALM)
   │
   └── Engineering service providers (capacity, skills)
```

**Failure modes at boundaries:**
- **OEM → Tier-1:** incomplete requirements (W1-006), unconnected abstraction levels (W1-002), unstable interfaces (W1-007), architecture inconsistency (W1-008).
- **Tier-1 → Tier-2:** supply-chain opacity beyond Tier-1 (G1-004), toolchain mismatch (W4-023), cybersecurity CSMS spanning supply chain (W4-015).
- **All ↔ tool vendors:** lock-in (W4-004, W2-005), licensing/cost (W4-027), onboarding (W4-022, W5-011).
- **All ↔ semiconductor suppliers:** obsolescence (G1-007), talent shortage (G1-034), geopolitical concentration (G1-006).

**Information loss points:** system-level requirements ownership (W1-006), interface governance (W1-007), architecture consistency (W1-008, W1-017), compliance evidence across organisations (W4-015, G3-017).

---

## 37.3 Map 3 — Toolchain and data flow

```
Requirements (DOORS/Polarion/Codebeamer)  [W4-006, W4-007]
   │  (manual export/import, weak APIs)
   ▼
System/Architecture (Rhapsody/Enterprise Architect/PREEvision/SystemDesk)  [W1-009, W1-010]
   │  (model formats tool-specific; ARXML exchange lossy)
   ▼
AUTOSAR Config (DaVinci/EB tresos/SystemDesk)  [W4-004, W2-001, W2-015]
   │  (ARXML drift, round-trip friction)
   ▼
Code (C/C++, generated)  [W4-008..W4-010, W4-021]
   │
   ├── Static analysis (QAC/PRQA, Parasoft, LDRA, Polyspace)  [W4-008..W4-010]
   ├── Unit test (Tessy, VectorCAST, GoogleTest)  [W3-032, W4-029]
   │
   ▼
Build/CI (Jenkins/GitLab/GitHub Actions)  [W4-020, W4-021]
   │
   ▼
MIL/SIL (MATLAB/Simulink, vECU)  [W3-001..W3-009, W4-019]
   │
   ▼
HIL (dSPACE, ETAS)  [W3-010..W3-016, W4-018, G1-032, G1-035]
   │
   ▼
Vehicle/Proving ground  [W3-017..W3-022, G2-031]
   │
   ▼
Release/OTA/Field  [W2-010, W4-016, G3-001, G3-017, G3-018, G3-019]
```

**Glue work (W4-028)** occurs at every arrow: manual data transformation via spreadsheets/scripts. **PLM/PDM (W4-024)** sits parallel to this chain and does not understand AUTOSAR semantics.

---

## 37.4 Map 4 — Requirements-to-release traceability

```
Customer reqs → OEM reqs → System reqs → Software reqs → Design → Code → Tests → Safety case → Release → Field
     │            │           │             │            │      │      │       │           │        │
     └─ trace links (W4-007, W1-003) ─────────────────────────────────────────────────────────┘
```

**Gaps:**
- Vertical traceability (requirement → test) exists but is manual and brittle (W4-007, W1-003).
- Horizontal traceability (MIL → SIL → HIL → vehicle) is missing (W3-023).
- Safety-case traceability (ISO 26262) is maintained manually (W4-011, W4-012).
- Fleet-scale software identity (RxSWIN) is not a first-class entity (G3-017, W4-016).
- AI/ML requirements (data, ODD, monitors) are not covered by classic traceability (W1-004).

---

## 37.5 Map 5 — Integration and validation workflow

```
Component unit test (W3-032)
   → Component integration (W1-006, W2-012)
      → ECU integration (G1-015, G1-023)
         → BSW/RTE integration (W2-012, W2-005)
            → Network integration (W1-018, G2-020)
               → Multi-ECU integration (G2-014, G2-032)
                  → HIL (W3-010…W3-016, G1-032, G1-035, W4-018)
                     → Vehicle/proving ground (W3-017, W3-021, G2-031)
                        → Fleet validation (W3-022, G3-015)
```

**Bottleneck:** HIL capacity (W3-010, W4-018) gates the whole chain; setup/reconfiguration (W3-011) dominates; invalid failures (W3-027) erode throughput.

---

## 37.6 Map 6 — Human skills and competency gap matrix

| Skill domain | Problem IDs | Scarcity level | Training time |
|---|---|---|---|
| Embedded software (C/C++, RTOS) | W5-001, W5-010 | High | 12–24 months |
| AUTOSAR Classic/Adaptive | W5-002, W2-019 | High | 12–24 months |
| Functional safety (ISO 26262) | W5-003, W5-019 | High | Years |
| Cybersecurity (ISO/SAE 21434) | W5-004 | High (~25% vacancy) | Years |
| AI/ML engineering (automotive) | W5-005 | High (surge ~40%) | Years |
| Requirements engineering | W5-006, W1-015 | Medium-High | Months |
| System/architecture engineering | W5-007, W1-015 | High | Years |
| Software integration | W5-008 | High | Years (tacit) |
| Testing/validation | W5-009, W3-038 | Medium-High | Months-Years |
| Hardware/software co-design | W5-010, G1-034 | High | Years |
| Toolchain (vendor-specific) | W5-011, W4-022 | Medium | Months |
| Cloud/data engineering | G3-026 | High | Months-Years |
| SDV/centralised computing | W5-024 | High | Years |

**Cross-cutting:** knowledge silos and turnover loss (W5-012, W5-023); communication gaps (W5-014); compensation disadvantage (W5-015); education pipeline mismatch (W5-022).

---

## 37.7 Map 7 — Engineering bottleneck catalogue (time/cost)

| Bottleneck | Problem IDs | Quantified (where sourced) |
|---|---|---|
| Requirements-driven rework | W1-012, W1-001, W1-005 | 35–45% of rework cost (cross-industry, Capers Jones) |
| HIL cost/scalability | W3-010, W4-018 | ~$50k+ per bench; ~100–200 tests/day |
| Invalid test failures | W3-027 | up to 91% invalid (4 projects, ACM ICSE-SEIP 2022) |
| V&V share of dev cost | W3 (all) | up to ~50% (Boehm/Basili, cross-industry) |
| Build/compilation time | W4-021 | Long for large AUTOSAR codebases (not quantified) |
| Glue work | W4-028 | 10–30% of effort (hypothesis, unverified) |
| Onboarding time | W5-013, W4-022 | 12–24 months to component ownership (inferred) |
| Chip shortage/obsolescence | G1-005, G1-008, G1-009 | Massive production loss; feature deletion (estimates) |
| Cloud data volume | G3-003, G3-028 | up to ~25 GB/hour/vehicle; 100 PB–10 EB/month (forecasts) |
| Inadequate OTA | G3-001 | ~$900M/year US+Europe (trade-group estimate) |
| Cloud HIL latency | G3-014, W3-015 | Not quantified |
| Charging interop testing | G4-032 | Scales with EVSE variants (not quantified) |

---

## 37.8 Map 8 — AI and automation opportunity map

| AI/automation opportunity | Problem IDs addressed | Feasibility | Blockers |
|---|---|---|---|
| Requirements ambiguity detection | W1-001, W5-006 | Medium-High | Data confidentiality (W5-016), hallucination (W5-017) |
| Trace link suggestion | W1-003, W4-007, W3-023 | Medium-High | Verification of links |
| ARXML intelligence / config gen | W2-001, W4-003, W4-005 | Medium | Safety validity |
| Code review / static analysis triage | W4-008, W4-010 | Medium | False-positive validity |
| Test case generation | W3-024, W3-026 | Medium | Coverage adequacy judgement |
| Scenario generation (ADAS) | W3-018, W4-017 | Low-Medium | Safety validation (W5-019) |
| HIL scheduling optimisation | W3-010, W3-016, W4-018 | Medium-High | Bench heterogeneity |
| Defect prediction / anomaly detection | G3-021, G3-012, G4-001 | Medium-High | Data quality (G3-020) |
| Knowledge capture (RAG, KG) | W5-012, W5-013 | Medium | Document quality (W4-006) |
| Compliance Q&A (RAG) | W1-014, W4-011, W3-025 | Medium | Hallucination (W5-017), grounding |
| Build optimisation / caching | W4-021 | Medium | Generated code invalidates caches |
| OTA targeting / eligibility | G3-019, G3-001 | High | SBOM tracking (G3-017) |

**Blockers (cross-cutting):** confidentiality/IP (W5-016), hallucination (W5-017), lack of datasets (W5-018), certification gap (W5-019), explainability (W5-020), governance absence (W5-025), tooling trust (W4-030).

---

## 37.9 Map 9 — Problem-to-root-cause matrix (consolidated)

See Chapter 36 §36.9 for the full matrix. Summary of counts by root-cause family:

| Root-cause family | Problem count (approx.) | Dominant domains |
|---|---|---|
| RC-1 Fragmentation & non-composition | ~90 | W4, W1, W2, G3 |
| RC-2 Late discovery / late integration | ~40 | W1, W3, G1 |
| RC-3 Unclear ownership & governance | ~35 | W1, W4, G3 |
| RC-4 Skill & knowledge scarcity | ~45 | W5, W1, W2, W3, G1, G3 |
| RC-5 Safety/compliance manual & heavy | ~40 | W4, W3, W1, W2, G2, G4 |
| RC-6 Timing & real-time unpredictability | ~15 | W1, W2, G2, G4 |
| RC-7 Model & simulation fidelity gaps | ~25 | W3, W4, G2, G4 |
| RC-8 AI/certification trust gap | ~12 | W4, W5 |

Counts are approximate groupings; a problem may belong to multiple families.

---

## 37.10 Map 10 — Existing solution vs unresolved gap matrix (consolidated)

From Chapter 27 §27.4 (Task.md artefact 12), consolidated:

| Existing solution | Problems addressed | Gaps left (problem IDs) |
|---|---|---|
| AUTOSAR standardised BSW interfaces | BSW integration | App interfaces not standardised; ARXML drift (W1-007, W2-001, W4-002) |
| ICDs / interface specs | Interface definition | Static, drift, not enforced (W1-007, W1-008) |
| MBSE (SysML) | Architecture consistency | Adoption low; culture/skill/tool gaps (W1-009, W5-007) |
| Gateway/translation layers | Classic/Adaptive interop | Semantic translation; timing not preserved (W2-002, W2-007, W2-008) |
| vECU/SiL | Early integration | Fidelity gaps; no acceptance criteria (W3-004, W4-019, CQ-034) |
| HIL farms | Multi-ECU integration | Cost, scarcity (W3-010, W4-018) |
| Architecture review boards | Governance | Lack enforcement (W1-008, W1-017) |
| Commercial ALM/req tools | Requirements mgmt | Legacy UX; weak quality enforcement; brittle traceability (W4-006, W4-007, W1-001, W1-003) |
| Commercial AUTOSAR config tools | BSW config | Lock-in; cost; ARXML friction (W4-004, W2-001, W2-015, W4-002, W4-003, W4-005) |
| Commercial HIL/sim platforms | Validation | Scarcity; fidelity; vECU integration (W4-018, W4-019, W3-010, W3-011, W3-012, W3-015) |
| Commercial static analysis/testing | Code quality; compliance | False positives; undecidable rules; evidence aggregation (W4-008, W4-009, W4-010, W4-029) |
| Generic CI/CD | Build/test automation | Unsuitable for safety-critical embedded; qualification (W4-020, W4-021) |
| PLM/PDM | Product data mgmt | Does not understand AUTOSAR semantics (W4-024) |
| Open-source ARXML parsers | ARXML handling | Partial coverage; no semantic validation (W4-002, W4-003, W4-025) |
| Open-source AUTOSAR platforms | Reference stacks | Incomplete BSW; no certification evidence (W4-004, W4-025, W2-003, W2-004, W2-018) |
| Open scenario standards | Scenario interop | Coverage completeness unknown; sim flakiness (W3-018, W3-019, W4-017) |
| Automation (build/test/ARXML/HIL/docs) | Speed | Semantic/safety tasks remain manual (W3-024, W3-027, W3-028, W4-003, W4-005, W3-010, W3-011, W3-012, W4-018, W5-017, W5-020) |
| AI (code/test/req/RAG) | Productivity | Not safety-certified; hallucination (W5-016…W5-020, W5-017, W5-019, CQ-041) |
| Standards (ISO 26262, ASPICE, ISO/SAE 21434, UNECE R155/R156, SOTIF, MISRA/CERT) | Safety/quality/cyber | Overhead; ML gaps; safety-case maintenance; supply-chain burden (W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-017, W4-008, W4-009, W4-010) |

---

## 37.11 Evidence limitations

- Dependency maps are analytical constructs synthesised from problem records; no measured program data validates the edges.
- Counts and quantified bottlenecks are estimates from sources, not audited.
- Some dependencies (e.g., G3 cloud HIL, digital-twin value) lack primary sources (G3 §4).

---

## 37.12 Cross-references

- **Chapter 35 (Catalogue):** full ID list.
- **Chapter 36 (Root-Cause):** root-cause families RC-1…RC-8.
- **Chapter 38 (Unsolved/Underserved):** gaps with insufficient existing solutions.
- **Chapter 39 (Emerging):** future challenges.
- **Part IV (Chapters 23–27):** existing solutions and limitations.

---

*End of Chapter 37.*