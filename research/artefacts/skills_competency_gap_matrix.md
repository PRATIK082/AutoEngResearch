# Skills and Competency Gap Matrix

**Source:** `research/W5_skills_ai.md` (25 problem records) plus cross-references W1-015, W2-019, W3-038, W4-022.
**Scope:** Human skills, knowledge and workforce limitations affecting automotive software engineering, and AI/automation adoption barriers.
**Reading:** Each row is a recorded skill/workforce problem. 'Automation/AI feasibility' records what can realistically be automated or assisted; 'Human approval' records what must remain under human authority.

## Competency domains covered

| Domain | Related problem IDs |
|---|---|
| Embedded software (C/C++, RTOS, low-level) | W5-001; W1-015; W4-022 |
| AUTOSAR Classic & Adaptive methodology/tooling | W5-002; W2-019 |
| Functional safety (ISO 26262) competency | W5-003 |
| Automotive cybersecurity (ISO/SAE 21434, UNECE R155/R156) | W5-004 |
| AI/ML engineering in automotive context | W5-005; W1-004 |
| Requirements engineering skills | W5-006; W1-001/003 |
| System/architecture engineering skills | W5-007; W1-008/009 |
| Software integration expertise | W5-008; W1-006/007 |
| Testing and validation expertise | W5-009; W3-038 |
| Hardware/software co-design knowledge | W5-010 |
| Toolchain knowledge (vendor-specific) | W5-011; W4-004/022 |
| Knowledge management, silos, turnover | W5-012; W4-022 |
| Training and onboarding cost/time | W5-013 |
| Cross-discipline and cross-org communication | W5-014 |
| Compensation disadvantage vs tech rivals | W5-015 |
| AI adoption barriers (confidentiality/IP/NDA) | W5-016; W4-030 |
| LLM hallucination in safety-critical engineering | W5-017 |
| Automotive-specific datasets for local LLMs | W5-018 |
| AI-generated code safety/certification gap | W5-019 |
| Explainability and accountability of AI outputs | W5-020 |
| Legacy skills obsolescence vs new competencies | W5-021 |
| Education pipeline mismatch | W5-022 |
| Aging workforce / retirement cliff | W5-023 |
| SDV / centralised computing skill gap | W5-024 |
| Absence of AI governance in engineering workflows | W5-025 |

## Workforce and skill-gap records (W5)

| ID | Problem (skill gap) | Who experiences | Evidence / source | Time/cost impact | Existing solutions | Why insufficient | Automation feasibility | AI applicability | Human approval |
|---|---|---|---|---|---|---|---|---|---|
| W5-001 | Demand for embedded engineers grows faster than supply; ~80% of embedded engineering job postings remain unfilled for months (S3); 65% of 200 leading embedded systems companies struggle to fill key roles in IoT, microcontroller programming and embedded software development (S1, electronica survey). Semiconductor industry alone faces ~1.5M skilled-worker shortage over next 5 years (S1). | All (development, integration, validation) | Human skills / Embedded software | Development backlogs growing; EV/ADAS/secure-connectivity delays; rising costs from contractors; technical debt accumulation (S1). | Global recruitment, ed-tech/bootcamp partnerships, cross-disciplinary training, AI-assisted/low-code tools, outsourcing/co-development (S1). | Training takes years; bootcamps rarely cover safety-critical real-time depth; outsourcing fragments architecture. | Medium — AI/low-code can reduce routine embedded coding but not safety-critical design. | Medium — LLM code generation experimental for automotive C (S9); requires verification. | Required for safety-relevant outputs. |
| W5-002 | Many organizations cannot find talent proficient in AUTOSAR (Classic configuration, RTE, BSW, OS, MCAL) and Adaptive (service-oriented, DDS/SOME-IP, POSIX/Linux) (S1, S8). | Design, configuration, integration | Human skills / AUTOSAR middleware | Configuration rework, integration delays (implied by W1/W2 domains; not separately quantified). | Vendor training, internal upskilling, external engineering partners (S8). | Vendor training lags product versions; external help creates dependency. | High for configuration generation/validation (AI/ARXML intelligence). | Medium-High — LLM/RAG for ARXML and config automation emerging (S9). | Required for safety-relevant configuration changes. |
| W5-003 | Shortage concentrated in specific lifecycle parts, not the whole discipline: engineers who combine safety understanding with hands-on system/software experience, especially quantitative hardware safety analysis (FMEDA) connected to system-level safety goals, are hardest to source (S8). | Concept, development, verification, validation, release | Human skills / Functional safety | Safety case delays; rework; certification risk (qualitative). | Internal training, specialized hiring, upskilling systems/embedded engineers, external bounded analysis, structured pipelines (S8). | Scarce profiles cannot be hired at scale quickly (S8). | Low-Medium — AI can assist HARA/test artifact generation (S9, AEGIS) but not replace safety argumentation. | Medium experimental — LLM-based HARA artifact generation reported (S9); certification unresolved. | Mandatory — safety case ownership must stay human. |
| W5-004 | Cybersecurity roles in automotive face ~25% vacancy rate (S2); demand surges with connected vehicles and data privacy regulations (UNECE R155/R156, ISO/SAE 21434). | Requirements, design, implementation, testing, post-production | Human skills / Cybersecurity | Rework, compliance delays, recall/warranty exposure (qualitative). | Training, external partners, AI-assisted analysis. | Talent pool thin; standards evolving. | Medium — AI can assist threat modeling and code review. | Medium — LLM-based security compliance analysis experimental. | Mandatory for security architecture. |
| W5-005 | Demand for data/AI professionals expected to surge ~40% over two years (S2); organizations lack engineers combining ML with automotive safety, real-time constraints and certification; AI/ML engineering capabilities called out as gap (Task.md §5). | Perception, ADAS/AD, data engineering, MLOps | Human skills / AI-ML | Program delays; compensation inflation. | Partnerships (Google Gemini, Nvidia Drive Thor) (S7); global sourcing. | Partnerships do not build internal capability; global sourcing hits compensation wall. | Low for model safety; medium for data pipelines. | High for tooling, low for safety certification. | Mandatory for safety-critical ML. |
| W5-006 | Requirement ambiguity, duplication, inconsistent decomposition, poor traceability; engineers lack skills to write unambiguous, testable, safety-traceable requirements (Task.md §5; S9 notes hundreds of thousands of requirements per vehicle). | Requirements engineering | Human skills / Requirements engineering | Rework, release delays (qualitative). | AI-based requirements analysis, RAG compliance checking (S9). | GenAI hallucination risk; requirements are IP-protected, limiting cloud LLM use (S9). | Medium-High — LLM/RAG for requirements handling emerging but gap remains (S9). | Medium-High experimental. | Required for safety requirements. |
| W5-007 | Shortage of engineers able to perform functional/logical/physical architecture, interface definitions, ECU allocation, timing/resource analysis, MBSE, zonal/centralized computing design (Task.md §5). | System engineering and architecture | Human skills / Systems engineering | Architecture rework, integration delays. | Training, external architects. | Scarce senior architects. | Medium — AI can assist dependency/change-impact analysis. | Medium experimental. | Mandatory for architecture decisions. |
| W5-008 | Shortage of engineers who can perform multi-ECU, BSW/RTE/middleware/network/supplier integration and debug integration failures (Task.md §5). | Software integration | Human skills / Integration | Integration cycle delays, debugging time (Task.md §8). | HIL/SIL automation; CI/CD. | Tools need expert operation; environments fragile. | Medium-High for build/integration automation. | Medium — intelligent debugging emerging. | Required for release. |
| W5-009 | Shortage of HIL/SIL/MIL, scenario-based ADAS validation, fault injection, safety/cybersecurity testing expertise (Task.md §5). | Testing and validation, vehicle-level validation | Human skills / Testing & validation | Test execution time, HIL utilization bottlenecks (Task.md §8). | Automated test generation, simulation-driven testing (Task.md §7). | AI test generation experimental; safety evidence requires human review. | High for test execution; medium for test design. | Medium-High experimental. | Required for safety validation. |
| W5-010 | Shortage of engineers understanding both silicon (SoC/MCU, memory, accelerators) and software (drivers, MCAL, RTOS) (Task.md §5). | Architecture, design, bring-up | Human skills / HW-SW co-design | Bring-up delays, rework. | Co-development, reference architectures. | Talent pool thin. | Low-Medium. | Low-Medium. | Required. |
| W5-011 | Engineers lack proficiency in specific commercial tools (Vector, dSPACE, ETAS, EB, MATLAB/Simulink, DOORS, etc.); vendor-specific knowledge not portable (Task.md §5, §6). | All | Human skills / Toolchain | Onboarding time, productivity loss. | Vendor training, documentation. | Training lags versions. | Medium. | Medium — AI-based tool assistance. | Low risk. |
| W5-012 | Critical engineering knowledge resides in individuals; employee turnover causes knowledge loss; tacit knowledge (rationale for safety decisions, integration quirks) not documented (Task.md §5; S11, S14). | All | Human skills / Knowledge management | Knowledge-loss costs; replacement cost 16–213% of annual salary by role (S11 secondary); up to 40% training-time reduction possible with AI capture (S11). | Documentation, mentorship, wikis, AI knowledge capture (S11). | Documentation lags; mentorship depends on availability. | High for documentation capture; medium for tacit knowledge. | High — LLM/RAG, engineering knowledge graphs (Task.md §7). | Required for safety rationale. |
| W5-013 | Long onboarding (months to years) to reach productivity in automotive embedded/safety/toolchain; training cost high (Task.md §5, §8). | All | Human skills / Training | Training/onboarding listed as productivity bottleneck (Task.md §8); no automotive-specific quantification found. | Bootcamps, ed-tech partnerships, internal mobility, AI-assisted tools (S1). | Bootcamps rarely reach safety-critical depth. | Medium. | Medium-High. | Low. |
| W5-014 | Poor communication between software/hardware/system teams and across OEM–Tier-1–Tier-2–semiconductor boundaries; information and accountability lost (Task.md §5, §10). | All | Human skills / Collaboration | Rework, delays (qualitative). | PLM, requirements tools, MBSE. | Tools fragmented; interoperability gaps. | Medium. | Medium-High. | Required for contractual changes. |
| W5-015 | Traditional OEMs pay automotive software engineers ~$100K–$158K (median ~$125K), below tech-native EV rivals; Big Three cut 20,000+ salaried roles while posting 2,000+ open roles (nearly 400 AI-specific) — a skills swap; manufacturing employment fell 16,200 YoY (S7, S10). | All | Human skills / Compensation & retention | Attrition; recruitment cost; program risk. | Global sourcing, partnerships, employer branding (S7). | Pay parity not realistic near term (S7). | N/A (HR). | Medium — AI sourcing/automation. | Required. |
| W5-016 | Requirements are high-value IP protected by NDAs; exposing them to third-party cloud LLMs is unacceptable; therefore locally deployable (fine-tuned) models are required, which demand datasets, hardware, time (S9). | All (especially requirements handling) | AI/automation / Data governance | Slower AI adoption; manual requirements work persists. | Local fine-tuned models (Llama 3); RAG; on-prem deployment. | Data/compute barriers; small models lower quality. | High if data barrier solved. | High potential, currently limited. | Required for data policy. |
| W5-017 | GenAI models hallucinate (nonsensical/unfaithful content) in believable ways; direct unverified use in automotive is almost impossible; human expert in loop required (S9). | Requirements, code generation, compliance | AI/automation / Reliability | Verification overhead; potential defect escape. | RAG grounding, multi-agent debate, self-consistency, RECSIP, uncertainty evaluation, formal methods (Model-Driven Engineering) (S9). | Mitigations reduce but do not eliminate hallucination; certification not established. | Medium — mitigation automatable; final verification human. | Medium — AI helps generate; cannot self-certify. | Mandatory. |
| W5-018 | Main bottleneck for local LLM adoption is lack of publicly available specialized automotive datasets for fine-tuning, due to IP/confidentiality (S9). | All | AI/automation / Data | Slower AI adoption; manual work persists. | Synthetic data; RAG over internal docs; small fine-tunes. | Quality gap remains. | N/A (data). | High potential. | Required for data sharing. |
| W5-019 | AI/ML introduces non-determinism into safety-critical systems; ISO 26262 framework has gaps for ML lifecycle (ASIL A/B focus in literature); certification of AI-assisted or AI-implemented safety functions unresolved (S12, S13). | Design, implementation, validation, release | AI/automation / Functional safety & certification | Certification delays; scope reduction. | SOTIF; ML-specific ISO 26262 enhancements (S13); evidence-gap analysis (S12). | Standards still evolving; only ASIL A/B covered (S13). | Low for certification. | Low-Medium — AI can assist analysis but not certify. | Mandatory. |
| W5-020 | AI outputs used in engineering decisions lack explainability; accountability for AI-generated artifacts unclear; hallucination confidence (Degeneration-of-Thought) misleads (S9). | All | AI/automation / Governance | Rework, audit findings. | Uncertainty evaluation; LLM-as-judge; multi-agent debate (S9). | Partial mitigation; no standard. | Medium. | Medium. | Mandatory for safety-relevant. |
| W5-021 | Legacy automotive skills becoming obsolete; organizational demand for hybrid skill sets (traditional automotive + digital) critical; 45% of current automotive roles could be automated by 2030 (S2). | All | Human skills / Workforce transformation | Retraining cost; attrition. | Upskilling, internal mobility (S2, S8). | Scale and speed. | N/A. | Medium (AI-assisted training). | N/A. |
| W5-022 | University programs emphasize general-purpose computing over embedded/safety; graduate supply projected to fall short by 30% in EV/smart-manufacturing fields (S2); few graduates prepared for software-hardware-safety intersection (S1). | All (long-term) | Human skills / Education | Onboarding cost, training burden. | Industry-academia collaboration, bootcamps (S1, S2). | Scaling lag. | N/A. | Low. | N/A. |
| W5-023 | Sizable part of existing embedded workforce nearing retirement with few qualified replacements (S1); aging workforce compounds talent gap (S2). | All | Human skills / Demographics | Knowledge loss; capacity. | Mentorship, knowledge capture. | Late start. | Medium (knowledge capture). | Medium-High. | N/A. |
| W5-024 | New roles (SDV engineer, autonomous systems developer, data analytics specialist) require skills in central compute, service-oriented architecture, virtualization, OTA, cloud; scarce (S2, S7). | Architecture, software, integration | Human skills / SDV | Program delays; compensation inflation. | Global sourcing, partnerships (S7). | Pay parity gap. | Low-Medium. | Medium. | Required. |
| W5-025 | No established governance for AI use in safety-critical engineering: which tasks may use AI, required verification, approval gates, IP handling, audit trails (S9, S20). | All | AI/automation / Governance | Audit findings; inconsistent quality. | Internal policies (ad hoc). | Not standardized. | Medium (governance tooling). | Medium. | Mandatory. |

## Cross-workstream skill gaps (not duplicated in W5 detail)

| ID | Skill gap | Source |
|---|---|---|
| W1-015 | System engineering skill and workforce gaps (MBSE, requirements, architecture) | W1 |
| W2-019 | AUTOSAR methodology and tooling knowledge gaps; attachment-level training insufficient | W2 |
| W3-038 | Testing and validation expertise gaps (HIL, simulation, ML, cloud, safety) | W3 |
| W4-022 | Tool onboarding, training time, single-person expertise | W4 |

## Competency gap assessment

| Competency | Current state | Gap severity | Can be automated/assisted? | Must remain human? |
|---|---|---|---|---|
| Low-level embedded C/C++, RTOS, hardware interfaces | Scarce; 65% of 200 surveyed embedded-systems firms struggle to fill roles; ~80% of postings unfilled for months | High | Low-Medium (AI code generation experimental) | Yes (safety-critical design) |
| AUTOSAR Classic configuration (ARXML, BSW, RTE, OS, MCAL) | Scarce; vendor-specific; onboarding long | High | High for generation/validation; Medium for semantic reconciliation | Yes (safety-relevant config) |
| AUTOSAR Adaptive (service-oriented, DDS/SOME-IP, POSIX/Linux) | Even scarcer; platform still evolving | High | Medium | Yes |
| Functional safety (ISO 26262) analysis (FMEDA, safety case) | Process knowledge more available than analytical depth; ASIL C/D acute | High | Low-Medium (assistive only) | Yes (safety case ownership) |
| Automotive cybersecurity (TARA, secure boot, crypto, OTA security) | ~25% vacancy rate reported; cross-disciplinary | High | Medium (assistive) | Yes (security architecture) |
| AI/ML engineering under safety/real-time constraints | Acute; few bridge ML and functional safety; 40% demand surge reported | High | Low for model safety | Yes (safety-critical ML) |
| Requirements engineering (unambiguous, testable, traceable) | Ambiguity, duplication, poor traceability common | Medium-High | Medium-High (NLP quality checks) | Yes (safety-relevant sign-off) |
| System/architecture engineering (MBSE, interface governance) | MBSE adoption blocked; 22% could cite clear MBSE definition (unverified 66% reliance) | High | Low-Medium | Yes |
| Software integration expertise (multi-ECU, supplier integration) | Assumption-driven development common; late integration surprises | Medium-High | Medium (change-impact analysis) | Yes |
| Testing/validation (HIL, simulation, scenario coverage, ML testing) | Shortage across HIL, simulation, ML, cloud, safety | High | High for generation/scheduling; Medium for triage | Yes (safety claims) |
| Hardware/software co-design | Gaps in SoC, memory, power, thermal awareness | Medium-High | Low | Yes |
| Toolchain knowledge (vendor-specific) | Single-person expertise; onboarding weeks-months | Medium-High | Medium | Partly |
| Knowledge management and knowledge transfer | Silos, single-person expertise, turnover-driven loss | High | Medium-High (AI knowledge capture) | Partly |
| Cross-discipline/cross-org communication | Gaps between software, hardware, system teams and OEM/Tier-1/supplier | Medium-High | Low | Yes |
| AI governance and standards-aware AI use | Absence of AI governance in engineering workflows | Medium-High | Medium | Yes |

## AI and automation adoption barriers (from W5/W4)

| Barrier | Problem IDs | Description |
|---|---|---|
| Confidentiality / IP / NDA constraints | W5-016, W4-030 | Automotive engineering data is confidential; cloud LLMs risk IP leakage; on-premise/local models needed |
| LLM hallucination in safety-critical contexts | W5-017, W5-020 | Ungrounded outputs unacceptable for safety-relevant engineering; verification required |
| AI-generated code safety and certification gap | W5-019 | No accepted certification path for AI-generated safety-critical code |
| Lack of automotive-specific training datasets | W5-018 | General LLMs lack AUTOSAR/ARXML/safety-domain depth; fine-tuning data scarce and confidential |
| Explainability and accountability | W5-020 | AI outputs must be auditable and attributable; unclear responsibility |
| Absence of AI governance in workflows | W5-025 | No standards for AI use in safety-critical engineering; approval gates undefined |
| Legacy skills obsolescence | W5-021, W5-022 | Workforce not prepared for SDV/AI-native methods; education pipeline mismatch |
| Aging workforce / retirement cliff | W5-023 | Tacit knowledge concentrated in retiring engineers; knowledge loss risk |

## What can be automated vs. what requires human authority

| Category | Automation feasibility | Human authority required? |
|---|---|---|
| ARXML validation, canonicalization, diff normalization | High | Review for safety-relevant config |
| Test scheduling, failure triage, regression selection | High | Approval for skipping tests |
| Requirements quality checks (ambiguity, duplication) | Medium-High | Sign-off for safety-relevant requirements |
| Test case / scenario generation | Medium-High | Verification and safety approval |
| Change-impact analysis | Medium-High | Decision on changes |
| AI-assisted code generation (non-safety) | Medium | Review and test evidence |
| Safety case, HARA, FMEDA, certification | Low-Medium (assistive) | Mandatory human ownership |
| Security architecture and TARA | Medium (assistive) | Mandatory human ownership |
| Architecture authority and interface governance | Low-Medium | Mandatory human ownership |

## Limitations

- Workforce statistics (e.g., 65% of firms, 80% postings unfilled, 25% vacancy, 40% demand surge) are from industry/commercial sources; treat as indicative, not audited.
- No independent verification of training efficacy or onboarding duration.
- Skill-gap severity ratings are author synthesis from source reports, not measured data.

*End of skills and competency gap matrix.*