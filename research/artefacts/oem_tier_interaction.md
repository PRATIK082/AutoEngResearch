# OEM–Tier-1–Tier-2 Engineering Interaction Map

**Artefact:** #4 — OEM–Tier-1–Tier-2 Engineering Interaction Map  
**Parent project:** THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING  
**Date compiled:** 2026-11-05  
**Scope:** Global automotive software industry — OEM, Tier-1, Tier-2, semiconductor, ECU manufacturer, software/tool vendor, ESP, testing service provider perspectives.  
**Evidence basis:** Task.md §3A/§10 (stakeholders, supply chain) + W1 (requirements/system engineering) + W4 (toolchain/supplier friction) + DOMAIN_MAP.md §6 (stakeholder map) + Part I/II cross-references.

---

## 0. How to read this map

This document maps how engineering work, information, accountability, and ownership flow between the organisations that build automotive software. It contains:

1. **Stakeholder role definitions** — who does what.
2. **Engineering interaction flows** — the end-to-end chain from OEM need to supplier delivery.
3. **Information exchange by lifecycle stage** — what artefacts cross organisational boundaries.
4. **Handoff friction points** — where information, accountability, and ownership get lost.
5. **RACI-style responsibility matrix** — who is Responsible/Accountable/Consulted/Informed for key activities.
6. **Problem-to-interaction mapping** — W-series problem IDs tied to cross-org friction.
7. **Supply-chain dependency on semiconductors** — where silicon enters the chain.

Core thesis: **automotive software is not built by one organisation; it is assembled across a multi-tier chain where abstraction levels, tools, and standards do not line up, and where "integration as discovery" is the default outcome of incomplete handoffs.**

---

## 1. Stakeholder role definitions

| Stakeholder | Role in software | Primary objectives | Key software assets owned |
|---|---|---|---|
| **OEM (Original Equipment Manufacturer)** | Vehicle owner, system integrator, architecture authority | Time-to-market, cost, compliance, platform strategy, brand differentiation | Vehicle architecture, system requirements, variant strategy, final software baseline, homologation |
| **Tier-1 supplier** | ECU/system supplier; often full software stack for a domain (ADAS, powertrain, body, cockpit) | Requirements completeness, tooling, integration, margins, reuse across OEMs | Domain ECU software, BSW config, application SW, integration test results |
| **Tier-2 supplier** | Component/subsystem supplier (sensors, actuators, semiconductors, ECUs) | Interface specs, volume, qualification, cost | Component firmware, sensor/actuator interfaces, MCU/SoC silicon |
| **Semiconductor manufacturer** | SoC/MCU/sensor silicon provider | Qualification, software enablement (MCAL/BSP), supply, design wins | Silicon, MCAL, BSP, reference drivers, AEC-Q100 qualification |
| **ECU manufacturer** | Hardware manufacturing | Cost, quality, test coverage, yield | PCB, mechanical, EOL test |
| **Software vendor / tool vendor** | AUTOSAR stacks, middleware, tools | Licensing, standards evolution, lock-in, support | BSW products, configurators, IDEs, test tools |
| **Engineering service provider (ESP)** | Contract engineering, integration, testing | Capacity, skill availability, tool access, utilisation | Labour, test benches, integration labs |
| **Testing service provider** | HIL, vehicle, homologation testing | Capacity, equipment, scenario coverage, accreditation | Test benches, tracks, scenario databases |
| **Automotive startup / EV manufacturer** | New entrant, SDV-first | Cost (tools), talent, speed, differentiation | Full vehicle software stack (often greenfield) |
| **Autonomous driving company** | AD software, sensor stacks | Safety case, data, compute, talent, ODD | Perception/planning/control, ML models, data pipelines |
| **EV manufacturer (non-traditional)** | Software-centric vehicle platform | Speed, OTA, UX, cost | Centralised compute platform, OTA infra |

---

## 2. The engineering interaction chain (end-to-end)

```
┌──────────────┐   needs/features/regulations   ┌──────────────┐
│   Market /   │ ─────────────────────────────► │     OEM      │
│  Regulators  │                                │ (architecture │
└──────────────┘                                │  authority)   │
                                                └──────┬───────┘
                                                       │ system requirements
                                                       │ (often incomplete, high-level)
                                                       ▼
                                                ┌──────────────┐
                                                │  Tier-1      │  ◄──►  Tier-2 (sensors/actuators/
                                                │  (domain ECU │         MCUs/semiconductors)
                                                │  supplier)   │
                                                └──────┬───────┘
                                                       │ component specs, MCAL/BSP
                                                       ▼
                                                ┌──────────────┐
                                                │ Semiconductor│
                                                │  vendor      │
                                                └──────┬───────┘
                                                       │ silicon + MCAL/BSP
                                                       ▼
                                                ┌──────────────┐
                                                │  Tier-1      │  (integrates SW on ECU)
                                                └──────┬───────┘
                                                       │ ECU software delivery
                                                       ▼
                                                ┌──────────────┐
                                                │     OEM      │  (integrates multi-ECU,
                                                │  (vehicle    │   vehicle validation,
                                                │  integrator) │   homologation)
                                                └──────┬───────┘
                                                       │ released baseline
                                                       ▼
                                                ┌──────────────┐
                                                │  Field /     │
                                                │  OTA / Svc   │
                                                └──────────────┘
```

**Feedback loops** run upward from every stage: defects found at vehicle validation (OEM) must propagate back to Tier-1 and Tier-2; field defects propagate all the way to semiconductor (obsolescence) and back to OEM (recall).

---

## 3. Information exchange by lifecycle stage

| Lifecycle stage | OEM → Tier-1 | Tier-1 → OEM | Tier-1 ↔ Tier-2 / Silicon | Key friction |
|---|---|---|---|---|
| Requirements (Stage 1) | High-level specs, feature requirements, regulations, safety goals | Clarifications, assumption logs, derived requirements | Component requirements, interface specs | Specs incomplete; suppliers fill gaps with assumptions (W1-006); abstraction mismatch (W1-002); late changes (W1-005) |
| System architecture (Stage 2) | System architecture, ECU allocation, network topology, ICDs, timing budgets | Domain architecture, feasibility, constraints | Component constraints, MCAL capabilities | No single source of truth for interfaces (W1-007); architecture inconsistency (W1-008); timing unsolved (W1-010) |
| Software design/dev (Stage 3) | Software requirements, coding standards, safety requirements | Software design, code, unit tests, static analysis | MCAL/BSP, drivers, component firmware | Vendor lock-in (W2-005, G1-021); skill gaps (W5-001, W5-002) |
| Configuration (Stage 4) | System extracts (ARXML), variant definitions | ECU configuration (ARXML), RTE, BSW config | MCAL config | ARXML drift/version mismatch (W1-013, W4-002); variant explosion (W1-011, W2-011) |
| Integration (Stage 5) | Integration plan, test environments | Integrated ECU SW, integration test results, defect reports | Component integration support | Integration becomes discovery (W1-006); interface governance gaps (W1-007); Classic/Adaptive interop (W2-002) |
| Testing (Stage 6) | Test requirements, scenarios, compliance evidence needs | Test results, coverage, defect reports | Component test results | HIL scarcity (W4-018); traceability gaps (W3-023, W4-007); vECU fidelity (W4-019) |
| Vehicle validation (Stage 7) | Validation plan, scenarios, homologation requirements | Vehicle-level findings, defect reports | — | Proving-ground scope explosion (W3-017); ADAS scenario coverage (W3-018) |
| Compliance (Stage 8) | Safety/cybersecurity requirements, assessment criteria | Safety case, TARA, compliance evidence | Qualification evidence (AEC-Q100) | Compliance overhead (W1-014, W4-011); UNECE R155 supply-chain cascade (W4-015) |
| Release (Stage 9) | Release approval, baselines | Released ECU SW, manufacturing data | — | Build/CI gaps (W4-020); OTA engineering (W4-016) |
| Field (Stage 10) | Field monitoring data, defect reports | Field defect analysis, service bulletins | Obsolescence notices (G1-007) | Lifecycle mismatch (G1-007); recall management |

---

## 4. Handoff friction points (where information/accountability/ownership get lost)

### 4.1 Requirements handoff (OEM → Tier-1)

- **Problem:** OEMs deliver high-level specs; suppliers fill gaps with assumptions; interactions between functions not specified (W1-006).
- **Consequence:** Integration becomes discovery; late defects; rework.
- **Root cause:** OEM capacity/time; unclear ownership of system-level requirements; inside-out component design (W1-006).
- **Ownership gap:** System-level requirements ownership is unclear between OEM and Tier-1 (W1-006); no authority to reject architecture-breaking changes (W1-005, W1-008).

### 4.2 Interface handoff

- **Problem:** Interface definitions ambiguous; suppliers may not receive updates; no single source of truth (W1-007).
- **Consequence:** Mismatched interfaces; integration failures; over-engineering (oversized ECUs, redundant sensors) (W1-007).
- **Root cause:** Distributed development by many teams; lack of interface governance authority; proprietary formats; AUTOSAR standardises BSW not application interfaces; ARXML interface descriptions drift (W1-007).

### 4.3 Architecture handoff

- **Problem:** Suppliers develop to their own architectural vision, misaligned with OEM's evolving architecture; no shared system-level requirements; no architecture governance (W1-008).
- **Consequence:** Technical debt; future cost growth; recalls.
- **Root cause:** No top-down architecture authority; late architecture definition; organisational silos (W1-008); "no authority to reject changes that break architecture" (Winner & Maurer, cited in W1-008/017).

### 4.4 Configuration handoff (ARXML)

- **Problem:** ARXML files version-specific (R4.0/R4.1/R4.2/R4.3/R20-11); tool-specific extensions; exchange fails or requires manual repair (W4-002).
- **Consequence:** Hours–days per conversion incident; invalid configurations reaching HIL.
- **Root cause:** AUTOSAR releases evolve; tool vendors lag; real projects pin to older releases; no universal migration tooling (W4-002).
- **Friction:** OEM system extract in R4.3 vs. Tier-1 BSW tool supporting R4.0 → downgrade/convert, losing/corrupting elements.

### 4.5 Toolchain handoff (OEM ↔ supplier)

- **Problem:** OEM and supplier toolchains mismatch; data exchange friction; supplier-specific workflows (W4-023).
- **Consequence:** Manual data transformation; integration friction; supplier onboarding delays.
- **Root cause:** Tool fragmentation; proprietary formats; no standardised authoring APIs; supplier heterogeneity (W4-001, W4-023).

### 4.6 Traceability handoff

- **Problem:** End-to-end traceability (req → code → tests → safety) maintained manually or partially; links break during change (W4-007).
- **Consequence:** After a late requirement change, engineers cannot quickly identify affected tests/safety cases; regression scope over- or under-estimated; audit finds dangling links (W4-007).
- **Root cause:** Tools store links in separate silos; identifiers inconsistent; manual link creation; no automated impact analysis (W4-007).

### 4.7 Change handoff

- **Problem:** Requirements change during development; suppliers may not receive updates in time; changes ignore cross-domain impacts; system-level requirements not updated after changes (W1-005, W1-017).
- **Consequence:** Rework, schedule slip, invalid test results, integration failures.
- **Root cause:** No authority to reject architecture-breaking changes; assumption-based updates without verification; lack of systems visibility; dependencies buried in disconnected legacy tools (W1-005, W1-017, Spread.ai 2025).

### 4.8 Compliance handoff (supply-chain cascade)

- **Problem:** UNECE R155 requires certified CSMS and vehicle type approval; obligations cascade to suppliers; evidence/audit readiness heavy; smaller suppliers struggle (W4-015).
- **Consequence:** Approval delays; supplier exclusion; cost increases.
- **Root cause:** Regulation makes OEM accountable for supply-chain cybersecurity; evidence requirements; limited supplier maturity (W4-015).
- **Cross-ref:** ISO/SAE 21434 integration cost (W4-014); ISO 26262 compliance overhead (W4-011).

### 4.9 Semiconductor handoff (Tier-2/silicon ↔ Tier-1/OEM)

- **Problem:** Supply-chain opacity: OEMs/Tier-1s lack visibility beyond first-tier suppliers where semiconductors reside; no end-to-end semiconductor BOM standard (G1-004).
- **Consequence:** During 2021–2022 many OEMs did not know which chips were in their vehicles; chip shortages caused ~15M vehicles cancelled (G1-005).
- **Root cause:** Multi-tier opacity; PLM/ERP gaps (W4-024); no semiconductor BOM standard.
- **Obsolescence (G1-007):** semiconductor EOL <7 yr vs. vehicle 15+ yr → ECU redesign; LTB risk (G1-009); service/spare unavailability; blocked updates.

### 4.10 Skills handoff

- **Problem:** AUTOSAR expertise gaps; toolchain knowledge gaps; communication gaps across disciplines and organisations (W5-002, W5-011, W5-014).
- **Consequence:** Dependency on individuals; quality variance; onboarding delays.
- **Root cause:** Education lags industry; vendor-specific tooling; turnover (W1-015, W5-012, W5-013, W5-023).

---

## 5. RACI-style responsibility matrix (key activities)

Legend: R = Responsible (does the work), A = Accountable (owns outcome), C = Consulted, I = Informed.

| Activity | OEM | Tier-1 | Tier-2 / Silicon | ESP | Tool vendor |
|---|---|---|---|---|---|
| Vehicle architecture authority | A/R | C | I | C | I |
| System requirements (SYRQ) | A/R | C/R | I | C | I |
| Software requirements (SWRQ) for domain ECU | C | A/R | C | R (contract) | I |
| Hardware requirements (HWRQ) | C | C | A/R (silicon) | I | I |
| ECU hardware design | I | C | A/R (Tier-2/ECU mfr) | I | I |
| MCAL / BSP delivery | I | C | A/R (silicon vendor) | I | I |
| BSW configuration (ARXML) | I | A/R | C | R (contract) | C |
| Application SWC development | C | A/R | C | R (contract) | I |
| ECU integration | C | A/R | C | R (contract) | I |
| Multi-ECU / network integration | A/R | R | C | C | I |
| Unit / component testing | I | A/R | R | R (contract) | I |
| HIL testing | C | A/R | C | R (contract) | C |
| Vehicle-level validation | A/R | C | I | C | I |
| Safety case (ISO 26262) | A | R (domain) | C | C | C |
| Cybersecurity case (ISO/SAE 21434) | A | R | C | C | C |
| Homologation / type approval | A | C | I | I | I |
| Release baseline | A/R | R | I | I | I |
| OTA campaign | A/R | C | I | C | C |
| Obsolescence management | A | R | A/R (silicon) | I | I |

**Key observation:** Accountability (A) and Responsibility (R) are frequently split across organisations with no single owner for cross-cutting concerns (architecture authority, interface governance, system-level requirements, safety case maintenance). This split is the organisational root cause behind W1-005, W1-007, W1-008, W1-017.

---

## 6. OEM vs. supplier objectives and tensions

| Dimension | OEM priority | Tier-1 priority | Tension |
|---|---|---|---|
| Cost | Minimise vehicle cost | Maximise margin, reuse across OEMs | OEM pushes cost down; Tier-1 seeks reuse/platform leverage |
| Time-to-market | Launch date | Development cost & risk | OEM late changes vs. Tier-1 stability |
| Architecture | Platform control, differentiation | Reuse, standardisation | OEM wants control; Tier-1 wants reuse |
| Tools | Standardise across suppliers | Tool of choice, existing investment | OEM mandates vs. supplier tool lock-in |
| IP | Own vehicle-level IP | Protect component IP | Sharing boundaries unclear |
| Compliance | Homologation accountability | Evidence delivery | Cascade of compliance burden (W4-015) |
| OTA / field | Vehicle-level features | ECU-level updates | Coordination of cross-ECU campaigns (W2-010) |
| Obsolescence | Long service life | Component lifecycle | Silicon EOL vs. vehicle life (G1-007) |

---

## 7. Information flow quality problems (cross-org)

| Problem | ID | Where it manifests |
|---|---|---|
| Ambiguous/underspecified requirements | W1-001 | OEM → Tier-1 handoff |
| Unconnected abstraction levels | W1-002 | OEM/Tier-1/Tier-2 |
| Traceability burden & link quality | W1-003 | All stages, all orgs |
| ML/AI requirements gaps | W1-004 | ADAS/ADS programmes |
| Late/uncontrolled changes | W1-005 | All stages |
| Incomplete specs to suppliers | W1-006 | OEM → Tier-1 |
| Unstable interfaces / governance gaps | W1-007 | Integration (Stage 5) |
| Inconsistent architecture | W1-008 | Architecture (Stage 2) |
| MBSE adoption barriers | W1-009 | All orgs |
| Timing analysis unsolved | W1-010 | Architecture/integration |
| Variant explosion | W1-011 | Configuration (Stage 4) |
| ARXML interoperability/drift | W1-013 | Configuration (Stage 4) |
| Compliance overhead/fragmentation | W1-014 | All stages |
| System skills gaps | W1-015 | All orgs |
| Zonal/central transition | W1-016 | Architecture (Stage 2) |
| Architecture change visibility | W1-017 | Architecture (Stage 2) |
| Network/timing | W1-018 | Integration (Stage 5) |
| Classic/Adaptive interop | W2-002 | Integration (Stage 5) |
| Vendor lock-in/BSW toolchain | W2-005 | Configuration (Stage 4) |
| Multicore distribution | W2-006 | Development (Stage 3) |
| SOME/IP discovery/jitter | W2-007 | Integration (Stage 5) |
| DDS determinism | W2-008 | Integration (Stage 5) |
| Hypervisor consolidation | W2-009 | Architecture (Stage 2) |
| OTA/FOTA complexity | W2-010 | Release/field (Stage 9/10) |
| Variant management | W2-011 | Configuration (Stage 4) |
| RTE contract/validation | W2-012 | Integration (Stage 5) |
| Classic OS scheduling limits | W2-013 | Development (Stage 3) |
| Adaptive dynamic memory | W2-014 | Development (Stage 3) |
| ARXML VCS friction | W2-015 | Configuration (Stage 4) |
| Classic→Adaptive migration | W2-016 | Architecture (Stage 2) |
| AP service maturity gaps | W2-017 | Development (Stage 3) |
| ASIL level gap (Adaptive) | W2-018 | Architecture (Stage 2) |
| AUTOSAR skill gaps | W2-019 | All orgs |
| Adaptive portability vs divergence | W2-020 | Development/deployment |
| Toolchain fragmentation | W4-001 | All stages |
| ARXML version/dialect | W4-002 | Configuration (Stage 4) |
| Manual ARXML editing | W4-003 | Configuration (Stage 4) |
| AUTOSAR config tool lock-in | W4-004 | Configuration (Stage 4) |
| Config diff/merge | W4-005 | Configuration (Stage 4) |
| Requirements tooling legacy | W4-006 | Requirements (Stage 1) |
| End-to-end traceability gaps | W4-007 | All stages |
| Static analysis FP/deviation | W4-008 | Development (Stage 3) |
| MISRA version churn | W4-009 | Development (Stage 3) |
| Undecidable MISRA rules | W4-010 | Verification (Stage 6) |
| ISO 26262 overhead | W4-011 | All stages |
| Safety case maintenance | W4-012 | Change management |
| ASPICE overhead | W4-013 | Process (all) |
| ISO/SAE 21434 integration | W4-014 | All stages |
| UNECE R155 CSMS cascade | W4-015 | Release (Stage 9) |
| UNECE R156/ISO 24089 OTA | W4-016 | Release/field (Stage 9/10) |
| SOTIF validation scale | W4-017 | Validation (Stage 7) |
| HIL bench scarcity | W4-018 | Validation (Stage 6) |
| vECU fidelity | W4-019 | Validation (Stage 6) |
| CI/CD suitability | W4-020 | Build (Stage 9) |
| Long build times | W4-021 | Build (Stage 9) |
| Tool onboarding/single-person expertise | W4-022 | All stages |
| Supplier/OEM toolchain mismatch | W4-023 | All stages |
| PLM/PDM vs ARXML integration | W4-024 | Data management |
| Open-source AUTOSAR immaturity | W4-025 | Toolchain |
| Standards/regional fragmentation | W4-026 | All stages |
| Tool licensing/cost | W4-027 | All stages |
| Manual data transformation (glue work) | W4-028 | All stages |
| Test tool interop/coverage evidence | W4-029 | Validation (Stage 6) |
| AI tooling trust/cert/IP | W4-030 | All stages |
| Semiconductor supply-chain opacity | G1-004 | Production (Stage 9) |
| Chip shortages/feature deletion | G1-005 | Development/validation (Stage 3/6) |
| Lifecycle mismatch/obsolescence | G1-007 | Field (Stage 10) |
| AEC-Q100 qualification | G1-010/011 | Component (Stage 2/3) |
| HW/SW co-design silos | G1-015 | All stages |
| Memory wall | G1-017 | Development (Stage 3) |
| eFlash scalability | G1-018 | Development (Stage 3) |
| External flash/XiP | G1-019 | Development/boot (Stage 3) |
| MCAL complexity/lock-in | G1-020/021 | Configuration (Stage 4) |
| BSP fragility | G1-022/031 | Build/integration (Stage 5) |
| HIL hardware dependency | G1-032/035 | Validation (Stage 6) |
| Sensor/actuator interface fragmentation | G1-027/028 | Integration (Stage 5) |
| Talent (semiconductor side) | G1-034 | Design capacity |

---

## 8. Supply-chain dependency on semiconductors (where silicon enters the chain)

```
Semiconductor vendor  ──►  Tier-2 (component/sensor)  ──►  Tier-1 (ECU/system)  ──►  OEM  ──►  Vehicle
       │                          │                            │                   │
       │  delivers: silicon,      │  delivers: sensors,         │  delivers: ECU    │  delivers: vehicle
       │  MCAL, BSP, AEC-Q100     │  actuators, MCUs            │  software,        │  software baseline
       │  qualification           │                             │  integration      │
       │                          │                             │                   │
       └── opacity: OEM/Tier-1 lack visibility beyond first tier (G1-004) ─────────────┘
       └── lifecycle mismatch: 15+ yr vehicle vs <7 yr silicon EOL (G1-007) ─────────┘
       └── shortages: 15M vehicles cancelled 2021-22 (G1-005) ────────────────────────┘
```

**Where silicon constraints land on the OEM/Tier chain:**
- **Design (Tier-1):** component choice limited by AEC-Q100 grade availability (G1-010/011); MCAL lock-in (G1-021); memory limits (G1-017/018).
- **Development (Tier-1):** BSP fragility on HW revisions (G1-022/031); memory budgets; multicore distribution (W2-006).
- **Integration (OEM):** HIL gated by ECU availability (G1-032/035); sensor interface fragmentation (G1-027/028).
- **Field (OEM):** obsolescence, LTB, blocked updates, recalls (G1-007/008/009/012).

---

## 9. Cross-cutting root causes (organisational)

1. **No single owner for system-level requirements** between OEM and suppliers → assumption-driven development (W1-006).
2. **No interface governance authority** → unstable interfaces, over-engineering (W1-007).
3. **No architecture authority with enforcement** → inconsistent architecture, technical debt (W1-008, W1-017).
4. **No single source of truth** for architecture, interfaces, or variants across orgs (W1-007, W1-017, W2-011).
5. **Tool fragmentation across orgs** → manual data transformation, drift, lock-in (W4-001, W4-023).
6. **Compliance cascade** → OEM accountable for supply-chain safety/cybersecurity; burden lands on Tier-1/2 (W4-011, W4-014, W4-015).
7. **Semiconductor opacity** → shortages and obsolescence handled reactively, not proactively (G1-004, G1-007).
8. **Skills concentrated in individuals** → single-person expertise risk, knowledge loss on turnover (W5-012, W5-023).

---

## 10. Sources

- Task.md §3A (Vehicle Engineering and Automotive Product Domains), §10 (Industry Stakeholder and Supply-Chain Analysis).
- W1 (Requirements & System Engineering), W2 (AUTOSAR & Middleware), W3 (Testing & Validation), W4 (Toolchain & Standards), W5 (Human Skills & AI).
- G1 (Semiconductor & Hardware) — G1-004, G1-005, G1-007, G1-008, G1-009, G1-010/011, G1-015, G1-020/021/022/031, G1-027/028, G1-032/035.
- DOMAIN_MAP.md §6 (Stakeholder Map) and §5 (Domain × ECU × Network × Standard cross-reference).
- Part I Ch. 4 (Semiconductor-to-Vehicle) and Part II (lifecycle chapters).

---

*End of oem_tier_interaction.md — OEM–Tier-1–Tier-2 Engineering Interaction Map (Artefact #4).*