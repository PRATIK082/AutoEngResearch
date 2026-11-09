# Semiconductor-to-Vehicle Dependency Map

**Artefact:** #3 — Semiconductor-to-Software-to-Vehicle Dependency Map  
**Parent project:** THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING  
**Date compiled:** 2026-11-05  
**Scope:** Global automotive software industry — ICE, hybrid, EV, ADAS/ADS; Classic + Adaptive AUTOSAR; OEM/Tier-1/Tier-2/silicon perspectives.  
**Evidence basis:** Task.md §3B (Semiconductor, Silicon and Hardware Engineering) + G1 semiconductor/hardware research (35 records, G1-001..G1-035) + Part I Ch. 4 (Semiconductor-to-Vehicle Software Ecosystem) + W1–W4 cross-references.

---

## 0. How to read this map

This document maps the **dependency chain from semiconductor design and manufacturing up to the vehicle**, and shows how problems originating "below" software propagate upward. It contains:

1. **Layered dependency stack** — from silicon to vehicle, with the interface between each layer.
2. **Problem propagation paths** — how a defect/shortage/obsolescence at a lower layer cascades upward, with cost multiplication at each layer.
3. **Lifecycle mismatch** — the structural 15+ year vehicle life vs. <7 year semiconductor EOL.
4. **Supply-chain dependency** — multi-tier opacity and concentration.
5. **Hardware/software co-design silo** — the organisational root cause.
6. **Memory wall, MCAL/BSP fragility, HIL hardware dependency** — the three technical bottlenecks that most directly gate software.
7. **Problem-to-layer matrix** — G1 problem IDs mapped to the layer where they originate and the layers they affect.

Core thesis: **many software bottlenecks are actually silicon, hardware, or supply-chain bottlenecks in disguise.** Cost is usually accounted only at the layer where it surfaces, hiding the true source.

---

## 1. The dependency chain (layered stack)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  LAYER 7  VEHICLE / FIELD SERVICE                                                  │
│  15+ yr service life · OTA · recalls · warranty · obsolescence management          │
└──────────────────────────────┬──────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  LAYER 6  INTEGRATION & VALIDATION (ECU, multi-ECU, vehicle; SIL/PIL/HIL)          │
│  Testing gated by hardware/ECU availability (G1-032, G1-035, W3-010..016)         │
└──────────────────────────────┬──────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  LAYER 5  APPLICATION SOFTWARE (SWCs) & DOMAIN SOFTWARE (G2 vehicle domains)       │
│  Runs on platforms below; timing/memory/power constraints come from L2–L4          │
└──────────────────────────────┬──────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  LAYER 4  AUTOSAR BSW / RTE / MIDDLEWARE  (W2)                                    │
│  MCAL config, RTE generation, COM stacks, SOME/IP, DDS, OS scheduling              │
└──────────────────────────────┬──────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  LAYER 3  BSP / MCAL / DEVICE DRIVERS                                             │
│  Maps standardised software to specific MCU resources (G1-020..G1-022, G1-031)     │
└──────────────────────────────┬──────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  LAYER 2  ECU HARDWARE ARCHITECTURE & BOARD DESIGN                                │
│  SoC vs companion MCU, memory, power, thermal, I/O, sensor/actuator interfaces     │
│  (G1-016..G1-030)                                                                │
└──────────────────────────────┬──────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  LAYER 1  SEMICONDUCTOR DESIGN & MANUFACTURING                                    │
│  SoC/MCU/sensor silicon, fab, packaging, qualification (AEC-Q100), supply chain    │
│  (G1-001..G1-015)                                                                │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

**Interface contracts between layers:**
- L1→L2: silicon datasheet, AEC-Q100 grade, package, pinout, memory sizes, peripheral set.
- L2→L3: board schematics, power sequences, clock tree, memory map, peripheral assignments.
- L3→L4: MCAL configuration parameters, driver APIs, BSP version.
- L4→L5: RTE interfaces, BSW services, OS tasks, memory budgets.
- L5→L6: built binaries, configuration, test harnesses.
- L6→L7: released software baseline, OTA packages.

Problems can originate at any layer and propagate **upward**. A discontinuation at L1 forces ECU redesign (L2), BSP rework (L3), MCAL reconfiguration (L3/L4), software revalidation (L6), and possibly OTA re-certification (L7). Cost multiplies at each layer, but is usually accounted only where it surfaces.

---

## 2. Layer 1 — Semiconductor design & manufacturing (G1-001..G1-015)

| Problem ID | Title | Key fact |
|---|---|---|
| G1-001 | Content growth vs. capacity | Semiconductor content per vehicle grows exponentially; design cycles 2–5 yr; fab build 2+ yr. |
| G1-002 | Competition with AI/datacenter for advanced nodes & memory | OEMs compete with hyperscalers for capacity. |
| G1-003 | Mature-node fab underinvestment / reallocation | Automotive relies on mature nodes (MCUs, analog, power, sensors); fab investment skews advanced; ~50% of future fab capacity planned in mainland China. |
| G1-004 | Supply-chain opacity | OEMs/Tier-1s lack visibility beyond first-tier suppliers where semiconductors reside; no end-to-end semiconductor BOM standard. |
| G1-005 | Chip shortages & feature deletion | ~15M vehicles cancelled (2021–2022); ~$210B loss (2021); inventories 40 days → <5 days; OEMs deleted features (heated seats, etc.). |
| G1-006 | Geopolitical concentration | Packaging/assembly concentrated in Asia; export controls; Nexperia 2025 chokepoint; reshoring slow/costly. |
| G1-007 | Lifecycle mismatch / obsolescence | Vehicle 15+ yr service life vs. semiconductor EOL <7 yr; EOL notices rising; service/spares unavailable; updates blocked by hardware obsolescence. |
| G1-008 | Obsolescence-driven ECU redesign | LTB, requalify alternative, or redesign ECU/module; ECU contains 300+ unique components; requalification ~6 months for an MCU swap (2 months inventory vs. 6 months requalification, WIN SOURCE 2026). |
| G1-009 | Last-time buy risk | Large capital outlay; excess/shortage risk; demand uncertainty over 15+ yr; storage/aging; counterfeit risk in grey market. |
| G1-010 / G1-011 | AEC-Q100 qualification burden | Stress tests, temperature grades (0–3); sample sizes (2400 devices ELFR vs 1500 commercial), durations (1000h HTOL); not all functions available in all grades; grade availability gaps force design compromises; ~30% of characterisation cost is qualification hardware (Infineon ATS 2024). |
| G1-012 | Zero-defect pressure | Near-zero defects (ppm); screening (burn-in, HTOL, ELFR) cost rises; soft errors (cosmic rays, alpha particles, JESD89). |
| G1-013 | Advanced node challenges | Sub-7nm ICs face reliability challenges (aging, variation, soft errors, thermal) in automotive; physical qualification hard; virtual hardware allows pre-silicon software dev but fidelity gaps. |
| G1-014 | Chiplets | Promise to address reticle limits/cost, but automotive chiplet ecosystem lacks standards (UCIe AUTO, packaging, AEC-Q for chiplets, die-to-die, thermal); "Lego-style" plug-and-play not yet real. |
| G1-015 | Hardware/software co-design silos | OEMs historically separate HW and SW into silos; HW-centric, siloed approach → SW tightly coupled to discrete HW, long cycles (2–5 yr), little iteration; integration becomes discovery. Root cause: organisational structure, waterfall processes, lack of shared models. |

---

## 3. Layer 2 — ECU hardware architecture & board design (G1-016..G1-031)

| Problem ID | Title | Key fact |
|---|---|---|
| G1-016 | SoC vs. companion MCU / safety island | High-performance SoCs lack built-in safety mechanisms and safety documentation; companion MCUs (ASIL B–D) handle real-time/safety; "safety island" inside SoC increases complexity/cost; OS functions must be separated from real-time functions (Infineon 2025). Cross-ref W2-009, W1-016. |
| G1-017 | Memory wall | Heterogeneous multi-core and accelerators (ADAS/SDV) outpace DRAM/SRAM bandwidth; memory becomes limiting factor; DRAM safety/reliability challenges (refresh, ECC, thermal) in safety-critical use. Fraunhofer IESE MEMTONOMY. |
| G1-018 | Embedded flash (eNVM) scalability | At advanced nodes, eFlash faces high cost (die area) and lack of scalability; more flash needed for growing code size (SDV/ADAS); external memory required; emerging NVM (RRAM/FRAM/MRAM) not yet automotive-mature (EEJournal 2025: decade before supplanting Flash). |
| G1-019 | External flash bandwidth for XiP | Zonal/centralised architectures use multi-core SoCs without embedded flash; external NOR/xSPI flash too slow (peaks ~200 MHz) for real-time execute-in-place; "shadow code" to DRAM adds cost/space. LPDDR flash (Infineon SEMPER X1) emerging. |
| G1-023 | Hardware bring-up late discovery | Integration often happens late; bring-up reveals pinout, timing, power-sequence, memory-map issues; early software validation limited by hardware availability; virtual platforms help but fidelity gaps (W3-004, W4-019). AVIN case: BSP development accelerated bring-up and validation by 40% (vendor claim). |
| G1-024 | ECU hardware consolidation, thermal, power, redundancy | Consolidating functions onto zonal/central HPC introduces mixed-criticality integration, resource allocation/scheduling, redundancy/fault tolerance, thermal/power management, single point of failure. ISO 26262 discourages low-power states in safety ECUs (Intel 2023). |
| G1-025 / G1-026 | Power management complexity | Cold crank, start-stop, load dump, EMI (CISPR 25), thermal, multiple rails, sequencing; mixed discrete regulators; no vehicle-level power management standard; cumulative ~316 Wh/mi. |
| G1-027 / G1-028 | Sensor/actuator interface fragmentation | No standardisation of sensor hardware interfaces; proprietary SerDes (GMSL, FPD-Link, APIX) dominate long-reach; MIPI CSI-2 short reach; trend to Ethernet sensors (IEEE 1722). >1/3 of engineers report sensor integration as biggest challenge (Fierce Sensors). AUTOSAR sensor interfaces exclude mechanical/electrical interface and raw data level. |
| G1-029 / G1-030 | ASIC/FPGA development | Long cycles, multiple tape-outs risk, mixed-signal/analogue verification, ISO 26262 compliance, low volumes making foundry scheduling hard; verification ~70% of SoC dev (SAE 2014). FPGA-to-ASIC conversion portability pitfalls (pinout/voltage, clocking, boundary-scan, test, tool flow). |
| G1-031 | Hardware revisions and BSP breakage | Hardware revisions (pin mapping, peripheral, power sequence) break BSP patches and toolchains; compatibility across revisions not guaranteed. Promwad 2025: hardware revision pin mapping change broke BSP patch after 3 weeks. |
| G1-033 | Hardware/software co-configuration complexity | Hardware and software configurations interdependent but managed separately; variant binding (design/run-time) complex; version constraints in automotive context. Jost & Sinz 2024 (Mercedes-Benz). Cross-ref W1-011, W2-011. |

---

## 4. Layer 3 — BSP / MCAL / device drivers (G1-020..G1-022, G1-031)

| Problem ID | Title | Key fact |
|---|---|---|
| G1-020 | MCAL development complexity | MCAL must map standardised software modules to specific MCU resources; complex interaction between hardware features and standardised requirements; non-standard complex drivers (CDD) must integrate; extensive configuration/dependency space makes verification/validation hard. SAE 2006: complex interaction is big challenge. TI MCAL docs: safety strategy, CSP evidence, CDD. Vendor-specific; tool coupling; safety release only final release assessed. |
| G1-021 | MCAL vendor lock-in | MCAL delivered by MCU vendor and tied to vendor tools (EB tresos, etc.); porting to another MCU requires new MCAL and reconfiguration; configuration tool-specific; lock-in. AUTOSAR standardises interfaces not implementations. Open-source MCAL immature (W4-025). Cross-ref W2-005, W4-004. |
| G1-022 | BSP fragility | Vendor BSPs written for one eval board; patches may not apply cleanly; toolchain validated only at bring-up. |
| G1-031 | (also) Hardware revisions break BSP | above. |

**MCAL is the seam where the semiconductor meets AUTOSAR.** It is the single most vendor-locked layer in the stack: the standard (AUTOSAR) specifies the interface, but the implementation is delivered by the silicon vendor and configured in that vendor's tool. Porting silicon = reconfiguring MCAL = re-validating BSW above.

---

## 5. Layer 4 — AUTOSAR BSW / RTE / middleware (W2)

Cross-reference W2 (20 records). The semiconductor/hardware constraints from L1–L3 directly shape this layer:

| Constraint from below | Effect on AUTOSAR/middleware (W2) |
|---|---|
| Multicore SoC (L2) | Multicore distribution complexity in Classic (W2-006); fixed-priority partitioned scheduling (W2-013). |
| Memory wall / flash limits (L2: G1-017/018/019) | Code size growth vs flash; external memory bandwidth; shadow code to DRAM cost. |
| SoC safety island (L2: G1-016) | Mixed-criticality integration (W2-009 hypervisors); Adaptive ASIL-B target gap (W2-018); FFI unsolved (W2-004). |
| Sensor/actuator interfaces (L2: G1-027/028) | AUTOSAR sensor interfaces exclude mechanical/electrical/raw-data level; gateway translation (W2-002). |
| Silicon EOL (L1: G1-007) | Obsolescence forces ECU redesign → BSW/MCAL reconfiguration → revalidation. |

---

## 6. Layer 5 — Application software (SWCs) & domain software (G2)

Application software inherits all constraints from below: timing budgets from L2 scheduling, memory budgets from L2 flash/DRAM, interface contracts from L3 MCAL, platform APIs from L4. The G2 vehicle-domain problems (ADAS perception ML, powertrain real-time, body cost pressure, cockpit Adaptive/QNX) are where silicon constraints become user-visible features (or deleted features under shortage, G1-005).

---

## 7. Layer 6 — Integration & validation (G1-032, G1-035 + W3)

**HIL hardware dependency (G1-032, G1-035):** HIL validation depends on physical rigs, sensor/actuator fidelity, and hardware availability; chip/ECU availability gates software validation; HIL rigs scarce/expensive; sensor/actuator physical constraints and latency; fixture verification hard. Cross-ref W3-010–W3-016, W4-018, W4-019. vECU/SIL helps but fidelity gaps remain (W3-004, W4-019); cloud HIL has latency/cyber/compliance challenges (W3-015). **Chip shortages directly delay software validation (G1-035).**

This is the layer where silicon problems become schedule problems: even if software is ready, it cannot be validated without the target hardware or an equivalent HIL rig.

---

## 8. Layer 7 — Vehicle / field service (G1-007, G1-009 + W2-010, W4-016)

The longest stage (15+ years). Dominated by:
- **Obsolescence (G1-007):** semiconductor EOL <7 yr vs. vehicle 15+ yr → ECU redesign, LTB (G1-009), service/spare unavailability, blocked updates.
- **OTA (W2-010, W4-016, W6):** update engineering across heterogeneous ECU landscape; safe-state activation; rollback; bootloader (out of AUTOSAR scope). Bootloader & flash problems expanded in W6 (W6-001..W6-030): no standardized bootloader, secure boot, memory layout, UDS flash protocol, power transients, recovery/brick prevention, testing, manufacturing/EOL, toolchain, OTA orchestration.
- **Recalls / warranty:** safety-critical; zero-defect pressure from L1 (G1-012) lands here.

---

## 9. The structural lifecycle mismatch (the defining problem)

```
Vehicle engineering life:        ████████████████████████████████████████  15+ years
Semiconductor design cycle:      ████████                                 2–5 years
Semiconductor EOL:               ███████████████                          <7 years
AEC-Q100 qualification:          ███                                      months–years
ECU redesign on obsolescence:    ██████                                   ~6 months
```

**Consequences:**
1. **Obsolescence management** becomes a permanent engineering function, not an exception (G1-007, G1-008, G1-009).
2. **Last-time buys** tie up capital and create storage/aging/counterfeit risk (G1-009).
3. **Software updates** may be blocked by hardware obsolescence — a safety-critical update cannot be deployed to an EOL ECU.
4. **Long-tail service** requires maintaining build environments for decades, conflicting with toolchain evolution (W4-009 MISRA churn, W4-020 CI/CD).

This mismatch is **structural** and not solvable by better forecasting alone (cross-cutting observation #1 in Ch. 4).

---

## 10. Supply-chain dependency and opacity (G1-004, G1-005, G1-006)

```
OEM ──► Tier-1 (ECU/system supplier) ──► Tier-2 (components/sensors) ──► Semiconductor vendor ──► Foundry/Packaging
        │                                    │                              │
        └── no end-to-end semiconductor BOM ─┘                              └── ~50% future fab capacity
                                                                             in mainland China (G1-003, G1-006)
```

- **Opacity (G1-004):** during 2021–2022 many OEMs did not know which chips were in their vehicles. No end-to-end semiconductor BOM standard; PLM/ERP gaps (W4-024).
- **Shortages (G1-005):** ~15M vehicles cancelled (2021–2022); ~$210B loss (2021); inventories 40 → <5 days; feature deletion (heated seats etc.); prioritisation of high-margin models.
- **Geopolitics (G1-006):** export controls; Nexperia 2025 chokepoint; reshoring slow/costly; automotive share of capacity small.
- **Mature-node underinvestment (G1-003):** automotive relies heavily on mature nodes; fab investment skews advanced; recurring shortages in analog/MEMS/power.

**Root-cause framing:** multi-tier visibility gaps turn local disruptions into global production stoppages; OEM-foundry partnerships help but cover few parts.

---

## 11. Hardware/software co-design silo (G1-015) — the organisational root cause

OEMs historically separate hardware and software into silos. The hardware-centric, siloed approach leads to software tightly coupled to discrete hardware, long cycles (2–5 years), little iteration, requirements defined upfront, and integration becoming discovery. Root cause: organisational structure, waterfall processes, lack of shared models.

This silo is the organisational root cause behind many technical problems: **BSP fragility (G1-022/031), late integration surprises (W1-006), MCAL lock-in (G1-021), and the "integration as discovery" pattern (W1-006, W1-007, W5-008, W5-010, W5-014).**

Proposed cure (HW/SW co-design) is blocked by culture, skills, and tools — the same blockers as MBSE (W1-009).

---

## 12. The three technical bottlenecks that most directly gate software

### 12.1 Memory wall (G1-017, G1-018, G1-019)

- Compute growth (ADAS/SDV/HPC) outpaces embedded flash and DRAM.
- DRAM safety/reliability challenges (refresh, ECC, thermal) in safety-critical use.
- SRAM real-time vs. DRAM density trade-offs.
- eFlash scalability at advanced nodes (cost/die area); external memory required.
- Emerging NVM (RRAM/FRAM/MRAM) not yet automotive-mature (decade before supplanting Flash, EEJournal 2025).
- External flash bandwidth too slow for real-time execute-in-place (peaks ~200 MHz); shadow code to DRAM adds cost/space.

**Effect on software:** code size limits, memory budget constraints, performance tuning, safety argument complexity, and the push toward external memory with its own bandwidth/latency trade-offs.

### 12.2 MCAL/BSP fragility (G1-020, G1-021, G1-022, G1-031)

- MCAL development complexity (mapping standardised modules to specific MCU resources).
- Vendor lock-in (MCAL delivered by MCU vendor, tied to vendor tools).
- BSP fragility (patches don't apply cleanly across hardware revisions; toolchain validated only at bring-up).
- Hardware revisions break BSP (pin mapping changes broke BSP patch after 3 weeks, Promwad 2025).

**Effect on software:** silicon porting requires MCAL reconfiguration + BSW revalidation; hardware revisions break software builds; toolchain lock-in limits multi-sourcing.

### 12.3 HIL hardware dependency (G1-032, G1-035)

- HIL validation depends on physical rigs, sensor/actuator fidelity, hardware availability.
- Chip/ECU availability gates software validation.
- HIL rigs scarce/expensive; sensor/actuator physical constraints and latency; fixture verification hard.
- Chip shortages directly delay software validation (G1-035).

**Effect on software:** software can be "done" but unvalidated; vECU/SIL substitutes with fidelity gaps; cloud HIL has latency/cyber/compliance challenges.

---

## 13. Problem-to-layer matrix (G1 mapped to origin and affected layers)

| G1 ID | Originates at | Also affects | Cross-ref |
|---|---|---|---|
| G1-001..003 | L1 (semiconductor) | L2 (component choice), L5 (feature deletion), L7 (cost) | G1-005 |
| G1-004 | L1 (supply chain) | L6 (validation gating), L7 (production stoppage) | W4-024 |
| G1-005 | L1 (shortage) | L5 (feature deletion), L6 (validation delay), L7 (revenue) | G1-035 |
| G1-006 | L1 (geopolitics) | L1 (supply), L7 (cost) | — |
| G1-007 | L1 (lifecycle) | L2 (redesign), L3 (MCAL), L4 (BSW), L6 (revalidation), L7 (service) | G1-008, G1-009 |
| G1-008 | L1/L2 (obsolescence) | L3, L4, L6, L7 | G1-007 |
| G1-009 | L1 (LTB) | L7 (capital, storage, counterfeit) | G1-007 |
| G1-010/011 | L1 (AEC-Q100) | L2 (design compromises), L5 (feature limits) | — |
| G1-012 | L1 (zero-defect) | L6 (screening cost), L7 (recalls) | — |
| G1-013 | L1 (advanced nodes) | L2 (reliability), L5 (software limits) | W3-004 |
| G1-014 | L1 (chiplets) | L2 (packaging), L4 (interfaces) | — |
| G1-015 | Org (HW/SW silo) | L2–L5 (coupling), L6 (late integration) | W1-006, W1-007, W5-008/010/014 |
| G1-016 | L2 (SoC/safety island) | L4 (mixed-criticality), L5 (safety) | W2-009, W1-016 |
| G1-017 | L2 (memory) | L4 (code size), L5 (performance) | — |
| G1-018 | L2 (eFlash) | L4 (storage), L5 (code size) | — |
| G1-019 | L2 (external flash) | L4 (XiP), L5 (boot) | — |
| G1-020 | L3 (MCAL) | L4 (BSW config) | W2-005, W4-004 |
| G1-021 | L3 (MCAL lock-in) | L4 (portability), supply chain | W2-005, W4-004 |
| G1-022 | L3 (BSP) | L4 (build), L6 (bring-up) | G1-031 |
| G1-023 | L2/L6 (bring-up) | L5 (validation delay) | W3-004, W4-019 |
| G1-024 | L2 (consolidation) | L4 (mixed-criticality), L5 (safety) | W1-016, W2-009 |
| G1-025/026 | L2 (power) | L5 (energy), L7 (efficiency) | — |
| G1-027/028 | L2 (sensor/actuator I/O) | L4 (interfaces), L5 (sensor fusion) | — |
| G1-029/030 | L2 (ASIC/FPGA) | L4 (drivers), L6 (verification) | — |
| G1-031 | L2 (HW revisions) | L3 (BSP), L4 (build) | G1-022 |
| G1-032/035 | L6 (HIL) | L5 (validation gating) | W3-010..016, W4-018/019 |
| G1-033 | L2/L3 (HW/SW co-config) | L4 (variants), L5 | W1-011, W2-011 |
| G1-034 | L1 (semiconductor talent) | L1 (design capacity) | W5-010 |

---

## 14. Cross-cutting observations (semiconductor-to-vehicle)

1. **Lifecycle mismatch is structural:** 15+ yr vehicle life vs. <7 yr semiconductor EOL drives obsolescence, LTB, redesign, service risk; not solvable by better forecasting alone.
2. **Automotive-grade qualification is both necessary and a barrier:** AEC-Q100 ensures reliability but overburdens advanced nodes and limits component choice; mission-profile assessment promising but not standardised.
3. **Supply chain opacity is a root cause:** multi-tier visibility gaps turn local disruptions into global production stoppages; OEM-foundry partnerships help but cover few parts.
4. **Hardware/software silos persist:** organisational separation, tool fragmentation, BSP fragility make hardware revisions break software; HW/SW co-design is the proposed cure but blocked by culture/skills/tools.
5. **Memory is becoming the bottleneck:** compute growth (ADAS/SDV/HPC) outpaces embedded flash and DRAM; external memory bandwidth and safety unsolved; emerging NVM not yet automotive-mature.
6. **HIL hardware dependency gates validation:** chip/ECU availability and HIL scarcity delay software validation; vECU/SIL helps but fidelity gaps remain.
7. **Chip shortages shifted from acute crisis to structural risk:** mature-node underinvestment, geopolitical concentration, and AI competition mean shortages will recur in new forms (advanced nodes/memory).
8. **Talent is a binding constraint:** semiconductor design/process/test capacity is aging and shrinking relative to demand (G1-034); cross-ref W5 (automotive software talent) and W5-010 (HW/SW co-design knowledge gaps).

---

## 15. Sources

- Task.md §3B (Semiconductor, Silicon and Hardware Engineering) and §10 (Industry Stakeholder and Supply-Chain Analysis).
- G1 semiconductor/hardware research (35 records, G1-001..G1-035).
- Part I Ch. 4 — The Semiconductor-to-Vehicle Software Ecosystem (dependency chain, cross-cutting observations).
- W1 (Requirements & System Engineering), W2 (AUTOSAR & Middleware), W3 (Testing & Validation), W4 (Toolchain & Standards), W5 (Human Skills & AI) — cross-references.
- DOMAIN_MAP.md (domains, ECUs, networks, standards, stakeholders).

---

*End of semiconductor_to_vehicle_dependency.md — Semiconductor-to-Vehicle Dependency Map (Artefact #3).*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

