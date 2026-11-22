# Automotive Engineering Toolchain Map

**Artefact:** #5 â€” Automotive Engineering Toolchain Map  
**Parent project:** THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING  
**Date compiled:** 2026-11-05  
**Scope:** Global automotive software industry â€” ICE, hybrid, EV, ADAS/ADS; Classic + Adaptive AUTOSAR; OEM/Tier-1/Tier-2 perspectives.  
**Evidence basis:** Task.md Â§6 (Engineering Tools and Technology Landscape) + W4 (Toolchain & Standards, 30 records) + W6 (Bootloader & Flash, 30 records) + W1/W2/W3 cross-references + DOMAIN_MAP.md.

---

## 0. How to read this map

This document maps the **automotive engineering toolchain**: the categories of tools, representative products, the lifecycle stages they support, their strengths, limitations, and the interoperability gaps between them. It contains:

1. **Toolchain landscape overview** â€” how many tools, how they connect (or don't).
2. **Tool categories by lifecycle stage** â€” what tools exist for each stage, with representative products.
3. **Cross-cutting tool categories** â€” tools that span multiple stages (PLM/ALM, CI/CD, version control, standards/compliance).
4. **Interoperability gap matrix** â€” where tools fail to exchange data semantically.
5. **Open-source tooling landscape** â€” what's available and its maturity.
6. **Toolchain economics** â€” licensing, infrastructure, training constraints.
7. **Problem-to-toolchain mapping** â€” W-series problem IDs tied to tool limitations.

Core thesis: **automotive programmes assemble 10â€“30+ specialised tools that were not designed to interoperate; data moves via manual export/import, scripts, and spreadsheets, producing a non-digital, error-prone thread. Tool fragmentation is a root cause, not just a symptom.**

---

## 1. Toolchain landscape at a glance

- A typical programme assembles **10â€“30+ tools** spanning requirements, modelling, AUTOSAR config, network analysis, static analysis, HIL, PLM, CI/CD, test management.
- **No end-to-end digital thread exists in practice** (W4-001). Data moves via file-based, often lossy transformations.
- **Hypothesis (W4-001): 10â€“30% of engineering effort spent on data transformation/reconciliation** (not independently quantified; treat as estimate).
- **AUTOSAR configuration tool lock-in:** DaVinci/EB tresos/SystemDesk dominate Classic BSW config; high licence cost; proprietary project formats (W4-004).
- **Requirements tooling legacy-heavy:** DOORS, Polarion, Codebeamer hard to onboard (W4-006).
- **CI/CD mismatch:** generic pipelines unsuitable for safety-critical embedded (W4-020); long build times (W4-021).
- **HIL scarcity:** benches ~$50k+, ~100â€“200 tests/day, not parallelizable (W4-018).

---

## 2. Tool categories by lifecycle stage

### 2.1 Requirements engineering tools (Stage 1)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| Requirements management (ALM) | IBM DOORS / DOORS Next, Siemens Polarion, PTC Codebeamer, Jama Connect, Visure, reqSuite | Traceability modules; compliance workflows; mature in automotive | Legacy UX; steep onboarding; poor APIs; tool-specific data models; fragmented | W4-006, W4-007, W1-003 |
| Requirements quality / NLP | (emerging) Trace.Space, AI assistants | Ambiguity detection, drafting | Experimental; not safety-certified | W1-001, W4-006 |
| Change impact | ALM change management modules | Change control boards | Impact not propagated automatically | W1-005, W1-017, W4-007 |

### 2.2 System engineering & architecture tools (Stage 2)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| MBSE / modelling | Cameo (SysML), Rhapsody, Enterprise Architect, PREEvision, SystemWeaver, MATLAB/Simulink | Model-based architecture; traceability; reuse | Cultural resistance; skill gaps; tool integration; model governance; SysML v2 migration cost | W1-009, W5-007 |
| Network design / analysis | Vector CANoe, CANalyzer, CANape, ETAS INCA | Bus simulation, measurement, calibration | Manual; tool-specific; network timing not captured in AUTOSAR | W1-018, W2-007 |
| Safety analysis | medini analyze, PREEvision, SCADE | HARA, FMEA, safety case drafting | Manual data entry; evidence assembly manual | W4-011, W4-012 |
| Cybersecurity analysis | ThreatGet, TARA tools | Threat modelling | Immature integration with ALM; manual workshops | W4-014, W4-015 |
| Timing analysis | Symtavision, AUTOSAR timing extensions | Timing modelling | Research-stage; not integrated into standard toolchains; not certified | W1-010, W2-004, W7-015, W7-017 |

### 2.3 Software design & development tools (Stage 3)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| IDEs / compilers | Eclipse, Visual Studio, IAR Embedded Workbench, Green Hills MULTI, GCC/LLVM | Mature; cross-compilation | Embedded toolchain qualification; safety-qualified variants costly | W4-020 |
| Model-based development | MATLAB/Simulink, Stateflow, Embedded Coder | Code generation; MIL/SIL/PIL | Feature restrictions (W3-008); model fidelity; unsupported features | W3-001, W3-002, W3-008 |
| Static analysis | QAC/PRQA, Parasoft, LDRA, Polyspace, PC-lint | MISRA/CERT compliance; defect detection | False positives; undecidable rules; tool disagreement; version churn | W4-008, W4-009, W4-010 |
| Code quality / review | SonarQube, Gerrit | General code quality | Not safety-certified; not AUTOSAR-aware | W4-008 |
| Version control | Git, GitLab, GitHub, Bitbucket, Perforce Helix | Collaboration; branching | ARXML merge conflicts; tag reordering noise | W2-015, W4-005 |

### 2.4 Configuration & engineering data tools (Stage 4)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| AUTOSAR configurators (Classic) | DaVinci Developer/Configurator (Vector), EB tresos Studio (Elektrobit), SystemDesk (dSPACE) | Deep BSW config; code generation | Lock-in; proprietary project formats; high licence cost; vendor-specific | W4-004, W2-005 |
| AUTOSAR authoring (Adaptive) | EB corbos, Vector MICROSAR Adaptive, Adaptive toolchains | Service-oriented config | Less mature than Classic; vendor divergence | W2-003, W2-020 |
| ARXML editors / parsers | AutoSAR.io, arxml-cli, AutosarFactory, ParseX, ARForge, IAV Kedu | Schema-aware editing; compare | Immature; limited semantic validation; version migration unsupported | W4-002, W4-003, W4-005 |
| Variant management | PLM variant modules, MATLAB variant blocks, AUTOSAR EvaluatedVariantSet | Variant binding | Tool-specific; no cross-tool consistency checking | W1-011, W2-011, W4-005, W7-024, G5-007 |
| RTE generation | Vendor RTE generators (Vector, EB, dSPACE) | Per-ECU RTE generation | Batch; contract phase manual; errors surface late | W2-012 |
| PLM / PDM | Siemens Teamcenter, PTC Windchill, Dassault ENOVIA | Product data, BOM, change management | Covers product data not engineering semantics (ARXML); integration gaps | W4-024 |

### 2.5 Integration tools (Stage 5)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| Network analysis / simulation | Vector CANoe, CANalyzer, CANape, ETAS INCA | Multi-bus simulation, measurement | Manual; timing not captured in AUTOSAR | W1-018, W2-007 |
| Integration frameworks | (vendor-specific) | ECU integration | Tool fragmentation; manual data transformation | W4-001, W4-028 |
| Gateway configuration | Vector, PREEvision, SystemDesk | Signalâ†”service mapping | Manual; gateway becomes bottleneck/SPOF | W2-002, G5-001 |

### 2.6 Testing & validation tools (Stage 6)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| Unit / component testing | VectorCAST, Tessy, Parasoft C/C++test, GoogleTest/GoogleMock, LDRA | Automated unit tests; coverage | Informal strategies; weak evidence; tool-specific | W3-032, W4-029 |
| MIL / SIL | MATLAB/Simulink, dSPACE, MathWorks AUTOSAR Blockset | Model-based testing; early validation | Model fidelity; feature restrictions; test reuse gaps | W3-001..009 |
| PIL | Embedded Coder, target compilers | Reveals compiler/processor faults | Slow; hardware-dependent; not parallelizable | W3-008, W3-009 |
| HIL | dSPACE (SCALEXIO), NI (VeriStand), OPAL-RT, AVL, Speedgoat | Real-time ECU validation | Cost ~$50k+; ~100â€“200 tests/day; not parallelizable; setup weeks; scarcity | W3-010..016, W4-018, G5-006, G5-010, G5-014, G5-018, G5-022 |
| vECU / virtual validation | dSPACE VEOS, MathWorks Virtual ECU, Synopsys Silver, QEMU, Renode | Shift-left; early validation | Fidelity gaps; integration friction; no standard | W3-004, W3-007, W4-019 |
| Test management / traceability | Polarion, Codebeamer, Jama, ALM suites | Requirement-to-test links | Horizontal (MILâ†”SILâ†”HIL) traceability lacking | W3-023, W4-007 |
| Scenario databases / simulation | CARLA, MetaDrive, ASAM OpenSCENARIO, OpenDRIVE, OSI | ADAS/ADS scenario testing | Nondeterminism (43.6% flaky in CARLA); scenario reuse unsafe; coverage unknown | W3-018, W3-019, W3-020, W4-017 |
| Test scheduling / lab ops | test.guide, PulseMo, SYNECT, HELIUM | Bench scheduling, utilization | Fragmented; manual; integration effort | W3-016, W4-018 |
| Fault injection | (vendor-specific), dSPACE, NI | Robustness testing | Manual, expertise-dependent, incomplete models | W3-033 |

### 2.7 Vehicle-level validation tools (Stage 7)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| Test fleet management | CONiX.tfs, telematics platforms | Fleet data collection | Fragmented; data silos; late health detection | W3-022 |
| Track / chamber instrumentation | (OEM-specific) | Physical validation | Capacity; weather; seasonal | W3-017, W3-021 |
| Data replay / analytics | (various) | Field data mining | Data volume; bandwidth | W3-022 |

### 2.8 Release & production tools (Stage 9)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| Build systems | Jenkins, GitLab CI, GitHub Actions, (safety-qualified variants) | Automation | Unsuitable for safety-critical embedded; tool qualification; reproducibility | W4-020 |
| Flashing / programming | (vendor-specific), production programmers, debuggers (JTAG/SWD), UDS flash tools | ECU programming | Bootloader validation; OTA coordination; toolchain fragmentation; secure boot provisioning; throughput | W2-010, W4-016, W6-007, W6-015, W6-019, W6-022, W6-023, W6-028, G5-027, G5-031 |
| EOL test | (manufacturing) | End-of-line verification | â€” | â€” |
| OTA platforms | Mender, Redstone, (OEM-specific) | Delivery | Compliance evidence manual; update engineering | W4-016 |

### 2.9 Field / maintenance tools (Stage 10)

| Category | Representative tools | Strengths | Limitations | Problems |
|---|---|---|---|---|
| Diagnostics | UDS (ISO 14229), DoIP (ISO 13400), OBD-II | Standardised diagnostics | â€” | â€” |
| Telematics / OTA | (TCU platforms) | Connectivity, updates | â€” | W2-010, W4-016 |
| Field data analytics | (OEM-specific) | Defect analysis | Data volume; privacy | W3-022 |

---

## 3. Cross-cutting tool categories

### 3.1 PLM / ALM (product & application lifecycle)

| Tools | Coverage | Gap |
|---|---|---|
| Siemens Teamcenter, PTC Windchill, Dassault ENOVIA (PLM) | Product data, BOM, change management, manufacturing | Does not cover engineering semantics (ARXML); integration with engineering-data layer weak (W4-024) |
| IBM DOORS/DOORS Next, Siemens Polarion, PTC Codebeamer, Jama Connect (ALM) | Requirements, traceability, test management | Legacy UX; poor APIs; fragmented; no semantic validation of links; cross-tool links brittle (W4-006, W4-007) |

### 3.2 CI/CD and DevOps

| Tools | Coverage | Gap |
|---|---|---|
| Jenkins, GitLab CI, GitHub Actions (generic) | Build, test automation | Unsuitable for safety-critical embedded: tool qualification, reproducibility, HIL-in-CI, cross-compilation (W4-020) |
| Safety-qualified variants | LDRA CI, IAR embedded CI/CD, Parasoft CI | Safety-qualified pipelines | Qualification burden; HIL integration; cost (W4-020) |
| Containerisation | Docker, Kubernetes | Environment reproducibility | Not standard in embedded/safety; tool qualification of containers unresolved (W4-020) |

### 3.3 Standards & compliance tooling

| Tools | Coverage | Gap |
|---|---|---|
| Safety tools | medini analyze, PREEvision, SCADE | HARA, safety case, code generation | Manual evidence assembly; safety case maintenance cost (W4-011, W4-012) |
| TARA / cybersecurity | ThreatGet, (emerging) | Threat modelling | Immature ALM integration; manual workshops (W4-014) |
| Compliance reporting | ALM reporting modules | Evidence collection | Documentation as afterthought; 41% time-consuming (W3-025) |
| Tool qualification | (vendor-specific) | TCL1â€“3 evidence | Cost; manual |

### 3.4 AI-assisted engineering tooling

| Tools | Coverage | Gap |
|---|---|---|
| LLM assistants, code generation, RAG | Requirements drafting, code understanding, ARXML intelligence, test generation | Trust/certification/IP/data constraints; hallucination in safety-critical; no automotive-specific datasets (W4-030, W5-016..020, W5-025) |

---

## 4. Interoperability gap matrix (where tools fail to exchange data semantically)

| From â†’ To | Data | Gap | Problem IDs |
|---|---|---|---|
| System model (SysML) â†’ ARXML | Architecture â†’ config | Manual transformation; schema/version mismatch | W4-001, W4-002, W1-013 |
| ARXML (OEM) â†’ ARXML (Tier-1 tool) | System extract â†’ BSW config | Version dialect; tool extensions; manual repair | W4-002, W4-003, W4-005 |
| Simulink model â†’ AUTOSAR SWC | Model â†’ code/config | Export/import drift; tag reordering; sync issues | W1-013, W4-003 |
| Requirements (DOORS) â†’ Tests (Polarion/Codebeamer) | Req â†’ test | Manual links; brittle cross-tool links | W4-007, W1-003 |
| Tests (SIL) â†’ Tests (HIL) | Test case identity | No common test case identity across XiL levels | W3-023 |
| Code â†’ Safety case | Implementation â†’ evidence | Manual evidence assembly | W4-011, W4-012 |
| PLM (BOM) â†’ Engineering (ARXML) | Product data â†’ engineering data | Semantic gap; PLM covers product not engineering semantics | W4-024 |
| Supplier ARXML â†’ OEM baseline | Config â†’ baseline | No semantic 3-way merge; manual reconciliation | W4-005 |
| MCAL (vendor tool) â†’ third-party tool | MCAL config â†’ reference | "For reference purposes only"; must configure in vendor tool | W2-005, W4-004 |

**Root cause (W4-001):** historical domain-specific tool evolution; vendor lock-in and proprietary formats; absence of a neutral, versioned, machine-readable engineering data backbone; organisations optimising tool purchase per function rather than flow.

---

## 5. Open-source tooling landscape

| Category | Open-source examples | Maturity | Limitations | Problems |
|---|---|---|---|---|
| AUTOSAR Classic stacks | OpenBSW, S-CORE, AGL, Arctic Core (legacy) | Partial | Incomplete BSW coverage; no certification evidence; community support uneven | W4-004, W4-025 |
| ARXML tools | ARForge, AutosarFactory, ParseX, arxml-cli | Emerging | Limited semantic validation; version migration unsupported | W4-002, W4-003 |
| vECU / emulation | QEMU, Renode | Growing | Fidelity gaps; no standard bus interface | W3-007, W4-019 |
| Simulation | CARLA, MetaDrive | Active | Nondeterminism; scenario coverage unknown | W3-018, W3-019 |
| CI/CD | Jenkins, GitLab, GitHub Actions | Mature (generic) | Unsuitable for safety-critical embedded without qualification | W4-020 |
| Testing | GoogleTest, GoogleMock | Mature (unit) | Not safety-certified; not AUTOSAR-aware | W3-032 |
| Standards-aware assistants | (emerging) | Experimental | Hallucination; no certification | W4-030, W5-017 |

**Conclusion (W4-025):** open-source AUTOSAR tooling is immature and unsupported relative to commercial stacks; the gap is a barrier for startups/SMEs and a lock-in amplifier for incumbents.

---

## 6. Toolchain economics (licensing, infrastructure, training)

| Constraint | Detail | Problems |
|---|---|---|
| Licensing cost | DaVinci/EB tresos/SystemDesk seats expensive; HIL benches ~$50k+; toolchain bundles (Vector, ETAS, dSPACE) | W4-004, W4-027 |
| Infrastructure | HIL lab space, benches, maintenance; cloud HIL latency/cyber/compliance | W4-018, W3-015 |
| Training / onboarding | Weeksâ€“months per tool; single-person expertise risk; vendor-specific skills not transferable | W4-022, W5-011, W5-013 |
| Procurement | Vendor lock-in; supplier mandates; M&A inheriting disparate stacks | W4-001, W4-023 |
| Tool qualification | ISO 26262 Part 8 TCL1â€“3; qualification evidence cost | W4-011, W4-020 |

**Startup/SME barrier (W4-004):** small EV startups cannot afford DaVinci + EB tresos seats; resort to open-source/partial stacks and manual ARXML, slowing development.

---

## 7. Toolchain map by lifecycle stage (summary view)

```
Stage 0 Strategy        : PLM (Teamcenter/Windchill), portfolio tools
Stage 1 Requirements    : ALM (DOORS/Polarion/Codebeamer/Jama), AI assistants (exp.)
Stage 2 Architecture    : MBSE (Cameo/Rhapsody/PREEvision/SystemWeaver), CANoe, medini, ThreatGet, Symtavision
Stage 3 Development     : IDEs (Eclipse/IAR/GHS), Simulink, static analysis (QAC/Parasoft/LDRA/Polyspace), Git
Stage 4 Configuration   : DaVinci, EB tresos, SystemDesk, arxml-cli/ARForge, PLM variants
Stage 5 Integration     : CANoe/CANalyzer, integration frameworks, gateway config tools
Stage 6 Testing         : VectorCAST/Tessy, Simulink/dSPACE, HIL (dSPACE/NI/OPAL-RT/AVL), vECU (VEOS/Silver), test.guide/PulseMo, CARLA/MetaDrive
Stage 7 Vehicle valid.  : Fleet mgmt (CONiX.tfs), track/chamber instrumentation, scenario DBs
Stage 8 Compliance      : medini/PREEvision/SCADE, TARA tools, ALM reporting, tool qualification
Stage 9 Release/Prod    : Jenkins/GitLab/GitHub Actions (or safety-qualified), flashing, EOL
Stage 10 Field          : UDS/DoIP diagnostics, telematics, OTA platforms, field analytics
```

**Cross-cutting (all stages):** PLM/ALM (Teamcenter/Windchill/ENOVIA, DOORS/Polarion/Codebeamer/Jama), version control (Git), CI/CD, standards/compliance tooling, AI assistants.

---

## 8. Problem-to-toolchain mapping (W/G-series â†’ tool limitations)

| Problem | Tool limitation implicated |
|---|---|
| W1-001 (ambiguity) | ALM stores text but does not enforce semantic quality |
| W1-002 (abstraction disconnect) | Cross-tool links brittle; no semantic validation |
| W1-003 (traceability burden) | Manual linking; tool friction |
| W1-009 (MBSE barriers) | Tool integration gaps; model governance immaturity |
| W1-010 (timing unsolved) | Timing tools research-stage; not in standard toolchains |
| W1-011 (variant explosion) | No cross-tool variant consistency checking |
| W1-013 (ARXML drift) | Tool-specific generators; no canonical serialization |
| W2-001 (ARXML config complexity) | Vendor configurators powerful but proprietary |
| W2-005 (vendor lock-in) | Standard specifies data format not tool behavior |
| W2-011 (variant binding) | Tool-specific variant handling |
| W2-012 (RTE contract) | Vendor generators differ in contract handling |
| W2-015 (ARXML VCS friction) | Non-canonical serialization; tag reordering |
| W3-004 (SiL limits) | vECU levels; host vs target compilation |
| W3-007 (vECU standardization) | FMI vs AUTOSAR; no bus comms standard |
| W3-010 (HIL cost/scale) | Capital-intensive; not parallelizable |
| W3-011 (HIL setup/reconfig) | Weeks to reconfigure; manual |
| W3-023 (horizontal traceability) | ALM vertical-only; no cross-XiL identity |
| W3-036 (test tool interop) | Fragmented test toolchain |
| W4-001 (fragmentation) | 10â€“30+ tools; manual transformation |
| W4-002 (ARXML version/dialect) | Converters vendor-specific and partial |
| W4-003 (manual ARXML editing) | No safe, schema-aware editors |
| W4-004 (config tool lock-in) | Proprietary project formats; high licence cost |
| W4-005 (config diff/merge) | No semantic 3-way merge |
| W4-006 (requirements tooling legacy) | Steep onboarding; poor APIs |
| W4-007 (traceability gaps) | Manual, brittle cross-tool links |
| W4-008 (static analysis FP) | Conservative analysis; undecidable rules |
| W4-009 (MISRA churn) | Tool support lags standard evolution |
| W4-010 (undecidable rules) | Tools flag candidates only |
| W4-011 (ISO 26262 overhead) | Evidence assembly manual |
| W4-012 (safety case maintenance) | No automated safety-impact analysis |
| W4-013 (ASPICE overhead) | Evidence collection not automated |
| W4-014 (ISO/SAE 21434) | TARA tools immature ALM integration |
| W4-016 (OTA engineering) | OTA platforms focus on delivery not evidence |
| W4-017 (SOTIF scale) | Scenario DBs/coverage tools research-stage |
| W4-018 (HIL scarcity) | Capital-intensive; poor scheduling |
| W4-019 (vECU fidelity) | Integration friction; no standard |
| W4-020 (CI/CD suitability) | Generic pipelines not safety-qualified |
| W4-021 (build times) | Large AUTOSAR codebases |
| W4-022 (tool onboarding) | Single-person expertise |
| W4-023 (supplier/OEM mismatch) | Toolchain heterogeneity |
| W4-024 (PLM vs ARXML) | Semantic gap |
| W4-025 (open-source immaturity) | Incomplete BSW; no certification |
| W4-027 (licensing/cost) | Seats; benches; training |
| W4-028 (glue work) | Manual data transformation |
| W4-029 (test tool interop) | Fragmented test chain |
| W4-030 (AI tooling trust) | Confidentiality, IP, hallucination, certification |
| W7-001 (ASIL-A implication set) | ALM templates default to QM or ASIL-D with no ASIL-A profile; trace tools do not flag ASIL-dependent method gaps. |
| W7-002 (ASIL-B implication set) | Safety-analysis tools compute metrics but do not validate architectural assumption coverage. |
| W7-003 (ASIL-C implication set) | Requirements/design tools do not enforce independence (author vs. reviewer vs. tester separation). |
| W7-004 (ASIL-D implication set) | Unqualified tools in D toolchain; coverage tools themselves untrusted. |
| W7-005 (QM components: obligations, limits, and misuse) | Component catalogues carry QM/ASIL labels but not interference-relevance metadata. |
| W7-006 (Freedom from interference: memory interference) | Static configuration checks exist but cross-tool MPU consistency (BSW vs. app vs. bootloader) rarely verified |
| W7-007 (Freedom from interference: timing interference) | Timing tools model single-core well, multicore/shared-interconnect poorly. |
| W7-008 (Freedom from interference: execution / control-flow) | OS configuration tools generate access masks but rarely verify least-privilege completeness. |
| W7-009 (QMâ€“ASIL coupling through shared libraries and common-cause) | DFA tools model hardware coupling; software coupling captured in prose, if at all. |
| W7-010 (SWC partitioning by ASIL) | Architecture tools show functional decomposition, not ASIL-homogeneity heatmaps. |
| W7-011 (Runnable-to-task mapping under ASIL constraints) | RTE/mapping tools optimise for CPU load, not ASIL separation or preemption analysis. |
| W7-012 (Data consistency across ASIL boundaries) | RTE generators emit configured mechanism correctly but do not flag safety-relevant data left unprotected. |
| W7-013 (End-to-end protection) | E2E generators configure chosen profile but do not verify end-to-end coverage across gateways and repacking. |
| W7-014 (Multicore core allocation for mixed-criticality) | Allocation tools balance load and affinity; none enforce ASIL-to-core capability rules. |
| W7-015 (Multicore scheduling and timing analysis gaps) | Single-core WCET and schedule simulation mature; multicore contention and cross-core blocking modelled poorly or not at |
| W7-016 (Inter-core communication and spinlocks) | Spinlock call-graph and lock-order extraction is manual; RTE/IOC generators emit code without blocking-time budgets or |
| W7-017 (Multicore timing verification) | Timing tools model single-core well but multicore contention analysis and worst-case alignment generation are immature; |
| W7-018 (OS-Applications and memory partitions) | OS configuration tools generate regions but lack cross-checks against the ASIL allocation and least-privilege |
| W7-019 (Hypervisor guests for mixed-criticality) | Guest-interference analysis tooling is immature; virtio data-flow and shared-page tracking across guest images lacks |
| W7-020 (Inter-partition communication gateways) | Gateway configuration tools check routability, not per-flow ASIL contracts, quota sufficiency, or information-flow |
| W7-021 (Mode management across partitions) | BswM/EcuM configuration tools validate per-partition rules, not cross-partition mode coherence or request-arbitration |
| W7-022 (SWC to RTE to OS integration chain) | RTE generators diff poorly across versions; contract-vs-generated consistency checkers exist but are rarely run in CI |
| W7-023 (COM and signal-service mapping under ASIL) | Bus-configuration tools optimise load and pack signals; cross-layer freshness-budget and atomicity verification is |
| W7-024 (Variant and calibration binding in mixed-criticality builds) | Variant tools manage code switches or calibration labels, rarely the cross-product binding with ASIL qualification |
| W7-025 (Application integration testing across ASILs) | SiL/HiL fault-injection and interference-generation tooling exists but is rig-specific and unqualified; coverage |
| W7-026 (BSW module integration under ASIL) | BSW generators compose modules without checking cross-module ASIL compatibility or exclusive-area conflicts. |
| W7-027 (BSW configuration consistency) | Config tools validate syntax per module, not cross-module semantic consistency or ASIL-relevant constraint violations. |
| W7-028 (RTE/BSW contract gaps) | RTE generators wire ports without checking state-dependent call legality or NvM/Dem error-path coverage. |
| W7-029 (MCAL portability and verification burden) | MCAL configuration tools migrate parameters across derivatives without flagging safety-relevant semantic changes. |
| W7-030 (MCU driver safety challenges) | Driver generators produce initialisation code without checking window, timing, or redundancy semantics against the |
| G5-001 (Middleware: signalâ†”service gateway correctness and timing) | Gateway config generation exists but end-to-end timing verification across signal+service legs is manual. |
| G5-002 (Middleware: time synchronisation and deterministic) | Lab jitter figures (e.g. TSN 17.12 Âµs in lab, W2-007) do not transfer to loaded vehicle networks; monitoring tooling |
| G5-003 (Middleware: serviceæŽ¥å£ versioning and mixed-version fleet) | ARXML/service-registry tooling checks single-release consistency, not N-version compatibility. |
| G5-004 (Middleware: cross-domain lesson â€” middleware as the) | No single contract dashboard; consistency checks per-tool only. |
| G5-005 (Body: BCM consolidation, I/O abstraction and load control) | Pin-mapping tables and BCM configurators check single-build consistency, not cross-trim I/O equivalence or load-budget |
| G5-006 (Body: access/security vs safety and power-state timing) | Security testing (fuzz, penetration) and safety timing analysis run in separate tools with no combined |
| G5-007 (Body: variant explosion across trims and personalization) | Variant-management tooling (W4) checks buildability, not post-OTA or post-replacement configuration correctness per VIN. |
| G5-008 (Body: cross-domain lesson â€” the quiet domain that stalls) | Domain HIL rigs stub body as ideal power and wake; vehicle-level wake/power dashboards arrive late. |
| G5-009 (Powertrain: EMS/TCM/aftertreatment coordination and the) | Per-controller MIL/SIL rigs validate local torque logic; cross-controller arbitration tested only at late powertrain |
| G5-010 (Powertrain: ASIL-C/D torque monitoring and OBD interplay) | Safety FTA/FMEDA tooling and OBD certification tooling do not cross-check reaction conflicts or shared-sensor |
| G5-011 (Powertrain: emission-relevant calibration protection and) | Calibration-diff tooling shows value changes, not emission-relevance rulings; RXSWIN/version reporting per UN-R156 |
| G5-012 (Powertrain: cross-domain lesson â€” torque and calibration as) | Domain SIL/HIL rigs emulate torque consumers with ideal models; no vehicle-level torque-consumer compatibility check. |
| G5-013 (AD/ADAS: perception/fusion/planning pipeline and sensor) | Sensor models and fusion simulators validate algorithms, not production calibration drift or loaded-network timing. |
| G5-014 (AD/ADAS: SOTIF + ASIL-D motion control, ODD exits, and) | Timing analysis tools cover single ECU/schedule; SOTIF scenario tools lack coverage argument linkage. |
| G5-015 (AD/ADAS: ML model, map-data, and calibration updates vs) | Model registries track weights, not behavioural regression coverage; map-version compatibility checks immature. |
| G5-016 (AD/ADAS: cross-domain lesson â€” ADAS turns every shared) | No cross-domain arbitration test harness standard; interference-inclusive HIL rare. |
| G5-017 (IVI/Infotainment: head-unit SoC with audio/nav stacks) | Android/Linux profiling tools diagnose lab builds, not production multi-guest contention (cf. W2-009). |
| G5-018 (IVI/Infotainment: QMâ€“ASIL coexistence and telltale/display) | FFI testing for GPU/display/audio paths immature; fault-injection coverage ad hoc. |
| G5-019 (IVI/Infotainment: app-store cadence vs vehicle release) | Package managers (UCM-style) install updates but do not prove N-version compatibility or regression coverage. |
| G5-020 (IVI/Infotainment: cross-domain lesson â€” QM exuberance must) | No shared-footprint dashboard (bus load + power + diagnostic rate) attributed per domain. |
| G5-021 (Cockpit: cluster/HUD/driver-monitoring consolidation on) | SoC timing/interference analysis tooling models CPU well but GPU and display-pipeline contention poorly. |
| G5-022 (Cockpit: ASIL-B telltales, display-pipeline integrity, and) | HMI test tools verify appearance, not pipeline fault coverage or deadline compliance under fault injection. |
| G5-023 (Cockpit: HMI personalization and variants vs) | HMI authoring tools preview appearance but do not enforce safety-layout constraints or check all variant combinations. |
| G5-024 (Cockpit: cross-domain lesson â€” the display is a safety) | No cross-domain warning-path dashboard; per-domain tests assume a perfect display. |
| G5-025 (Zonal: I/O consolidation, power distribution, and network) | Pin/power/network configured in separate tools; cross-budget (current vs thermal vs timing) consistency unchecked. |
| G5-026 (Zonal: mixed-criticality up to ASIL-D on one controller and) | FFI analysis tooling covers memory well but timing/execution interference under worst-case I/O storms is measured ad |
| G5-027 (Zonal: software packaging, partial updates, and rollback) | Package managers verify single-ECU images, not cross-zone dependency and rollback graphs. |
| G5-028 (Zonal: cross-domain lesson â€” the zone is where domains) | Domain tools verify domain functions; no zone-level conflict dashboard spans power, timing, safety, and update coupling. |
| G5-029 (HPC: central-compute consolidation, virtualization) | Hypervisor configuration and Adaptive manifest tooling check single-VM consistency, not fleet-wide allocation |
| G5-030 (HPC: mixed-criticality up to ASIL-D on shared SoC,) | Lab FFI figures do not transfer to loaded production configurations; interference-measurement tooling is |
| G5-031 (HPC: container/VM image updates, atomic fleet rollout, and) | UCM/package tooling validates single-vehicle install, not fleet-wide campaign atomicity, rollback coverage, or |
| G5-032 (HPC: cross-domain lesson â€” HPC as the convergence point) | No unified HPC dashboard combining allocation, FFI, timing, compatibility, and campaign health. |

---

## 9. Sources

- Task.md Â§6 (Engineering Tools and Technology Landscape) and Â§3D (lifecycle stages).
- W4 (Toolchain & Standards) â€” 30 records W4-001..W4-030.
- W7 (ASIL & Mixed-Criticality) - 30 records W7-001..W7-030; G5 (Vehicle-Domain Deep Dive) - 32 records G5-001..G5-032.
- W1, W2, W3 â€” cross-referenced tool limitations.
- DOMAIN_MAP.md (domains, ECUs, networks, standards, stakeholders).
- Vendor documentation: dSPACE, Vector, ETAS, Elektrobit, MathWorks, Parasoft, LDRA, Perforce/PRQA, PTC, Siemens, IBM, MathWorks, Synopsys, IPG, AVL, OPAL-RT, NI, Aliaro, Renault.
- Standards: AUTOSAR, ISO 26262, ASPICE, ISO/SAE 21434, UNECE, MISRA, ASAM.

---

## G6–G11 Tool Mapping

| ID | Tool Category | Design | Verification | HIL | PMIC Characterisation | Semiconductor-to-Vehicle Integration |
|---|---|---|---|---|---|---|
| G6-045 | Semiconductor CDD tools | Commercial vendor platforms | Limited cross-partition validation | N/A | N/A | CDD schema compliance checking |
| G6-046 | Semiconductor CDD tools | ARXML model validation | Semantic diff tools | N/A | N/A | Cross-partition consistency |
| G6-047 | BCM design tools | ECU configuration suites | I/O abstraction verification | N/A | N/A | Pinmap-to-physical mapping |
| G6-048 | PMIC design tools | Power domain simulators | Power-state verification | N/A | HSM/crypto validation | Power integrity sign-off |
| G6-049 | Zonal design tools | E/E architecture tools | Zone-level FFI analysis | N/A | N/A | Power/bridge budget checks |
| G6-050 | Zonal design tools | Cross-domain interference | Worst-case I/O storm | N/A | N/A | Update coupling analysis |
| G6-051 | Update tools | A/B partition managers | Partial-update verification | N/A | N/A | Dependency-graph checks |
| G6-052 | Update tools | Rollback policy enforcement | Campaign consistency | N/A | N/A | Semantic compatibility |
| G6-053 | Verification tools | Mixed-criticality FFI suites | Interference stress testing | N/A | N/A | Cross-domain test coverage |
| G6-054 | Compliance tools | ISO 26262/21434 evidence | Qualification kits | N/A | N/A | Traceability mapping |
| G6-055 | HIL tools | Semiconductor-in-the-loop | Real-time validation | Target ECU | N/A | Integration test harness |

| G7-010 | Zonal tools | Zone-level requirement sets | Cross-domain timing | FFI test cells | Power-domain validation | Update rollout scripts |
| G7-011 | Update tools | A/B packaging managers | Rollback graphs | Zonal HIL | Bootloader fallback | Cross-zone dependency |
| G7-012 | HIL tools | Mixed-criticality campaigns | WCET stress tests | Multi-core HIL | Interference measurement | Semiconductor-in-the-loop |
| G7-013 | Validation tools | Cabinet-level test | Sensor injection | Real vehicle | Attestation evidence | Semiconductor validation |
| G7-014 | Compliance tools | Type-argument packages | Safety case documents | Evidence bundles | FFI certification | Type-approval packages |
| G7-015 | Production tools | Flash programming | EOL testing | Production HIL | Secure boot enablement | Manufacturing traceability |
| G7-016 | OTA tools | Update packages | Campaign atomicity | Fleet verification | Secure boot verification | OTA rollout control |
| G7-017 | Compliance tools | FFI evidence packages | Safety argument docs | Test evidence | Attestation records | Regulatory submission |
| G7-018 | Package tools | Configuration signing | Variant combinations | Start-up tests | Key lifecycle management | Release decision gating |
| G7-018 | Release tools | Jenkins/GitLab pipelines | Integration gates | Deployment pipelines | Key derivation workflows | Fleet-wide update control |
| G7-019 | EOL tools | Production programmers | End-of-line testing | Flash verification | Manufacturing test | EOL finalisation |
| G7-020 | Field tools | OTA platform monitoring | Telematics data | Field HIL | Key lifecycle tracking | Fleet analytics |

| G8-024 | Middleware tools | Service mesh config | E2E protection profiles | Network validation | Diagnostics config | Interface contract checks |
| G8-025 | Interface tools | RTE contract generation | Version consistency checks | Trigger map verification | Calibration binding | Vehicle contract dashboards |
| G8-026 | Timing tools | Schedule analysis | Interference-channel checklist | Real-time deadline monitoring | Power budget analysis | Cross-domain timing arbitration |
| G8-027 | Bootloader tools | Safe self-update design | Recovery mode testing | Dual-bank verification | HSM monotonic counters | Recovery path validation |
| G8-028 | Universal flash tools | Vendor-independent interface | Memory-map verification | Boundary checks in CI | Rollback policy standardisation | Universal flash tool interface |
| G8-028 | Safety tools | Bootloader safety patterns | Evidence automation | Safety case templates | Doc automation | Mandatory for interface changes |
| G8-029 | Compliance tools | ISO 26262 processes | ASPICE safety case | V&V evidence | Bootloader safety patterns | Mandatory for safety |
| G8-030 | Documentation tools | Safety case templates | Evidence automation | Doc generation | Bootloader docs | Mandatory for safety |
| G8-031 | Test tools | Power-fault test plans | Automated injection | Power emulation | Write-budget correlation | Reliability targets |
| G8-031 | Analysis tools | Verification depth | Performance trade-off | Verification depth | Performance trade-off | For safety |
| G8-032 | Calibration tools | Version binding | Automated image binding | Calibration-image versioning | Safety-relevant calibration | For safety-relevant calibration |
| G8-033 | Test coverage tools | Test plan template | Coverage tracking | XiL/HIL coverage | Write-budget per ECU | For reliability targets |
| G8-034 | Safety evidence tools | Cost of late rework | vECU acceptance | Cost quantification | vECU acceptance | For process |

| G9-006 | Test tools | Test plan template | Coverage tracking | XiL/HIL coverage | Write-budget per ECU | For reliability targets |
| G9-007 | Validation tools | Mixed-criticality XiL/HIL | Interference stress suites | Multi-core HIL | Interference measurement | Cross-domain test coverage |
| G9-008 | Compliance tools | Type-approval packages | Safety case documents | Evidence bundles | FFI certification | Type-approval evidence |
| G9-008 | Verification tools | E2E protection checks | Deadline/freshness monitoring | Profile consistency | FFI evidence tracing | Coverage-gap flags |
| G9-009 | Documentation tools | Safety case templates | Evidence automation | Doc generation | FFI evidence tracing | Sign-off requirement |
| G9-010 | Compliance tools | ISO 26262/21434 evidence | Qualification-scope traceability | FFI regression tests | Qualification-scope traceability | For all FFI claims |
| G9-011 | Documentation tools | Change-classification gate | Pinned model+map+calibration bill | Regression-suite enforcement | Behavioural-delta summariser | Approval authority |
| G9-012 | Test fleet tools | Management and scheduling | Vehicle availability | Test fleet instrumentation | Post-sale defect remediation | Campaign risk prediction |
| G9-013 | Field tools | OTA platform monitoring | Telematics data analysis | Field analytics | Key lifecycle tracking | Post-production validation |
| G9-014 | Field tools | Callback and recall campaigns | Defect analysis | Returned vehicle analysis | Warranty/recall process | Post-sale defect remediation |
| G9-015 | Lifecycle tools | Last-time-buy coordination | ECN tracking | Obsolete-part management | EOL part sourcing | Last-time-buy procurement |
| G9-016 | Lifecycle tools | End-of-life management | Obsolescence mitigation | EOL planning | Part sourcing | ECN and last-time-buy coordination |

| G10-015 | Compliance tools | PMIC characterization suites | Safety manuals | Interference test suites | PMIC safety manuals | For all FFI claims and hypervisor qualification |
| G10-016 | Compliance tools | ISO 26262/21434 evidence | Qualification kits | Interference regression tests | Qualification-scope traceability | For all FFI claims and hypervisor qualification |
| G10-017 | Documentation tools | Change-classification gate | Behavioural-delta summariser | Regression-suite enforcement | Behavioural-delta summariser | Approval authority |
| G10-018 | Compliance tools | Type-approval and certification | Workflow gating | Integration gates | Key derivation workflows | Fleet-wide update control |
| G10-019 | Production tools | PMIC field programming | EOL testing | Flash verification | Manufacturing test | EOL finalisation |
| G10-020 | OTA tools | Update compliance monitoring | Telematics data | Field analytics | Key lifecycle tracking | Fleet analytics |
| G10-021 | FFI tools | Production configuration analysis | Interference testing | Production HIL | Production configuration | For FFI claims |
| G10-022 | Traceability tools | Qualification-scope traceability | Configuration diffusion | Campaign risk prediction | Log-volume anomaly detection | Campaign-risk prediction |
| G10-022 | Field tools | Field analytics and anomaly detection | Returned vehicle analysis | Warranty/recall process | Post-sale defect remediation | Post-sale defect remediation |
| G10-023 | Field tools | Field analytics and anomaly detection | Returned vehicle analysis | Warranty/recall process | Post-sale defect remediation | Post-sale defect remediation |
| G10-024 | OTA tools | Platform security and update governance | Firmware validation | OTA platform security | Key lifecycle tracking | OTA platform security |
| G10-025 | Lifecycle tools | End-of-life and regulatory re-certification | Part sourcing | EOL planning | Part sourcing | EOL and regulatory re-certification |

| G11-008 | Semiconductor roadmap tools | Roadmap alignment with CDD | Schema governance | Version mapping | ESD protection mapping | Roadmap-consistency checks |
| G11-009 | CDD governance tools | Schema governance and versioning | Cross-project consistency | CDD version mapping | Governance policy enforcement | Cross-project CDD governance |
| G11-010 | PMIC certification tools | Safety/ESD mapping to ISO 26262/21434 | Safety manuals | Interference test suites | PMIC safety manuals | For all FFI claims and hypervisor qualification |
| G11-011 | CDD governance gap tools | Cross-project CDD governance gap | Policy enforcement | Cross-project consistency | Governance policy enforcement | Cross-project CDD governance |
| G11-012 | Zonal consolidation tools | Zonal controller software consolidation | Dependency analysis | A/B update validation | Power-domain consolidation | Consolidation dependency graphs |
| G11-013 | A/B update tools | Partial update semantic compatibility | Rollback test matrices | Package graphs | Dependency manifests | Partial-update/rollback test matrices |
| G11-014 | OTA campaign tools | Rollback and campaign consistency | Atomicity/rollback tests | Logging-budget enforcement | Campaign gates | Image-compatibility matrices |
| G11-015 | Mixed-criticality FFI tools | FFI validation gaps | Interference path analysis | Stress test suites | Interference evidence tracing | Gap analysis between interference catalogue and executed cases |
| G11-016 | Obsolescence tools | Semiconductor obsolescence impact assessment | Last-time-buy planning | EOL part sourcing | EOL part sourcing | Last-time-buy procurement and ECU redesign |
| G11-017 | Test infrastructure tools | Test infrastructure for cross-domain validation | XiL/HIL coverage | Test infrastructure | Test infrastructure | Test infrastructure for cross-domain validation |
| G11-018 | Lifecycle tools | Last-time-buy, obsolescence, and EOL coordination | ECN tracking | EOL planning | Part sourcing | ECN and last-time-buy coordination |
*End of toolchain_map.md â€” Automotive Engineering Toolchain Map (Artefact #5).*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

