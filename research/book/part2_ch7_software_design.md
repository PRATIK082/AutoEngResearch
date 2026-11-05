# Part II — Chapter 7: Software Design and Development

**Book:** *The State of Automotive Software Engineering*  
**Part:** II — Complete Automotive Software Lifecycle  
**Chapter:** 7 — Software Design and Development  
**Source workstreams:** W2 (AUTOSAR & Middleware), W3 (Testing & Validation), W4 (Toolchain & Standards), W5 (Skills & AI)  
**Problem IDs in scope:** W2-006, W2-013, W2-014, W2-019, W3-001, W3-002, W3-032, W4-008, W4-009, W4-010, W4-020, W4-021, W4-028, W5-001, W5-005, W5-008, W5-009, W5-010, W5-011, W5-013, W5-015, W5-019, W5-021, W5-022, W5-023, W5-024

---

## 7.0 Chapter purpose and problem map

Software design and development in automotive covers embedded C/C++, Rust, Python automation, Java/TypeScript tooling, model-based development, AUTOSAR software components (SWC) and basic software (BSW), driver development, middleware, integration interfaces, reusable components, and code quality/maintainability. The research record shows that this stage is constrained by talent shortages, multicore distribution complexity, static-analysis burden, long build times, and a widening gap between legacy embedded skills and modern digital competencies.

| Problem ID | Title | Primary root cause (short) |
|---|---|---|
| W2-006 | Multicore distribution complexity in Classic | Static partitioned scheduling; manual optimisation |
| W2-013 | Classic OS (OSEK) scheduling limitations | OSEK fixed-priority; scope limits |
| W2-014 | Adaptive dynamic memory allocation non-determinism | C++ dynamic alloc vs. determinism tension |
| W2-019 | Skill/knowledge gaps in AUTOSAR methodology and tooling | Standard complexity; attachment-level training |
| W3-001 | MIL plant/control models simplified, validated late | Model simplification; late validation |
| W3-002 | MIL test cases not reused for code verification | Disconnect between model and code tests |
| W3-032 | Unit testing informal, weak strategies, poor evidence | No embedded unit-test culture; tool friction |
| W4-008 | Static analysis false positives and manual deviation effort (MISRA/CERT) | Rule ambiguity; tool precision limits |
| W4-009 | MISRA/CERT standard version churn and migration burden | Standard evolution; legacy codebases |
| W4-010 | Undecidable MISRA rules require manual review | Rules that cannot be decided automatically |
| W4-020 | Generic CI/CD pipelines unsuitable for safety-critical embedded | Pipeline assumptions mismatch embedded constraints |
| W4-021 | Long build/compilation times for large AUTOSAR codebases | Codebase size; toolchain performance |
| W4-028 | Manual data transformation via spreadsheets/scripts ("glue work") | Tool gaps; no canonical data flow |
| W5-001 | Embedded software engineer shortage | Capacity bottleneck; compensation gap |
| W5-005 | AI/ML engineering capabilities gap in automotive | Talent scarcity in ML + safety |
| W5-008 | Software integration expertise gaps | Integration is late and expert-dependent |
| W5-009 | Testing and validation expertise gaps | XiL expertise scarce |
| W5-010 | Hardware/software co-design knowledge gaps | Disciplinary silos |
| W5-011 | Toolchain knowledge gaps (vendor-specific) | Tool-specific expertise required |
| W5-013 | Training and onboarding time/cost | Long time-to-competence |
| W5-015 | Compensation disadvantage vs. tech-native rivals | Pay bands below tech industry |
| W5-019 | AI-generated code safety and certification gap | No safety case for AI-generated code |
| W5-021 | Legacy skills obsolescence vs. new digital competencies | Skill transformation pressure |
| W5-022 | Education pipeline mismatch | Curricula lag industry |
| W5-023 | Aging workforce / retirement cliff | Demographic risk |
| W5-024 | SDV / centralized computing skill gap | New stack requires new skills |

---

## 7.1 Model-based development (W3-001, W3-002)

Model-based development (MBD) is widespread in automotive, especially for control software (Simulink/Stateflow, TargetLink). The research record documents two persistent problems:

1. **MIL plant/control models are simplified and validated late** (W3-001). Plant models omit second- and third-order effects; control models are validated against simplified plants; fidelity gaps surface late in HIL or vehicle testing.
2. **MIL test cases are not reused for code verification** (W3-002). Test artifacts created at model level are rarely carried through to SIL/PIL/HIL, so effort is duplicated and coverage gaps appear.

These are not purely testing problems; they are design-stage problems because the model is the design artifact. The disconnect between model-level and code-level verification is a recurring theme (W3-023 horizontal traceability).

---

## 7.2 Embedded languages and runtimes (W2-013, W2-014, W5-021)

### 7.2.1 Classic AUTOSAR (C, OSEK) (W2-013, W2-006)

Classic AUTOSAR targets hard real-time, ASIL-D-capable systems using C and an OSEK-based OS. The OS is fixed-priority, partitioned per core, with no dynamic task migration and no dynamic scheduling; multimedia/telematics features (streams, file systems, GUI) are out of scope, forcing multi-OS integration (W2-013). Multicore distribution is manual and non-trivial: runtime spikes in brake ECUs require splitting Com_MainFunctionRx into multiple functions with different periods (W2-006). Amdahl's law, synchronisation overhead, and resource contention limit gains.

### 7.2.2 Adaptive AUTOSAR (C++) (W2-014, W2-003, W2-020)

Adaptive AUTOSAR uses C++ on POSIX OSs, enabling service-oriented, dynamic architectures for HPC/zonal/ADAS. However, dynamic memory allocation causes fragmentation and non-deterministic alloc/dealloc times, violating real-time and safety requirements; the standard discusses deterministic allocators and phase-limited allocation but leaves implementation to vendors (W2-014). Adaptive is still stabilising (W2-003), and vendor implementation divergence limits portability (W2-020).

### 7.2.3 Rust and other languages

Rust is increasingly discussed for automotive safety-critical code, but the research record does not yet document a mature, certified Rust automotive stack. The broader pattern is legacy skills obsolescence vs. new digital competencies (W5-021): engineers trained in C/OSEK face steep retraining for C++/Rust/Python/TypeScript stacks, and education pipelines are slow to adapt (W5-022).

---

## 7.3 Multicore and scheduling (W2-006, W2-013)

Multicore ECUs are increasingly common, but Classic AUTOSAR's static, partitioned scheduling model makes distribution manual and error-prone (W2-006). Optimisation is typically done late, after integration, with no automated distribution tooling. Classic OS scheduling limits (W2-013) force hypervisor/multi-OS complexity when consolidating feature sets. These problems compound as zonal/centralised architectures increase core counts (Chapter 6, W1-016).

---

## 7.4 Code quality and static analysis (W4-008, W4-009, W4-010)

MISRA C/C++ and CERT C/C++ are standard in automotive, but:

- Static analysis produces false positives, requiring manual deviation effort (W4-008).
- Standard version churn (MISRA C:2012 → C:2023, CERT updates) creates migration burden for legacy codebases (W4-009).
- Some MISRA rules are undecidable by static analysis and require manual review (W4-010).

The result is that compliance with coding standards consumes significant engineering effort, and the boundary between tool-detectable and human-judgement rules is not always clear. Static analysis is also a DevOps/CI concern (W4-020).

---

## 7.5 Build, CI/CD, and tooling (W4-020, W4-021, W4-028)

Large AUTOSAR codebases have long build/compilation times (W4-021), and generic CI/CD pipelines (Jenkins, GitLab CI, GitHub Actions) are unsuitable for safety-critical embedded software because they assume cloud-native, stateless, containerised workflows that do not match toolchain licensing, hardware-in-the-loop dependencies, and safety evidence requirements (W4-020). Manual data transformation via spreadsheets and scripts ("glue work") fills gaps between tools (W4-028). These are productivity bottlenecks that scale with codebase and variant count.

---

## 7.6 Unit testing and code-level verification (W3-032)

Unit testing in automotive is often informal, with weak strategies and poor evidence (W3-032). ASPICE and ISO 26262 require unit-level verification, but practice lags: tests are written late, coverage is uneven, and evidence is hard to maintain. This is a design/development-stage problem as much as a testing problem because it reflects the absence of a testable design culture.

---

## 7.7 Talent and workforce (W5-001, W5-005, W5-008, W5-009, W5-010, W5-011, W5-013, W5-015, W5-019, W5-021, W5-022, W5-023, W5-024)

The research record documents a structural talent shortage across embedded software, AUTOSAR, functional safety, cybersecurity, AI/ML, systems, integration, testing, hardware/software co-design, and toolchain (W5-001, W5-005, W5-008, W5-009, W5-010, W5-011, W5-024). Contributing factors include compensation disadvantage vs. tech-native rivals (W5-015), long training/onboarding time (W5-013, 12–24 months inferred for ASIL B ownership), education pipeline mismatch (W5-022), aging workforce/retirement cliff (W5-023), and legacy skills obsolescence (W5-021).

AI-generated code safety and certification gaps (W5-019) are particularly relevant to this chapter: LLMs can generate code and tests, but there is no safety case or certification precedent for AI-generated safety-critical code, and hallucination risk (W5-017) makes verification mandatory. AI applicability is therefore high for non-safety scaffolding and low for safety-critical code.

---

## 7.8 Cross-cutting root causes

1. **Talent shortage is structural** and spans embedded, AUTOSAR, safety, cybersecurity, AI/ML, systems, integration, testing, and toolchain (W5-001, W5-005, W5-008, W5-009, W5-010, W5-011, W5-024).
2. **Multicore and scheduling complexity** is manual and late (W2-006, W2-013).
3. **Static analysis burden** (false positives, version churn, undecidable rules) consumes significant effort (W4-008, W4-009, W4-010).
4. **Build and CI/CD pipelines do not fit** safety-critical embedded constraints (W4-020, W4-021).
5. **Model-based development has a model-to-code verification gap** (W3-001, W3-002).
6. **AI code generation is not certifiable** for safety-critical use (W5-019).
7. **Skills are obsolescing faster than education and training can adapt** (W5-021, W5-022, W5-023).

---

## 7.9 Evidence limitations

- Talent-shortage figures (80% unfilled postings, 40% demand surge) are single-source (recruiting/commercial firms) and flagged as unverified (CROSS_CHECK, CQ-056).
- Build-time and CI/CD claims are based on vendor and practitioner sources; no industry-wide benchmark is available.
- MISRA/CERT burden is well documented qualitatively but not quantified across organisations.
- Rust-in-automotive maturity is not covered by the research record; claims about Rust should be treated as emerging, not established.

---

## 7.10 Cross-references

- **Chapter 5 (Requirements Engineering):** ambiguity and traceability (W1-001, W1-003) drive rework that surfaces in code.
- **Chapter 6 (System Engineering and Architecture):** timing (W1-010), zonal architecture (W1-016), and variant explosion (W1-011) shape design constraints.
- **Chapter 8 (AUTOSAR and Middleware):** Classic/Adaptive platforms (W2-003, W2-014, W2-018) and middleware (W2-007, W2-008) are the runtime substrate.
- **Chapter 9 (Configuration and Engineering Data):** ARXML configuration (W2-001) and glue work (W4-028) are development-stage burdens.
- **Chapter 11 (Testing and Validation):** unit testing (W3-032), model fidelity (W3-001), and coverage (W3-026) validate design.
- **Part V (Future Engineering):** AI-native engineering and agentic workflows (W5-016–W5-020, W5-025).

---

*End of Chapter 7.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

