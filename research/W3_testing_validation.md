# W3 — Testing and Validation (SIL/MIL/PIL/HIL/Vehicle)

## Research Report: Automotive Software Testing and Validation Problems

**Workstream:** W3 (Testing and validation: MIL/SIL/PIL/HIL/vehicle)
**Parent task:** `Task.md` — DEEP RESEARCH MASTER PROMPT
**Date produced:** 2026-11-05
**Scope:** Global automotive software industry; ICE/hybrid/EV/ADAS/ADS; Classic + Adaptive AUTOSAR; OEM/Tier-1/Tier-2 perspectives.
**Evidence tier:** Verify (Tier 2) — graph not applicable (research subagent); findings grounded in published sources, surveys, case studies, and standards. No invented statistics.

---

## 1. Executive Summary

Testing and validation (V&V) is the dominant cost and schedule driver in automotive software engineering. Across sources:

- **Verifying and validating software is the most time-consuming activity (47%)** among ~600 automotive software professionals surveyed (Perforce 2024 State of Automotive Software Development Report).
- **"Testing efforts are not exhaustive — and we do not have time to test more"** is the top quality concern (24%), with 44% of respondents with 5–10 years experience citing it.
- **Struggling to test efficiently** is the top testing concern (26%); **not testing early enough** (bugs found late) is 2nd (24%); **documenting automated/manual tests for compliance** is 3rd (20%).
- **Software testing accounts for up to ~50% of total software development cost** (Boehm/Basili 2001; Jones 2001; cited in ISO 26262 context).
- **Invalid test failures dominate:** in four automotive system-testing projects, **up to 91% of failed test executions were invalid**; **27–53% of failed executions were linked to unreliable test infrastructure** (ACM ICSE-SEIP 2022).
- **HIL benches cost ~$50k+ each**, handle only ~100–200 tests/day, and cannot parallelize; SiL is orders of magnitude cheaper per test.
- **80% of integration issues surface ~6 months before SOP**; feedback to dev teams traditionally takes 4–6 weeks (KPIT 2025).
- **OEMs face SOP timeline delays of >2 years** due to late identification of software errors in the V-cycle (HCLTech 2025).
- **43.6% of benchmark ADAS/ADS scenarios are flaky** due to simulation nondeterminism in CARLA (2026 study).
- **MIL/SIL are the most challenging XiL loops** for development; SiL must simulate the whole vehicle computationally (rapid review SAST 2024).
- **Traceability across MIL–SIL–HIL is lacking** (SIMEA 2026); vertical requirement-to-test traceability exists but not horizontal across XiL levels.
- **HIL model instability** prevents deterministic CI execution; **plant models must be simplified** (nonlinearities, thermal effects omitted), limiting fidelity in safety-critical scenarios.

Key theme: **testing is the last bottleneck standing between shift-left ambition and SDV reality.** The industry is moving from physical-prototype-centric V-model to layered XiL + cloud + AI, but gaps (traceability, fidelity, standardization of vECUs, scenario coverage/completeness, test data management, fixture verification, test case quality) remain severe and largely unsolved.

---

## 2. Methodology and Scope

### 2.1 Research approach
- Web search across academic (IEEE, ACM, SAE, Springer, MDPI, arXiv), industry (Perforce, dSPACE, NI, AVL, MathWorks, IPG, HCLTech), standards (ISO 26262, ASPICE, ASAM, AUTOSAR, UNECE), and practitioner sources.
- Cross-referenced findings; distinguished facts vs estimates vs hypotheses.
- No fabricated statistics; all quantitative claims cite sources.

### 2.2 Problem record format
Each problem uses the structured fields: ID, Domain, Lifecycle stage, Problem, Scenario, Who, Root cause, Contributing factors, Technical limits, Human/skill limits, Tool limits, Process limits, Frequency/severity, Time/cost impact, Downstream effects, Existing solutions, Why insufficient, Improvement areas, Automation feasibility, AI applicability, Human approval, Evidence, Confidence, Unanswered questions.

### 2.3 Evidence tiers
Tier 2 (Verify) default. Sources dated 2020–2026; older sources (2011–2018) cited only for structural context.

---

## 3. Problem Catalogue

### 3.1 MIL (Model-in-the-Loop) Problems

#### W3-001 — MIL plant/control models are simplified and validated late, masking real-world behavior
- **Domain:** MIL / model-based development
- **Lifecycle stage:** Software design & development / unit verification (SWE.4)
- **Problem:** Plant models used in MIL must be simplified to meet real-time/computational constraints (reduced nonlinearities, omitted thermal effects and wear, reduced-order models). As a result, physical behaviors observed in real vehicles are not fully reproduced, especially in safety-critical scenarios.
- **Scenario:** Control algorithm validated in MIL passes; same algorithm in vehicle exhibits instability due to unmodeled thermal/aging effects.
- **Who:** Control engineers, model-based developers, safety engineers.
- **Root cause:** Computational/real-time constraints force model reduction; plant model validation (V&V of the model itself) is under-resourced and treated as secondary.
- **Contributing factors:** Lack of standardized model validation methodology; no "model V-cycle" routinely run; model fidelity not tracked as a KPI.
- **Technical limits:** Real-time execution constraints; inability to capture second/third-order effects.
- **Human/skill limits:** Shortage of multidisciplinary modeling expertise (mechanical + control + software).
- **Tool limits:** Simulink/MATLAB model fidelity tools; co-simulation interfaces.
- **Process limits:** Model validation not gated; no model credibility assessment in CI.
- **Frequency/severity:** Pervasive in MIL-heavy workflows; severity high for safety-critical (ASIL C/D).
- **Time/cost impact:** Late discovery in HIL/vehicle rework; defect escape to integration.
- **Downstream effects:** False confidence; rework from early stages; safety-case gaps.
- **Existing solutions:** Digital twins; model credibility frameworks; VIL/road-to-rig; model validation V-cycle (dSPACE/Harlammer).
- **Why insufficient:** Regulators still mandate physical testing; model validation requires continuous real-world data feed; high initial investment.
- **Improvement areas:** Automated model validation against recorded data; model fidelity metrics; model versioning in CI.
- **Automation feasibility:** Medium (model validation can be automated; fidelity assessment needs expert judgement).
- **AI applicability:** Medium — AI for model calibration and anomaly detection; hallucination risk for safety claims.
- **Human approval:** Required for safety-relevant model acceptance.
- **Evidence:** SIMEA 2026 paper (simplified models, model instability); IEEE/AV whitepaper (simulation realism gap); IPG whitepaper (models only as good as hardware mimicked).
- **Confidence:** High.
- **Unanswered:** How much model error is acceptable per ASIL? No unified KPIs for model fidelity.

#### W3-002 — MIL test cases are not reused for code verification; disconnect between model and code tests
- **Domain:** MIL/SIL
- **Lifecycle stage:** Software unit verification
- **Problem:** MIL random test cases generated for the model are not reused to verify generated production code; completely different test cases are used for model and code, increasing effort and missing equivalence checking.
- **Scenario:** Model passes MIL; generated code fails SIL on cases never run at MIL; back-to-back comparison missing.
- **Who:** Software developers, test engineers.
- **Root cause:** Tool/workflow gap; test case portability not designed in; manual test case creation dominates.
- **Contributing factors:** Lack of automated test case generation from models; domain terminology ambiguity.
- **Technical limits:** Continuous sample times, multiword fixed-point signals not supported in SIL/PIL (MathWorks limitations).
- **Human/skill limits:** Test engineers lack modeling skills; modelers lack test skills.
- **Tool limits:** Simulink Test harness generation; SIL/PIL Manager restrictions.
- **Process limits:** No mandatory back-to-back (model vs code) equivalence gate in CI.
- **Frequency/severity:** Common in MBD workflows; severity medium-high.
- **Time/cost impact:** Duplicated test effort; late defect discovery.
- **Downstream effects:** Rework; compliance evidence gaps.
- **Existing solutions:** Simulink Test equivalence tests; MathWorks AUTOSAR SIL/PIL verification.
- **Why insufficient:** Tool-specific; partial feature support; manual harness setup.
- **Improvement areas:** Automated equivalence test generation; unified test case format.
- **Automation feasibility:** High for code-generated paths; medium for hand-written.
- **AI applicability:** Medium — AI test generation from models.
- **Human approval:** Required for safety-relevant equivalence claims.
- **Evidence:** MathWorks MIL/SIL/PIL paradigm doc (reuse problem); arXiv test case spec review; BMW MBTCC survey.
- **Confidence:** High.

#### W3-003 — MIL/SIL model instability prevents deterministic CI execution
- **Domain:** MIL/SIL/HIL
- **Lifecycle stage:** CI/CT
- **Problem:** Complex plant models in HIL exhibit unstable behavior during initialization depending on initial conditions and real-time delays, requiring multiple restarts before stable state; this breaks deterministic automated test execution in CI.
- **Scenario:** Overnight CI regression fails because ECU not operational after flashing new software; flaky test results.
- **Who:** CI/CT engineers, validation engineers.
- **Root cause:** Real-time delay sensitivity; model initialization transients; lack of stabilization procedures.
- **Contributing factors:** Simplified models; hardware timing jitter; no health-check gating.
- **Technical limits:** Real-time constraints; model stiffness.
- **Human/skill limits:** Limited expertise in numerical stabilization.
- **Tool limits:** HIL real-time OS; model compilation.
- **Process limits:** No standardized "ECU ready" handshake; no retry/quarantine policy.
- **Frequency/severity:** Frequent in complex HIL; severity high for CI adoption.
- **Time/cost impact:** Wasted CI cycles; manual intervention; reduced automation coverage.
- **Downstream effects:** Low CI trust; fallback to manual testing.
- **Existing solutions:** Health checks; retry policies; model warm-up procedures; Rapid HIL.
- **Why insufficient:** Not standardized; vendor-specific.
- **Improvement areas:** Deterministic initialization contracts; model health telemetry; quarantine + auto-retry.
- **Automation feasibility:** Medium-High.
- **AI applicability:** Medium — anomaly detection on init telemetry.
- **Human approval:** For safety-critical test gating.
- **Evidence:** SIMEA 2026 (model instability); PulseMo metrics.
- **Confidence:** High.

---

### 3.2 SIL (Software-in-the-Loop) Problems

#### W3-004 — SiL cannot test all functionalities that HiL can; hardware coupling limits vECU fidelity
- **Domain:** SIL/vECU
- **Lifecycle stage:** Software integration verification (SWE.5)
- **Problem:** SiL cannot test all functionalities a HiL can. Hardware-specific tests (timing, electrical effects, bus latency, interrupts, multi-core parallelism) remain majority. vECU creation requires considerable effort with strong hardware coupling or platform-specific code; host-compiled is faster but less accurate; target-compiled is slower but more accurate.
- **Scenario:** SiL passes; HiL fails on interrupt timing; bug escapes to vehicle.
- **Who:** Software/integration engineers, test engineers.
- **Root cause:** Abstraction gap between virtual and physical; host PC single-task execution vs MCU microsecond real-time; analog signal response <1ms hard to simulate.
- **Contributing factors:** No uniform vECU approach; proprietary formats (Silver, VEOS, Simulink); varying quality.
- **Technical limits:** Multi-core parallelism; interrupts; analog; microsecond timing; cloud scaling vs fidelity trade-off.
- **Human/skill limits:** vECU development expertise scarce.
- **Tool limits:** vECU levels 0–4; commercial tools up to Level 3; Level 4 needs custom peripheral models.
- **Process limits:** No standardized vECU exchange format (ProSTEP SmartSE requirements vs. reality).
- **Frequency/severity:** Pervasive; severity high for MCAL/CDD and timing-critical code.
- **Time/cost impact:** Duplicated HiL effort; limited SiL ROI.
- **Downstream effects:** Late hardware-dependent defect discovery.
- **Existing solutions:** Level-4 vECU (Renode, QEMU); ProSTEP SmartSE vECU standardization; AUTOSAR vECU requirements.
- **Why insufficient:** Level-4 custom per MCU; no universal standard; IP protection; scalability vs fidelity trade-off; false positives at high parallelism (Robot Framework slower than vECU).
- **Improvement areas:** Standardized vECU container (FMI 3.0 + bus interface); cloud-native Level-4; automated hardware-dependency classification.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium — AI for vECU model generation; verification needed.
- **Human approval:** Required for hardware-dependent claims.
- **Evidence:** MDPI vECU evaluation; Synopsys vECU WP; ProSTEP SmartSE; MDPI Level-4 vECU paper (false positives at 40+ jobs).
- **Confidence:** High.

#### W3-005 — Temporal synchronization in hybrid SiL/HiL environments
- **Domain:** SiL/HiL co-simulation
- **Lifecycle stage:** Integration testing
- **Problem:** Simulators operate on different time bases (virtual vs real time), making precise co-simulation difficult; leads to communication delays and data loss, affecting test reliability.
- **Scenario:** Multi-ECU co-simulation with mixed real-time/virtual nodes produces intermittent message loss; false failures.
- **Who:** Integration/test engineers.
- **Root cause:** Heterogeneous time bases; no unified time synchronization standard across tools.
- **Contributing factors:** Vendor-specific simulators; network jitter.
- **Technical limits:** Real-time constraints; distributed clocks.
- **Tool limits:** FMI co-simulation; tool interoperability.
- **Process limits:** No standard time-sync contract.
- **Frequency/severity:** Common in multi-ECU; severity medium-high.
- **Time/cost impact:** Flaky results; manual triage.
- **Downstream effects:** Loss of trust in automation.
- **Existing solutions:** Relative time sync; formal analysis of time effects (SAST 2024); FMI 3.0.
- **Why insufficient:** Still vendor-fragmented; no industry-wide adoption.
- **Improvement areas:** Standardized time-sync protocol; deterministic replay.
- **Automation feasibility:** Medium.
- **AI applicability:** Low-Medium.
- **Human approval:** For safety-relevant timing claims.
- **Evidence:** SIMEA 2026 (temporal sync); SAST rapid review.
- **Confidence:** High.

#### W3-006 — SiL simulation inputs limited; realistic stimuli hard to generate
- **Domain:** SIL
- **Lifecycle stage:** Software unit/integration verification
- **Problem:** A weakness of SiL is limitation in simulating inputs to the system. Generating realistic, varied input stimuli (sensor signals, bus traffic) is hard; GAN-based approaches explored but not mainstream.
- **Scenario:** SiL tests use idealized inputs; real sensor noise/latency not captured; defects escape.
- **Who:** Test engineers, developers.
- **Root cause:** Lack of realistic environment models; input space explosion.
- **Contributing factors:** Simplified plant models; no standardized scenario databases for component-level.
- **Technical limits:** Input space dimensionality; real-time.
- **Human/skill limits:** Scenario engineering expertise.
- **Tool limits:** Stimulus generation tools.
- **Process limits:** No systematic input coverage criteria.
- **Frequency/severity:** Pervasive; severity medium.
- **Time/cost impact:** Incomplete coverage; false confidence.
- **Downstream effects:** Late defect discovery in HiL/vehicle.
- **Existing solutions:** GAN-based stimuli (research); scenario databases; data replay.
- **Why insufficient:** Research-stage; not integrated.
- **Improvement areas:** Automated realistic stimulus generation from recorded data; coverage metrics.
- **Automation feasibility:** Medium-High (with data).
- **AI applicability:** High — generative models for stimuli.
- **Human approval:** For safety-relevant stimuli.
- **Evidence:** SAST rapid review (SiL input limitation, GAN exploration); ASAM scenario standardization.
- **Confidence:** Medium-High.

#### W3-007 — vECU standardization gap (FMI vs AUTOSAR; bus communication representation)
- **Domain:** vECU / toolchain
- **Lifecycle stage:** Integration testing
- **Problem:** Lack of consolidated standardization for vECUs. Initiatives based on FMI and AUTOSAR exist but limitations remain in bus communication representation and interoperability among vendor tools. Heterogeneity increases costs and complexity in OEM-supplier collaboration.
- **Scenario:** Supplier delivers vECU in proprietary format; OEM cannot run in own SiL farm; rework/re-creation.
- **Who:** OEMs, Tier-1 suppliers, tool vendors.
- **Root cause:** No single standard covering all vECU levels + bus/network simulation + IP protection + multi-simulator execution; FMI too simple for bus comms; AUTOSAR too complex for non-AUTOSAR.
- **Contributing factors:** Vendor lock-in; IP protection requirements; time pressure.
- **Technical limits:** Complex data exchange; real-time vs non-real-time platforms.
- **Human/skill limits:** Standardization expertise.
- **Tool limits:** Vendor-specific containers.
- **Process limits:** No enforced standard in procurement.
- **Frequency/severity:** Pervasive in multi-party; severity high.
- **Time/cost impact:** Duplicated vECU creation; integration delays.
- **Downstream effects:** Reduced SiL reuse; late integration.
- **Existing solutions:** ProSTEP SmartSE requirements (9 reqs); AUTOSAR vECU; FMI 3.0; ASAM.
- **Why insufficient:** Requirements not fully met; "enormous time pressure"; no short-term roadmap clarity.
- **Improvement areas:** Industry-wide vECU standard with bus interface; procurement mandates.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** Standards governance.
- **Evidence:** SIMEA 2026; ProSTEP SmartSE V4; MDPI vECU eval (no uniform approach across 5 ECUs).
- **Confidence:** High.

---

### 3.3 PIL (Processor-in-the-Loop) Problems

#### W3-008 — PIL limitations: unsupported modeling features and tool-specific restrictions
- **Domain:** PIL
- **Lifecycle stage:** Software unit verification
- **Problem:** PIL (and SIL) have significant limitations: no local data stores, no multiword fixed-point signals across boundaries, no continuous sample times at component boundary (top-model), no tunable dialog box parameters, no code coverage for SIL blocks (MathWorks), conditional execution restrictions, sample-time-independent model constraints. PIL block code coverage depends on target connectivity.
- **Scenario:** PIL run fails or silently skips coverage due to unsupported feature; team unaware of gaps.
- **Who:** Developers using MathWorks/AUTOSAR toolchains.
- **Root cause:** Code generation/runtime instrumentation constraints; host-target mismatch.
- **Contributing factors:** Tool-specific feature matrices; poor documentation awareness.
- **Technical limits:** As listed.
- **Human/skill limits:** Engineers not aware of limitations.
- **Tool limits:** Embedded Coder, AUTOSAR target.
- **Process limits:** No tool-limitation checklist in test planning.
- **Frequency/severity:** Frequent in complex models; severity medium.
- **Time/cost impact:** Rework; false coverage claims.
- **Downstream effects:** Compliance evidence gaps.
- **Existing solutions:** MathWorks documentation; workarounds; different SIL/PIL modes.
- **Why insufficient:** Tool-specific; requires manual adaptation.
- **Improvement areas:** Automated tool-capability checking; standard feature matrix.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** For coverage claims.
- **Evidence:** MathWorks SIL/PIL limitations doc; AUTOSAR SIL/PIL verification doc.
- **Confidence:** High.

#### W3-009 — PIL reveals compiler/processor faults but is slow and hardware-dependent
- **Domain:** PIL
- **Lifecycle stage:** Software unit verification
- **Problem:** PIL validates object code on production target hardware, revealing compiler/processor/architecture faults, but is slower than SIL, hardware-dependent, and harder to parallelize; target connectivity for code coverage varies.
- **Scenario:** PIL campaign takes days on single target; cannot scale for regression.
- **Who:** Developers, test engineers.
- **Root cause:** Physical target dependency; serial execution.
- **Contributing factors:** Limited target boards; probe/debug setup complexity.
- **Technical limits:** Target throughput; debug probe bandwidth.
- **Human/skill limits:** Target toolchain expertise.
- **Tool limits:** Target connectivity.
- **Process limits:** No PIL-in-CI strategy.
- **Frequency/severity:** Common; severity medium.
- **Time/cost impact:** Slow feedback; limited coverage.
- **Downstream effects:** Defects escape to HiL.
- **Existing solutions:** Instruction set simulators; cloud emulation; rapid PIL.
- **Why insufficient:** ISS fidelity gaps; cloud target emulation costly/complex.
- **Improvement areas:** Scalable target farms; ISS validation; cloud PIL.
- **Automation feasibility:** Medium.
- **AI applicability:** Low.
- **Human approval:** For safety-critical object code claims.
- **Evidence:** MathWorks MIL/SIL/PIL doc (PIL reveals compiler faults); NI HIL fundamentals.
- **Confidence:** High.

---

### 3.4 HIL (Hardware-in-the-Loop) Problems

#### W3-010 — HIL cost and scalability: expensive, slow, not parallelizable
- **Domain:** HIL
- **Lifecycle stage:** System/integration verification
- **Problem:** Enterprise HIL systems cost ~$50k+ each (up to much more), handle ~100–200 tests/day per bench, cannot parallelize on a single bench; scaling requires ~10x benches/engineers/capital. Conventional system HIL is too expensive, too slow, not scalable; becomes bottleneck in validation.
- **Scenario:** Team needs 10,000 tests; one bench takes ~50 days; cannot add benches fast enough.
- **Who:** Validation managers, test engineers, OEMs/Tier-1s.
- **Root cause:** Physical hardware dependency; capital-intensive; single-DUT-per-bench architecture.
- **Contributing factors:** Long procurement; lab space; maintenance; specialized staff.
- **Technical limits:** I/O, real-time compute, ECU count per bench.
- **Human/skill limits:** HIL setup expertise; model fidelity.
- **Tool limits:** Vendor platforms (dSPACE, NI, OPAL-RT).
- **Process limits:** No shared/scalable resource model; poor scheduling.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Capital ~$50k+/bench; throughput wall; schedule risk.
- **Downstream effects:** Late validation; SOP delays.
- **Existing solutions:** Rapid HIL (IPG); HIL farms (Aliaro, Renault dSPACE HELIUM); shared scalable platforms (AVL Nexus); cloud-based HIL (research).
- **Why insufficient:** Still hardware-bound; reconfiguration time; cloud HIL latency/cybersecurity/compliance concerns; legacy integration.
- **Improvement areas:** vECU-first filtering; HIL farms with swappable I/O; centralized scheduling (test.guide, PulseMo).
- **Automation feasibility:** Medium-High (scheduling/automation); hardware remains.
- **AI applicability:** Medium — scheduling optimization; predictive maintenance.
- **Human approval:** For safety-critical HIL acceptance.
- **Evidence:** NTQ Europe cost analysis; IPG Rapid HIL; HIL challenges article; AVL Nexus; Aliaro HIL farm; Renault/dSPACE.
- **Confidence:** High.

#### W3-011 — HIL setup latency, reconfiguration, and variant explosion
- **Domain:** HIL
- **Lifecycle stage:** Test environment preparation
- **Problem:** HIL test benches need frequent updates to support new hardware/test scenarios/variants; reconfiguration can take weeks; supporting multiple vehicle platforms/incarnations requires version control of the test tool; variant explosion (drive/gearbox/vehicle combos) makes manual env creation unmanageable.
- **Scenario:** New vehicle program requires weeks to reconfigure HIL; test start delayed.
- **Who:** HIL engineers, test managers.
- **Root cause:** Monolithic bench customization; no automated env build; manual signal mapping.
- **Contributing factors:** Lack of variant management; tool fragmentation.
- **Technical limits:** I/O reconfiguration; model assembly.
- **Human/skill limits:** Specialized HIL engineers.
- **Tool limits:** Configuration tools; SYNECT/HELIUM.
- **Process limits:** Manual env creation; no CI for test env.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Weeks per reconfiguration; idle benches.
- **Downstream effects:** Reduced test throughput; capacity planning failures.
- **Existing solutions:** Ford SYNECT data-driven HIL; Renault HELIUM; dSPACE variant-based automation; Rapid HIL (reconfigure in a day).
- **Why insufficient:** Requires significant upfront investment and toolchain adoption; vendor lock-in.
- **Improvement areas:** Automated env build pipelines; infrastructure-as-code for HIL; variant-aware scheduling.
- **Automation feasibility:** High (with tooling investment).
- **AI applicability:** Medium — config generation.
- **Human approval:** For env acceptance.
- **Evidence:** Ford/dSPACE case; Renault/dSPACE; dSPACE variant automation; HIL challenges article.
- **Confidence:** High.

#### W3-012 — HIL model fidelity: simplified plant models and missing second/third-order effects
- **Domain:** HIL
- **Lifecycle stage:** System verification
- **Problem:** HIL models are only as good as hardware mimicked; capturing second/third-order effects and dynamic behaviors makes models complex and demands powerful simulation hardware; inaccurate/incomplete models lead to misleading test outcomes.
- **Scenario:** HIL passes; vehicle fails due to unmodeled actuator dynamics.
- **Who:** Validation engineers, model engineers.
- **Root cause:** Computational/real-time constraints; model simplification; cost.
- **Contributing factors:** Lack of high-fidelity component data; supplier model quality.
- **Technical limits:** Real-time execution; I/O bandwidth.
- **Human/skill limits:** Multidisciplinary modeling.
- **Tool limits:** Real-time simulators.
- **Process limits:** No model credibility assessment.
- **Frequency/severity:** Pervasive; severity high for safety-critical.
- **Time/cost impact:** False passes; late rework.
- **Downstream effects:** Safety-case gaps; recalls.
- **Existing solutions:** Digital twins; component characterization; road-to-rig; component test benches.
- **Why insufficient:** Requires physical characterization; expensive; time-consuming.
- **Improvement areas:** Automated model calibration from test data; fidelity KPIs; hybrid testing.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium-High — AI for model calibration/surrogate models.
- **Human approval:** Required.
- **Evidence:** HIL challenges article; IPG whitepaper; IPG virtual endurance.
- **Confidence:** High.

#### W3-013 — HIL sensor/actuator physical constraints and latency/desynchronization
- **Domain:** HIL
- **Lifecycle stage:** System verification
- **Problem:** Sensors, actuators, other hardware have physical/signal constraints; some real-world behavior cannot be fully replicated; even minor latency or signal desynchronization skews results and masks faults. Hardware must coordinate multiple ECUs/protocols avoiding timing mismatches.
- **Scenario:** Timing-sensitive function passes HIL but fails in vehicle due to bus latency differences.
- **Who:** HIL engineers.
- **Root cause:** Lab hardware cannot replicate all physical signal characteristics; real-time latency.
- **Contributing factors:** Incomplete characterization; cost of high-fidelity I/O.
- **Technical limits:** Signal fidelity; latency; jitter.
- **Human/skill limits:** Expertise to identify and close gaps.
- **Tool limits:** I/O modules; real-time OS.
- **Process limits:** No documented "simulation limits" per bench.
- **Frequency/severity:** Common; severity medium-high.
- **Time/cost impact:** Misleading results; engineering time to uncover gaps.
- **Downstream effects:** False confidence; late discovery.
- **Existing solutions:** High-fidelity I/O; characterization; documented limitations.
- **Why insufficient:** Cost; cannot close all gaps.
- **Improvement areas:** Standardized fidelity documentation; gap-tracking per bench.
- **Automation feasibility:** Low-Medium.
- **AI applicability:** Low.
- **Human approval:** Required.
- **Evidence:** HIL challenges article.
- **Confidence:** High.

#### W3-014 — HIL fixture/test-bench verification is itself difficult; "quality of test fixture" unquantified
- **Domain:** HIL
- **Lifecycle stage:** Test infrastructure qualification
- **Problem:** The test and verification of the test fixture becomes more difficult with HIL complexity; making tangible statements about quality/limits of HIL takes time and effort; understanding veracity and limits of HIL is nontrivial. No standardized fixture qualification.
- **Scenario:** HIL reports pass; later found bench wiring fault caused false pass; no fixture-level evidence.
- **Who:** HIL engineers, quality.
- **Root cause:** Fixture complexity; lack of fixture V&V standards; no "golden" fixture data.
- **Contributing factors:** Configuration drift; manual wiring.
- **Technical limits:** Fixture complexity.
- **Human/skill limits:** Specialized knowledge.
- **Tool limits:** No fixture test tools.
- **Process limits:** No fixture qualification gate.
- **Frequency/severity:** Common; severity medium-high.
- **Time/cost impact:** False results; rework.
- **Downstream effects:** Loss of trust; audit findings.
- **Existing solutions:** ASAM XIL; configuration management; golden test data.
- **Why insufficient:** Not standardized; manual.
- **Improvement areas:** Automated fixture health checks; golden data comparison; fixture V&V standard.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium — anomaly detection.
- **Human approval:** Required.
- **Evidence:** HIL challenges article ("veracity and limits of the HIL").
- **Confidence:** Medium-High.

#### W3-015 — Cloud-based HIL: latency, cybersecurity, compliance, legacy integration
- **Domain:** Cloud HIL
- **Lifecycle stage:** System verification
- **Problem:** Cloud-based HIL offers virtualization/scalability but faces latency constraints for real-time, cybersecurity and regulatory compliance concerns, integration complexity with legacy systems, connectivity dependence.
- **Scenario:** OEM wants cloud HIL for global teams; real-time constraints and compliance block adoption.
- **Who:** IT, validation, security.
- **Root cause:** Real-time requirements vs cloud latency; regulatory mandate for physical testing; legacy.
- **Contributing factors:** Data sovereignty; safety certification.
- **Technical limits:** Latency; bandwidth; determinism.
- **Human/skill limits:** Cloud + safety expertise.
- **Tool limits:** Cloud real-time platforms.
- **Process limits:** Compliance acceptance.
- **Frequency/severity:** Emerging; severity medium.
- **Time/cost impact:** Delayed cloud adoption; continued capital spend.
- **Downstream effects:** Slower scaling.
- **Existing solutions:** Cloud-based HIL research; hybrid edge-cloud.
- **Why insufficient:** Not yet production-ready for safety-critical real-time.
- **Improvement areas:** Edge-real-time + cloud orchestration; compliance frameworks.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Required for safety/compliance.
- **Evidence:** SIMEA 2026 (cloud HIL challenges); HCLTech (regulatory concerns).
- **Confidence:** Medium.

#### W3-016 — HIL test environment management: idle benches, scheduling, utilization
- **Domain:** HIL operations
- **Lifecycle stage:** Test execution management
- **Problem:** HIL labs suffer underutilized/idle benches, fragmented scheduling across teams, manual regression, limited visibility into bench health/availability, repeated config/troubleshooting, increasing operational costs.
- **Scenario:** Benches idle while other teams queue; no centralized scheduling; utilization low.
- **Who:** Lab managers, test engineers.
- **Root cause:** Fragmented tooling; no centralized orchestration; manual scheduling.
- **Contributing factors:** Departmental silos; proprietary tools.
- **Technical limits:** Heterogeneous benches.
- **Human/skill limits:** Lab ops expertise.
- **Tool limits:** Scheduling tools (test.guide, PulseMo, SYNECT).
- **Process limits:** No KPI-driven lab management.
- **Frequency/severity:** Pervasive; severity medium.
- **Time/cost impact:** Wasted capital; delays.
- **Downstream effects:** Reduced throughput.
- **Existing solutions:** test.guide, PulseMo Edge, SYNECT, HELIUM.
- **Why insufficient:** Requires adoption/investment; integration effort.
- **Improvement areas:** Centralized orchestration; utilization KPIs; remote access.
- **Automation feasibility:** High.
- **AI applicability:** Medium — predictive scheduling.
- **Human approval:** For lab policy.
- **Evidence:** PulseMo; test.guide; HIL farm docs.
- **Confidence:** High.

---

### 3.5 Vehicle-Level Validation Problems

#### W3-017 — Physical proving-ground testing scope explosion and cost
- **Domain:** Vehicle testing
- **Lifecycle stage:** System/vehicle validation
- **Problem:** Regulatory tightening (GSR, Euro NCAP, UNECE) increases scope/cost of proving-ground testing for ADAS/ADS; >60 dynamic test case variations per regulation set, doubling if all cases included; physical testing remains indispensable for type approval.
- **Scenario:** ADAS validation requires hundreds of proving-ground runs; budget/time overrun.
- **Who:** Validation engineers, homologation teams.
- **Root cause:** Regulatory expansion; scenario combinatorics; physical test dependency.
- **Contributing factors:** Variant explosion; regional differences.
- **Technical limits:** Track capacity; weather/season.
- **Human/skill limits:** Test drivers; scenario engineers.
- **Tool limits:** Track instrumentation.
- **Process limits:** No systematic scenario reduction methodology.
- **Frequency/severity:** Pervasive for ADAS/ADS; severity high.
- **Time/cost impact:** Weeks of track time; high cost.
- **Downstream effects:** SOP delays; homologation risk.
- **Existing solutions:** Complex scenario construction methodology (ZalaZONE); virtual testing; real-world data collection.
- **Why insufficient:** Regulators still require physical end testing; virtual results need credibility analysis; standardization of simulation credibility lacking.
- **Improvement areas:** Scenario prioritization; combined complex scenarios; virtual type-approval acceptance.
- **Automation feasibility:** Medium (scenario generation); physical execution remains.
- **AI applicability:** Medium — scenario selection.
- **Human approval:** Regulators.
- **Evidence:** Future Transportation 2025 (GSR, >60 variations); Euro NCAP feature; SAE 2025-01-8052.
- **Confidence:** High.

#### W3-018 — ADAS/ADS scenario coverage and completeness unknown; ODD coverage metrics immature
- **Domain:** ADAS/ADS validation
- **Lifecycle stage:** System/vehicle validation
- **Problem:** Impossible to cover all real-world situations; Euro NCAP "can barely scratch the surface"; scenario-based testing can only cover parameters in ODD spec; many parameters/critical scenarios (fault injection) not in ODD specs; coverage metrics for scenario databases are research-stage; 100% coverage claims need conditions.
- **Scenario:** System passes all Euro NCAP tests but fails in untested real-world edge case; recall.
- **Who:** ADAS/ADS developers, safety engineers.
- **Root cause:** Infinite scenario space; ODD specification limits; lack of standardized scenario validation.
- **Contributing factors:** Data acquisition scale; scenario parameter explosion.
- **Technical limits:** Computational; data volume.
- **Human/skill limits:** Scenario engineering.
- **Tool limits:** Scenario databases; coverage tools.
- **Process limits:** No agreed completeness criterion.
- **Frequency/severity:** Pervasive; severity very high.
- **Time/cost impact:** Unbounded testing effort; residual risk.
- **Downstream effects:** Safety incidents; regulatory pushback.
- **Existing solutions:** Scenario databases; coverage metrics (arXiv 2024); ASAM OpenSCENARIO; critical scenario identification (ACM TOSEM 2024).
- **Why insufficient:** Coverage metrics research-stage; industry lacks joint effort; tools/platforms lacking; critical scenario identification approaches have limitations.
- **Improvement areas:** Standardized ODD coverage criteria; scenario completeness metrics; fault-injection scenario integration.
- **Automation feasibility:** Medium-High (generation); completeness judgement human.
- **AI applicability:** High — scenario generation/mining; risk of hallucinated scenarios.
- **Human approval:** Safety case; regulators.
- **Evidence:** Euro NCAP (Bernal); ACM TOSEM 2024; arXiv coverage metrics; IEEE STV2 whitepaper; SAE 2025-01-8052.
- **Confidence:** High.

#### W3-019 — Simulation nondeterminism/flakiness in ADS testing
- **Domain:** ADAS/ADS simulation
- **Lifecycle stage:** System verification
- **Problem:** 43.6% of benchmark scenarios flaky due to nondeterministic executions in CARLA; test scenarios produce varying outcomes; breaks reproducibility and trust. MetaDrive shows high repeatability, indicating simulator-dependent.
- **Scenario:** Same scenario passes/fails across reruns; CI cannot gate on flaky tests.
- **Who:** ADS developers, CI engineers.
- **Root cause:** Simulator internal nondeterminism (physics, rendering, multi-threading); randomness in events.
- **Contributing factors:** Lack of deterministic execution contracts; floating point; scheduling.
- **Technical limits:** Simulator architecture.
- **Human/skill limits:** Debugging nondeterminism.
- **Tool limits:** CARLA vs MetaDrive differences.
- **Process limits:** No flakiness tracking/quarantine.
- **Frequency/severity:** 43.6% (CARLA) — significant; severity high.
- **Time/cost impact:** Repeated runs; false confidence; wasted compute.
- **Downstream effects:** Unreliable safety evidence.
- **Existing solutions:** Deterministic mode configs; multiple reevaluations; MetaDrive repeatability; DeFT deterministic test extraction.
- **Why insufficient:** Not standardized; simulator-specific; extraction adds effort.
- **Improvement areas:** Determinism standards for simulators; flakiness dashboards; deterministic replay.
- **Automation feasibility:** Medium-High.
- **AI applicability:** Medium — flakiness prediction.
- **Human approval:** For safety evidence.
- **Evidence:** Osikowicz 2026 (43.6% flaky); DeFT ICSE 2026.
- **Confidence:** High.

#### W3-020 — Test scenario reuse is unsafe for ADAS/ADS (system-dependent quality)
- **Domain:** ADAS/ADS testing
- **Lifecycle stage:** Regression testing
- **Problem:** Concrete test scenarios are system-dependent; reusing recorded/catalog scenarios for new system versions may not trigger relevant behavior; faulty behavior can stay unrevealed. Naive reuse cannot guarantee test quality; regression testing methodology needs rethinking.
- **Scenario:** Best scenario for version A fails to reveal bug in version B; regression passes falsely.
- **Who:** Test engineers, safety engineers.
- **Root cause:** System behavior changes alter scenario effectiveness; scenario quality not invariant.
- **Contributing factors:** Lack of system-specific scenario generation; reliance on catalogs.
- **Technical limits:** Scenario space.
- **Human/skill limits:** Scenario quality assessment.
- **Tool limits:** Scenario management.
- **Process limits:** Reuse assumed without quality verification.
- **Frequency/severity:** Pervasive in ADS; severity high.
- **Time/cost impact:** False regression confidence; missed defects.
- **Downstream effects:** Safety risk; late discovery.
- **Existing solutions:** System-specific scenario generation; quality metrics; re-evaluation.
- **Why insufficient:** Generation cost; no standard quality metric.
- **Improvement areas:** Automated scenario quality scoring; regeneration triggers on system change.
- **Automation feasibility:** Medium.
- **AI applicability:** High — scenario generation/optimization.
- **Human approval:** For safety regression claims.
- **Evidence:** TUM paper (reusing concrete scenarios generally bad idea).
- **Confidence:** High.

#### W3-021 — Environmental/EMC/durability validation remains largely physical and slow
- **Domain:** Environmental validation
- **Lifecycle stage:** Vehicle/system validation
- **Problem:** EMC, climate, durability, vibration testing require physical prototypes, environmental chambers, proving grounds; physical dissection destructive; virtual robust design emerging but not replacing; test-to-pass qualification provides little engineering info; extreme conditions (-40 to +85°C, etc.) hard to virtualize.
- **Scenario:** Climate chamber test runs 1000–2500 hours; virtual replication in progress but not validated.
- **Who:** Reliability/robustness engineers, OEMs.
- **Root cause:** Physical phenomena complexity; lack of validated virtual models; regulatory requirements.
- **Contributing factors:** Prototype availability; seasonal/weather dependence.
- **Technical limits:** Virtual model fidelity; computational.
- **Human/skill limits:** Robustness engineering.
- **Tool limits:** Virtual robust design tools.
- **Process limits:** Test-to-pass culture.
- **Frequency/severity:** Pervasive; severity medium-high.
- **Time/cost impact:** Weeks-months; prototype consumption.
- **Downstream effects:** Late defect discovery; SOP risk.
- **Existing solutions:** Virtual robust design (Audi); virtual endurance (IPG); customized env simulation (Siemens).
- **Why insufficient:** Not yet validated/replacement; regulators require physical.
- **Improvement areas:** Virtual-physical correlation programs; damage-equivalent condensation.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium — report analysis, surrogate models.
- **Human approval:** Required.
- **Evidence:** Audi virtual robust design; IPG virtual endurance; ZVEI robustness handbook; Siemens bulletproof validation.
- **Confidence:** High.

#### W3-022 — Test fleet management and real-world data collection inefficiency
- **Domain:** Vehicle/fleet validation
- **Lifecycle stage:** Vehicle validation
- **Problem:** Managing test fleets, data collection, health monitoring, campaign planning is complex; disruptions detected late; data silos; remote diagnostics/OTA updates needed; efficiency of data collection determines cost/time.
- **Scenario:** Test drive data unusable due to vehicle health issue discovered post-hoc; wasted campaign.
- **Who:** Test fleet managers, developers.
- **Root cause:** Fragmented data management; lack of real-time health monitoring; manual planning.
- **Contributing factors:** Tool fragmentation; connectivity.
- **Technical limits:** Data volume; bandwidth.
- **Human/skill limits:** Fleet ops expertise.
- **Tool limits:** Fleet management platforms (CONiX.tfs).
- **Process limits:** No standardized data pipeline.
- **Frequency/severity:** Common; severity medium.
- **Time/cost impact:** Wasted test miles; delayed feedback.
- **Downstream effects:** Incomplete ODD coverage data.
- **Existing solutions:** Fleet management systems; remote diagnostics; OTA.
- **Why insufficient:** Integration effort; vendor-specific.
- **Improvement areas:** Standardized test data management; real-time health monitoring; cloud analytics.
- **Automation feasibility:** High.
- **AI applicability:** Medium-High — anomaly detection, campaign optimization.
- **Human approval:** For campaign changes.
- **Evidence:** ADT test fleet management; IEEE scenario coverage.
- **Confidence:** Medium-High.

---

### 3.6 Cross-Cutting Testing Problems

#### W3-023 — Lack of traceability across MIL–SIL–HIL (horizontal) levels
- **Domain:** Test traceability
- **Lifecycle stage:** V&V across lifecycle
- **Problem:** Traceability is primarily vertical (requirements↔tests) for ISO 26262 compliance but does not systematically extend across MIL/SIL/HIL levels. If a test fails in HIL, no established traceability to determine whether equivalent MIL test functioned. Hinders result comparison, test reuse, fault origin identification.
- **Scenario:** HIL failure; team cannot determine if MIL equivalent passed; manual investigation weeks.
- **Who:** Test managers, quality, safety.
- **Root cause:** Different tools/models/time bases; no common test case identity across levels; vertical-only ALM.
- **Contributing factors:** Tool fragmentation; lack of standard test case format.
- **Technical limits:** Model differences; time bases; stimulation methods.
- **Human/skill limits:** Traceability engineering.
- **Tool limits:** ALM/PLM tools; test management.
- **Process limits:** Compliance focuses on vertical traceability.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Weeks of investigation; reduced reuse.
- **Downstream effects:** Late fault origin; audit gaps.
- **Existing solutions:** ALM tools; ASAM test spec standard (proposed); requirements-to-test mapping.
- **Why insufficient:** Horizontal traceability not standardized; tool-specific.
- **Improvement areas:** Unified cross-level test case identity; standardized test spec format (ASAM); automated result correlation.
- **Automation feasibility:** Medium-High.
- **AI applicability:** Medium — traceability mining.
- **Human approval:** For compliance claims.
- **Evidence:** SIMEA 2026 (lack of traceability MIL-SIL-HIL); ASAM recommendations (test spec standard); Magna SiL article (traceability).
- **Confidence:** High.

#### W3-024 — Test case specification quality problems
- **Domain:** Test documentation
- **Lifecycle stage:** Test design
- **Problem:** Test case specifications suffer availability/content problems with input artifacts, lack of knowledge, description ambiguity, process/communication/tool issues, weak quality assurance. Producers and consumers differ; faulty specs cause testing to take too long, be too expensive, or detect no defects. High communication effort to clarify ambiguities; incorrectly implemented test cases.
- **Scenario:** Test designer writes spec; tester implements wrong case due to ambiguity; defect missed.
- **Who:** Test designers, testers, OEMs/suppliers.
- **Root cause:** Informal specs; poor input artifact quality; knowledge gaps; no quality gates.
- **Contributing factors:** Time pressure; distributed teams; tool limitations.
- **Technical limits:** Natural language ambiguity.
- **Human/skill limits:** Test design skills; domain knowledge.
- **Tool limits:** Test spec tools; ALM.
- **Process limits:** No spec review/quality criteria; no traceability to requirements.
- **Frequency/severity:** Pervasive (26 internal + 10 external assessed); severity high.
- **Time/cost impact:** Redundant tests; missed defects; clarifications.
- **Downstream effects:** Late defect discovery; compliance gaps.
- **Existing solutions:** Reviews; templates; ISO 29119; machine-readable test specs (Springer 2024 XML schema).
- **Why insufficient:** Manual; inconsistent; tool-specific.
- **Improvement areas:** Machine-readable test spec standard; automated spec quality checks; NLP-based ambiguity detection.
- **Automation feasibility:** Medium-High (NLP).
- **AI applicability:** High — LLM for spec drafting/review; risk of hallucination.
- **Human approval:** Required for safety-relevant specs.
- **Evidence:** Juhnke et al. 2020 (9 challenge categories, 36 assessors); Springer 2024 (machine-readable spec); arXiv test case spec review 2025.
- **Confidence:** High.

#### W3-025 — Compliance documentation burden (ISO 26262/ASPICE) for testing
- **Domain:** Compliance
- **Lifecycle stage:** V&V documentation
- **Problem:** Documenting automated and manual testing efforts for compliance is difficult (20% top testing concern); ISO 26262/ASPICE require extensive evidence (verification plans/specs/logs/reports); 50% find ensuring ISO 26262 requirements challenging and time-consuming; documentation is 2nd most time-consuming activity (41%).
- **Scenario:** Team spends weeks assembling compliance evidence; audit findings due to missing logs.
- **Who:** Quality, safety managers, developers.
- **Root cause:** Standards demand extensive documentation; manual collection; tool fragmentation.
- **Contributing factors:** Late documentation; poor traceability.
- **Technical limits:** Evidence formats; tool interoperability.
- **Human/skill limits:** Compliance expertise.
- **Tool limits:** ALM/CM tools.
- **Process limits:** Documentation as afterthought.
- **Frequency/severity:** Pervasive; severity medium-high.
- **Time/cost impact:** 41% time-consuming; audit risk.
- **Downstream effects:** Release delays; certification delays.
- **Existing solutions:** Automated reporting; SCADE; Parasoft; VectorCAST; tool qualification.
- **Why insufficient:** Still manual gaps; tool qualification costs; hidden costs (TASKING whitepaper).
- **Improvement areas:** Automated evidence generation in CI; continuous compliance.
- **Automation feasibility:** High.
- **AI applicability:** Medium-High — report summarization; risk of hallucination in safety evidence.
- **Human approval:** Required for certification.
- **Evidence:** Perforce 2024 (20% doc concern; 47% V&V most time-consuming; 41% documentation); TASKING hidden cost WP; SCADE handbook.
- **Confidence:** High.

#### W3-026 — Incomplete test coverage and time pressure ("not enough time to test more")
- **Domain:** Test coverage
- **Lifecycle stage:** All test levels
- **Problem:** "Testing efforts are not exhaustive — and we do not have time to test more" is top quality concern (24% overall; 44% for 5–10 yrs exp). Testing verifies/vast software complexity; coverage metrics often structural only; requirements coverage gaps; corner cases missed in SDV transition.
- **Scenario:** Release with known untested corner cases; post-SOP defect.
- **Who:** Developers, test managers, quality.
- **Root cause:** Exponential scenario growth; limited time/resources; coverage metrics inadequate.
- **Contributing factors:** Late testing; poor prioritization; tool limitations.
- **Technical limits:** State space explosion; continuous dynamics.
- **Human/skill limits:** Test design expertise.
- **Tool limits:** Coverage tools (structural vs requirements).
- **Process limits:** No coverage gating; no risk-based prioritization.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Defect escape; recalls.
- **Downstream effects:** Safety incidents; warranty.
- **Existing solutions:** Risk-based testing; coverage tools; MBT; AI test generation.
- **Why insufficient:** Coverage ≠ correctness; structural coverage misses requirements; MBT model effort.
- **Improvement areas:** Requirements-based coverage metrics; risk-based prioritization; AI-assisted test generation with human review.
- **Automation feasibility:** Medium-High.
- **AI applicability:** High — test generation; must verify.
- **Human approval:** Required for coverage claims.
- **Evidence:** Perforce 2024 (24%); IPG whitepaper (corner cases); IEEE scenario coverage.
- **Confidence:** High.

#### W3-027 — Invalid test failures and unreliable test infrastructure
- **Domain:** Test execution quality
- **Lifecycle stage:** System testing
- **Problem:** Up to 91% of failed test executions are invalid (non-responding components, faulty sim models, faulty test case impl, misconfigurations); 27–53% of failed executions linked to unreliable test infrastructure. Manual triage tedious and costly.
- **Scenario:** CI red; engineer spends days determining failure is infra-related; real defects masked.
- **Who:** Test engineers, CI engineers.
- **Root cause:** Fragile test infra; lack of automated failure classification; environment drift.
- **Contributing factors:** Hardware flakiness; config errors.
- **Technical limits:** Infra reliability.
- **Human/skill limits:** Triage expertise.
- **Tool limits:** No automated failure classification.
- **Process limits:** No quarantine/flakiness policy.
- **Frequency/severity:** Up to 91% invalid failures; severity high.
- **Time/cost impact:** Massive wasted effort; delayed feedback.
- **Downstream effects:** Loss of trust in automation; manual fallback.
- **Existing solutions:** Failure classification research; quarantine; retry; infra monitoring.
- **Why insufficient:** Not productized; vendor-specific.
- **Improvement areas:** Automated failure root-cause classification; infra health monitoring; deterministic env.
- **Automation feasibility:** High.
- **AI applicability:** High — ML classification of failure causes.
- **Human approval:** For quarantining tests.
- **Evidence:** ACM ICSE-SEIP 2022 (91% invalid; 27–53% infra).
- **Confidence:** High.

#### W3-028 — Regression testing scalability and repeated regression overhead
- **Domain:** Regression testing
- **Lifecycle stage:** All levels
- **Problem:** Regression testing overhead grows with codebase; repeated regression across ECUs/targets/environments; regression testing does not scale efficiently; manual unit testing limited scalability (SRM Tech case: automated framework reduced effort 20%).
- **Scenario:** Every change triggers full regression; days to complete; blocks releases.
- **Who:** Developers, test managers.
- **Root cause:** Test suite growth; lack of impact-based selection; environment constraints.
- **Contributing factors:** Test duplication; no test prioritization.
- **Technical limits:** Execution time; parallelism.
- **Human/skill limits:** Test architecture.
- **Tool limits:** Test runners; CI.
- **Process limits:** No risk-based regression selection.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Days per regression; release delays.
- **Downstream effects:** Reduced change velocity.
- **Existing solutions:** Impact-based test selection; test parallelization; cloud; test.guide.
- **Why insufficient:** Requires test metadata; tooling.
- **Improvement areas:** Change-impact analysis for tests; predictive test selection; cloud parallelization.
- **Automation feasibility:** High.
- **AI applicability:** High — predictive test selection.
- **Human approval:** For skipping tests.
- **Evidence:** SRM Tech case study; Perforce (QA cycles long decreasing); industry.
- **Confidence:** High.

#### W3-029 — Non-determinism and flaky tests in ML-based automotive systems
- **Domain:** ML testing
- **Lifecycle stage:** Software testing
- **Problem:** ML systems introduce non-determinism, large input spaces, non-explainable code; flaky tests; tolerant oracles needed; test maintenance burden; knowledge sharing gaps.
- **Scenario:** ML-based perception test fails intermittently; team cannot distinguish SUT vs test flakiness.
- **Who:** ML engineers, test engineers.
- **Root cause:** ML non-determinism; tolerance thresholds; large input spaces.
- **Contributing factors:** Lack of ML test practices; explainability gaps.
- **Technical limits:** Determinism.
- **Human/skill limits:** ML testing expertise.
- **Tool limits:** Test frameworks.
- **Process limits:** No ML-specific test strategy.
- **Frequency/severity:** Growing; severity medium-high.
- **Time/cost impact:** Maintenance effort; delayed feedback.
- **Downstream effects:** Coverage gaps; safety concerns.
- **Existing solutions:** Tolerant oracles; forced determinism; test maintenance practices (Chalmers study).
- **Why insufficient:** Emerging; not standardized.
- **Improvement areas:** ML test patterns; deterministic replay; tolerance frameworks.
- **Automation feasibility:** Medium.
- **AI applicability:** High (but self-referential risk).
- **Human approval:** Required.
- **Evidence:** Chalmers ML test maintenance study.
- **Confidence:** Medium-High.

#### W3-030 — Test data management and artifact versioning across XiL
- **Domain:** Test data management
- **Lifecycle stage:** All test levels
- **Problem:** Test data, models, software, parameters, results must be versioned and correlated across MIL/SIL/HIL/VIL/proving ground; without TDM, reproducibility breaks; large-scale simulation needs cloud data management; ASAM recommends TDM deep dive; traceability of test data not sufficiently considered.
- **Scenario:** Cannot reproduce a 6-month-old HIL result; model/software versions unknown.
- **Who:** All test stakeholders.
- **Root cause:** Fragmented data storage; no unified TDM; tool-specific formats.
- **Contributing factors:** Data volume; IP protection.
- **Technical limits:** Storage; bandwidth; versioning.
- **Human/skill limits:** Data engineering.
- **Tool limits:** TDM platforms.
- **Process limits:** No TDM standard; traceability gaps.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Wasted reruns; audit gaps.
- **Downstream effects:** Non-reproducible results; compliance risk.
- **Existing solutions:** SYNECT; cloud workflow management (IEEE 2025); ASAM TDM recommendations.
- **Why insufficient:** Not standardized; integration effort.
- **Improvement areas:** Standardized TDM; cloud-native; automated provenance.
- **Automation feasibility:** High.
- **AI applicability:** Medium — data mining.
- **Human approval:** For data governance.
- **Evidence:** ASAM recommendations; IEEE 2025 customized data/workflow; Ford/SYNECT.
- **Confidence:** High.

#### W3-031 — Shift-left adoption incomplete; testing still late
- **Domain:** Process
- **Lifecycle stage:** Development lifecycle
- **Problem:** 15% have no plan to implement shift-left (up 6% YoY); not testing early enough is 2nd top testing concern (24%, up 5%); 33% implemented, 26% in process, 26% aware. Traditional hardware-centric sequential validation too slow; real ECU dependency delays decoupling.
- **Scenario:** Bugs found months before SOP; 4–6 week feedback loops; SOP delays >2 years.
- **Who:** OEMs, suppliers.
- **Root cause:** Cultural resistance; legacy systems; high initial investment; talent shortage; regulatory physical testing mandates.
- **Contributing factors:** Hardware dependency; organizational inertia.
- **Technical limits:** Tool integration; legacy.
- **Human/skill limits:** Simulation/ML/cloud talent shortage.
- **Tool limits:** Legacy toolchains.
- **Process limits:** V-model culture; supplier contracts with uncertainty.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** SOP delays >2 years; 4–6 week feedback.
- **Downstream effects:** Cost overruns; competitive disadvantage.
- **Existing solutions:** Virtual validation; CI/CD; vECU; shift-left strategies.
- **Why insufficient:** Cultural; investment; regulatory; talent.
- **Improvement areas:** Incremental adoption; training; virtual-first pilots.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium.
- **Human approval:** Organizational change.
- **Evidence:** Perforce 2024 (shift-left stats); HCLTech 2025; KPIT 2025 (4–6 weeks; >2 years).
- **Confidence:** High.

#### W3-032 — Unit testing informal, weak strategies, poor evidence (ASPICE SWE.4 weakness)
- **Domain:** Unit testing
- **Lifecycle stage:** Software unit verification
- **Problem:** Software unit verification (SWE.4) and integration (SWE.5) practices often perform poorly due to informal execution and lack of well-defined test strategies (no explicit scope, plans, methods); little evidence (specs/logs/reports); heavy reliance on verifier experience; no clear targets/procedures.
- **Scenario:** Unit tests run ad hoc; no coverage evidence; audit findings.
- **Who:** Developers, quality.
- **Root cause:** Standards require but don't prescribe concrete methods; tooling gaps; culture.
- **Contributing factors:** Time pressure; skill gaps.
- **Technical limits:** Tool support.
- **Human/skill limits:** Test design expertise.
- **Tool limits:** Unit test frameworks; coverage tools.
- **Process limits:** No defined unit test strategy.
- **Frequency/severity:** Pervasive; severity medium-high.
- **Time/cost impact:** Rework; compliance risk.
- **Downstream effects:** Late defect discovery.
- **Existing solutions:** Automated unit test frameworks (SRM Tech 20% effort reduction); VectorCAST; GoogleTest; Tessy.
- **Why insufficient:** Adoption; integration; maintenance.
- **Improvement areas:** Standardized unit test strategy templates; automated coverage reporting; CI integration.
- **Automation feasibility:** High.
- **AI applicability:** Medium — test generation.
- **Human approval:** For safety-relevant unit tests.
- **Evidence:** arXiv review 2025; SAST rapid review; SRM Tech case.
- **Confidence:** High.

#### W3-033 — Fault injection: manual, expertise-dependent, incomplete fault models
- **Domain:** Fault injection / robustness
- **Lifecycle stage:** Safety validation
- **Problem:** Fault injection (FI) is strongly recommended by ISO 26262 but standard provides limited guidance on design/implementation/execution; FI often manual and expertise-dependent; standard fault models (bit flips) don't represent timing/software faults; simulation-based FI can't assess real-time properties; random fault injection misses safety-relevant combos; test pattern generation for fault excitation is the bottleneck; FI tools not ready-to-use; no open AUTOSAR FI framework; FI underused in design stage.
- **Scenario:** Safety mechanism claimed effective; FI campaign uses bit flips only; timing faults escape; FMEDA numbers rely on assumed detection.
- **Who:** Safety engineers, V&V engineers.
- **Root cause:** Standard vagueness; lack of AUTOSAR-specific fault models; tool gaps; manual process.
- **Contributing factors:** Domain knowledge not public; timing representation hard.
- **Technical limits:** Simulation real-time; fault model representativeness.
- **Human/skill limits:** FI expertise; safety analysis.
- **Tool limits:** FI frameworks (GRINDER); tool qualification.
- **Process limits:** FI as late verification only.
- **Frequency/severity:** Pervasive for ASIL B+; severity high.
- **Time/cost impact:** Manual effort; incomplete coverage; safety-case gaps.
- **Downstream effects:** Undetected safety gaps; recalls.
- **Existing solutions:** AUTOSAR FI frameworks; formal-assisted fault campaigns; NLP+HIL FI automation; ASFIT.
- **Why insufficient:** Research-stage; not standardized; tool qualification.
- **Improvement areas:** AUTOSAR-specific fault models; automated FI test generation (NLP); formal methods for untestable faults; FI in design stage.
- **Automation feasibility:** Medium-High.
- **AI applicability:** Medium-High — NLP-based fault scenario generation.
- **Human approval:** Required for safety claims.
- **Evidence:** Lancs AUTOSAR FI paper; DVCon formal fault campaign; beefed.ai; PMC NLP+HIL FI; ASFIT MDPI; Koopman 2018.
- **Confidence:** High.

#### W3-034 — Safety validation: statistical evidence and testing effort manageability
- **Domain:** Functional safety validation
- **Lifecycle stage:** Safety validation
- **Problem:** Major challenge in ISO 26262 compliance is providing statistical evidence that safety goals won't be violated during operation within reasonable testing effort; safety testing accounts for >50% of cost for safety-critical software; defect inflow profiles show late detection.
- **Scenario:** Safety case requires unmanageable test volume; effort/cost overrun.
- **Who:** Safety managers, OEMs.
- **Root cause:** Combinatorial safety state space; late detection; standards stringent.
- **Contributing factors:** Lack of early defect classification (safety-relevant vs not).
- **Technical limits:** State space.
- **Human/skill limits:** Safety engineering.
- **Tool limits:** Safety analysis tools.
- **Process limits:** Late V&V.
- **Frequency/severity:** Pervasive for ASIL C/D; severity high.
- **Time/cost impact:** >50% of dev cost for safety-critical.
- **Downstream effects:** Release/certification delays.
- **Existing solutions:** Early defect classification; fault injection; MBT; shift-left.
- **Why insufficient:** Not integrated; standards vague.
- **Improvement areas:** Safety-relevant defect triage early; probabilistic safety arguments; virtual safety validation.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium — risk prediction.
- **Human approval:** Required (safety case).
- **Evidence:** Scitepress 2013 (50% cost); ISO 26262 context.
- **Confidence:** Medium-High.

#### W3-035 — Mixed discrete-continuous (hybrid) system testing intractability
- **Domain:** Model-based testing
- **Lifecycle stage:** System verification
- **Problem:** Automotive systems exhibit hybrid (discrete+continuous) behavior; state space explosion makes automated test generation intractable; continuous imprecision requires fuzziness in time/values; test case generation difficult.
- **Scenario:** MBT tool cannot generate tests for hybrid powertrain controller; manual fallback.
- **Who:** Test engineers, modelers.
- **Root cause:** Hybrid state space explosion; numerical precision.
- **Contributing factors:** Lack of hybrid test methods; tool limitations.
- **Technical limits:** State space; computation.
- **Human/skill limits:** Hybrid systems expertise.
- **Tool limits:** MBT tools.
- **Process limits:** No hybrid test strategy.
- **Frequency/severity:** Common in powertrain/chassis; severity medium-high.
- **Time/cost impact:** Manual test creation; coverage gaps.
- **Downstream effects:** Late defects.
- **Existing solutions:** Separate discrete/continuous models; tolerance-based oracles.
- **Why insufficient:** Manual split; not integrated.
- **Improvement areas:** Hybrid-aware test generation; surrogate models.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium-High — surrogate models.
- **Human approval:** Required.
- **Evidence:** TUM hybrid testing paper; MBT review.
- **Confidence:** Medium-High.

#### W3-036 — Test tool interoperability and fragmented toolchain
- **Domain:** Toolchain
- **Lifecycle stage:** All
- **Problem:** Fragmented automotive engineering tools; poor interoperability; manual data transformation; proprietary formats; legacy tool dependencies; gaps between tools; no seamless chain from requirements to integration/testing.
- **Scenario:** Test results cannot flow from HIL tool to ALM; manual re-entry; errors.
- **Who:** All engineers.
- **Root cause:** Vendor silos; proprietary formats; historical tool evolution.
- **Contributing factors:** Standards lag; IP.
- **Technical limits:** Format heterogeneity.
- **Human/skill limits:** Toolchain expertise.
- **Tool limits:** Most tools.
- **Process limits:** No integration architecture.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Manual effort; errors; delays.
- **Downstream effects:** Reduced automation; data silos.
- **Existing solutions:** Standards (ASAM, AUTOSAR, FMI, OSLC); middleware; integrations (SYNECT).
- **Why insufficient:** Partial adoption; vendor resistance.
- **Improvement areas:** Open standards mandates; API-first tooling; data fabric.
- **Automation feasibility:** Medium.
- **AI applicability:** Medium — data transformation.
- **Human approval:** Procurement/governance.
- **Evidence:** Task.md cross-industry; arXiv review; tool docs.
- **Confidence:** High.

#### W3-037 — Test prioritization and risk-based testing underdeveloped
- **Domain:** Test strategy
- **Lifecycle stage:** Test planning
- **Problem:** Lack of systematic risk-based test prioritization; test decisions often ad hoc; no unified KPIs for test quality; no consolidated quality indicators across XiL levels.
- **Scenario:** Teams run all tests every cycle; no prioritization; wasted effort.
- **Who:** Test managers.
- **Root cause:** No standard KPIs; lack of data; organizational inertia.
- **Contributing factors:** Immature metrics.
- **Technical limits:** Measurement.
- **Human/skill limits:** Test management expertise.
- **Tool limits:** Test management tools.
- **Process limits:** No KPI framework.
- **Frequency/severity:** Pervasive; severity medium.
- **Time/cost impact:** Inefficient allocation.
- **Downstream effects:** Missed high-risk areas.
- **Existing solutions:** KPI frameworks (PulseMo metrics); risk-based testing research.
- **Why insufficient:** Not standardized.
- **Improvement areas:** Industry-wide test KPI standard; risk-based prioritization.
- **Automation feasibility:** Medium-High.
- **AI applicability:** High — predictive prioritization.
- **Human approval:** For risk acceptance.
- **Evidence:** SIMEA 2026 (lack of unified KPIs); PulseMo metrics.
- **Confidence:** Medium-High.

#### W3-038 — Knowledge and skill gaps in testing/validation
- **Domain:** Human skills
- **Lifecycle stage:** All
- **Problem:** Shortage of testing/validation expertise (HIL, simulation, ML, cloud, functional safety, cybersecurity); onboarding time; knowledge silos; single-person expertise; training gaps; communication between software/hardware/system teams.
- **Scenario:** HIL bench down; only one engineer can fix; weeks delay.
- **Who:** All.
- **Root cause:** Rapid tech evolution; education lag; specialization.
- **Contributing factors:** Turnover; silos.
- **Technical limits:** n/a.
- **Human/skill limits:** As above.
- **Tool limits:** Complexity.
- **Process limits:** Poor knowledge transfer.
- **Frequency/severity:** Pervasive; severity high.
- **Time/cost impact:** Delays; quality gaps.
- **Downstream effects:** Automation failures.
- **Existing solutions:** Training; documentation; knowledge bases; AI assistants.
- **Why insufficient:** Slow; not scaled.
- **Improvement areas:** Standardized training; AI-assisted knowledge capture; pair engineering.
- **Automation feasibility:** Low (human).
- **AI applicability:** High — knowledge retrieval; risk of hallucination.
- **Human approval:** Required.
- **Evidence:** HCLTech (talent shortage); HIL challenges (specialized skill); Perforce.
- **Confidence:** High.

---

## 4. Cross-Cutting Themes

1. **Shift-left vs reality gap:** Ambition (33% implemented) vs barriers (culture, investment, talent, regulation); not testing early enough rising.
2. **Fidelity vs scalability trade-off:** Higher fidelity (Level-4 vECU, HIL) = lower scalability; SiL scales but misses hardware effects.
3. **Traceability gap (horizontal):** Vertical traceability for compliance exists; horizontal across XiL levels missing.
4. **Scenario completeness unknown:** No agreed completeness criterion for ADAS/ADS; coverage metrics research-stage.
5. **Invalid failures dominate:** Up to 91% invalid failures; infra reliability is itself a problem.
6. **Cost concentration:** Testing ~50% of dev cost; HIL capital ~$50k+/bench; proving-ground scope explosion.
7. **Standardization lag:** vECU, test spec, TDM, fault models all lack consolidated standards.
8. **Non-determinism:** 43.6% flaky scenarios in CARLA; MIL/SIL/HIL time-base mismatches.
9. **Compliance overhead:** Documentation 41% time-consuming; hidden costs.
10. **Skill gaps:** Talent shortage across simulation, safety, cybersecurity, ML.

---

## 5. Quantitative Evidence Summary

| Metric | Value | Source |
|---|---|---|
| V&V most time-consuming activity | 47% | Perforce 2024 |
| Testing efforts not exhaustive / no time | 24% (44% for 5–10 yrs) | Perforce 2024 |
| Struggling to test efficiently (top testing concern) | 26% | Perforce 2024 |
| Not testing early enough (bugs late) | 24% | Perforce 2024 |
| Documenting tests for compliance | 20% | Perforce 2024 |
| QA cycles long | 8% (down from 22%) | Perforce 2024 |
| ISO 26262 requirements challenging/time-consuming | 50% | Perforce 2024 (DE) |
| Testing cost share of dev | up to 50% | Boehm/Basili 2001; Jones 2001 |
| Invalid failed test executions | up to 91% | ACM ICSE-SEIP 2022 |
| Failed executions linked to unreliable infra | 27–53% | ACM ICSE-SEIP 2022 |
| HIL bench cost | ~$50k+ (enterprise) | NTQ Europe 2026 |
| HIL throughput | ~100–200 tests/day/bench | NTQ Europe 2026 |
| Integration issues surface pre-SOP | ~6 months | KPIT 2025 |
| Feedback to dev team (traditional) | 4–6 weeks | KPIT/HCLTech 2025 |
| SOP delays due to late software errors | >2 years | HCLTech 2025 |
| Flaky scenarios in CARLA | 43.6% | Osikowicz 2026 |
| ADAS test case variations per regulation set | 60+ | Future Transportation 2025 |
| Shift-left implemented | 33% | Perforce 2024 |
| No plan for shift-left | 15% (up 6%) | Perforce 2024 |
| Code LoC per vehicle | up to 100M | IPG whitepaper |
| Tata Elxsi testing time reduction via SIL | 10 days → 8 hours | Vector case study |
| SRM Tech unit test effort reduction | 20% | SRM Tech case |
| SiL share of tests (research/pre/series) | 17–23% | Altinger et al. (cited MDPI) |

---

## 6. Standards and Regulatory Context

- **ISO 26262:** strongly recommends fault injection; limited guidance on design/implementation/execution; requires traceability/evidence; testing cost overhead; statistical evidence challenge.
- **ASPICE:** SWE.4/SWE.5 weaknesses (informal execution, poorly defined strategies); process improvement focus.
- **ISO 21448 (SOTIF):** scenario-based safety for intended functionality; completeness challenge.
- **ISO/SAE 21434 / UNECE R155/R156:** cybersecurity validation; testing overhead.
- **UNECE R157/R10, GSR, Euro NCAP:** ADAS/ADS scenario validation; proving-ground scope.
- **AUTOSAR:** Classic/Adaptive; vECU levels; BSW/MCAL; no vECU standard yet.
- **ASAM:** test specification standard recommended; test data management deep dive; OpenSCENARIO/OpenDRIVE.
- **FMI:** co-simulation standard; simpler interface; insufficient for bus comms.
- **MISRA C/C++:** coding standards; static analysis.

---

## 7. Existing Solutions Landscape (and limitations)

- **XiL (MIL/SIL/PIL/HIL/VIL):** layered validation; gaps in traceability/fidelity/standardization.
- **vECU (Levels 0–4):** enables SiL; proprietary formats; fidelity/scalability trade-off.
- **Rapid HIL (IPG):** DUT physical + rest virtual; deploy in weeks; reconfigure in a day.
- **HIL farms (Aliaro, Renault HELIUM, Ford SYNECT):** parallelization; scheduling; variant management.
- **Cloud-based HIL/SIL:** scalability; latency/compliance/legacy barriers.
- **Model-based testing (MBT):** automated generation; model effort; hybrid intractability.
- **Automated unit testing frameworks (SRM Tech, VectorCAST, GoogleTest):** effort reduction; integration effort.
- **Test management (test.guide, SYNECT, PulseMo):** scheduling/utilization; adoption.
- **Scenario-based testing (ASAM OpenSCENARIO, Euro NCAP):** standardization; completeness gaps.
- **Digital twins / virtual robust design (Audi, IPG):** emerging; validation needed.
- **AI-assisted testing (test generation, report analysis, scheduling):** promising; verification/hallucination risks.

---

## 8. Root-Cause Synthesis (5 Whys style)

- **Why is testing the bottleneck?** → Because validation is hardware-dependent, late, manual, and fragmented.
- **Why hardware-dependent?** → Because real-time/physical effects cannot be fully virtualized; vECU standardization lacking.
- **Why late?** → Because V-model culture, supplier uncertainty, real ECU availability, and organizational inertia.
- **Why manual?** → Because test case creation, environment setup, triage, documentation not automated; tool gaps.
- **Why fragmented?** → Because vendor silos, proprietary formats, no end-to-end standards, organizational boundaries.
- **Underlying:** Complexity growth (100M LoC), SDV/OTA, variant explosion, regulatory expansion, talent shortage, cost pressure.

---

## 9. Unanswered Research Questions

1. What is the acceptable model fidelity threshold per ASIL for virtual validation?
2. How to measure scenario completeness/coverage for ADAS/ADS ODDs with agreed criteria?
3. Can a universal vECU standard (FMI 3.0 + bus interface) achieve industry adoption?
4. What is the true cost/benefit of SiL-first vs HiL-first across vehicle programs (quantified)?
5. How to automate failure root-cause classification at scale (infra vs SUT vs test)?
6. How to make simulation deterministic enough for safety evidence?
7. What test KPIs should be standardized across XiL levels?
8. How much of environmental/EMC/durability testing can be credibly virtualized?
9. How to balance AI-generated tests with human safety assurance?
10. What organizational changes enable shift-left in legacy OEMs?

---

## 10. Confidence and Limitations

- Findings based on published sources (2020–2026); some statistics from vendor-sponsored surveys (Perforce) — treat as indicative, not audited.
- No direct access to OEM internal data; case studies are success stories (selection bias).
- Quantitative values not independently verified; ranges cited as reported.
- Graph/coverage tools not applicable (research subagent); no codebase indexing performed.
- Some sources are marketing content; technical claims cross-checked where possible.

---

## 11. Source Bibliography (selected)

1. Perforce Software (2024). *2024 State of Automotive Software Development Report* (with Automotive IQ & Eclipse Foundation). https://propermotors.co.uk/wp-content/uploads/2024/05/report-sa-2024-state-of-automotive-software-development.pdf
2. IPG Automotive (2024/2025). *The Evolution of Testing in Automotive Development – From Validation Bottlenecks to Strategic Enablers* (whitepaper). https://www.ipg-automotive.com/fileadmin/data/consulting_and_services/consulting/Test-Strategy_Whitepaper_EN_final.pdf
3. HCLTech (2025-04-15). *Shift-Left validation for software-defined vehicles*. https://www.hcltech.com/en-us/trends-and-insights/software-defined-vehicles-new-era-validation-shift-left-strategies
4. KPIT/MATLAB (2025). *Accelerating Automotive Software Defect Detection via Shift-Left with Virtual ECU Testing*. https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/company/events/conferences/automotive-conference-india/2025/in-mac-2025-kpit-software-defect-detection.pdf
5. AutoElectronics (2025-04-10). *Automotive Software Validation with Shift-Left and AI-driven Approach*. https://www.autoelectronics.co.kr/article/articleView.asp?idx=6150
6. SIMEA 2026 paper (Blucher). *MIL, SIL and HIL Testing Strategies for Automotive Software*. https://pdf.blucher.com.br/engineeringproceedings/SIMEA2026/PAP79.pdf
7. SAST 2024 rapid review. *A Rapid Review on Advanced Software-in-the-Loop Methods for Automotive Software Verification*. https://doi.org/10.5753/sast.2024.3883
8. MDPI (2024-04-18). *Evaluation of SiL Testing Potential—Shifting from HiL by Identifying Compatible Requirements with vECUs*. https://www.mdpi.com/2624-8921/6/2/44
9. MDPI (2024-08-02). *AUTOSAR-Compatible Level-4 Virtual ECU for the Verification of the Target Binary for Cloud-Native Development*. https://www.mdpi.com/2079-9292/13/18/3704
10. Synopsys (2024). *Accelerating Development of Software Defined Vehicles with Virtual ECUs* (whitepaper). https://www.synopsys.com/content/dam/synopsys/verification/white-papers/virtual-ecu-wp.pdf
11. ProSTEP (SmartSE V4). *PART F VIRTUAL ELECTRONIC CONTROL UNITS*. https://www.prostep.org/fileadmin/prod-pay-download-8c1d/PSI_11_V4_SmartSE_Part_F_VECU.pdf
12. NI (2024/2025). *HIL Testing Fundamentals*; *Software Validation for SDVs*. https://www.ni.com/content/dam/web/pdfs/niconnect/2024/automotive/HIL_Testing_Fundamentals.pdf
13. Automotive Testing Technology International (2025-09-16). *Hardware-in-the-loop: key challenges and considerations*. https://www.automotivetestingtechnologyinternational.com/industry-opinion/hardware-in-the-loop-key-challenges-and-considerations.html
14. IPG Automotive (2025-08-26). *Rapid HIL*. https://www.ipg-automotive.com/company/who-we-are/corporate-news/news/news-detail/rapid-hil-the-next-step-in-modern-vehicle-development
15. AVL. *AVL HiL Nexus*. https://www.avl.com/en/development-speed-and-methodology/avl-hil
16. dSPACE/Ford (case study). *Ford: New Approaches to Variant Management*. https://www.dspace.com/en/pub/home/applicationfields/stories/ford-new-approaches.cfm
17. dSPACE/Renault (case study). *RENAULT: Cumulative Test Power*. https://www.dspace.com/en/pub/home/applicationfields/stories/renault-cumulative-test-power.cfm
18. dSPACE. *Variant-based Automation* use case. https://www.dspace.com/en/pub/home/applicationfields/stories/variant-based-automation.cfm
19. Aliaro (whitepaper). *Leveraging the xMove HIL Platform in a HIL Farm*. https://www.aliaro.com/files/_LeveragingthexMoveHILPlatforminaHILFarm.pdf
20. PulseMo. *Managed HIL Testing & Lab Operations*; *PulseMo Edge*. https://www.pulsemo.com/services/hil-as-a-managed-service ; https://www.pulsemo.com/edge
21. tracetronic. *test.guide*. https://www.tracetronic.com/products/test-guide/
22. NTQ Europe (2026-10-16). *HiL vs SiL: Where Should Automotive Teams Invest First?* https://ntq-europe.com/hil-vs-sil-where-to-invest-first/
23. Magna (2026). *Why Software-in-the-Loop Testing is Only as Strong as the System Around It*. https://www.magna.com/stories/tech-talks/2026/why-software-in-the-loop-testing-is-only-as-strong-as-the-system-around-it
24. Juhnke, K., Tichy, M., Houdek, F. (2020). *Challenges concerning test case specifications in automotive software testing*. Software Quality Journal 29:39–100. https://doi.org/10.1007/s11219-020-09523-0
25. arXiv (2025-12-29). *Test Case Specification Techniques and System Testing Tools in the Automotive Industry: A Review*. https://arxiv.org/html/2512.23780v1
26. Springer (2024-12-16). *Machine-readable specification and intelligent cloud-based execution of logical test cases for automated driving functions*. https://link.springer.com/article/10.1007/s10515-024-00481-6
27. Springer (2020). *Unreliable test infrastructures in automotive testing setups*. ACM ICSE-SEIP 2022. https://dl.acm.org/doi/10.1145/3510457.3513069
28. Osikowicz (2026). *Simulation Nondeterminism and Its Impact on ...* (PDF). https://olek-osikowicz.github.io/assets/pdf/2026/osikowicz2026nondeterminism.pdf
29. DeFT (ICSE 2026). *Maintaining Determinism and Extracting Unit Tests for Autonomous Driving Planning*. https://conf.researchr.org/details/icse-2026/icse-2026-research-track/210/...
30. TUM paper. *Re-Using Concrete Test Scenarios Generally Is a Bad Idea*. https://mediatum.ub.tum.de/doc/1602418/1602418.pdf
31. arXiv (2024-09). *Coverage Metrics for a Scenario Database for the Scenario-Based Assessment of Automated Driving Systems*. https://arxiv.org/html/2409.01139v2
32. ACM TOSEM (2024-04-18). *Industry Practices for Challenging Autonomous Driving Systems with Critical Scenarios*. https://dl.acm.org/doi/10.1145/3640334
33. SAE (2025-03-31). *Advancing Verification and Validation for ADAS and ADS*. https://doi.org/10.4271/2025-01-8052
34. Future Transportation (2025-12-19). *Complex Test Scenarios for Functional Validation Prior to Type Approval*. https://doi.org/10.3390/futuretransp6010001
35. IEEE STV2 whitepaper (2024-10). *Simulation, Testing, Verification, and Validation of Autonomous Driving*. https://sagroups.ieee.org/adwg/wp-content/uploads/sites/661/2024/10/ADWG_STV2_whitepaper.pdf
36. Euro NCAP feature (2024-11-17). ADAS & Autonomous Vehicle International. https://www.autonomousvehicleinternational.com/news/testing/feature-nhtsa-and-euro-ncap-look-to-update-test-procedures-and-technologies.html
37. ADT (2025-04-07). *Efficient Test Fleet Management for ADAS and Autonomous Vehicles*. https://www.adt.media/autonomous-driving-systems/efficient-test-fleet-management-for-adas-and-ad/104389
38. Autonomous Vehicle International (2024-11-29). *Can virtual testing meet robust validation requirements?* https://www.autonomousvehicleinternational.com/features/feature-can-virtual-testing-meet-robust-validation-requirements.html
39. Lancs (Thorsten). *On the Effective Use of Fault Injection for the Assessment of AUTOSAR Safety Mechanisms*. https://ssg.lancs.ac.uk/wp-content/uploads/thorsten-on-the.pdf
40. DVCon (formal-assisted fault campaign). https://dvcon-proceedings.org/wp-content/uploads/formal-assisted-fault-campaign-for-iso26262-certification.pdf
41. PMC/MDPI (2024). *Automating Fault Test Cases Generation and Execution for Automotive Safety Validation via NLP and HIL Simulation*. https://pmc.ncbi.nlm.nih.gov/articles/PMC11125427/
42. MDPI (2020). *ASFIT: AUTOSAR-Based Software Fault Injection Test for Vehicles*. https://www.mdpi.com/2079-9292/9/5/850
43. Koopman (2018). *Practical Experience Report: Automotive Safety Practices vs. Accepted Principles*. https://users.ece.cmu.edu/~koopman/pubs/koopman18_safecomp.pdf
44. Sture (2019). *Fault Injection Testing for Automotive Software*. https://www.stureid.info/wp-content/uploads/2019/11/Fault-Injection-Testing-for-Automotive-Software.pdf
45. beefed.ai. *Fault Injection Techniques for Automotive Safety Testing*. https://beefed.ai/en/fault-injection-techniques-automotive-safety
46. Chalmers (2023). *Test Maintenance for Machine Learning Systems: A Case Study in the Automotive Industry*. https://research.chalmers.se/publication/536373/file/536373_Fulltext.pdf
47. MathWorks. *SIL and PIL Limitations*; *Verify AUTOSAR Code with SIL and PIL*. https://ww2.mathworks.cn/help/ecoder/ug/sil-and-pil-simulation-limitations.html
48. MathWorks (2014). *MIL, SIL and PIL: A New Paradigm in Model-Based Development*. https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/solutions/automotive/files/in-expo-2014/mil-sil-pil-a-new-paradigm-in-model-based-development.pdf
49. BMW/SERWTH. *Improving Model-Based Testing in Automotive Software Engineering*. https://www.se-rwth.de/publications/Improving-Model-based-Testing-in-Automotive-Software-Engineering.pdf
50. ACM (2015). *Model-based testing of automotive software: some challenges and solutions*. https://dl.acm.org/doi/10.1145/2744769.2747935
51. arXiv (1303.1006). *Industrial-Strength Model-Based Testing*. https://ar5iv.labs.arxiv.org/html/1303.1006
52. Quviq/Model-Based Testing of AUTOSAR. *Modelling of Autosar Libraries for Large Scale Testing*. https://doi.org/10.4204/eptcs.244.7
53. Tasking (whitepaper). *The hidden cost of ISO 26262 compliance*. https://www.tasking.com/files/content/technical-papers/tasking-whitepaper-the-hidden-cost-of-iso-26262-compliance.pdf
54. Ansys/SCADE (2022). *Efficient Development of Safe Automotive Software with ISO 26262 and ASPICE using SCADE*. https://ansys.synopsys.com/content/dam/amp/2022/april/webpage-requests/2022-ansys-scade-handbook.pdf
55. Qt. *Navigating Automotive Software Compliance: ASPICE vs. ISO 26262*. https://www.qt.io/software-insights/navigating-automotive-software-compliance-aspice-vs.-iso-26262
56. SRM Tech (2026-01-30). *Automated Unit Testing Framework for Automotive Embedded Software*. https://www.srmtech.com/knowledge-base/case-studies/enhancing-productivity-with-an-automated-unit-testing-framework-for-automotive-embedded-software/
57. Vector/Tata Elxsi (case study). *ECU Virtualization for Automated SIL Testing*. https://www.vector.com/en/industries/automotive/magazine/case-study-tata-elxsi-ecu-virtualization-for-automated-sil-testing/
58. MathWorks/Continental (2024). *How to Simulate and Test Heterogeneous AUTOSAR Software Compositions in Simulink*. https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/company/events/conferences/automotive-conference-stuttgart/2024/emea-mac-2024-continental-autosar-simulation.pdf
59. IEEE (2025-09-30). *Customized Data and Workflow Management for Continuous Development and Testing of Driving Functions*. https://doi.org/10.1109/iavvc61942.2025.11219482
60. AUDI (2026-03-23). *With virtual methods towards the future*. https://www.audi.com/en/articles/with-virtual-methods-towards-the-future-learn-more-about-robust-design-and-ai-at-audi-17878
61. IPG (2026-09-30). *No seasons, no limits: Rethink how vehicles are validated*. https://www.ipg-automotive.com/blog/no-seasons-no-limits-rethink-how-vehicles-are-validated
62. Siemens (2025-07-05). *Building bulletproof validation workflows*. https://blogs.sw.siemens.com/electronics-semiconductors/2025/07/05/bulletproof-validation/
63. ZVEI (2013). *Handbook for Robustness Validation of Automotive Electrical/Electronic Modules*. https://www.zvei.org/fileadmin/user_upload/Presse_und_Medien/Publikationen/2013/Oktober/Handbook_for_Robustness_Validation_of_Automotive_Electrical/Robust-Validation-EE-Modules.pdf
64. IEEE (2020). *An Automotive EHPS Software Reliability and Testing*. https://doi.org/10.1109/rams48030.2020.9153725
65. ASAM (2021-12-07). *Recommendations (Test Specification Study)*. https://report.asam.net/recommendations
66. NI (2023-11-02). *Shifting Left: The Evolution of Automotive Validation Test*. https://www.ni.com/en/solutions/transportation/adas-and-autonomous-driving-testing/adas-and-autonomous-driving-validation/shifting-left-the-evolution-of-automotive-validation-test.html
67. AUTOSAR (2020). *Overview of Functional Safety Measures in AUTOSAR* (cited in Lancs paper).
68. Scitepress (2013). *Increasing Efficiency of ISO 26262 V&V by Combining Fault Injection and Mutation Testing with Model based Development*. https://www.scitepress.org/papers/2013/45920/45920.pdf
69. DATE 2011. *Formal Specification and Systematic Model-Driven Testing of Embedded Automotive Systems*. https://past.date-conference.com/proceedings-archive/2011/DATE11/PDFFILES/02.7_5.PDF
70. MathWorks/NI (2025). *Validation of AUTOSAR Software Via Virtual ECU Using MATLAB and Simulink*. https://ww2.mathworks.cn/content/dam/mathworks/mathworks-dot-com/company/events/conferences/automotive-conference-india/2022/validation-of-autosar-software-via-virtual-ecu-using-matlab-and-simulink.pdf
71. FPT Software (2024). *Hitting The Road Faster with Virtual ECU Based On AUTOSAR*. https://fptsoftware.com/resource-center/blogs/hitting-the-road-faster-with-virtual-ecu-based-on-autosar
72. NI STL. *vECU Abstraction Levels*. https://ni.github.io/stl-model-framework-documentation/topic/vecu-abstraction-levels.html
73. Wayve (job posting). *HIL Platform Scalability Engineer*. https://jobsbyculture.com/jobs/wayve/hil-platform-scalability-engineer
74. TUM (2018). *Tests for mixed discrete-continuous systems*. https://mediatum.ub.tum.de/doc/1253910/1253910.pdf
75. SERWTH (2019). *Model-Based Testing of Software-Based System Functions*. https://www.se-rwth.de/publications/Model-Based-Testing-of-Software-Based-System-Functions.pdf

---

## 12. Closing

W3 documents 38 distinct problem records spanning MIL, SIL, PIL, HIL, vehicle-level validation, and cross-cutting test concerns. The dominant pattern: **testing is the last hardware-bound, late, manual, fragmented bottleneck** in automotive software. Shift-left/virtualization offers a path, but gaps in fidelity, traceability, standardization, scenario completeness, and skill remain the binding constraints. Quantitative anchors (47% V&V time, 91% invalid failures, 43.6% flaky scenarios, ~50% dev cost, $50k+/bench HIL) support prioritization but are indicative, not audited.

**End of W3 report.**


---
See also: [CROSS_REFERENCE_INDEX.md](./CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](./KNOWLEDGE_INDEX.md)

