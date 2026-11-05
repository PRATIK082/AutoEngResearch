# Part III — Chapter 18: Toolchain and Interoperability Problems

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 18 — Toolchain and Interoperability Problems  
**Source workstreams:** W4, W1, W2, W3, G3  

---

## 18.0 Chapter purpose and problem map

Toolchain fragmentation is a root cause, not just a symptom (knowledge index cross-cutting theme 2). This chapter catalogues the 30 W4 problems plus related toolchain issues in W1/W2/W3/G3.

| Group | Problem IDs | Count |
|---|---|---|
| Fragmentation/formats | W4-001, W4-002, W4-003, W4-005 | 4 |
| AUTOSAR config tools | W4-004 | 1 |
| Requirements/ALM tools | W4-006, W4-007 | 2 |
| Static analysis/standards | W4-008, W4-009, W4-010 | 3 |
| Compliance tools/process | W4-011, W4-012, W4-013 | 3 |
| Cybersecurity tooling | W4-014, W4-015 | 2 |
| Software update tooling | W4-016 | 1 |
| SOTIF validation | W4-017 | 1 |
| HIL infrastructure | W4-018 | 1 |
| vECU/simulation | W4-019 | 1 |
| CI/CD | W4-020 | 1 |
| Build | W4-021 | 1 |
| Tool onboarding | W4-022 | 1 |
| Supplier/OEM exchange | W4-023 | 1 |
| PLM/PDM integration | W4-024 | 1 |
| Open-source tooling | W4-025 | 1 |
| Standards evolution | W4-026 | 1 |
| Licensing/cost | W4-027 | 1 |
| Glue work (spreadsheets/scripts) | W4-028 | 1 |
| Test tool interop/evidence | W4-029 | 1 |
| AI-assisted tooling trust | W4-030 | 1 |
| ARXML authoring (W1/W2) | W1-013, W2-001, W2-015 | 3 |
| AUTOSAR tool lock-in (W2) | W2-005 | 1 |
| Cloud tooling (G3) | G3-010, G3-011, G3-024 | 3 |

---

## 18.1 Fragmented heterogeneous toolchain (W4-001, W4-002, W4-003, W4-005, W4-028)

- **W4-001 — Fragmented heterogeneous toolchain, poor interoperability.**
- **W4-002 — Proprietary data formats and ARXML version/dialect fragmentation.**
- **W4-003 — Manual ARXML authoring/editing error-prone and unscalable.**
- **W4-005 — Configuration diff/merge and variant management in ARXML immature.**
- **W4-028 — Manual data transformation via spreadsheets/scripts ("glue work").**

**Root cause:** tools are chosen per organisation for local optimisation; exchange formats are proprietary or inconsistently implemented; a large share of engineering effort is spent moving data between tools by hand.

---

## 18.2 Requirements and ALM tools (W4-006, W4-007)

- **W4-006 — Requirements management tools legacy-heavy, hard to onboard.**
- **W4-007 — End-to-end traceability gaps (req → code → tests → safety).**

**Root cause:** DOORS-centric legacy tooling; traceability links are manual and not semantically validated (W1-003).

---

## 18.3 AUTOSAR configuration tool lock-in (W4-004, W2-001, W2-005, W2-015)

- **W4-004 — AUTOSAR configuration tool lock-in and licensing cost.**
- **W2-001 — ARXML configuration complexity and manual burden.**
- **W2-005 — Vendor lock-in and BSW toolchain interoperability.**
- **W2-015 — ARXML round-trip and version-control friction.**

**Root cause:** AUTOSAR standardises methodology, not tooling; configuration tools (DaVinci, EB tresos, SystemDesk) are proprietary and expensive; ARXML round-trips badly.

---

## 18.4 Static analysis and coding standards (W4-008, W4-009, W4-010)

- **W4-008 — Static analysis false positives and manual deviation effort (MISRA/CERT).**
- **W4-009 — MISRA/CERT standard version churn and migration burden.**
- **W4-010 — Undecidable MISRA rules require manual review.**

**Root cause:** coding standards evolve faster than tool support; some rules are undecidable by construction, forcing manual review that does not scale.

---

## 18.5 Compliance tooling and process overhead (W4-011, W4-012, W4-013, W4-026)

- **W4-011 — ISO 26262 compliance heavy documentation/process overhead.**
- **W4-012 — Safety case maintenance and change-impact re-assessment cost.**
- **W4-013 — Automotive SPICE assessment overhead and tailoring burden.**
- **W4-026 — Standards evolution and regional regulatory fragmentation.**

**Root cause:** compliance evidence is generated and maintained manually; safety cases are not incrementally updated; standards overlap and conflict across regions.

---

## 18.6 Cybersecurity tooling (W4-014, W4-015)

- **W4-014 — ISO/SAE 21434 cybersecurity engineering integration cost.**
- **W4-015 — UNECE R155 type-approval and CSMS supply-chain burden.**

**Root cause:** cybersecurity engineering is bolted onto existing processes; CSMS evidence spans the supply chain, which most OEMs cannot fully observe.

---

## 18.7 Software update tooling (W4-016, G4-023)

- **W4-016 — UNECE R156 / ISO 24089 software update engineering complexity.**
- **G4-023 — Charging software OTA and version management (EVSE + vehicle).**

**Root cause:** software update engineering requires a full SUMS, version identity, and rollback capability across heterogeneous fleets.

---

## 18.8 SOTIF validation (W4-017)

- **W4-017 — SOTIF validation of unknown unsafe scenarios doesn't scale.**

**Root cause:** SOTIF (ISO 21448) requires enumerating unknown unsafe scenarios; no tool can generate or bound this set.

---

## 18.9 HIL infrastructure (W4-018, W3-010, W3-016)

- **W4-018 — HIL test bench scarcity and scheduling bottlenecks.**
- **W3-010 — HIL cost and scalability.**
- **W3-016 — HIL test environment management (idle benches, scheduling).**

**Root cause:** HIL benches are capital-intensive and not virtualisable; scheduling is manual.

---

## 18.10 vECU and simulation (W4-019, W3-004, W3-007)

- **W4-019 — vECU fidelity gaps and integration friction with simulation.**
- **W3-004 — SiL cannot test all HiL functionalities; vECU fidelity.**
- **W3-007 — vECU standardisation gap (FMI vs AUTOSAR).**

**Root cause:** vECU standardisation is split between FMI (simulation) and AUTOSAR (software); fidelity is vendor-specific and unverified.

---

## 18.11 CI/CD and build (W4-020, W4-021)

- **W4-020 — Generic CI/CD pipelines unsuitable for safety-critical embedded.**
- **W4-021 — Long build/compilation times for large AUTOSAR codebases.**

**Root cause:** CI/CD tooling (Jenkins, GitLab CI, GitHub Actions) assumes cloud-native, stateless builds; safety-critical embedded builds require licensed tools, hardware-in-the-loop, and long compilation.

---

## 18.12 Tool onboarding and single-person expertise (W4-022, W5-011, W5-013)

- **W4-022 — Tool onboarding, training time, single-person expertise.**
- **W5-011 — Toolchain knowledge gaps (vendor-specific).**
- **W5-013 — Training and onboarding time/cost.**

**Root cause:** tools are vendor-specific and complex; training is informal; expertise concentrates in individuals (W5-012).

---

## 18.13 Supplier/OEM toolchain mismatch (W4-023)

- **W4-023 — Supplier/OEM toolchain mismatch and data exchange friction.**

**Root cause:** see Chapter 16.6; toolchains are chosen per organisation.

---

## 18.14 PLM/PDM integration (W4-024)

- **W4-024 — PLM/PDM vs engineering-data (ARXML) integration gaps.**

**Root cause:** PLM systems manage mechanical/ECU hardware data; ARXML manages software configuration; the two do not synchronise.

---

## 18.15 Open-source tooling (W4-025)

- **W4-025 — Open-source AUTOSAR tooling immature and unsupported.**

**Root cause:** open-source AUTOSAR tools (OpenAUTOSAR, Arctic Core) exist but lack certification, support, and feature parity with commercial tools.

---

## 18.16 Standards evolution and regional fragmentation (W4-026, W1-014)

- **W4-026 — Standards evolution and regional regulatory fragmentation.**
- **W1-014 — Compliance overhead and standards fragmentation.**

**Root cause:** standards (ISO, UNECE, AUTOSAR) evolve on different cycles; regional regulations diverge; OEMs must satisfy multiple overlapping regimes.

---

## 18.17 Licensing, infrastructure, and cost (W4-027)

- **W4-027 — Tool licensing, infrastructure and cost constraints.**

**Root cause:** automotive toolchains require expensive per-seat licences, dongles, and on-premise infrastructure; cloud-hosted options are limited by IP and security concerns.

---

## 18.18 AI-assisted tooling trust (W4-030, W5-016 … W5-020, W5-025)

- **W4-030 — AI-assisted engineering tooling trust, certification, IP/data constraints.**
- **W5-016** AI adoption barriers: confidentiality, IP, NDA constraints.
- **W5-017** LLM hallucination in safety-critical engineering.
- **W5-018** Lack of automotive-specific datasets for fine-tuning local LLMs.
- **W5-019** AI-generated code safety and certification gap.
- **W5-020** Explainability and accountability of AI outputs.
- **W5-025** Absence of AI governance in engineering workflows.

**Root cause:** AI tooling is promising but blocked by confidentiality, hallucination, certification, and governance gaps (see Chapter 19).

---

## 18.19 Cloud and fleet tooling (G3-010, G3-011, G3-024)

- **G3-010 — Fleet-management platform fragmentation across mixed-OEM fleets.**
- **G3-011 — Lack of common vehicle-data standards and interoperability.**
- **G3-024 — Third-party telematics and API integration fragmentation.**

**Root cause:** fleet and cloud tooling is fragmented; no common vehicle-data standard; third-party APIs are inconsistent.

---

## 18.20 Cross-cutting toolchain root causes

1. **Tool fragmentation is a root cause, not a symptom** (W4-001, W1 cross-cutting theme 2).
2. **Data does not flow between tools** without manual glue work (W4-028, W4-024).
3. **Lock-in is economic and technical** (W4-004, W4-027).
4. **Compliance multiplies documentation** (W4-011, W4-013, W4-026).
5. **AI tooling is blocked by governance** (W4-030, W5-025).

---

## 18.21 Evidence limitations

- Tool-capability claims reflect vendor positioning and should be treated as indicative.
- Licensing cost figures are list prices; negotiated discounts are not public.

---

## 18.22 Cross-references

- **Chapter 14 (Technical):** static analysis (W4-008 … W4-010) and build time (W4-021).
- **Chapter 15 (Architecture):** MBSE tooling (W1-009) depends on toolchain.
- **Chapter 16 (Integration):** ARXML and toolchain mismatch (W4-002, W4-023).
- **Chapter 17 (Testing):** test tool interop (W3-036, W4-029).
- **Chapter 19 (Human Skills):** toolchain skills (W5-011) and onboarding (W4-022).
- **Chapter 20 (Organisational/Supplier):** supplier toolchain mismatch (W4-023).
- **Chapter 22 (Quality/Safety/Cyber):** compliance overhead (W4-011 … W4-013).

---

*End of Chapter 18.*