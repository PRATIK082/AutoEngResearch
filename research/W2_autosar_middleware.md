# W2 — AUTOSAR Classic/Adaptive and Middleware: Problem Discovery Report

**Workstream:** W2 (AUTOSAR Classic/Adaptive and middleware)
**Parent project:** THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING
**Date:** 2026-11-05
**Scope:** AUTOSAR Classic Platform (CP), AUTOSAR Adaptive Platform (AP), AUTOSAR Foundation, RTE, BSW, MCAL, OS, middleware (SOME/IP, DDS), hypervisors/virtualization, gateways, bootloaders/OTA, ARXML/configuration, interoperability.
**Method:** Deep web research across AUTOSAR specifications (R19–R25), SAE/IEEE papers, vendor whitepapers (ETAS, Vector, Elektrobit), academic studies, and practitioner sources. Evidence tiers: primary (standards docs) preferred; secondary (papers/blogs) for context.

---

## 1. Executive Summary

AUTOSAR remains the dominant automotive software standard, but its dual-platform reality (Classic + Adaptive) creates a persistent impedance mismatch. Classic is mature, hard-real-time, signal-based, ASIL-D capable, but rigid; Adaptive is service-oriented, POSIX/C++-based, and intended for HPC/zonal/ADAS, yet still evolving with documented "Known Limitations" across releases. Middleware (SOME/IP, DDS) bridges these worlds but introduces discovery latency, jitter, and real-time determinism problems. Configuration is dominated by ARXML, which is massive, version-fragile, and poorly diffable. Toolchain fragmentation and vendor lock-in persist despite the interoperability standard. Migration between Classic and Adaptive is a communication-model redesign, not a recompile. Safety (freedom from interference) on Adaptive remains an open problem per SAE 2025-01-8074.

Key quantitative anchors (all attributed):
- ETAS: >4 billion ECUs worldwide rely on RTA-CAR (Classic AUTOSAR); ETAS built first multicore AUTOSAR stack for series production in 2009 (ETAS white paper, 2025).
- DDS data-delivery latency analysis shows DDS threads (dispatch, listen, liveliness, GC) must be scheduled for real-time; FastDDS-based evaluation on Bosch WATERS 2019 challenge (ECRTS 2023 paper).
- SOME/IP-SD subscription latency sensitivity analysis (SAE 2015-01-0197).
- SOME/IP+TSN average jitter 17.12 µs in lab (Sungkyunkwan 2025).
- Adaptive DDS-to-SOME/IP bridge median latency ~440 µs (PC) / ~4700 µs (Jetson) at 256 KB (MDPI 2025).
- Only Tesla (as of NHTSA 2020 report) performing OTA of safety-critical ECUs; most OEMs OTA infotainment only.

---

## 2. Problem Catalogue

### W2-001 — ARXML Configuration Complexity and Manual Burden

- **Domain:** AUTOSAR Classic/Adaptive, configuration
- **Lifecycle stage:** Software design & development / configuration
- **Problem:** ARXML files are massive, deeply nested, reference-heavy; hand-editing is impractical; schema differs across releases (e.g., CP R4.4.0 vs R4.3.1); a single typo in a reference path breaks the whole configuration.
- **Scenario:** Engineer configures BSW modules, RTE, COM stack, OS tasks for an ECU; thousands of parameters must be consistent; tool generates code. Re-configuring after a system change requires regenerating base ECU configuration.
- **Who:** BSW/configuration engineers, system integrators, Tier-1 suppliers.
- **Root cause:** AUTOSAR principle "configuration instead of implementation" pushes all variability into data; the data format (XML) and schema are the contract; no abstraction layer between engineer intent and serialized XML.
- **Contributing factors:** Release-to-release schema drift; tag reordering by different tools breaks VCS diffs; multi-tool workflows require import/export; MCAL from one vendor (EB tresos) must be referenced by another toolchain.
- **Technical limitations:** XML verbosity; reference integrity; binding-time variability (SystemDesign/CodeGen/PreCompile/Link/PostBuild).
- **Human skill:** Requires deep AUTOSAR M&T knowledge; ESOL reports >90% of CP training Q&A asks "how much AUTOSAR compliance is enough" — indicating conceptual confusion.
- **Tool limitations:** Commercial tools (DaVinci, EB tresos, SystemDesk) are powerful but proprietary; round-trip engineering requires iterative verification; VCS diffs noisy due to tag reordering.
- **Process limitations:** Configuration is treated as a one-time generation step rather than a maintained asset; limited automated validation of semantics beyond schema.
- **Frequency/severity:** Pervasive; affects every ECU project.
- **Time/cost impact:** Manual configuration time is a named bottleneck in Task.md §8; ESOL notes BSW/RTE config "costs a lot" and leads to "AUTOSAR has no benefit" conclusions.
- **Downstream effects:** Late integration failures; rework; vendor lock-in.
- **Existing solutions:** Specialized configurators (DaVinci, EB tresos), web editors (AutoSAR.io), MathWorks AUTOSAR Blockset, automated rule-based correction.
- **Why insufficient:** Tools are vendor-specific; round-trip between tools lossy; semantic errors surface late.
- **Automation feasibility:** High — validation, diff normalization, config generation from higher-level models.
- **AI applicability:** Medium-High — schema-aware assistants, ARXML intelligence, change-impact analysis.
- **Human approval:** Required for safety-relevant config changes.
- **Evidence:** autosar.io ARXML guide (2026); Vector config article (2014); MathWorks AUTOSAR guide; ESOL blog (2025); AUTOSAR RTE SWS variant handling.
- **Confidence:** High.
- **Open questions:** How much config can be derived vs authored? What is the minimal human-authored subset?

### W2-002 — Classic vs Adaptive Interoperability & Gateway Translation

- **Domain:** Classic/Adaptive interoperability
- **Lifecycle stage:** System architecture / integration
- **Problem:** Classic is signal-based (CAN/LIN/FlexRay, static config); Adaptive is service-based (SOME/IP/DDS over Ethernet, dynamic). Bridging requires gateway ECUs translating signals to services, with conversion of configuration format; signal-to-service mapping is manual and error-prone. Incompatible SOME/IP messages require a gateway.
- **Scenario:** Mixed ECU network: brake ECU (Classic, CAN) → zonal controller (Adaptive, Ethernet). Signals must be packed into services; timing and consistency must be preserved; gateway becomes a bottleneck and single point of failure.
- **Who:** System architects, integration engineers, gateway developers.
- **Root cause:** Two fundamentally different communication paradigms (signal-based static vs service-oriented dynamic) standardized separately; "cross-platform" concepts only introduced R21-11.
- **Contributing factors:** Classic ECUs that are purely signal-based cannot pack signals into services → UDP frames + signal-to-service mapping on Adaptive side; gateway configuration is large; variant explosion.
- **Technical limitations:** Gateway latency; atomicity of signal-group routing; payload length adaptation; SOME/IP message compatibility.
- **Human skill:** Requires expertise in both Classic BSW and Adaptive ARA; scarce.
- **Tool limitations:** PREEvision, SystemDesk support SOA modeling but mixed-view tooling still maturing; AUTOSAR methodology "limited scope" for mixed sub-systems (AP methodology R24-11).
- **Process limitations:** Mixed sub-systems handled with "limited scope" in methodology; no end-to-end verification standard.
- **Frequency/severity:** High — every modern vehicle with mixed architectures.
- **Time/cost impact:** Migration requires complete communication-model redesign (Promwad 2026).
- **Downstream effects:** Integration failures; timing violations; safety gaps.
- **Existing solutions:** SOME/IP gateways, signal-to-service mapping, unified data models, Vector CANoe CO communication objects for unified testing.
- **Why insufficient:** Gateways add latency and complexity; no standardized end-to-end timing verification; tooling partial.
- **Automation feasibility:** Medium — gateway config generation, consistency checking.
- **AI applicability:** Medium — impact analysis, config validation.
- **Human approval:** Required for safety-critical signal routing.
- **Evidence:** Cyient migration whitepaper (2022); Vector SOA article (2019); AUTOSAR AP methodology R24-11/R25-11; Promwad (2026); AUTOSAR gateway SRS.
- **Confidence:** High.

### W2-003 — Adaptive Platform Immaturity & Stabilization Churn

- **Domain:** AUTOSAR Adaptive
- **Lifecycle stage:** Architecture / development
- **Problem:** AP specifications are still evolving; even after R24-11 "stabilization release," known technical deficiencies persist (Network Management UdpNM-only, no node detection, cannot be master coordinator; Persistency; PHM; Core; Time Sync; Diagnostics; IDS Manager). R25-11 solved "remaining issues" but minimal C++ raised to C++17; Rust code only released R25-11. Methodology has pending chapters (Software Cluster Integration, Software Packaging) since R24-11.
- **Scenario:** OEM adopts AP for central compute; hits a "Known Limitation" in Execution Management (resource limitation/fault tolerance not complete) or a stabilization gap; must customize or wait for next release; upgrading to newer version "requires enormous effort and time" (Cyient).
- **Who:** Adaptive platform adopters, OEM platform teams, Tier-1 HPC suppliers.
- **Root cause:** AP was introduced 2017 to address HPC/SDV needs; standardization lags implementation needs; market demands stable APIs but spec churn continues.
- **Contributing factors:** Vendor implementations vary; spec depth vs. stabilization tension; annual release cycle.
- **Technical limitations:** Incomplete resource limitation/fault tolerance specs; dynamic deployment complexity; per-vehicle Software Cluster variation.
- **Human skill:** Requires platform expertise; few engineers.
- **Tool limitations:** Adaptive toolchains less mature than Classic (Promwad 2026).
- **Process limitations:** Methodology gaps; pending chapters.
- **Frequency/severity:** High for early adopters.
- **Time/cost impact:** Enormous upgrade effort; pilot-only maturity historically (R18-03 "recommended for pilot projects only").
- **Downstream effects:** Vendor fragmentation; proprietary extensions; platform debt.
- **Existing solutions:** Stabilization release, CAPI (Common Adaptive Platform Implementation), vendor platforms (EB corbos, Vector MICROSAR Adaptive).
- **Why insufficient:** Known limitations remain; vendor implementations diverge.
- **Automation feasibility:** Low for spec gaps; medium for conformance testing.
- **AI applicability:** Low-Medium.
- **Human approval:** Required.
- **Evidence:** AUTOSAR AP Release Overview R24-11/R25-11; AP methodology R24-11 known limitations; Cyient whitepaper; Promwad (2026); SystemWeaver (2025).
- **Confidence:** High.

### W2-004 — Freedom from Interference on Adaptive: Unsolved Real-Time Safety

- **Domain:** AUTOSAR Adaptive, functional safety
- **Lifecycle stage:** Architecture / safety
- **Problem:** Classic has robust safety mechanisms (memory/timing/execution interference freedom) for ASIL-D; Adaptive struggles to achieve interference-free operation under ISO 26262 due to dynamic memory allocation (fragmentation, non-deterministic alloc time), multi-process scheduling, and shared SoC resources. SAE 2025-01-8074 (Bosch) explicitly raises open questions on real-time problems in live projects not solved by existing safety mechanisms.
- **Scenario:** ASIL-rated function running as Adaptive application on shared HPC SoC; memory fragmentation or CPU contention from a non-safety app degrades timing; hypervisor partitioning required but adds overhead.
- **Who:** Safety engineers, platform architects.
- **Root cause:** Adaptive targets soft real-time, dynamic deployment, POSIX OS — inherently less deterministic than Classic's static, time-triggered OSEK model; safety must be retrofitted.
- **Contributing factors:** Dynamic memory allocation in C++; ASIL target historically ASIL-B (target D); multi-application resource sharing.
- **Technical limitations:** Deterministic memory allocators; resource limitation incomplete (Execution Management §7.6/7.7); hypervisor overhead.
- **Human skill:** Safety + platform co-design expertise scarce.
- **Tool limitations:** Timing analysis tools (Symtavision) can model but not eliminate.
- **Process limitations:** Safety case for dynamic systems harder; SEooC approach for hypervisors uncertain.
- **Frequency/severity:** High for safety-critical Adaptive use.
- **Time/cost impact:** Significant verification effort; potential re-certification.
- **Downstream effects:** Limits Adaptive to non-safety or ASIL-B domains; Classic retained for ASIL-D.
- **Existing solutions:** ASIL-compliant hypervisors, memory partitioning, Platform Health Management, resource limits, deterministic allocators.
- **Why insufficient:** Open real-time problems remain (SAE 2025); hypervisor certification uncertain.
- **Automation feasibility:** Medium.
- **AI applicability:** Low (safety-critical).
- **Human approval:** Mandatory.
- **Evidence:** SAE 2025-01-8074; AUTOSAR AP Platform Design (resource limitation); Safety Overview R24-11; arxiv 2607.05227 industry study.
- **Confidence:** High.

### W2-005 — Vendor Lock-in and BSW Toolchain Interoperability

- **Domain:** Toolchain / BSW
- **Lifecycle stage:** Configuration / integration
- **Problem:** Despite AUTOSAR interoperability standard (TR_InteroperabilityOfAutosarTools), tools use proprietary exchange formats bypassing ARXML; vendor-specific module definitions; MCAL from one vendor (EB tresos) must be configured in that tool and only referenced by others; profile mismatches between producing/consuming tools. Commercial tools are licensed, expensive, and vendor-specific.
- **Scenario:** OEM uses DaVinci for system config, EB tresos for MCAL, Vector for network; importing MCAL ARXML into third-party tool is "for reference purposes only"; MCAL must be configured in EB tresos. Integration friction.
- **Who:** Integrators, toolchain managers, suppliers.
- **Root cause:** Standard specifies data format but not tool behavior; vendors differentiate via proprietary features; no enforcement of interoperability profiles.
- **Contributing factors:** Proprietary extensions; vendor-specific parameters; profile mismatch; licensing.
- **Technical limitations:** ARXML schema version drift; profile compatibility.
- **Human skill:** Tool-specific expertise required.
- **Tool limitations:** Tools strong within domain but gaps between them.
- **Process limitations:** Manual data transformation between tools; no automated profile checking.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** Repeated configuration work; manual transformation.
- **Downstream effects:** Duplicate effort; late errors; supplier friction.
- **Existing solutions:** Interoperability standard, profile compatibility checker concept, ARXML.
- **Why insufficient:** Standard is advisory; vendors still diverge; profiles often mismatch.
- **Automation feasibility:** Medium-High (profile checker, automated import).
- **AI applicability:** Medium.
- **Human approval:** Required for toolchain changes.
- **Evidence:** AUTOSAR TR_InteroperabilityOfAutosarTools; TI MCAL integration guide (2025); AutosarToday BSW naming; Vector/PREEvision.
- **Confidence:** High.

### W2-006 — Multicore Distribution Complexity in Classic

- **Domain:** AUTOSAR Classic OS / RTE
- **Lifecycle stage:** Development / integration
- **Problem:** Classic was designed single-core; multicore support added later via OS-Applications, IOC, master/satellite BSW pattern. Distribution is manual and non-trivial: Amdahl's law, synchronization overhead, context switching, resource contention, shared memory, legacy integration. Initial AUTOSAR multicore approach had performance issues (all BSW on one core with RPCs). ETAS case studies show runtime spikes in brake ECUs requiring Com stack splitting.
- **Scenario:** Multicore brake ECU has runtime spikes; engineers must split Com_MainFunctionRx into multiple functions with different periods; redistribute load; risk of incorrect scheduling.
- **Who:** BSW/performance engineers, OS configurators.
- **Root cause:** Static, partitioned scheduling (OSEK) with no dynamic task migration; coarse-grained OS-Application model; legacy single-core design assumptions.
- **Contributing factors:** No dynamic load balancing; shared resources; memory access times.
- **Technical limitations:** Fixed priority scheduling; IOC overhead; master/satellite pattern complexity.
- **Human skill:** Deep OS + timing expertise; scarce.
- **Tool limitations:** Configurators assist but optimization is manual; ETAS provides lock editors.
- **Process limitations:** Optimization done late, after integration; no automated distribution.
- **Frequency/severity:** High for multicore ECUs (increasing).
- **Time/cost impact:** Significant manual tuning; performance bottlenecks late.
- **Downstream effects:** Missed deadlines; safety timing violations.
- **Existing solutions:** ETAS RTA-CAR multicore, lock editors, master/satellite, schedule tables per core.
- **Why insufficient:** Manual; Amdahl limits; requires expert tuning.
- **Automation feasibility:** Medium — automated distribution/search is research area.
- **AI applicability:** Medium.
- **Human approval:** Required for safety timing.
- **Evidence:** ETAS white paper (2025); ETAS case studies; MDH many-core paper (2015); AUTOSAR OS SWS multicore assumptions.
- **Confidence:** High.

### W2-007 — SOME/IP Service Discovery Latency & Jitter

- **Domain:** Middleware (SOME/IP)
- **Lifecycle stage:** Network integration / communication
- **Problem:** SOME/IP-SD subscription latency depends on many factors (TTL, repetitions, multicast); worst-case can be large; Ethernet introduces jitter under congestion; UDP packet loss under heavy load; multicast may be dropped by Docker bridges/unmanaged switches; service discovery failures common in containerized/dev environments.
- **Scenario:** Camera ECU sends emergency braking command over congested Ethernet; delayed 100 ms; too late. Or service discovery fails in Docker due to multicast drop.
- **Who:** Network/middleware engineers, test engineers.
- **Root cause:** SOME/IP designed for automotive but relies on IP multicast/UDP; QoS negotiation dynamic not standardized; network-layer behavior variable.
- **Contributing factors:** Unmanaged switches; container networking; lack of TSN; clock drift.
- **Technical limitations:** No built-in real-time guarantees; QoS static (IEEE 802.1Q priorities) only; dynamic QoS negotiation missing.
- **Human skill:** Network tuning expertise.
- **Tool limitations:** Packet capture/tracing needed; CANoe assists but manual.
- **Process limitations:** Network validation late.
- **Frequency/severity:** Moderate-High.
- **Time/cost impact:** Debugging time; field issues.
- **Downstream effects:** Safety timing; ADAS misalignment.
- **Existing solutions:** TSN + SOME/IP (jitter 17.12 µs lab), CANoe CO, QoS negotiation research, hybrid CAN+Ethernet.
- **Why insufficient:** TSN deployment not universal; lab results not field; discovery failures in dev envs.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** Required for safety timing.
- **Evidence:** SAE 2015-01-0197; SOME/IP SOME/IP blog; Gengini troubleshooting; Sungkyunkwan 2025; LHPES blog.
- **Confidence:** Medium-High.

### W2-008 — DDS Real-Time Determinism & Threading Complexity

- **Domain:** Middleware (DDS)
- **Lifecycle stage:** Network integration / communication
- **Problem:** DDS is a complex multi-threaded middleware (dispatch, listen, liveliness, GC threads) with implementation-specific queuing; achieving real-time guarantees requires careful thread scheduling; latency bounds hard to prove; DDS-to-SOME/IP bridge adds conversion overhead (µs to ms on embedded). DDS QoS rich but configuration complex; memory footprint high for low-end ECUs.
- **Scenario:** ADAS perception pipeline over DDS on Autoware.Auto; communication latencies neglected in local deadline supervision; real-time guarantees unverified.
- **Who:** Middleware engineers, ADAS developers.
- **Root cause:** DDS designed for data-centric pub/sub with rich QoS but not originally for hard real-time embedded; implementation threading dominates latency.
- **Contributing factors:** OS scheduling; garbage collection; QoS misconfiguration.
- **Technical limitations:** No standard real-time profile; thread scheduling OS-dependent; memory footprint.
- **Human skill:** DDS QoS expertise.
- **Tool limitations:** Analysis tools (ECRTS methods) academic; commercial (RTI) expensive.
- **Process limitations:** Real-time verification late.
- **Frequency/severity:** Moderate-High for ADAS/autonomous.
- **Time/cost impact:** Debugging; performance tuning.
- **Downstream effects:** Missed deadlines; safety.
- **Existing solutions:** Real-time DDS profiles, SCHED_DEADLINE, QNX adaptive partitioning, ECRTS latency bounding methods.
- **Why insufficient:** Academic methods not mainstream; config complexity.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** Required.
- **Evidence:** ECRTS 2023 paper; DATE 2021 paper; Mizutani 2018 comparison; MDPI bridge 2025; Vector middleware article 2020.
- **Confidence:** Medium-High.

### W2-009 — Hypervisor/Mixed-Criticality Consolidation Challenges

- **Domain:** Virtualization / hypervisors
- **Lifecycle stage:** Architecture / integration
- **Problem:** Consolidating multiple ECUs onto HPC SoCs via hypervisors introduces certification uncertainty (hypervisor as SEooC under ISO 26262), performance overhead, real-time interference, I/O virtualization timing, and shared-resource safety. Open-source hypervisors (Jailhouse) lack certification; commercial ones (QNX, EB corbos) are costly and vendor-locked. Timing analysis shows hypervisor can match AUTOSAR only "under certain circumstances."
- **Scenario:** Domain controller consolidates ASIL-B ADAS + QM infotainment on one SoC; hypervisor must isolate; certification of hypervisor unclear; I/O virtualization adds latency.
- **Who:** Platform architects, safety engineers.
- **Root cause:** Hypervisor is new software element in safety case; ISO 26262 SEooC approach requires extensive evidence; real-time virtualization is hard.
- **Contributing factors:** Mixed OS (Linux/QNX/AUTOSAR); shared peripherals; cost of certified hypervisors.
- **Technical limitations:** I/O virtualization overhead; scheduling; memory footprint; lack of certified open hypervisors.
- **Human skill:** Virtualization + safety expertise.
- **Tool limitations:** Fault injection testing frameworks academic.
- **Process limitations:** Certification path unclear.
- **Frequency/severity:** Growing with zonal/central compute.
- **Time/cost impact:** Certification cost; re-certification risk.
- **Downstream effects:** Limits consolidation; keeps ECU count high.
- **Existing solutions:** Partitioning hypervisors (Jailhouse), QNX Hypervisor, EB corbos, timing analysis tools.
- **Why insufficient:** Certification gaps; overhead; cost.
- **Automation feasibility:** Low-Medium.
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Evidence:** SAE 2017-01-1621; arxiv 2205.12596; ACM survey 2025; Biondi 2018; HyFAR 2024.
- **Confidence:** Medium-High.

### W2-010 — OTA/FOTA Background Install, Safe-State Activation, Rollback Complexity

- **Domain:** Bootloader / OTA
- **Lifecycle stage:** Release / maintenance
- **Problem:** AUTOSAR FOTA requires background installation without disturbing running SW, activation only in vehicle safe-state, rollback across all affected ECUs on failure, resumption after interruption. Bootloader is out of AUTOSAR scope; bootloader upgradability conflicts with read-only security; most OEMs only OTA infotainment, not safety-critical ECUs (NHTSA 2020). Cross-ECU campaign consistency is hard.
- **Scenario:** OTA campaign updates 5 ECUs; one fails mid-install; must roll back all; activation requires vehicle safe-state (parked); bootloader must recover.
- **Who:** OTA platform engineers, safety, cybersecurity.
- **Root cause:** Bootloader outside standard; safety activation constraints; heterogeneous ECU landscape; security of update channel.
- **Contributing factors:** Battery/power constraints; connectivity; HSM availability; user responsibility shift (NHTSA).
- **Technical limitations:** No standardized bootloader; rollback coordination; safe-state definition OEM-specific.
- **Human skill:** Security + safety + systems.
- **Tool limitations:** FOTA master/target diagnostic services; progress tracking.
- **Process limitations:** Approval cycles; homologation.
- **Frequency/severity:** High for SDV/connected vehicles.
- **Time/cost impact:** Field failure risk; warranty.
- **Downstream effects:** Limits OTA scope; delays features.
- **Existing solutions:** AUTOSAR FOTA requirements, UCM (Adaptive), dual-bank flash, secure boot, HSM.
- **Why insufficient:** Bootloader not standardized; safety-critical OTA rare; complexity.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium (risk prediction).
- **Human approval:** Mandatory.
- **Evidence:** AUTOSAR FOTA EXP R19–R22; NHTSA cybersecurity of firmware updates (2020); Elektrobit OTA webinar; AUTOSAR UCM requirements R20/R24.
- **Confidence:** High.

### W2-011 — Variant Management Complexity and Binding-Time Confusion

- **Domain:** Configuration / variants
- **Lifecycle stage:** Design / integration
- **Problem:** AUTOSAR supports variants at multiple binding times (SystemDesign, CodeGen, PreCompile, Link, PostBuild) with complex condition logic; variant explosion across vehicle lines; consistency of variant conditions across tools; postbuild variants require runtime handling. MathWorks documents many variant configuration paths; errors surface late.
- **Scenario:** OEM has 30 vehicle variants; SWC has variant-dependent ports; RTE generation must bind variants; mismatched variant conditions cause compile/runtime errors.
- **Who:** System/config engineers, developers.
- **Root cause:** Variability is first-class but binding-time semantics are complex; tools handle subsets differently.
- **Contributing factors:** Vehicle variant explosion; manual condition management.
- **Technical limitations:** No single source of truth for variant conditions; tool-specific handling.
- **Human skill:** Variant modeling expertise.
- **Tool limitations:** Round-trip of variants lossy.
- **Process limitations:** Variant planning late.
- **Frequency/severity:** High.
- **Time/cost impact:** Rework; combinatorial testing.
- **Downstream effects:** Integration failures; test explosion.
- **Existing solutions:** Variant blocks (Simulink), system constants, postbuild conditions.
- **Why insufficient:** Tool-specific; complex; late errors.
- **Automation feasibility:** Medium-High.
- **AI applicability:** Medium.
- **Human approval:** Required for variant binding changes.
- **Evidence:** AUTOSAR RTE SWS variant handling; MathWorks AUTOSAR variants docs; AUTOSAR ARXML serialization variability.
- **Confidence:** Medium-High.

### W2-012 — RTE Generation Contract Phase & Consistency Validation Gaps

- **Domain:** RTE
- **Lifecycle stage:** Integration
- **Problem:** RTE generation has a contract phase (RTE contract, BSW scheduler contract, PreBuild data set) that must align across tools; inconsistent component implementations rejected at compile time but semantic errors (mapping, timing) surface later; RTE generation is ECU-specific and must be regenerated per mapping; multi-ECU composition consistency manual.
- **Scenario:** Component mapped to different ECU; RTE regenerated; timing assumptions break; contract phase mismatch between tool vendors causes generation failure.
- **Who:** Integration engineers, RTE configurators.
- **Root cause:** RTE is generated per ECU from system description; contract phase is a manual synchronization point; no continuous consistency checking.
- **Contributing factors:** Tool profile mismatches; late changes.
- **Technical limitations:** Contract phase must be repeated; no incremental regeneration standard.
- **Human skill:** RTE generation expertise.
- **Tool limitations:** Vendors' generators differ in contract handling.
- **Process limitations:** Batch generation; late error detection.
- **Frequency/severity:** Moderate-High.
- **Time/cost impact:** Regeneration cycles; debugging.
- **Downstream effects:** Integration delays.
- **Existing solutions:** RTE contract phase, rejection of invalid configs, tracing.
- **Why insufficient:** Still batch; errors late.
- **Automation feasibility:** High.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence:** AUTOSAR RTE SWS R21-11/R22-11/R24-11; RTE requirements R19–R24.
- **Confidence:** Medium-High.

### W2-013 — Classic OS (OSEK) Scheduling Limitations

- **Domain:** AUTOSAR Classic OS
- **Lifecycle stage:** Development
- **Problem:** Classic OS is OSEK-based: fixed-priority, partitioned per core, no dynamic task migration, no dynamic scheduling, limited to core features (scheduling/sync); multimedia/telematics features (streams, file systems, GUI) are out of scope, requiring other OSs (QNX/Linux) alongside. Schedule tables provide time-triggered execution but require expert configuration; resource locking across cores not supported.
- **Scenario:** Infotainment + safety on same ECU requires AUTOSAR OS + QNX; integration of feature sets into monolithic OS not possible under AUTOSAR.
- **Who:** OS engineers, platform architects.
- **Root cause:** OSEK designed for deeply embedded, single-core, safety-critical control; scope deliberately limited; evolution constrained by backward compat.
- **Contributing factors:** Multicore added later; no dynamic scheduling; safety partitioning.
- **Technical limitations:** Fixed priority; no dynamic migration; resource algorithm not cross-core.
- **Human skill:** OSEK/AUTOSAR OS expertise.
- **Tool limitations:** Configurator-based; manual schedule table design.
- **Process limitations:** OS config treated as static.
- **Frequency/severity:** Moderate-High (increasing with consolidation).
- **Time/cost impact:** Manual tuning; integration complexity.
- **Downstream effects:** Forces hypervisor/multi-OS; complexity.
- **Existing solutions:** OSAL, schedule tables, OS-Applications, memory protection.
- **Why insufficient:** No dynamic capability; scope limits.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** Required.
- **Evidence:** AUTOSAR OS SWS R20-24; OS requirements R20-22; many-core paper; ETAS multicore paper.
- **Confidence:** High.

### W2-014 — Adaptive Dynamic Memory Allocation Non-Determinism

- **Domain:** AUTOSAR Adaptive
- **Lifecycle stage:** Development / safety
- **Problem:** Adaptive allows dynamic memory allocation in C++; standard allocators cause fragmentation and non-deterministic alloc/dealloc times, violating real-time and safety requirements. Spec discusses deterministic allocators and limiting allocation to certain phases, but leaves implementation to vendors; MISRA C++ constraints add complexity.
- **Scenario:** Adaptive application allocates during runtime; heap fragmentation over time; latency spikes; safety case requires deterministic behavior.
- **Who:** Adaptive developers, safety engineers.
- **Root cause:** POSIX/C++ programming model inherently dynamic; safety requires determinism; tension unresolved by standard.
- **Contributing factors:** C++ exceptions/throwing constructors (spec debates); vendor allocator quality.
- **Technical limitations:** No standard deterministic allocator mandated; phase-limited allocation optional.
- **Human skill:** Memory management expertise.
- **Tool limitations:** Static analysis tools (LDRA, QAC) can check but not solve.
- **Process limitations:** Safety analysis of dynamic allocation.
- **Frequency/severity:** Moderate-High for safety-rated Adaptive.
- **Time/cost impact:** Verification; custom allocator development.
- **Downstream effects:** Limits Adaptive safety use.
- **Existing solutions:** Deterministic allocators, phase-limited allocation, MISRA C++.
- **Why insufficient:** Optional; vendor-dependent.
- **Automation feasibility:** Low-Medium.
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Evidence:** AUTOSAR AP SWArchitecturalDecisions (R21-11/R24-11) dynamic memory section; AP Platform Design; SAE 2025-01-8074.
- **Confidence:** High.

### W2-015 — ARXML Round-Trip & Version-Control Friction

- **Domain:** Configuration data management
- **Lifecycle stage:** Configuration / collaboration
- **Problem:** ARXML is text-based and VCS-manageable, but different tools generate tags in different orders; meaningless diffs become massive; semantic merge conflicts; schema version mismatch breaks import; no canonical serialization mandated strongly enough. Large projects (Audi BUSNET) face data volume and performance bottlenecks.
- **Scenario:** Two engineers modify ARXML in different tools; merge produces huge diff; review impossible; integration breaks due to tag reordering.
- **Who:** All AUTOSAR engineers, integrators.
- **Root cause:** Serialization not canonicalized; tool-specific formatting; weak enforcement of interoperability profiles.
- **Contributing factors:** Large file sizes; nested references; version drift.
- **Technical limitations:** No canonical form; diff tools inadequate.
- **Human skill:** ARXML literacy.
- **Tool limitations:** Authoring tools; Git merge conflicts.
- **Process limitations:** Manual review of ARXML.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** Review overhead; integration friction.
- **Downstream effects:** Knowledge silos; errors.
- **Existing solutions:** Normalization tools; AutoSAR.io; profile checkers.
- **Why insufficient:** Not standard; tool-specific.
- **Automation feasibility:** High.
- **AI applicability:** Medium-High.
- **Human approval:** Required for merges.
- **Evidence:** autosar.io guide (2026); Audi BUSNET case (Ascendro 2024); AUTOSAR IOAT; ARXML serialization rules.
- **Confidence:** High.

### W2-016 — Classic-to-Adaptive Migration Requires Full Communication Redesign

- **Domain:** Migration / architecture
- **Lifecycle stage:** Architecture / development
- **Problem:** Migrating Classic applications to Adaptive is not a recompile: requires redesigning data flow from signals to services; safety partitioning on shared SoC; toolchain maturity gap; OSS evaluation for safety; upgrading to newer AP versions requires enormous effort. Most organizations keep Classic for real-time ECUs and add Adaptive for HPC/zonal controllers (hybrid).
- **Scenario:** OEM wants to move body-control logic to central compute; must rewrite communication model, revalidate safety, retrain team.
- **Who:** OEM platform teams, Tier-1s, engineering service providers.
- **Root cause:** Paradigm shift (signal→service, static→dynamic, C→C++, hard→soft real-time) with no automated migration path.
- **Contributing factors:** Legacy code volume; safety re-certification; skill gaps.
- **Technical limitations:** No automated signal-to-service conversion; hypervisor constraints.
- **Human skill:** Both Classic and Adaptive expertise rare.
- **Tool limitations:** Migration tools limited.
- **Process limitations:** Big-bang migration risky; incremental hybrid adds gateway complexity.
- **Frequency/severity:** High (ongoing).
- **Time/cost impact:** Enormous (Cyient).
- **Downstream effects:** Hybrid complexity; platform debt.
- **Existing solutions:** Hybrid architectures, gateways, unified data models, vendor migration services.
- **Why insufficient:** Manual; costly; risky.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium (code understanding, reverse engineering).
- **Human approval:** Mandatory.
- **Evidence:** Cyient whitepaper (2022); Promwad (2025/2026); SystemWeaver (2025).
- **Confidence:** High.

### W2-017 — Adaptive Platform Diagnostics, Health Management & Network Management Maturity

- **Domain:** AUTOSAR Adaptive services
- **Lifecycle stage:** Development / validation
- **Problem:** Several Adaptive functional clusters have documented deficiencies: Network Management only UdpNM, no node detection, cannot be master coordinator; Execution Management resource limitation/fault tolerance incomplete; Software Cluster Integration/Packaging methodology pending; diagnostics maturity. These limit production readiness.
- **Scenario:** Adaptive ECU needs network node detection or master coordination; not available; must implement custom or use Classic gateway.
- **Who:** Adaptive developers, diagnostic engineers.
- **Root cause:** Spec prioritized core communication/execution; advanced features deferred.
- **Contributing factors:** Stabilization focus; vendor divergence.
- **Technical limitations:** Listed above.
- **Human skill:** Spec navigation.
- **Tool limitations:** Tool support for new features lags.
- **Process limitations:** Workarounds accumulate as technical debt.
- **Frequency/severity:** Moderate.
- **Time/cost impact:** Custom development; delays.
- **Downstream effects:** Platform debt; interoperability gaps.
- **Existing solutions:** Vendor extensions; R25-11 improvements.
- **Why insufficient:** Gaps remain; vendor-specific.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence:** AUTOSAR AP Release Overview R24-11/R25-11 known deficiencies; AP methodology R24-11.
- **Confidence:** High.

### W2-018 — Safety Integrity Level Gap: Adaptive Targets ASIL-B (Target D)

- **Domain:** Functional safety
- **Lifecycle stage:** Architecture
- **Problem:** Classic supports up to ASIL-D; Adaptive historically targets ASIL-B (target D) and relies on hypervisor/OS isolation for higher ASIL. Safety-critical functions (braking, steering) remain on Classic; Adaptive use for safety is limited and requires extensive additional evidence.
- **Scenario:** OEM wants to run ASIL-D braking control on central compute HPC; cannot rely on Adaptive alone; must partition with Classic or hypervisor; complexity.
- **Who:** Safety engineers, architects.
- **Root cause:** Adaptive's dynamic, soft-real-time, multi-process model is less suited to hard safety; standard targets lower ASIL.
- **Contributing factors:** Freedom from interference unsolved; hypervisor certification gaps.
- **Technical limitations:** ASIL target; memory/timing interference.
- **Human skill:** Safety + platform.
- **Tool limitations:** Safety analysis tools.
- **Process limitations:** Safety case complexity.
- **Frequency/severity:** High for safety-critical consolidation.
- **Time/cost impact:** Keeps ECU count high; limits SDV benefits.
- **Downstream effects:** Hybrid architectures; gateway complexity.
- **Existing solutions:** Classic for ASIL-D, hypervisors, PHM, resource limits.
- **Why insufficient:** Does not close gap.
- **Automation feasibility:** Low.
- **AI applicability:** Low.
- **Human approval:** Mandatory.
- **Evidence:** AP vs CP comparison (China Day 2024); Safety Overview R24-11; SAE 2025-01-8074.
- **Confidence:** High.

### W2-019 — Skill/Knowledge Gaps in AUTOSAR Methodology and Tooling

- **Domain:** Human factors
- **Lifecycle stage:** All
- **Problem:** AUTOSAR Methodology and Template require deep understanding; "attachment-level" knowledge insufficient; ESOL reports >90% of CP training Q&A asks about compliance boundaries; onboarding takes long; single-person expertise risk; communication gaps between software/hardware/system teams and OEM/Tier-1/supplier.
- **Scenario:** New engineer cannot determine how much AUTOSAR compliance is "enough"; misuse of ARXML; late errors.
- **Who:** Engineers, managers, training.
- **Root cause:** Standard complexity; lack of pragmatic guidance; tool-centric rather than methodology-centric practice ("code generator's servant").
- **Contributing factors:** Vendor training varies; documentation fragmented; rapid spec evolution.
- **Technical limitations:** N/A (human).
- **Human skill:** Core gap.
- **Tool limitations:** Tools assume expert users.
- **Process limitations:** Training not scaled; knowledge silos.
- **Frequency/severity:** Pervasive.
- **Time/cost impact:** Onboarding time; errors.
- **Downstream effects:** Quality; delays.
- **Existing solutions:** Training courses, vendor docs, methodology docs.
- **Why insufficient:** Attachment-level insufficient (ESOL); gaps persist.
- **Automation feasibility:** Medium (AI assistants, knowledge graphs).
- **AI applicability:** High (standards-aware assistants).
- **Human approval:** N/A.
- **Evidence:** ESOL blog (2025); Task.md §5; vendor training.
- **Confidence:** Medium-High.

### W2-020 — Adaptive Application Portability vs. Vendor Implementation Divergence

- **Domain:** AUTOSAR Adaptive
- **Lifecycle stage:** Development / deployment
- **Problem:** Source-code portability (RS_AP_00111) is a stated goal, but vendor implementations differ; minimal C++ version changes (C++17 in R25-11) break older code; platform extension interfaces require review/approval; proprietary Platform Extension APIs create fragmentation; "platform debt" risk.
- **Scenario:** Adaptive app developed against one vendor's AP compiles but fails on another due to platform extension usage or C++ version; migration effort.
- **Who:** Adaptive developers, OEM platform teams.
- **Root cause:** Spec allows platform extensions; vendor differentiation; standard evolution.
- **Contributing factors:** C++ version bumps; extension interface governance.
- **Technical limitations:** No strict conformance test suite publicly enforcing portability.
- **Human skill:** Platform expertise.
- **Tool limitations:** Vendor toolchains.
- **Process limitations:** Conformance testing.
- **Frequency/severity:** Moderate.
- **Time/cost impact:** Porting effort; vendor lock-in.
- **Downstream effects:** Limits multi-sourcing.
- **Existing solutions:** Conformance testing, CAPI, platform extension review.
- **Why insufficient:** Gaps remain.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required.
- **Evidence:** AUTOSAR AP general requirements R24-11; AP release overview R25-11; Promwad (2025); arxiv 2607.05227 (architectural rigidity, vendor coupling).
- **Confidence:** Medium.

---

## 3. Cross-Cutting Analysis

- **Classic is not going away:** SystemWeaver (2025) confirms Classic still strong for real-time/safety; "no plans to replace." Adaptive has not gained traction as hoped; future may become more proprietary or new standards.
- **Hybrid is the reality:** Most vehicles run Classic + Adaptive + non-AUTOSAR (Linux/Android/QNX) with gateways; middleware bridges are critical and immature.
- **Safety limits Adaptive:** ASIL-D stays on Classic; Adaptive for HPC/ADAS/connectivity; hypervisors for consolidation.
- **Config is the bottleneck:** ARXML dominates effort; variant management multiplies; toolchain fragmentation compounds.
- **Standards evolution creates churn:** Annual releases; stabilization attempts; known limitations persist; upgrade cost enormous.

---

## 4. Consolidated Problem-to-Root-Cause Matrix (summary)

| ID | Problem | Primary root cause |
|---|---|---|
| W2-001 | ARXML config complexity | Configuration-instead-of-implementation pushes variability into verbose, version-fragile XML |
| W2-002 | Classic/Adaptive interop | Paradigm mismatch (signal vs service) without automated bridge |
| W2-003 | AP immaturity | Standardization lags market needs; known limitations persist |
| W2-004 | FFI on Adaptive | Dynamic/soft-RT model conflicts with safety determinism |
| W2-005 | Vendor lock-in/toolchain | Standard specifies data format not tool behavior |
| W2-006 | Multicore distribution | Static partitioned scheduling; manual optimization |
| W2-007 | SOME/IP discovery/jitter | IP multicast/UDP; no standardized dynamic QoS/TSN |
| W2-008 | DDS determinism | Multi-threaded middleware; impl-specific scheduling |
| W2-009 | Hypervisor consolidation | Certification uncertainty; real-time virtualization overhead |
| W2-010 | OTA/FOTA complexity | Bootloader out of scope; safe-state activation; cross-ECU rollback |
| W2-011 | Variant management | Multi binding-time semantics; tool-specific handling |
| W2-012 | RTE contract/validation gaps | Per-ECU generation; batch contract phase |
| W2-013 | Classic OS scheduling limits | OSEK fixed-priority; scope limits |
| W2-014 | Adaptive dynamic memory | C++ dynamic alloc vs determinism tension |
| W2-015 | ARXML VCS friction | Non-canonical serialization; tag reordering |
| W2-016 | Migration redesign | Signal→service paradigm shift; no automated path |
| W2-017 | AP service maturity gaps | Deferred features; known deficiencies |
| W2-018 | ASIL level gap | Adaptive targets lower ASIL; FFI unsolved |
| W2-019 | Skill/knowledge gaps | Standard complexity; attachment-level training insufficient |
| W2-020 | Portability vs divergence | Platform extensions; version bumps; conformance gaps |

---

## 5. Evidence & Source References (selected)

- AUTOSAR Classic Platform page; AUTOSAR Adaptive Platform page; AUTOSAR standards docs R19–R25 (OS, RTE, BSW, Gateway, FOTA, ARXML serialization, interoperability of tools, safety overview, platform design, methodology, release overviews).
- SAE 2025-01-8074, "Freedom from Interference Challenges for Adaptive Autosar Based Platform" (Bosch, 2025).
- SAE 2015-01-0197, "Insights on the Configuration and Performances of SOME/IP Service Discovery" (Seyler et al., 2015).
- SAE 2017-01-1621, "Timing Analysis for Hypervisor-based I/O Virtualization in Safety-Related Automotive Systems" (Kohn et al., 2017).
- IEEE ICSA 2026, "An Investigation of the AUTOSAR Adaptive Platform from an Industry Perspective" (Haraldsson, Basu et al., 2026); arxiv 2607.05227.
- ECRTS 2023, "Bounding the Data-Delivery Latency of DDS Messages in Real-Time Applications."
- DATE 2021, "Online latency monitoring of time-sensitive event chains..." (Autoware.Auto/DDS).
- Sungkyunkwan 2025, "Approach for Integrating Real-time Service-Oriented Architecture in Automotive Networks" (SOME/IP+TSN, jitter 17.12 µs).
- MDPI Electronics 2025, "A Dynamic Bridge Architecture for Efficient Interoperability Between AUTOSAR Adaptive and ROS2" (DDS↔SOME/IP latency).
- ETAS white paper, "Optimizing AUTOSAR Multicore Distributions" (2025); ETAS news (2025).
- Cyient whitepaper, "Migrating AUTOSAR Classic to AUTOSAR Adaptive" (2022).
- Promwad, "Automotive middleware: AUTOSAR Adaptive vs custom stacks" (2025); "AUTOSAR Adaptive on Central Compute..." (2026).
- SystemWeaver, "How automotive software development is changing..." (2025).
- ResearchInChina, "Automotive AUTOSAR Platform Research Report" (2024).
- NHTSA, "Cybersecurity of Firmware Updates" (2020).
- ESOL blog, "AUTOSAR導入で Code Generatorのしもべに?" (2025).
- autosar.io, "What is ARXML? Complete Guide" (2026).
- Vector, "User-friendly configuration of AUTOSAR ECUs" (2014); "SOA bridging Classic/Adaptive" (2019); "Middleware protocols" (2020).
- MathWorks, AUTOSAR Blockset docs (variants, limitations, codegen).
- TI, "AUTOSAR Integration Guide" (F29H85x MCAL, 2025).
- Ascendro, "Legacy System Integration: Scaling Audi's BUSNET Platform" (2024).
- ACM Computing Surveys, "Virtualization Technology for Dependable Embedded Systems" (2025).
- arxiv 2205.12596, "Certify the Uncertified..." (hypervisor SEooC).

---

## 6. Research Limitations

- No direct access to proprietary tool implementations; claims based on public docs/vendor marketing, which may overstate capability.
- Quantitative data sparse; most figures are vendor-specific or academic lab results, not industry-wide statistics.
- Adaptive Platform still evolving; findings tied to R24-11/R25-11; future releases may resolve some gaps.
- Scope limited to W2; cross-workstream dependencies (testing, toolchain, skills) referenced but not deeply analyzed here.
- Some sources (Chinese/Japanese) partially interpreted via machine translation.

---

## 7. Open Research Questions

- What is the minimal human-authored ARXML subset if config were derived from higher-level models?
- Can a conformance test suite enforce Adaptive portability across vendors?
- What is the true industry-wide cost of ARXML-related rework?
- Will TSN+SOME/IP achieve field-grade determinism, or will DDS dominate HPC?
- Can hypervisors be certified as SEooC at acceptable cost, enabling true ECU consolidation?
- What fraction of Classic ECUs are realistically migratable to Adaptive within 10 years?
- How much of AUTOSAR config can be safely automated by AI without safety-case invalidation?

---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

