# Industry Stakeholders and Supply-Chain Deep Dive

**Artefact:** Stakeholder analysis (Task.md §10)  
**Parent project:** THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING  
**Date compiled:** 2026-11-05  
**Scope:** Global automotive software industry — all major stakeholder classes in the software supply chain, including non-traditional entrants.  
**Evidence basis:** Task.md §3A/§10; `research/artefacts/oem_tier_interaction.md`; `research/DOMAIN_MAP.md` §6; workstreams W1–W5, G1–G4; Part III Ch. 20.  
**Problem-ID convention:** `W1-xxx` requirements/system; `W2-xxx` AUTOSAR/middleware; `W3-xxx` testing/validation; `W4-xxx` toolchain/standards; `W5-xxx` skills/AI; `G1-xxx` semiconductor/hardware; `G2-xxx` vehicle domains; `G3-xxx` cloud/fleet; `G4-xxx` energy/thermal.

---

## 0. Purpose and reading guide

This document deep-dives the stakeholders that build, integrate, test, supply, and operate automotive software. For each stakeholder it records:

1. **Objectives** — what the organisation optimises for.
2. **Processes owned** — where in the lifecycle it is responsible.
3. **Interfaces** — with whom and through which artefacts.
4. **Responsibilities** — R/A/C/I-style ownership (Responsible/Accountable/Consulted/Informed).
5. **Where ownership gets lost** — the specific cross-boundary failure modes, interlinked to problem IDs.

The core thesis (inherited from `oem_tier_interaction.md`): automotive software is not built by one organisation. It is assembled across a multi-tier chain where abstraction levels, tools, and standards do not line up, and where "integration as discovery" is the default outcome of incomplete handoffs. Ownership is most often lost not inside an organisation but **in the gaps between organisations**, where no single party is both Responsible and Accountable.

---

## 1. Stakeholder system overview

### 1.1 The engineering chain

```
Market / Regulators
        │ needs, features, regulations
        ▼
┌──────────────┐
│     OEM      │  architecture authority, system integrator, homologation owner
└──────┬───────┘
       │ system requirements, architecture, ICDs, system extracts (ARXML)
       ▼
┌──────────────┐  ◄──►  Tier-2 (sensors, actuators, MCUs, semiconductors)
│   Tier-1     │  ◄──►  ECU manufacturer (hardware)
│ (domain ECU  │  ◄──►  Software/tool vendors (BSW, middleware, tools)
│  supplier)   │  ◄──►  Engineering service providers (contract labour)
└──────┬───────┘  ◄──►  Testing service providers (HIL, vehicle, homologation)
       │ ECU software delivery, integration & test results, defect reports
       ▼
┌──────────────┐
│     OEM      │  multi-ECU integration, vehicle validation, release baseline
└──────┬───────┘
       │ released baseline
       ▼
   Field / OTA / Service  ◄──►  Cloud/fleet platforms, data pipelines
```

Feedback loops run upward at every stage: defects found at vehicle validation (OEM) must propagate to Tier-1 and Tier-2; field defects propagate all the way to semiconductor (obsolescence) and back to OEM (recall). Where those loops are slow or lossy, ownership is lost.

### 1.2 Shared artefacts that cross organisational boundaries

| Artefact | Typical owner | Consumers | Failure mode (problem IDs) |
|---|---|---|---|
| System requirements (SyRQ) | OEM | Tier-1, Tier-2, ESP | Incomplete/ambiguous (W1-001, W1-006) |
| Software requirements (SWRQ) | Tier-1 (domain) | OEM, ESP, test providers | Abstraction mismatch (W1-002) |
| Hardware requirements (HWRQ) | Tier-2 / silicon vendor | Tier-1, ECU mfr | Interface fragmentation (G1-027, G1-028) |
| Interface Control Documents (ICDs) | OEM (nominally) | Tier-1, Tier-2 | No single source of truth (W1-007) |
| System architecture / ECU allocation | OEM | Tier-1, suppliers | Inconsistent architecture (W1-008, W1-017) |
| System extracts (ARXML) | OEM | Tier-1 | Version/dialect drift (W4-002, W1-013) |
| ECU configuration (ARXML) | Tier-1 | OEM, test providers | Config diff/merge immaturity (W4-005) |
| MCAL / BSP | Silicon vendor | Tier-1, ECU mfr | Lock-in, fragility (G1-020, G1-021, G1-022, G1-031) |
| Test results / coverage evidence | Tier-1 / ESP | OEM, test providers | Traceability gaps (W4-007, W3-023) |
| Safety case (ISO 26262) | OEM (vehicle) / Tier-1 (domain) | Assessors, regulators | Maintenance cost (W4-012) |
| Cybersecurity case (ISO/SAE 21434) | OEM (accountable) / Tier-1 (responsible) | Regulators | Supply-chain cascade (W4-014, W4-015) |
| Released software baseline | OEM | Manufacturing, field/OTA | Build/CI gaps (W4-020), OTA complexity (W4-016) |

### 1.3 Recurring mechanisms by which ownership is lost

These eight mechanisms recur across every stakeholder and are the organisational root causes behind the problem IDs cited throughout:

1. **No single owner for system-level requirements** between OEM and suppliers → assumption-driven development (W1-006).
2. **No interface governance authority** → unstable interfaces, over-engineering (W1-007).
3. **No architecture authority with enforcement** → inconsistent architecture, technical debt (W1-008, W1-017).
4. **No single source of truth** for architecture, interfaces, or variants across organisations (W1-007, W1-017, W2-011).
5. **Tool fragmentation across organisations** → manual data transformation, drift, lock-in (W4-001, W4-023).
6. **Compliance cascade** → OEM accountable for supply-chain safety/cybersecurity; burden lands on Tier-1/2 (W4-011, W4-014, W4-015).
7. **Semiconductor opacity** → shortages and obsolescence handled reactively (G1-004, G1-007).
8. **Skills concentrated in individuals** → single-person expertise risk, knowledge loss on turnover (W5-012, W5-023).

---

## 2. Stakeholder deep dives

### 2.1 OEM (Original Equipment Manufacturer)

**Objectives.** Time-to-market; vehicle cost; compliance and homologation; platform strategy and reuse across model lines; brand differentiation through features; long service life and field quality; transition to SDV/zonal/centralised computing (W1-016, W5-024).

**Processes owned.** Vehicle architecture authority; system requirements (SyRQ) definition; ECU allocation and network architecture; multi-ECU and network integration; vehicle-level validation and homologation; release baseline approval; OTA campaign authority; field monitoring and defect triage; obsolescence policy; supplier governance.

**Interfaces.** Upstream: market, regulators, standards bodies. Downstream: Tier-1 (system requirements, architecture, ICDs, system extracts, test requirements, compliance criteria); Tier-2 indirectly via Tier-1; testing providers (validation plans, scenarios); tool vendors (tool mandates, data standards). Feedback: field defects and obsolescence notices flow back from Tier-1/silicon.

**Responsibilities (RACI).** Vehicle architecture: A/R. System requirements: A/R. Software requirements (domain ECU): C. Hardware requirements: C. ECU hardware design: I. MCAL/BSP: I. BSW configuration: I. Application SWC: C. ECU integration: C. Multi-ECU/network integration: A/R. Unit/component testing: I. HIL testing: C. Vehicle-level validation: A/R. Safety case (ISO 26262): A. Cybersecurity case (ISO/SAE 21434): A. Homologation/type approval: A. Release baseline: A/R. OTA campaign: A/R. Obsolescence management: A.

**Where ownership gets lost.**
- System-level requirements ownership is unclear between OEM and Tier-1; OEMs deliver high-level specs and suppliers fill gaps with assumptions (W1-006).
- Interface definitions are ambiguous and there is no single source of truth; suppliers may not receive updates (W1-007).
- Architecture is defined at OEM level but implemented at supplier level; no enforcement authority (W1-008, W1-017).
- Late, uncontrolled requirement changes ignore cross-domain impacts; system-level requirements are not updated after changes (W1-005, W1-017).
- OEMs lack visibility beyond Tier-1, so semiconductor shortages and obsolescence are handled reactively (G1-004, G1-005, G1-007).
- Compliance burden cascades: OEM is accountable for supply-chain safety and cybersecurity but cannot fully observe suppliers (W4-011, W4-014, W4-015).
- Tool mandates conflict with supplier tool lock-in; data exchange is manual (W4-023, W4-001).
- OEMs are shifting from hardware maker to platform operator faster than capabilities mature (G3-027).

**Key problem IDs.** W1-001, W1-002, W1-005, W1-006, W1-007, W1-008, W1-009, W1-010, W1-011, W1-013, W1-014, W1-015, W1-016, W1-017, W1-018, W2-010, W2-011, W4-001, W4-002, W4-007, W4-011, W4-014, W4-015, W4-016, W4-023, W4-026, W5-006, W5-007, G1-004, G1-005, G1-007, G3-008, G3-009, G3-010, G3-027.

---

### 2.2 Tier-1 supplier

**Objectives.** Requirements completeness; tooling and integration capability; margins; reuse of domain platforms across multiple OEMs; on-time delivery; compliance evidence; protection of component IP.

**Processes owned.** Domain ECU software (often the full stack for ADAS, powertrain, body, cockpit, chassis); BSW configuration; application SWC development; ECU integration; unit, component, and HIL testing; domain-level safety and cybersecurity case; ECU software release; obsolescence response; supplier management of Tier-2.

**Interfaces.** Upstream: OEM (system requirements, architecture, ICDs, system extracts, test requirements, compliance criteria). Downstream: Tier-2/silicon (component specs, MCAL/BSP, component constraints); ECU manufacturer (hardware); ESPs (contracted engineering); testing providers (HIL, vehicle). Feedback to OEM: integrated ECU software, test results, defect reports, feasibility constraints.

**Responsibilities (RACI).** Vehicle architecture: C. System requirements: C/R. Software requirements (domain ECU): A/R. Hardware requirements: C. ECU hardware design: C. MCAL/BSP delivery: C. BSW configuration (ARXML): A/R. Application SWC development: A/R. ECU integration: A/R. Multi-ECU/network integration: R. Unit/component testing: A/R. HIL testing: A/R. Vehicle-level validation: C. Safety case (domain): R. Cybersecurity case (domain): R. Homologation: C. Release baseline: R. OTA campaign: C. Obsolescence management: R.

**Where ownership gets lost.**
- Tier-1s receive incomplete specs and fill gaps with assumptions; integration then becomes discovery (W1-006).
- Each Tier-1 develops to its own architectural vision, misaligned with the OEM's evolving architecture; no shared system-level requirements (W1-008).
- Interface governance is weak; AUTOSAR standardises BSW, not application interfaces; ARXML interface descriptions drift (W1-007).
- ARXML exchange fails across AUTOSAR releases and tool-specific extensions; hours to days per conversion incident (W4-002, W1-013, W4-003, W4-005).
- Toolchain mismatch with OEMs causes manual data transformation and onboarding delays (W4-023, W4-001, W4-028).
- Classic/Adaptive interoperability, gateway translation, and migration require full communication redesign (W2-002, W2-016).
- Vendor lock-in at BSW and configuration-tool level (W2-005, W4-004, G1-021).
- Multicore distribution, SOME/IP discovery jitter, DDS determinism, hypervisor consolidation (W2-006, W2-007, W2-008, W2-009).
- OTA/FOTA background install, safe-state activation, rollback complexity (W2-010, W4-016).
- Variant management and RTE contract validation gaps (W2-011, W2-012, W1-011).
- Compliance cascade: Tier-1 delivers safety/cybersecurity evidence under UNECE R155/R156 and ISO/SAE 21434 (W4-014, W4-015, W4-011).
- HIL bench scarcity and vECU fidelity gaps gate integration (W4-018, W4-019, W3-010, W3-011).
- Build and CI/CD pipelines are unsuitable for safety-critical embedded software (W4-020, W4-021).
- Skills: AUTOSAR expertise, integration, testing, toolchain knowledge (W5-002, W5-008, W5-009, W5-011, W5-019).
- Semiconductor constraints: MCAL complexity/lock-in, BSP fragility across HW revisions, sensor interface fragmentation, HIL hardware dependency (G1-020, G1-021, G1-022, G1-027, G1-028, G1-031, G1-032, G1-035).

**Key problem IDs.** W1-002, W1-005, W1-006, W1-007, W1-008, W1-010, W1-011, W1-013, W1-014, W1-017, W2-002, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-011, W2-012, W2-013, W2-014, W2-015, W2-016, W2-017, W2-018, W2-019, W2-020, W3-004, W3-007, W3-010, W3-011, W3-012, W3-013, W3-014, W3-016, W3-023, W3-026, W3-027, W3-028, W3-032, W4-002, W4-003, W4-004, W4-005, W4-007, W4-008, W4-009, W4-010, W4-011, W4-012, W4-014, W4-015, W4-016, W4-018, W4-019, W4-020, W4-021, W4-022, W4-023, W4-028, W5-001, W5-002, W5-003, W5-004, W5-008, W5-009, W5-011, W5-012, G1-004, G1-005, G1-007, G1-010, G1-015, G1-020, G1-021, G1-022, G1-027, G1-028, G1-031, G1-032, G1-035.

---

### 2.3 Tier-2 supplier

**Objectives.** Interface specification clarity; volume and qualification; cost; component firmware quality; silicon/design wins; long-term supply agreements.

**Processes owned.** Component and subsystem supply (sensors, actuators, MCUs, semiconductors); component firmware; sensor/actuator interfaces; qualification evidence (AEC-Q100 for silicon); component-level testing; delivery logistics.

**Interfaces.** Upstream: semiconductor vendors (silicon, MCAL/BSP, qualification). Lateral: ECU manufacturer (hardware). Downstream: Tier-1 (component specs, constraints, MCAL config, component test results). Indirect: OEM (via Tier-1, through component requirements and interface specs).

**Responsibilities (RACI).** Vehicle architecture: I. System requirements: I. Software requirements: C. Hardware requirements: A/R (silicon). ECU hardware design: A/R (Tier-2/ECU mfr). MCAL/BSP delivery: A/R (silicon vendor). BSW configuration: C. Application SWC: C. ECU integration: C. Multi-ECU/network integration: C. Unit/component testing: R. HIL testing: C. Vehicle-level validation: I. Safety case: C. Cybersecurity case: C. Homologation: I. Release baseline: I. OTA: I. Obsolescence management: A/R (silicon).

**Where ownership gets lost.**
- Supply-chain opacity: OEMs and Tier-1s lack visibility beyond Tier-1, where semiconductors and Tier-2 components reside (G1-004).
- Chip shortages caused massive production loss and feature deletion; many OEMs did not know which chips were in their vehicles (G1-005).
- Geopolitical concentration of semiconductor manufacturing (G1-006).
- Lifecycle mismatch: vehicle service life (15+ years) vs. semiconductor EOL (<7 years) → ECU redesign, LTB risk, service/spare unavailability, blocked updates (G1-007, G1-008, G1-009).
- AEC-Q100 qualification burden and temperature-grade availability gaps; temperature-grade mismatch with real mission profiles (G1-010, G1-011).
- Zero-defect (ppm) pressure and screening cost (G1-012).
- Advanced-node reliability and automotive qualification challenges; chiplet standardisation gaps (G1-013, G1-014).
- Hardware/software co-design silos and late integration (G1-015).
- SoC vs. companion MCU split and safety-island complexity (G1-016).
- Memory wall, eFlash scalability, external flash/XiP limits (G1-017, G1-018, G1-019).
- MCAL development complexity and vendor lock-in; BSP fragility across hardware revisions (G1-020, G1-021, G1-022, G1-031).
- Hardware bring-up late discovery and integration surprises (G1-023).
- ECU hardware architecture: mixed-criticality consolidation, thermal, power, redundancy (G1-024, G1-025, G1-026).
- Sensor and actuator interface fragmentation and proprietary SerDes; lack of standardised hardware interfaces (G1-027, G1-028).
- ASIC/FPGA development cycles, tape-out risk, conversion portability pitfalls (G1-029, G1-030).
- HIL hardware dependencies: availability, fidelity, sensor/actuator interfaces (G1-032, G1-035).
- Hardware/software co-configuration complexity (variant binding) (G1-033).
- Semiconductor talent shortage (G1-034).
- Commercial-vehicle/off-highway and two-/three-wheeler variant and cost constraints (G2-029, G2-030).

**Key problem IDs.** G1-004, G1-005, G1-006, G1-007, G1-008, G1-009, G1-010, G1-011, G1-012, G1-013, G1-014, G1-015, G1-016, G1-017, G1-018, G1-019, G1-020, G1-021, G1-022, G1-023, G1-024, G1-025, G1-026, G1-027, G1-028, G1-029, G1-030, G1-031, G1-032, G1-033, G1-034, G1-035, G2-029, G2-030.

---

### 2.4 Semiconductor manufacturer

**Objectives.** Silicon qualification and yield; software enablement (MCAL/BSP) as a design-win enabler; supply and capacity allocation; design wins across automotive and other sectors; advanced-node competitiveness.

**Processes owned.** SoC/MCU/sensor silicon design; fabrication and packaging; AEC-Q100 qualification; MCAL and BSP development; reference drivers; hardware bring-up support; obsolescence and last-time-buy management; automotive-grade reliability screening.

**Interfaces.** Downstream: Tier-2 (component/sensor suppliers), ECU manufacturers (silicon supply), Tier-1 (MCAL/BSP, drivers). Upstream: foundries, IP providers, EDA tool vendors. Lateral: standards bodies (AEC-Q, ISO 26262 for silicon IP). Feedback: obsolescence notices to Tier-1/OEM; errata and BSP patches.

**Responsibilities (RACI).** Hardware requirements: A/R. ECU hardware design: C (silicon). MCAL/BSP delivery: A/R. BSW configuration: C. Application SWC: I. ECU integration: C. Multi-ECU/network integration: I. Unit/component testing: C (component). HIL testing: C. Vehicle-level validation: I. Safety case: C. Cybersecurity case: C. Homologation: I. Release baseline: I. OTA: I. Obsolescence management: A/R.

**Where ownership gets lost.**
- Automotive semiconductor content growth outpaces design and manufacturing capacity; SoCs compete with AI/datacenter for advanced nodes and memory (G1-001, G1-002).
- Mature-node fab underinvestment and capacity reallocation (G1-003).
- Multi-tier opacity means the silicon vendor cannot see end demand; shortages cascade (G1-004, G1-005).
- Geopolitical concentration of manufacturing (G1-006).
- Lifecycle mismatch forces semiconductor vendors to maintain or redesign long after normal EOL (G1-007, G1-008, G1-009).
- AEC-Q100 qualification burden and temperature-grade availability gaps; advanced-node (<7 nm) reliability challenges (G1-010, G1-011, G1-013).
- Chiplet standardisation and automotive qualification gaps (G1-014).
- Hardware/software co-design silos: silicon and software teams do not co-design early enough (G1-015).
- Memory wall: compute growth vs. memory bandwidth/capacity/safety (G1-017).
- Embedded flash scalability and cost at advanced nodes; external flash/XiP limits (G1-018, G1-019).
- MCAL complexity/lock-in and BSP fragility (G1-020, G1-021, G1-022, G1-031).
- Sensor/actuator interface fragmentation and proprietary SerDes (G1-027, G1-028).
- ASIC/FPGA long cycles, tape-out risk, mixed-signal verification (G1-029).
- HIL hardware dependency on silicon availability (G1-032, G1-035).
- Semiconductor talent shortage (design, process, test) (G1-034).
- Automotive is often a secondary customer to consumer/datacenter volumes, so tooling and support lag.

**Key problem IDs.** G1-001, G1-002, G1-003, G1-004, G1-005, G1-006, G1-007, G1-008, G1-009, G1-010, G1-011, G1-012, G1-013, G1-014, G1-015, G1-017, G1-018, G1-019, G1-020, G1-021, G1-022, G1-027, G1-028, G1-029, G1-031, G1-032, G1-034, G1-035.

---

### 2.5 ECU manufacturer

**Objectives.** Cost; quality and yield; test coverage; manufacturability; hardware revision control; on-time delivery to Tier-1/OEM.

**Processes owned.** PCB and mechanical design/manufacturing; EOL (end-of-line) testing and programming; hardware revision management; yield analysis; hardware bring-up support; ECU-level diagnostics and traceability.

**Interfaces.** Upstream: Tier-1/Tier-2 (hardware requirements, designs, ECU architecture). Downstream: Tier-1 (manufactured ECUs, EOL test data, revision notices). Lateral: semiconductor vendors (silicon supply); test equipment vendors.

**Responsibilities (RACI).** Vehicle architecture: I. System requirements: I. Software requirements: I. Hardware requirements: C. ECU hardware design: A/R. MCAL/BSP delivery: C. BSW configuration: I. Application SWC: I. ECU integration: C. Multi-ECU/network integration: I. Unit/component testing: C. HIL testing: C (hardware). Vehicle-level validation: I. Safety case: C. Cybersecurity case: C. Homologation: I. Release baseline: I. OTA: I. Obsolescence management: C.

**Where ownership gets lost.**
- BSP fragility across hardware revisions: a small PCB change breaks the BSP and requires re-validation (G1-022, G1-031).
- Hardware bring-up discovers integration surprises late, after software assumptions are baked in (G1-023).
- HIL testing is gated by physical ECU availability; benches sit idle or are over-subscribed (G1-032, G1-035, W4-018, W3-010, W3-016).
- Mixed-criticality ECU consolidation (hypervisor, virtualisation) complicates hardware design and certification (G1-024, W2-009).
- Power management, cold-crank, EMI, and thermal constraints are discovered late (G1-025).
- Zero-defect (ppm) pressure and screening cost (G1-012).
- AEC-Q100 temperature-grade mismatch with real mission profiles (G1-010, G1-011).
- Sensor/actuator interface fragmentation at the hardware level (G1-027, G1-028).
- Hardware revisions and compatibility are poorly tracked against software baselines (G1-031, W4-024).
- EOL test coverage and programming are bottlenecks for high-volume, high-variant production.

**Key problem IDs.** G1-010, G1-011, G1-012, G1-022, G1-023, G1-024, G1-025, G1-027, G1-028, G1-031, G1-032, G1-035, W4-018, W4-024.

---

### 2.6 Software vendor (AUTOSAR stacks, middleware, platforms)

**Objectives.** Licensing revenue; standards evolution influence; platform lock-in and design wins; support and maintenance contracts; reuse of BSW across customers.

**Processes owned.** BSW product development (AUTOSAR Classic/Adaptive stacks); middleware (SOME/IP, DDS, communication, diagnostic, memory, security stacks); hypervisor and virtualisation platforms; Adaptive platform services; configuration tools; updates and patches; technical support.

**Interfaces.** Customers: OEM and Tier-1 (BSW products, configurators, middleware). Standards: AUTOSAR consortium, ISO/UNECE. Tooling: integration with configuration and test tools. Feedback: defect reports, feature requests, certification evidence.

**Responsibilities (RACI).** Vehicle architecture: C. System requirements: I. Software requirements: C. Hardware requirements: I. ECU hardware design: I. MCAL/BSP delivery: C. BSW configuration: A/R (product). Application SWC: I. ECU integration: C. Multi-ECU/network integration: C. Unit/component testing: C. HIL testing: C. Vehicle-level validation: I. Safety case: C. Cybersecurity case: C. Homologation: I. Release baseline: I. OTA: C. Obsolescence management: C.

**Where ownership gets lost.**
- Vendor lock-in at BSW and configuration-tool level: switching costs are prohibitive (W2-005, W4-004, G1-021).
- Classic/Adaptive interoperability and gateway translation are unsolved; migration requires full communication redesign (W2-002, W2-016).
- Adaptive platform immaturity and stabilisation churn; AP service maturity gaps (W2-003, W2-017).
- Freedom from interference on Adaptive is unsolved for real-time safety; ASIL level gap (Adaptive targets ASIL-B, target D) (W2-004, W2-018).
- Adaptive dynamic memory allocation non-determinism (W2-014).
- Hypervisor and mixed-criticality consolidation challenges (W2-009).
- SOME/IP service discovery latency/jitter and DDS real-time determinism (W2-007, W2-008).
- Classic OS (OSEK) scheduling limitations and multicore distribution complexity (W2-013, W2-006).
- OTA/FOTA complexity (W2-010).
- Variant management and RTE contract validation gaps (W2-011, W2-012).
- ARXML round-trip and version-control friction (W2-015).
- Adaptive application portability vs. vendor implementation divergence (W2-020).
- Open-source AUTOSAR tooling is immature and unsupported, reinforcing lock-in (W4-025).
- Tool licensing and cost constraints (W4-027).
- AI tooling trust, certification, IP, and data constraints (W4-030).
- Standards evolution and regional fragmentation (W4-026).

**Key problem IDs.** W2-002, W2-003, W2-004, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-011, W2-012, W2-013, W2-014, W2-015, W2-016, W2-017, W2-018, W2-020, W4-002, W4-004, W4-025, W4-026, W4-027, W4-030, G1-021.

---

### 2.7 Engineering service provider (ESP)

**Objectives.** Capacity and utilisation; skill availability; tool access; margins; delivery against contract; knowledge retention.

**Processes owned.** Contract engineering (SWC development, BSW configuration, integration, testing); onboarding to OEM/supplier toolchains; test bench operation; documentation; knowledge transfer.

**Interfaces.** Customers: OEM and Tier-1 (contracted tasks, requirements, tool access). Suppliers: Tier-2, software vendors. Testing: test providers. Feedback: defect reports, effort estimates, skill-gap signals.

**Responsibilities (RACI).** Vehicle architecture: C. System requirements: C. Software requirements (domain ECU): R (contract). Hardware requirements: I. ECU hardware design: I. MCAL/BSP delivery: I. BSW configuration: R (contract). Application SWC: R (contract). ECU integration: R (contract). Multi-ECU/network integration: C. Unit/component testing: R (contract). HIL testing: R (contract). Vehicle-level validation: C. Safety case: C. Cybersecurity case: C. Homologation: I. Release baseline: I. OTA: C. Obsolescence management: I.

**Where ownership gets lost.**
- Tool onboarding and training time; single-person expertise (W4-022).
- Supplier/OEM toolchain mismatch and data exchange friction (W4-023).
- Skills gaps across the board: embedded software, AUTOSAR, requirements, architecture, integration, testing, safety, cybersecurity, AI/ML, HW/SW co-design, toolchain (W5-001, W5-002, W5-006, W5-007, W5-008, W5-009, W5-010, W5-011, W5-013, W5-014, W5-015, W5-021, W5-022, W5-023, W5-024).
- Knowledge silos and turnover-driven knowledge loss (W5-012).
- Communication gaps across disciplines and organisations (W5-014).
- AI adoption barriers: data confidentiality, IP protection, NDA constraints; hallucination; explainability; AI-generated code safety/certification gap; absence of AI governance (W5-016, W5-017, W5-018, W5-019, W5-020, W5-025).
- Incomplete requirements handed to suppliers; ESPs inherit assumption-driven development (W1-006).
- Inconsistent architecture across suppliers; ESPs implement to documents, not governed models (W1-008).
- Unstable interfaces and governance gaps (W1-007).
- HIL bench scarcity and vECU fidelity (W4-018, W4-019).
- Traceability gaps (W4-007).
- Compliance overhead and standards fragmentation (W1-014, W4-011, W4-013, W4-014, W4-015).

**Key problem IDs.** W1-006, W1-007, W1-008, W1-014, W4-007, W4-011, W4-013, W4-014, W4-015, W4-018, W4-019, W4-022, W4-023, W5-001, W5-002, W5-005, W5-006, W5-007, W5-008, W5-009, W5-010, W5-011, W5-012, W5-013, W5-014, W5-015, W5-016, W5-017, W5-018, W5-019, W5-020, W5-021, W5-022, W5-023, W5-024, W5-025.

---

### 2.8 Tool vendor

**Objectives.** Licensing and subscription revenue; standards influence and lock-in; ecosystem integration; support contracts; installed-base retention.

**Processes owned.** Development and maintenance of engineering tools: requirements management (DOORS/Polarion/Codebeamer-class), modelling (MATLAB/Simulink, Rhapsody), AUTOSAR configurators (EB tresos, DaVinci), network analysis (CANoe/CANalyzer), calibration (CANape), static analysis (QAC/PRQA, LDRA, Parasoft), test (Tessy, VectorCAST), HIL automation, simulation/vECU platforms, PLM/PDM, CI/CD integration, ARXML editors/parsers, AI-assisted engineering tools.

**Interfaces.** Customers: all stakeholders (OEM, Tier-1, ESP, test providers). Standards: AUTOSAR, ISO 26262, ASPICE, MISRA, ISO/SAE 21434. Data: proprietary formats and ARXML. Feedback: defect reports, feature requests, certification needs.

**Responsibilities (RACI).** Vehicle architecture: I. System requirements: I. Software requirements: I. Hardware requirements: I. ECU hardware design: I. MCAL/BSP delivery: C. BSW configuration: C. Application SWC: I. ECU integration: I. Multi-ECU/network integration: I. Unit/component testing: C. HIL testing: C. Vehicle-level validation: I. Safety case: C. Cybersecurity case: C. Homologation: I. Release baseline: I. OTA: C. Obsolescence management: I.

**Where ownership gets lost.**
- Toolchain fragmentation with poor end-to-end interoperability (W4-001).
- Proprietary data formats and ARXML version/dialect fragmentation (W4-002).
- Manual ARXML authoring/editing is error-prone and unscalable (W4-003).
- AUTOSAR configuration tool lock-in and licensing cost (W4-004).
- Configuration diff/merge and variant management in ARXML are immature (W4-005).
- Requirements management tools are legacy-heavy and hard to onboard (W4-006).
- End-to-end traceability gaps between requirements, design, code, tests, safety (W4-007).
- Static analysis false positives and manual deviation effort (W4-008).
- MISRA/CERT standard version churn and migration burden (W4-009).
- Undecidable MISRA rules require manual review and create inconsistency (W4-010).
- Standards evolution and regional regulatory fragmentation (W4-026).
- Tool licensing, infrastructure, and cost constraints (W4-027).
- Manual data transformation via spreadsheets/scripts ("glue work") (W4-028).
- Test tool interoperability and coverage evidence management (W4-029).
- Supplier/OEM toolchain mismatch and data exchange friction (W4-023).
- PLM/PDM vs. engineering-data (ARXML) integration gaps (W4-024).
- Open-source AUTOSAR tooling immaturity reinforces commercial lock-in (W4-025).
- Tool onboarding, training time, single-person expertise (W4-022).
- Generic CI/CD pipelines unsuitable for safety-critical embedded software (W4-020).
- Long build/compilation times (W4-021).
- HIL bench scarcity and vECU fidelity (W4-018, W4-019).
- SOTIF validation scale (W4-017).
- OTA engineering complexity (W4-016).
- UNECE R155 CSMS supply-chain burden (W4-015).
- ISO/SAE 21434 integration cost (W4-014).
- ASPICE overhead (W4-013); ISO 26262 overhead (W4-011); safety case maintenance (W4-012).
- AI-assisted tooling trust, certification, IP, data constraints (W4-030).
- AI adoption barriers and hallucination in safety-critical engineering (W5-016, W5-017, W5-018, W5-019, W5-020, W5-025).
- Standards/regional fragmentation (W4-026).

**Key problem IDs.** W4-001, W4-002, W4-003, W4-004, W4-005, W4-006, W4-007, W4-008, W4-009, W4-010, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-017, W4-018, W4-019, W4-020, W4-021, W4-022, W4-023, W4-024, W4-025, W4-026, W4-027, W4-028, W4-029, W4-030, W5-011, W5-016, W5-017, W5-018, W5-019, W5-020, W5-025.

---

### 2.9 Testing service provider

**Objectives.** Capacity and equipment utilisation; scenario coverage; accreditation; on-time delivery of test results; cost efficiency.

**Processes owned.** HIL bench operation; vehicle-level testing (proving ground, road, climate, EMC, durability); homologation testing; scenario databases and test fleet management; cloud HIL; test environment management; accreditation and audit readiness.

**Interfaces.** Customers: OEM and Tier-1 (test requirements, scenarios, compliance evidence needs). Equipment: HIL benches, tracks, chambers, sensor rigs. Standards: ISO 26262, ASPICE, SOTIF, UNECE. Feedback: test results, defect reports, coverage metrics.

**Responsibilities (RACI).** Vehicle architecture: I. System requirements: I. Software requirements: I. Hardware requirements: I. ECU hardware design: I. MCAL/BSP delivery: I. BSW configuration: I. Application SWC: I. ECU integration: C. Multi-ECU/network integration: C. Unit/component testing: C. HIL testing: R (contract). Vehicle-level validation: R (contract). Safety case: C. Cybersecurity case: C. Homologation: R (contract). Release baseline: I. OTA: C. Obsolescence management: I.

**Where ownership gets lost.**
- HIL cost and scalability: expensive, slow, not parallelisable (W3-010).
- HIL setup latency, reconfiguration, and variant explosion (W3-011).
- HIL model fidelity: simplified plant models and missing second/third-order effects (W3-012).
- HIL sensor/actuator physical constraints and latency/desynchronisation (W3-013).
- HIL fixture/test-bench verification is itself difficult; "quality of test fixture" unquantified (W3-014).
- Cloud-based HIL: latency, cybersecurity, compliance, legacy integration (W3-015, G3-014).
- HIL test environment management: idle benches, scheduling, utilisation (W3-016, W4-018).
- Physical proving-ground testing scope explosion and cost (W3-017).
- ADAS/ADS scenario coverage and completeness unknown; ODD coverage metrics immature (W3-018, G2-001, G2-031).
- Simulation nondeterminism/flakiness in ADS testing (W3-019).
- Test scenario reuse is unsafe for ADAS/ADS (system-dependent quality) (W3-020).
- Environmental/EMC/durability validation remains largely physical and slow (W3-021).
- Test fleet management and real-world data collection inefficiency (W3-022, G3-015).
- Lack of traceability across MIL-SIL-HIL (horizontal) (W3-023).
- Test case specification quality problems (W3-024).
- Compliance documentation burden for testing (W3-025, W4-011, W4-013).
- Incomplete test coverage and time pressure (W3-026).
- Invalid test failures and unreliable test infrastructure (W3-027).
- Regression testing scalability and repeated regression overhead (W3-028).
- Non-determinism and flaky tests in ML-based automotive systems (W3-029).
- Test data management and artifact versioning across XiL (W3-030).
- Shift-left adoption incomplete; testing still late (W3-031).
- Unit testing informal, weak strategies, poor evidence (W3-032).
- Fault injection: manual, expertise-dependent, incomplete fault models (W3-033).
- Safety validation: statistical evidence and testing effort manageability (W3-034).
- Mixed discrete-continuous (hybrid) system testing intractability (W3-035).
- Test tool interoperability and fragmented toolchain (W3-036, W4-029, W4-001).
- Test prioritisation and risk-based testing underdeveloped (W3-037).
- Knowledge and skill gaps in testing/validation (W3-038, W5-009).
- vECU fidelity gaps and integration friction with simulation (W4-019, W3-004, W3-007).
- HIL hardware dependencies (G1-032, G1-035).
- SOTIF validation scale (W4-017).
- Test tool interop/coverage evidence (W4-029).
- Cloud HIL latency, security, compliance (G3-014).

**Key problem IDs.** W3-010, W3-011, W3-012, W3-013, W3-014, W3-015, W3-016, W3-017, W3-018, W3-019, W3-020, W3-021, W3-022, W3-023, W3-024, W3-025, W3-026, W3-027, W3-028, W3-029, W3-030, W3-031, W3-032, W3-033, W3-034, W3-035, W3-036, W3-037, W3-038, W4-001, W4-017, W4-018, W4-019, W4-029, W5-009, G1-032, G1-035, G2-001, G2-031, G3-014, G3-015.

---

### 2.10 Automotive startup

**Objectives.** Cost (especially tool licensing); talent acquisition; speed to market; differentiation; SDV-first architecture; often greenfield with no legacy baggage.

**Processes owned.** Full vehicle software stack (often); greenfield architecture; toolchain adoption; OTA and cloud infrastructure; supplier onboarding; compliance from scratch.

**Interfaces.** Suppliers (costly, often unwilling to support small volumes); tool vendors (licensing barriers); semiconductor vendors (design wins, support); regulators (homologation); cloud providers; investors.

**Responsibilities (RACI).** Vehicle architecture: A/R. System requirements: A/R. Software requirements: A/R. Hardware requirements: C. ECU hardware design: C. MCAL/BSP delivery: C. BSW configuration: A/R. Application SWC: A/R. ECU integration: A/R. Multi-ECU/network integration: A/R. Unit/component testing: A/R. HIL testing: C/R. Vehicle-level validation: A/R. Safety case: A/R. Cybersecurity case: A/R. Homologation: A. Release baseline: A/R. OTA: A/R. Obsolescence management: A/R.

**Where ownership gets lost.**
- Tool licensing and cost constraints block access to industry-standard toolchains (W4-027).
- Open-source AUTOSAR tooling is immature and unsupported (W4-025).
- Tool onboarding and training time (W4-022).
- Standards evolution and regional regulatory fragmentation (W4-026).
- Skills: embedded software shortage, AI/ML capabilities gap, compensation disadvantage vs. tech rivals, education pipeline mismatch (W5-001, W5-005, W5-015, W5-022).
- SDV/centralised computing skill gap (W5-024).
- AI adoption barriers: data confidentiality, IP, NDA; hallucination; explainability; AI-generated code safety/certification; AI governance absence (W5-016, W5-017, W5-018, W5-019, W5-020, W5-025).
- Compliance overhead and standards fragmentation (W1-014, W4-011, W4-013, W4-014, W4-015).
- Semiconductor supply-chain opacity and shortages; obsolescence (G1-004, G1-005, G1-007).
- Semiconductor talent shortage (G1-034).
- Fleet/platform business-model shift and cloud cost (G3-027, G3-007).
- Data privacy, consent, and monetisation economics (G3-008, G3-022).
- Cloud/backend cybersecurity attack surface (G3-009).
- Zonal/centralised architecture transition (W1-016, W2-016, W5-024).
- Startups lack supplier leverage, so Tier-2/semiconductor support is thin and late.

**Key problem IDs.** W1-014, W1-016, W2-016, W4-011, W4-013, W4-014, W4-015, W4-022, W4-025, W4-026, W4-027, W5-001, W5-005, W5-015, W5-016, W5-017, W5-018, W5-019, W5-020, W5-022, W5-024, W5-025, G1-004, G1-005, G1-007, G1-034, G3-007, G3-008, G3-009, G3-022, G3-027.

---

### 2.11 EV manufacturer (non-traditional, software-centric)

**Objectives.** Speed; OTA capability; user experience; cost; software-centric platform; energy efficiency and range; charging ecosystem integration.

**Processes owned.** Centralised compute platform; OTA infrastructure; BMS, charging, and thermal management software; vehicle energy management; zonal I/O gateways; cloud/fleet data pipelines; charging interoperability testing.

**Interfaces.** Charging ecosystem: vehicles, EVSE, grid operators, payment/PKI (ISO 15118, OCPP, CCS/CHAdeMO/GB-T/NACS). Cloud: data pipelines, fleet management, OTA. Suppliers: semiconductor, Tier-1/2 for battery, power electronics, sensors. Regulators: safety, cybersecurity, grid.

**Responsibilities (RACI).** Vehicle architecture: A/R. System requirements: A/R. Software requirements: A/R. Hardware requirements: C. ECU hardware design: C. MCAL/BSP delivery: C. BSW configuration: A/R. Application SWC: A/R. ECU integration: A/R. Multi-ECU/network integration: A/R. Unit/component testing: A/R. HIL testing: C/R. Vehicle-level validation: A/R. Safety case: A. Cybersecurity case: A. Homologation: A. Release baseline: A/R. OTA: A/R. Obsolescence management: A/R.

**Where ownership gets lost.**
- SoC/SoH estimation uncertainty and drift; battery aging model inaccuracy; cell balancing limitations (G4-001, G4-002, G4-003).
- Thermal runaway detection latency and false positives/negatives; thermal model fidelity and sensor placement (G4-004, G4-005).
- Fast-charging thermal derating and preconditioning control (G4-006).
- Cold-weather range loss and heating energy management (G4-007).
- Heat pump software control complexity and efficiency (G4-008).
- Cabin HVAC energy consumption vs. comfort trade-off (G4-009).
- Regenerative braking blending with friction brake (G4-010).
- Vehicle energy management optimisation across conflicting objectives (G4-011).
- DC fast charging curve control and charger-vehicle communication timing (G4-012).
- ISO 15118 Plug & Charge PKI and certificate management complexity (G4-013).
- OCPP version fragmentation and interoperability (G4-014).
- Charging protocol heterogeneity (CCS/CHAdeMO/GB-T/NACS) (G4-015).
- Charging session reliability (timeouts, retries, drops) (G4-016).
- V2G bidirectional charging complexity and grid safety (G4-017).
- High-voltage safety monitoring (isolation, contactor welding, precharge) (G4-018).
- 12V/48V auxiliary power net and load shedding (G4-019).
- Range estimation (GOM) uncertainty and driver trust (G4-020).
- Battery cell voltage measurement accuracy and scaling (modular BMS) (G4-021).
- Thermal management of power electronics and e-motor (G4-022).
- Charging software OTA and version management (EVSE + vehicle) (G4-023).
- Functional safety (ASIL) of charging and BMS software (G4-024).
- Cybersecurity of charging interfaces (G4-025).
- Fleet energy management and predictive analytics (cloud) (G4-026).
- Battery preconditioning and thermal scheduling for fast charge (G4-027).
- Energy recuperation and one-pedal driving calibration (G4-028).
- High-voltage battery diagnostics and DTC complexity (G4-029).
- Thermal runaway propagation containment and software response (G4-030).
- Energy/thermal HIL and vECU validation gaps (G4-031).
- Charging interoperability testing coverage (G4-032).
- OTA campaign orchestration at fleet scale (G3-001, G3-002, G3-017, G3-018, G3-019, G3-023).
- Cloud cost, egress fees, vendor lock-in (G3-007).
- Data privacy, consent, and monetisation economics (G3-008, G3-022).
- Cloud/backend cybersecurity attack surface (G3-009).
- OEM business-model shift to platform operator (G3-027).
- OTA/FOTA complexity (W2-010, W4-016).
- OTA engineering complexity (W4-016).
- Zonal/centralised architecture transition (W1-016, W2-016).

**Key problem IDs.** G4-001, G4-002, G4-003, G4-004, G4-005, G4-006, G4-007, G4-008, G4-009, G4-010, G4-011, G4-012, G4-013, G4-014, G4-015, G4-016, G4-017, G4-018, G4-019, G4-020, G4-021, G4-022, G4-023, G4-024, G4-025, G4-026, G4-027, G4-028, G4-029, G4-030, G4-031, G4-032, G3-001, G3-002, G3-007, G3-008, G3-009, G3-017, G3-018, G3-019, G3-022, G3-023, G3-027, W1-016, W2-010, W2-016, W4-016.

---

### 2.12 Autonomous driving company

**Objectives.** Safety case and regulatory approval; data acquisition and curation; compute platform; talent; ODD definition and expansion; perception/planning/control performance; ML model lifecycle.

**Processes owned.** Perception, sensor fusion, planning, control software; ML model training and validation; scenario and ODD management; data pipelines and fleet data collection; safety case for ADS; compute platform (SoC/GPU/NPU) integration; simulation and digital twins.

**Interfaces.** Sensor suppliers (cameras, radar, lidar, ultrasonics); compute/semiconductor vendors; OEM partners (if not building vehicles); cloud providers; regulators and safety assessors; test providers (tracks, scenarios).

**Responsibilities (RACI).** Vehicle architecture: C (if partner OEM) / A/R (if full-stack). System requirements: C/R. Software requirements: A/R (ADS). Hardware requirements: C. ECU hardware design: C. MCAL/BSP delivery: C. BSW configuration: C. Application SWC: A/R. ECU integration: R. Multi-ECU/network integration: R (if full-stack). Unit/component testing: A/R. HIL testing: C/R. Vehicle-level validation: A/R. Safety case (ADS): A/R. Cybersecurity case: A/R. Homologation: A. Release baseline: A/R. OTA: A/R. Obsolescence management: C.

**Where ownership gets lost.**
- ML/AI-specific requirements and traceability gaps (ODD, data, monitors, KPIs) (W1-004).
- ADAS perception software: sensor fusion, ML model lifecycle, ODD specification (G2-001).
- ADAS real-time scheduling and priority assignment risks (G2-002).
- ADAS scenario validation and real-world coverage gap (G2-031).
- Cross-domain integration: motion, energy, and ADAS actuator conflicts (G2-032).
- ADAS/ADS scenario coverage and completeness unknown; ODD coverage metrics immature (W3-018).
- Simulation nondeterminism/flakiness in ADS testing (W3-019).
- Test scenario reuse is unsafe for ADAS/ADS (system-dependent quality) (W3-020).
- Non-determinism and flaky tests in ML-based automotive systems (W3-029).
- SOTIF validation of unknown unsafe scenarios does not scale (W4-017).
- Safety validation: statistical evidence and testing effort manageability (W3-034).
- AI/ML engineering capabilities gap in automotive (W5-005).
- AI adoption barriers: data confidentiality, IP protection, NDA constraints (W5-016).
- LLM/AI hallucination in safety-critical engineering (W5-017).
- Lack of automotive-specific datasets for fine-tuning (W5-018).
- AI-generated code safety and certification gap (probabilistic vs. deterministic) (W5-019).
- Explainability and accountability of AI outputs (W5-020).
- Absence of AI governance in engineering workflows (W5-025).
- AI-assisted engineering tooling trust, certification, IP, data constraints (W4-030).
- Semiconductor content growth and SoC competition with AI/datacenter (G1-001, G1-002).
- Advanced-node reliability and memory wall for AI compute (G1-013, G1-017).
- ECU hardware architecture: mixed-criticality consolidation, thermal, power, redundancy (G1-024).
- Zonal/centralised architecture transition (W1-016, W2-016, W5-024).
- Timing analysis and predictability unsolved for AUTOSAR (W1-010).
- Hypervisor and mixed-criticality consolidation (W2-009).
- Cloud data volume overwhelms pipelines; real-time latency; digital twin fidelity (G3-003, G3-004, G3-013).
- Test fleet management and real-world data collection inefficiency (G3-015, W3-022).
- Cloud HIL latency, security, compliance (G3-014, W3-015).
- Vehicle-to-cloud connectivity reliability (G3-016).
- Cloud/edge skill gaps (G3-026).

**Key problem IDs.** W1-004, W1-010, W1-016, W2-009, W2-016, W3-018, W3-019, W3-020, W3-022, W3-029, W3-034, W4-017, W4-030, W5-005, W5-016, W5-017, W5-018, W5-019, W5-020, W5-024, W5-025, G1-001, G1-002, G1-013, G1-017, G1-024, G2-001, G2-002, G2-031, G2-032, G3-003, G3-004, G3-013, G3-014, G3-015, G3-016, G3-026.

---

## 3. Cross-stakeholder ownership-loss matrix

| # | Ownership gap | Where it occurs | Primary stakeholders | Problem IDs |
|---|---|---|---|---|
| 1 | System-level requirements ownership unclear | OEM → Tier-1 handoff | OEM, Tier-1, ESP | W1-006 |
| 2 | Interface governance authority absent | Integration (Stage 5) | OEM, Tier-1, Tier-2 | W1-007 |
| 3 | Architecture authority without enforcement | Architecture (Stage 2) | OEM, Tier-1, suppliers | W1-008, W1-017 |
| 4 | No single source of truth (architecture, interfaces, variants) | All stages | All | W1-007, W1-017, W2-011 |
| 5 | Tool fragmentation across organisations | All stages | All, tool vendors | W4-001, W4-023 |
| 6 | Compliance cascade (safety/cybersecurity) | Release (Stage 9) | OEM, Tier-1, Tier-2 | W4-011, W4-014, W4-015 |
| 7 | Semiconductor opacity (shortages, obsolescence) | Production/field (Stages 9–10) | OEM, Tier-1, Tier-2, silicon | G1-004, G1-007 |
| 8 | Skills concentrated in individuals | All stages | ESP, Tier-1, OEM | W5-012, W5-023 |
| 9 | Business model shifting faster than capabilities | Field/cloud | OEM, EV makers, AD companies | G3-027 |
| 10 | OTA/field feedback loops slow or lossy | Release/field (Stages 9–10) | OEM, Tier-1, cloud | W2-010, W4-016, G3-001, G3-018 |
| 11 | Testing evidence not portable across organisations | Validation (Stage 6–7) | OEM, Tier-1, test providers | W3-023, W4-007, W4-029 |
| 12 | Variant definitions not shared as a single model | Configuration (Stage 4) | OEM, Tier-1, tool vendors | W1-011, W2-011, W4-005 |

---

## 4. Problem-ID index by stakeholder

| Stakeholder | Problem IDs (primary) |
|---|---|
| OEM | W1-001, W1-002, W1-005, W1-006, W1-007, W1-008, W1-009, W1-010, W1-011, W1-013, W1-014, W1-015, W1-016, W1-017, W1-018, W2-010, W2-011, W4-001, W4-002, W4-007, W4-011, W4-014, W4-015, W4-016, W4-023, W4-026, W5-006, W5-007, G1-004, G1-005, G1-007, G3-008, G3-009, G3-010, G3-027 |
| Tier-1 | W1-002, W1-005, W1-006, W1-007, W1-008, W1-010, W1-011, W1-013, W1-014, W1-017, W2-002, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-011, W2-012, W2-013, W2-014, W2-015, W2-016, W2-017, W2-018, W2-019, W2-020, W3-004, W3-007, W3-010, W3-011, W3-012, W3-013, W3-014, W3-016, W3-023, W3-026, W3-027, W3-028, W3-032, W4-002, W4-003, W4-004, W4-005, W4-007, W4-008, W4-009, W4-010, W4-011, W4-012, W4-014, W4-015, W4-016, W4-018, W4-019, W4-020, W4-021, W4-022, W4-023, W4-028, W5-001, W5-002, W5-003, W5-004, W5-008, W5-009, W5-011, W5-012, G1-004, G1-005, G1-007, G1-010, G1-015, G1-020, G1-021, G1-022, G1-027, G1-028, G1-031, G1-032, G1-035 |
| Tier-2 | G1-004, G1-005, G1-006, G1-007, G1-008, G1-009, G1-010, G1-011, G1-012, G1-013, G1-014, G1-015, G1-016, G1-017, G1-018, G1-019, G1-020, G1-021, G1-022, G1-023, G1-024, G1-025, G1-026, G1-027, G1-028, G1-029, G1-030, G1-031, G1-032, G1-033, G1-034, G1-035, G2-029, G2-030 |
| Semiconductor | G1-001, G1-002, G1-003, G1-004, G1-005, G1-006, G1-007, G1-008, G1-009, G1-010, G1-011, G1-012, G1-013, G1-014, G1-015, G1-017, G1-018, G1-019, G1-020, G1-021, G1-022, G1-027, G1-028, G1-029, G1-031, G1-032, G1-034, G1-035 |
| ECU manufacturer | G1-010, G1-011, G1-012, G1-022, G1-023, G1-024, G1-025, G1-027, G1-028, G1-031, G1-032, G1-035, W4-018, W4-024 |
| Software vendor | W2-002, W2-003, W2-004, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-011, W2-012, W2-013, W2-014, W2-015, W2-016, W2-017, W2-018, W2-020, W4-002, W4-004, W4-025, W4-026, W4-027, W4-030, G1-021 |
| Engineering service provider | W1-006, W1-007, W1-008, W1-014, W4-007, W4-011, W4-013, W4-014, W4-015, W4-018, W4-019, W4-022, W4-023, W5-001, W5-002, W5-005, W5-006, W5-007, W5-008, W5-009, W5-010, W5-011, W5-012, W5-013, W5-014, W5-015, W5-016, W5-017, W5-018, W5-019, W5-020, W5-021, W5-022, W5-023, W5-024, W5-025 |
| Tool vendor | W4-001, W4-002, W4-003, W4-004, W4-005, W4-006, W4-007, W4-008, W4-009, W4-010, W4-011, W4-012, W4-013, W4-014, W4-015, W4-016, W4-017, W4-018, W4-019, W4-020, W4-021, W4-022, W4-023, W4-024, W4-025, W4-026, W4-027, W4-028, W4-029, W4-030, W5-011, W5-016, W5-017, W5-018, W5-019, W5-020, W5-025 |
| Testing provider | W3-010, W3-011, W3-012, W3-013, W3-014, W3-015, W3-016, W3-017, W3-018, W3-019, W3-020, W3-021, W3-022, W3-023, W3-024, W3-025, W3-026, W3-027, W3-028, W3-029, W3-030, W3-031, W3-032, W3-033, W3-034, W3-035, W3-036, W3-037, W3-038, W4-001, W4-017, W4-018, W4-019, W4-029, W5-009, G1-032, G1-035, G2-001, G2-031, G3-014, G3-015 |
| Startup | W1-014, W1-016, W2-016, W4-011, W4-013, W4-014, W4-015, W4-022, W4-025, W4-026, W4-027, W5-001, W5-005, W5-015, W5-016, W5-017, W5-018, W5-019, W5-020, W5-022, W5-024, W5-025, G1-004, G1-005, G1-007, G1-034, G3-007, G3-008, G3-009, G3-022, G3-027 |
| EV manufacturer | G4-001, G4-002, G4-003, G4-004, G4-005, G4-006, G4-007, G4-008, G4-009, G4-010, G4-011, G4-012, G4-013, G4-014, G4-015, G4-016, G4-017, G4-018, G4-019, G4-020, G4-021, G4-022, G4-023, G4-024, G4-025, G4-026, G4-027, G4-028, G4-029, G4-030, G4-031, G4-032, G3-001, G3-002, G3-007, G3-008, G3-009, G3-017, G3-018, G3-019, G3-022, G3-023, G3-027, W1-016, W2-010, W2-016, W4-016 |
| Autonomous driving company | W1-004, W1-010, W1-016, W2-009, W2-016, W3-018, W3-019, W3-020, W3-022, W3-029, W3-034, W4-017, W4-030, W5-005, W5-016, W5-017, W5-018, W5-019, W5-020, W5-024, W5-025, G1-001, G1-002, G1-013, G1-017, G1-024, G2-001, G2-002, G2-031, G2-032, G3-003, G3-004, G3-013, G3-014, G3-015, G3-016, G3-026 |

---

## 5. Sources and limitations

**Sources.**
- Task.md §3A (Vehicle Engineering and Automotive Product Domains) and §10 (Industry Stakeholder and Supply-Chain Analysis).
- `research/artefacts/oem_tier_interaction.md` (Artefact #4) — role definitions, interaction chain, RACI matrix, handoff friction, supply-chain dependency.
- `research/DOMAIN_MAP.md` §6 (Stakeholder Map) and §5 (Domain × ECU × Network × Standard cross-reference).
- Workstreams W1–W5, G1–G4 (problem catalogues and evidence).
- Part III Ch. 20 (Organisational and Supplier Problems).
- Part I Ch. 4 (Semiconductor-to-Vehicle) and Part II (lifecycle chapters).

**Limitations.**
- Problem IDs cited are the research's own canonical IDs; counts and coverage reflect the state of the research artefacts as of 2026-11-05.
- Stakeholder objectives and tensions are synthesised from published industry analysis and the research workstreams; they are not drawn from confidential interviews.
- Some categories overlap (e.g., EV manufacturers may also be autonomous-driving companies or startups); problem-ID sets are indicative, not exhaustive per organisation.
- Financial, headcount, and market-share figures are intentionally not invented; where a claim requires quantification, the cited problem record should be consulted for its evidence tier.

---

*End of research/stakeholders.md — Industry Stakeholders and Supply-Chain Deep Dive.*