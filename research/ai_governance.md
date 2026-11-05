# AI governance and regulations in automotive software

Author: Pratik Patil — pratik.patil082@gmail.com  
Copyright (c) 2026 Pratik Patil. All rights reserved. Permission required to use or reproduce.

## Scope

Emerging AI governance, regional regulations, EU AI Act, functional safety/cybersecurity of AI, data confidentiality, IP protection, certification challenges, human oversight.

## Key themes

### EU AI Act and high-risk systems
- High-risk AI systems in safety-critical automotive contexts require risk management, data governance, transparency, human oversight, and conformity assessment.
- Automotive AI (perception, decision-making, driver monitoring) likely falls under high-risk categories depending on use and deployment.

### Functional safety and SOTIF
- ISO 26262 functional safety must be extended for AI/ML components; traditional V-model assumptions break for learned behavior.
- SOTIF (ISO 21448) addresses unknown unsafe scenarios, including AI perception failures and distribution shift.

### Cybersecurity
- ISO/SAE 21434 and UNECE R155 apply to AI pipelines, data flows, model updates, and OTA.
- AI models and training data are attack surfaces (data poisoning, model extraction, adversarial inputs).

### Data confidentiality and IP protection
- Training data, model weights, telemetry, and scenario libraries are sensitive.
- Cross-border data transfer constraints (GDPR, China data security laws) affect fleet learning and cloud training.

### Certification challenges
- Lack of standards for AI component certification, explainability, and reproducibility.
- Tool qualification (ISO 26262-8) for AI-assisted engineering tools is unresolved.

### Human oversight
- AI cannot be solely responsible for safety-critical decisions; human-in-the-loop required.
- Automation bias and over-reliance risks in engineering and vehicle operation.

## Problem interlinks

- W5-012..W5-025 (AI adoption barriers)
- W3-033 (ML testing), W3-034 (scenario coverage)
- W4-029 (AI tool trust/certification)
- G3-016 (privacy/consent), G3-020 (cybersecurity backend)

## Regional regulations (summary)

- EU: AI Act, GDPR, UNECE R155/R156, ISO 21434.
- US: NHTSA guidelines, state privacy laws, FTC data practices.
- China: Data Security Law, Personal Information Protection Law, MIIT requirements.

## Evidence limitations

- Regulatory landscape evolving; dates and requirements subject to change.
- No automotive-specific AI certification standard fully established as of 2026.

## Recheck status

- Expanded from initial stub to full research note (2026-11-06).
- Cross-linked to W5, W3, W4, G3.
- Further source verification pending.