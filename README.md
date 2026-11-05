# AutoEngResearch — Automotive Software Engineering Research

Author: Pratik Patil  
Contact: pratik.patil082@gmail.com  
Copyright (c) 2026 Pratik Patil. All rights reserved. Permission required to use or reproduce.

## Project purpose

This repository is a professional automotive engineering research publication and knowledge base. It investigates problems, gaps, root causes, and opportunities across the complete automotive software engineering lifecycle, with a focus on AUTOSAR, embedded systems, testing and validation, toolchains, skills, AI governance, and future engineering.

## Research mission

To build a structured, evidence-based, continuously maintainable research corpus that supports automotive software engineering improvement, problem discovery, and future solution design.

## Research scope

- Requirements and system engineering
- AUTOSAR Classic/Adaptive and middleware
- Software design, integration, and configuration
- Testing, validation, HIL, and vehicle certification
- Toolchains, standards, and compliance
- Human skills and organisational gaps
- Semiconductor-to-vehicle dependencies
- Vehicle domains and energy/thermal systems
- Cloud, fleet, and connected vehicle systems
- AI governance and regulations

## Research methodology

INSPECT → RESEARCH → VALIDATE → DOCUMENT → REVIEW → PUBLISH

All findings are traceable to problem IDs (W*, G*) and cited sources. Data verification is recorded in research/DATA_VERIFY.md.

## Repository structure

- `research/` — primary research content (book, artefacts, workstreams)
- `research/book/` — 45-chapter research book (Parts I–VII)
- `research/artefacts/` — 14 supporting artefacts
- `docs/` — supporting documentation and solution design
- `engineering/` — (planned) Rust AUTOSAR tooling and templates (currently top-level crates/tools/schemas/templates/platforms/stacks/drivers)

## Research navigation

- `research/REPO_MIGRATION_PLAN.md`
- `research/CROSS_REFERENCE_INDEX.md`
- `research/KNOWLEDGE_INDEX.md`
- `research/book/INDEX.md`
- `research/artefacts/`

## Problem catalogue

- Workstreams: `research/W1_requirements_system.md` … `research/W5_skills_ai.md`
- Gap reports: `research/G1_semiconductor_hardware.md` … `research/G4_energy_thermal.md`
- Consolidated catalogue: `research/book/part6_ch35_problem_catalogue.md`

## AI governance research

- `research/ai_governance.md`

## Contribution guidelines

Contributions require permission from the author. Please contact pratik.patil082@gmail.com.

## References and citations

- `research/artefacts/bibliography.md`
- `research/artefacts/glossary.md`

## Engineering code

The original Rust AUTOSAR ARXML tooling and templates remain available in `crates/`, `tools/`, `schemas/`, `templates/`, `platforms/`, `stacks/`, `drivers/`. See `README_CODE.md`.