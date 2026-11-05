# Part II — Chapter 8: AUTOSAR and Middleware Engineering

**Book:** *The State of Automotive Software Engineering*  
**Part:** II — Complete Automotive Software Lifecycle  
**Chapter:** 8 — AUTOSAR and Middleware Engineering  
**Source workstreams:** W2 (AUTOSAR & Middleware), W1 (Requirements & System Engineering), W4 (Toolchain & Standards), W5 (Skills & AI)  
**Problem IDs in scope:** W2-001, W2-002, W2-003, W2-004, W2-005, W2-006, W2-007, W2-008, W2-009, W2-010, W2-011, W2-012, W2-013, W2-014, W2-015, W2-016, W2-017, W2-018, W2-019, W2-020, W1-010, W1-013, W1-018, W4-004, W4-005

---

## 8.0 Chapter purpose and problem map

AUTOSAR remains the dominant automotive software standard, but its dual-platform reality (Classic + Adaptive) creates a persistent impedance mismatch. Classic is mature, hard-real-time, signal-based, and ASIL-D capable but rigid; Adaptive is service-oriented, POSIX/C++-based, and intended for HPC/zonal/ADAS, yet still evolving with documented "Known Limitations" across releases. Middleware (SOME/IP, DDS) bridges these worlds but introduces discovery latency, jitter, and real-time determinism problems. Configuration is dominated by ARXML, which is massive, version-fragile, and poorly diffable. Toolchain fragmentation and vendor lock-in persist despite the interoperability standard.

All 20 W2 problem records are in scope for this chapter; selected W1 and W4 records provide cross-cutting context.

| Problem ID | Title | Primary root cause (short) |
|---|---|---|
| W2-001 | ARXML configuration complexity and manual burden | Configuration-instead-of-implementation pushes variability into verbose XML |
| W2-002 | Classic vs Adaptive interoperability & gateway translation | Paradigm mismatch (signal vs service) without automated bridge |
| W2-003 | Adaptive Platform immaturity & stabilisation churn | Standardisation lags market needs; known limitations persist |
| W2-004 | Freedom from interference on Adaptive: unsolved real-time safety | Dynamic/soft-RT model conflicts with safety determinism |
| W2-005 | Vendor lock-in and BSW toolchain interoperability | Standard specifies data format not tool behaviour |
| W2-006 | Multicore distribution complexity in Classic | Static partitioned scheduling; manual optimisation |
| W2-007 | SOME/IP service discovery latency & jitter | IP multicast/UDP; no standardised dynamic QoS/TSN |
| W2-008 | DDS real-time determinism & threading complexity | Multi-threaded middleware; implementation-specific scheduling |
| W2-009 | Hypervisor/mixed-criticality consolidation challenges | Certification uncertainty; real-time virtualisation overhead |
| W2-010 | OTA/FOTA background install, safe-state activation, rollback | Bootloader out of scope; safe-state activation; cross-ECU rollback |
| W2-011 | Variant management complexity and binding-time confusion | Multi binding-time semantics; tool-specific handling |
| W2-012 | RTE generation contract phase & consistency validation gaps | Per-ECU generation; batch contract phase |
| W2-013 | Classic OS (OSEK) scheduling limitations | OSEK fixed-priority; scope limits |
| W2-014 | Adaptive dynamic memory allocation non-determinism | C++ dynamic alloc vs. determinism tension |
| W2-015 | ARXML round-trip & version-control friction | Non-canonical serialisation; tag reordering |
| W2-016 | Classic-to-Adaptive migration requires full communication redesign | Signal→service paradigm shift; no automated path |
| W2-017 | Adaptive platform diagnostics, health management & NM maturity | Deferred features; known deficiencies |
| W2-018 | Safety integrity level gap: Adaptive targets ASIL-B | Adaptive targets lower ASIL; FFI unsolved |
| W2-019 | Skill/knowledge gaps in AUTOSAR methodology and tooling | Standard complexity; attachment-level training insufficient |
| W2-020 | Adaptive application portability vs. vendor implementation divergence | Platform extensions; version bumps; conformance gaps |
| W1-010 | Timing analysis unsolved for AUTOSAR | Abstraction mismatch; cache effects |
| W1-013 | ARXML tool interoperability and configuration drift | No canonical serialisation; tool-specific generators |
| W1-018 | Network architecture and timing of communication | Heterogeneous legacy networks; timing not captured |
| W4-004 | AUTOSAR configuration tool lock-in and licensing cost | Commercial tools proprietary and expensive |
| W4-005 | Configuration diff/merge and variant management in ARXML immature | No cross-tool variant consistency checking |

---

## 8.1 AUTOSAR Classic Platform

Classic AUTOSAR is mature, hard-real-time, signal-based, and supports up to ASIL-D. It dominates deeply embedded control ECUs. Key problems:

- **ARXML configuration complexity** (W2-001, W1-013): massive, deeply nested, reference-heavy files; schema differs across releases; a single typo breaks configuration. ESOL reports that >90% of CP training Q&A asks "how much AUTOSAR compliance is enough" (W2-001, W2-019).
- **Multicore distribution** (W2-006): manual, late, non-trivial; runtime spikes require manual splitting of communication functions.
- **Classic OS (OSEK) scheduling limits** (W2-013): fixed-priority, partitioned, no dynamic migration; scope excludes multimedia/telematics features.
- **Vendor lock-in** (W2-005, W4-004): tools use proprietary exchange formats; MCAL from one vendor (EB tresos) must be configured in that tool; commercial tools are licensed and expensive.
- **RTE generation contract phase** (W2-012): per-ECU generation; contract phase is a manual synchronisation point; semantic errors surface late.
- **Variant management** (W2-011, W4-005): multiple binding times; tool-specific handling; no single source of truth for variant conditions.

ETAS reports >4 billion ECUs worldwide rely on RTA-CAR (Classic AUTOSAR) and built the first multicore AUTOSAR stack for series production in 2009 (W2 executive summary).

---

## 8.2 AUTOSAR Adaptive Platform

Adaptive AUTOSAR targets HPC, zonal, and ADAS use cases with service-oriented, POSIX/C++, dynamically deployable applications. Its problems are:

- **Immaturity and stabilisation churn** (W2-003): even after R24-11 "stabilisation release," known technical deficiencies persist (Network Management UdpNM-only, no node detection, cannot be master coordinator; Persistency; Platform Health Management; Core; Time Sync; Diagnostics; IDS Manager). R25-11 raised minimal C++ to C++17 and released Rust code. Methodology has pending chapters (Software Cluster Integration, Software Packaging) since R24-11. Upgrading to newer versions "requires enormous effort and time" (Cyient, cited in W2-003).
- **Freedom from interference (FFI) unsolved** (W2-004): Classic has robust safety mechanisms (memory/timing/execution interference freedom) for ASIL-D; Adaptive struggles due to dynamic memory allocation, multi-process scheduling, and shared SoC resources. SAE 2025-01-8074 (Bosch) explicitly raises open real-time problems in live projects not solved by existing safety mechanisms.
- **Dynamic memory non-determinism** (W2-014): standard allocators cause fragmentation and non-deterministic alloc/dealloc times; deterministic allocators and phase-limited allocation are optional and vendor-dependent.
- **Diagnostics, health management, and network management maturity** (W2-017): UdpNM-only, no node detection, cannot be master coordinator; limits production readiness.
- **Safety integrity level gap** (W2-018): Classic supports up to ASIL-D; Adaptive historically targets ASIL-B (target D) and relies on hypervisor/OS isolation for higher ASIL. Safety-critical functions (braking, steering) remain on Classic.
- **Application portability vs. vendor divergence** (W2-020): source-code portability is a stated goal (RS_AP_00111), but vendor implementations differ; minimal C++ version changes break older code; proprietary Platform Extension APIs create fragmentation.
- **Skill gaps** (W2-019): deep methodology knowledge required; attachment-level training insufficient.

IEEE ICSA 2026 (Haraldsson, Basu et al.) investigates the AUTOSAR Adaptive Platform from an industry perspective and documents architectural rigidity and vendor coupling (arxiv 2607.05227).

---

## 8.3 Classic vs Adaptive interoperability (W2-002, W2-016)

Classic is signal-based (CAN/LIN/FlexRay, static config); Adaptive is service-based (SOME/IP/DDS over Ethernet, dynamic). Bridging requires gateway ECUs translating signals to services, with manual and error-prone signal-to-service mapping. Incompatible SOME/IP messages require a gateway (W2-002). Migration from Classic to Adaptive is not a recompile: it requires redesigning data flow from signals to services, safety re-partitioning on shared SoC, toolchain maturity catch-up, and retraining (W2-016). Most organisations keep Classic for real-time ECUs and add Adaptive for HPC/zonal controllers (hybrid) (W2 cross-cutting).

**Automation feasibility:** Medium for gateway config generation and consistency checking; Low for paradigm redesign.

---

## 8.4 Middleware: SOME/IP and DDS (W2-007, W2-008, W1-018)

### SOME/IP (W2-007)

SOME/IP-SD subscription latency depends on TTL, repetitions, and multicast; worst-case can be large. Ethernet introduces jitter under congestion; UDP packet loss under heavy load; multicast may be dropped by Docker bridges or unmanaged switches; service discovery failures are common in containerised/dev environments. QoS negotiation is dynamic and not standardised; TSN+SOME/IP achieves 17.12 µs average jitter in lab (Sungkyunkwan 2025), but deployment is not universal and lab results are not field results (W2-007).

### DDS (W2-008)

DDS is a complex multi-threaded middleware (dispatch, listen, liveliness, GC threads) with implementation-specific queuing. Achieving real-time guarantees requires careful thread scheduling; latency bounds are hard to prove. DDS-to-SOME/IP bridge adds conversion overhead (~440 µs median on PC, ~4700 µs on Jetson at 256 KB) (MDPI 2025). DDS QoS is rich but configuration is complex; memory footprint is high for low-end ECUs. Academic latency-bounding methods (ECRTS 2023) are not mainstream (W2-008).

Network architecture and timing across bus/Ethernet is a system-level problem (W1-018) that neither SOME/IP nor DDS resolves alone.

---

## 8.5 Hypervisors and mixed-criticality consolidation (W2-009, W1-016)

Consolidating multiple ECUs onto HPC SoCs via hypervisors introduces certification uncertainty (hypervisor as SEooC under ISO 26262), performance overhead, real-time interference, I/O virtualisation timing, and shared-resource safety (W2-009). Open-source hypervisors (Jailhouse) lack certification; commercial ones (QNX, EB corbos) are costly and vendor-locked. Timing analysis shows hypervisor can match AUTOSAR only "under certain circumstances" (SAE 2017-01-1621). This limits true ECU consolidation and keeps ECU count high, partially offsetting SDV benefits (W1-016).

---

## 8.6 Bootloader, OTA, and software update (W2-010, W4-016)

AUTOSAR FOTA requires background installation without disturbing running software, activation only in vehicle safe-state, rollback across all affected ECUs on failure, and resumption after interruption (W2-010). The bootloader is out of AUTOSAR scope; bootloader upgradability conflicts with read-only security; most OEMs only OTA infotainment, not safety-critical ECUs (NHTSA 2020, flagged as stale — CROSS_CHECK C4). Cross-ECU campaign consistency is hard. UNECE R156 / ISO 24089 add software-update engineering complexity (W4-016). The NHTSA 2020 anchor is 6 years old and presented without staleness caveat in W2; OTA maturity may have advanced since.

---

## 8.7 RTE and BSW (W2-012, W2-005, W2-001)

RTE generation is ECU-specific and must be regenerated per mapping; the contract phase (RTE contract, BSW scheduler contract, PreBuild data set) must align across tools; inconsistent component implementations are rejected at compile time but semantic errors (mapping, timing) surface later; multi-ECU composition consistency is manual (W2-012). BSW toolchain interoperability is weak (W2-005): vendor-specific module definitions, profile mismatches, MCAL configured in EB tresos and only referenced by others. ARXML configuration complexity (W2-001) underpins all of this.

---

## 8.8 ARXML configuration and round-trip (W2-001, W2-015, W1-013, W4-002, W4-003, W4-005)

ARXML is the lingua franca of AUTOSAR. Its problems are covered in depth in Chapter 9. In summary: massive, deeply nested, reference-heavy files; schema drift across releases; non-canonical serialisation causing noisy diffs and merge conflicts; tag reordering by different tools; interface definitions in ARXML drift from Simulink models; large projects (Audi BUSNET) face data volume and performance bottlenecks (W2-001, W2-015, W1-013, W4-002, W4-003, W4-005).

---

## 8.9 Skills and knowledge (W2-019, W5-002, W5-010, W5-011)

AUTOSAR methodology and template require deep understanding; "attachment-level" knowledge is insufficient; ESOL reports >90% of CP training Q&A asks about compliance boundaries; onboarding takes long; single-person expertise risk; communication gaps across disciplines and organisations (W2-019). These are amplified by the dual-platform reality: engineers must now understand both Classic and Adaptive, plus hypervisors, middleware, and zonal architectures (W5-002, W5-010, W5-011).

---

## 8.10 Cross-cutting root causes

1. **Dual-platform impedance mismatch** (Classic signal/static vs. Adaptive service/dynamic) with no automated bridge (W2-002, W2-016).
2. **Adaptive Platform is still stabilising**, with known limitations and enormous upgrade cost (W2-003, W2-017).
3. **Safety (freedom from interference) on Adaptive is unsolved**, limiting Adaptive to ASIL-B domains (W2-004, W2-018).
4. **ARXML configuration dominates effort** and is version-fragile, non-canonical, and poorly diffable (W2-001, W2-015).
5. **Vendor lock-in** persists despite interoperability standards (W2-005, W4-004).
6. **Middleware (SOME/IP, DDS) introduces real-time determinism problems** not fully solved by TSN or real-time profiles (W2-007, W2-008).
7. **Hypervisor consolidation is blocked** by certification uncertainty and overhead (W2-009).
8. **OTA for safety-critical ECUs is rare**, limiting field updatability (W2-010, W4-016).
9. **Skills are scarce** across both platforms and the migration path between them (W2-019, W5-002).

---

## 8.11 Evidence limitations

- Adaptive Platform findings are tied to R24-11/R25-11; future releases may resolve some gaps (W2 §6).
- Vendor claims (ETAS, Vector, Elektrobit) are marketing-positioned and may overstate capability.
- Quantitative data is sparse; most figures are vendor-specific or academic lab results, not industry-wide statistics.
- The NHTSA 2020 OTA claim is stale and presented without staleness caveat (CROSS_CHECK C4).
- Some sources are Chinese/Japanese and partially interpreted via machine translation (W2 §6).

---

## 8.12 Cross-references

- **Chapter 5 (Requirements Engineering):** requirements ambiguity (W1-001) and traceability (W1-003) propagate into configuration.
- **Chapter 6 (System Engineering and Architecture):** timing (W1-010), zonal architecture (W1-016), interface governance (W1-007).
- **Chapter 7 (Software Design and Development):** multicore (W2-006), embedded skills (W5-001), code quality (W4-008–W4-010).
- **Chapter 9 (Configuration and Engineering Data):** ARXML round-trip (W2-015), variant management (W2-011), RTE contract (W2-012).
- **Chapter 11/12 (Testing, HIL, Vehicle):** middleware timing verification (CQ-017, CQ-018), vECU fidelity (W3-004, W3-007, W4-019).
- **Part V (Future Engineering):** SDV, zonal computing, AI-native engineering.

---

*End of Chapter 8.*