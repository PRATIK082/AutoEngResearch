# End-to-End Automotive Software Lifecycle Map

**Artefact:** #2 â€” End-to-End Automotive Software Lifecycle Map  
**Parent project:** THE STATE OF AUTOMOTIVE SOFTWARE ENGINEERING  
**Date compiled:** 2026-11-05  
**Scope:** Global automotive software industry â€” ICE, hybrid, EV, ADAS/ADS; Classic + Adaptive AUTOSAR; OEM/Tier-1/Tier-2 perspectives.  
**Evidence basis:** Task.md Â§3D (Complete Software Development Lifecycle) + W1â€“W4 problem reports + Part II book chapters (Ch. 5â€“13) + DOMAIN_MAP.md.

---

## 0. How to read this map

This document maps the **complete automotive software lifecycle** from stakeholder need to fielded, maintained software. For each stage it records:

1. **Purpose** â€” what the stage must deliver.
2. **Key activities** â€” what engineers actually do.
3. **Inputs / outputs** â€” artefacts flowing in and out.
4. **Stakeholders** â€” who owns or contributes.
5. **Tool categories** â€” representative tools.
6. **Standards / regulations** â€” normative drivers.
7. **Principal problems** â€” cross-referenced problem IDs (W1â€“W4).
8. **Typical duration / effort** â€” where evidenced (estimates flagged).

Stages are not strictly sequential: iteration, rework loops, and late changes are the norm (W1-005, W1-012). The "V-model" is the nominal shape; the reality is an iterative, multi-organisation network.

---

## 1. Lifecycle at a glance (end-to-end flow)

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 0  Vehicle & product strategy / stakeholder needs (market, regulation, brand)         â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 1  Requirements Engineering  (SHRQ â†’ SYRQ â†’ SWRQ â†’ HWRQ)                            â”‚
â”‚  Problems: W1-001..006, W1-012, W1-014, W1-015, W4-006, W4-007, W5-006                    â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 2  System Engineering & Architecture  (functional/logical/physical; ECU allocation)   â”‚
â”‚  Problems: W1-007..011, W1-016..018, W2-002, W2-009, W2-016, W2-018, W4-012, W5-007      â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 3  Software Design & Development  (SWC, BSW, drivers, models, code)                  â”‚
â”‚  Problems: W2-001, W2-005, W2-006, W2-012..014, W4-003, W4-008..010, W5-001, W5-002     â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 4  Configuration & Engineering Data  (ARXML, ECU config, variants, RTE gen)          â”‚
â”‚  Problems: W1-011, W1-013, W2-001, W2-011, W2-012, W2-015, W4-002..005                   â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 5  Software Integration  (component â†’ ECU â†’ multi-ECU â†’ network â†’ vehicle)            â”‚
â”‚  Problems: W1-006, W1-007, W1-017, W2-002, W2-007..009, W2-012, W5-008                    â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 6  Testing & Validation  (unit, component, SIL, MIL, PIL, HIL, system)               â”‚
â”‚  Problems: W3-001..W3-038, W4-018..021, W4-029                                            â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 7  Vehicle-Level Validation  (prototype, road/track, EMC, climate, ADAS scenarios)    â”‚
â”‚  Problems: W3-017..W3-022, W4-017, W4-015                                                 â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 8  Safety / Cybersecurity / Compliance  (ISO 26262, ASPICE, ISO/SAE 21434, UNECE)    â”‚
â”‚  Problems: W1-014, W3-025, W3-034, W4-011..015, W4-026                                    â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 9  Release & Production  (baselines, flashing, EOL test, manufacturing)               â”‚
â”‚  Problems: W2-010, W4-016, W4-020                                                          â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  STAGE 10  Field / Maintenance / OTA  (monitoring, defect analysis, updates, recall)        â”‚
â”‚  Problems: G1-007, W2-010, W4-016, G1-035                                                  â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

Feedback loops run from every later stage back to Stage 1 (requirements) and Stage 2 (architecture): late defects are the dominant cost (W1-012: requirements-driven rework 35â€“45% of total rework, cross-industry).

---

## 2. Stage-by-stage detail

### Stage 0 â€” Vehicle & product strategy / stakeholder needs

| Field | Content |
|---|---|
| Purpose | Define what vehicle, features, markets, regulations, and brand promise the product must satisfy. |
| Activities | Market analysis, regulatory horizon scanning, feature planning, platform/variant strategy, business case. |
| Inputs | Market data, regulatory roadmaps, competitor analysis, prior-generation field data. |
| Outputs | Product brief, feature list, high-level requirements seed, platform strategy. |
| Stakeholders | OEM product planning, marketing, regulatory affairs, R&D leadership. |
| Tool categories | PLM (Windchill/Teamcenter), portfolio tools, market research. |
| Standards | Regional regulations, UNECE, Euro NCAP, EPA/Euro. |
| Problems | W1-011 (variant explosion seeds here), W5-022 (education pipeline). |
| Duration | Monthsâ€“years (platform cycles 4â€“7 years). |

---

### Stage 1 â€” Requirements Engineering

| Field | Content |
|---|---|
| Purpose | Translate stakeholder intent into unambiguous, verifiable, traceable requirements at four abstraction levels. |
| Activities | Elicitation, analysis, decomposition (SHRQ â†’ SYRQ â†’ SWRQ â†’ HWRQ), prioritisation, change management, traceability link creation, compliance mapping. |
| Inputs | Product brief, regulations, prior requirements, field defect data, competitor specs. |
| Outputs | Requirements database (DOORS/Polarion/Codebeamer), ICDs, traceability matrices. |
| Stakeholders | OEM requirements engineers, Tier-1/Tier-2 systems engineers, safety/cybersecurity leads, suppliers. |
| Tool categories | ALM/requirements tools (IBM DOORS, Siemens Polarion, PTC Codebeamer, Jama Connect, Visure, reqSuite). |
| Standards | ISO 26262 (Part 8 requirements), ASPICE (SWE.1/SWE.2), ISO/SAE 21434, SOTIF, UNECE R155/R156. |
| Problems | W1-001 (ambiguity), W1-002 (abstraction disconnect), W1-003 (traceability burden), W1-004 (ML/AI requirements), W1-005 (late changes), W1-006 (incomplete specs to suppliers), W1-012 (rework cost), W1-014 (compliance overhead), W1-015 (skills), W4-006 (legacy tooling), W4-007 (traceability gaps), W5-006 (RE skills). |
| Duration | Weeksâ€“months per release; continuous throughout program. |

**Key structural fact:** Wang et al. (ASE 2026, Infineon data) report up to **67 product requirements per stakeholder requirement** in extreme decomposition cases across 8,082 SHRQ / 5,870 PRQ. The abstraction gap is structural, not cosmetic.

---

### Stage 2 â€” System Engineering & Architecture

| Field | Content |
|---|---|
| Purpose | Define functional, logical, and physical architecture; allocate functions to ECUs/compute; define interfaces, timing, variants. |
| Activities | Functional architecture, logical/physical decomposition, ECU allocation, network architecture, interface definition (ICDs), timing budgeting, variant planning, MBSE modelling, safety concept (HARA/ASIL), cybersecurity concept (TARA). |
| Inputs | Requirements (Stage 1), hardware constraints, prior architecture. |
| Outputs | System architecture model, ICDs, ECU allocation matrix, network topology, timing budgets, safety/cybersecurity concepts. |
| Stakeholders | OEM system architects, Tier-1 systems engineers, safety/cybersecurity engineers. |
| Tool categories | MBSE (Cameo/SysML, Rhapsody, Enterprise Architect, PREEvision, SystemWeaver), network tools (CANoe, CANalyzer), architecture tools. |
| Standards | ISO 26262 (Part 3/4 concept & system), ASPICE (SYS.1â€“SYS.4), ISO/SAE 21434, AUTOSAR methodology. |
| Problems | W1-007 (unstable interfaces), W1-008 (inconsistent architecture), W1-009 (MBSE barriers), W1-010 (timing unsolved), W1-011 (variant explosion), W1-016 (zonal/central transition), W1-017 (change visibility), W1-018 (network timing), W2-009 (hypervisors), W2-016 (Classicâ†’Adaptive migration), W2-018 (ASIL gap), W4-012 (safety case maintenance), W5-007 (system skills)., W7-001..W7-005, W7-010, W7-014, W7-018, W7-019, G5-001, G5-002, G5-026, G5-029, G5-030, G5-032 |
| Duration | Months; architecture frozen relatively early but evolves. |

**Critical unsolved problem:** Timing analysis for AUTOSAR software remains "important and still largely unsolved" (TU Munich); AUTOSAR standard lacks timing aspects (TU Braunschweig). Timing violations surface at integration, not design.

---

### Stage 3 â€” Software Design & Development

| Field | Content |
|---|---|
| Purpose | Implement software components, basic software, drivers, and middleware on target platforms. |
| Activities | SWC development (C/C++/Rust), model-based development (Simulink), BSW configuration, MCAL/driver development, middleware (SOME/IP, DDS) implementation, code generation, unit testing, static analysis (MISRA/CERT). |
| Inputs | Architecture (Stage 2), requirements (Stage 1), hardware specs. |
| Outputs | Source code, generated code, unit tests, static-analysis reports, build artefacts. |
| Stakeholders | Embedded developers, model-based developers, BSW engineers, driver engineers (OEM/Tier-1/Tier-2/ESPs). |
| Tool categories | IDEs (Eclipse, Visual Studio), compilers (IAR, Green Hills, GCC), model tools (MATLAB/Simulink), static analysis (QAC/PRQA, Parasoft, LDRA, Polyspace), version control (Git). |
| Standards | MISRA C/C++, CERT C/C++, AUTOSAR C++14, ISO 26262 Part 6 (software), ASPICE SWE.3/SWE.4. |
| Problems | W2-001 (ARXML config complexity), W2-005 (vendor lock-in), W2-006 (multicore distribution), W2-012 (RTE contract), W2-013 (Classic OS limits), W2-014 (Adaptive dynamic memory), W4-003 (manual ARXML editing), W4-008 (static-analysis false positives), W4-009 (MISRA version churn), W4-010 (undecidable rules), W5-001 (embedded shortage), W5-002 (AUTOSAR skills), W5-010 (HW/SW co-design)., W7-010..W7-013, W7-022, W7-026, W7-028..W7-030, G5-005, G5-009, G5-013, G5-017, G5-021, G5-025 |
| Duration | Monthsâ€“years (dominant development effort). |

---

### Stage 4 â€” Configuration & Engineering Data

| Field | Content |
|---|---|
| Purpose | Manage and generate the engineering data that defines ECU configuration: ARXML, system descriptions, ECU extracts, software component descriptions, variant conditions, RTE generation. |
| Activities | ARXML authoring/editing, system extract generation, ECU configuration, variant binding (SystemDesign/CodeGen/PreCompile/Link/PostBuild), RTE generation, consistency checking, version control of ARXML. |
| Inputs | Architecture (Stage 2), requirements (Stage 1), SWC definitions. |
| Outputs | ARXML files (system, ECU extract, SWC desc, BSW config), generated RTE code, configuration baselines. |
| Stakeholders | Configuration engineers, system integrators, toolchain managers. |
| Tool categories | AUTOSAR configurators (DaVinci Developer/Configurator, EB tresos Studio, SystemDesk), ARXML editors (AutoSAR.io, arxml-cli), MATLAB AUTOSAR Blockset. |
| Standards | AUTOSAR ARXML Serialization Rules, AUTOSAR Methodology & Templates. |
| Problems | W1-011 (variant explosion), W1-013 (ARXML interoperability/drift), W2-001 (ARXML complexity), W2-011 (variant binding), W2-012 (RTE contract), W2-015 (ARXML VCS friction), W4-002 (ARXML version/dialect), W4-003 (manual editing), W4-004 (tool lock-in), W4-005 (diff/merge)., W7-023, W7-024, W7-027, G5-003, G5-007, G5-023, G5-027, G5-031 |
| Duration | Continuous; peaks at integration. |

**Key fact:** ARXML is the central engineering data format; a single ECU's configuration can reach hundreds of MB; tag reordering by different tools makes diffs meaningless (W2-015, W4-003).

---

### Stage 5 â€” Software Integration

| Field | Content |
|---|---|
| Purpose | Compose components into ECUs, ECUs into networks, networks into vehicles; verify interfaces and timing. |
| Activities | Component integration, ECU integration (BSW + RTE + SWC), network integration (CAN/Ethernet/gateways), multi-ECU integration, supplier integration, integration debugging, environment management. |
| Inputs | Built software (Stage 3), configuration (Stage 4), hardware. |
| Outputs | Integrated ECU software, integrated vehicle software baseline, integration test reports, defect list. |
| Stakeholders | Integration engineers (OEM/Tier-1), supplier integration teams, system engineers. |
| Tool categories | Integration frameworks, network analysers (CANoe, CANalyzer), debuggers, HIL (early). |
| Standards | ISO 26262 Part 4 (system integration), ASPICE SYS.5/SWE.5. |
| Problems | W1-006 (incomplete specs â†’ integration as discovery), W1-007 (interface governance), W1-017 (change visibility), W2-002 (Classic/Adaptive interop & gateways), W2-007 (SOME/IP discovery/jitter), W2-008 (DDS determinism), W2-009 (hypervisors), W2-012 (RTE consistency), W5-008 (integration skills)., W7-020..W7-022, W7-025, W7-026, G5-001, G5-004, G5-008, G5-012, G5-016, G5-020, G5-024, G5-028, G5-032 |
| Duration | Weeksâ€“months; often the critical path. |

**Pattern:** "Incomplete requirements + no system-level architecture + no interface governance = integration as discovery" (W1 cross-cutting). This is the single most cited root cause of late integration surprises.

---

### Stage 6 â€” Testing & Validation (XiL)

| Field | Content |
|---|---|
| Purpose | Verify and validate software at multiple levels of abstraction: unit, component, integration, system. |
| Activities | Unit testing, component testing, integration testing, MIL, SIL, PIL, HIL, system testing, regression testing, performance/stress/fault-injection/boundary/robustness/negative testing, safety validation, cybersecurity testing, diagnostic/network validation. |
| Inputs | Requirements (Stage 1), architecture (Stage 2), integrated software (Stage 5). |
| Outputs | Test results, coverage reports, defect reports, compliance evidence. |
| Stakeholders | Test engineers, developers, quality/safety managers, testing service providers. |
| Tool categories | Unit/component (VectorCAST, Tessy, GoogleTest/GoogleMock, Parasoft), MIL/SIL (MATLAB/Simulink, dSPACE), HIL (dSPACE, NI, OPAL-RT, AVL), test management (Polarion, Codebeamer). |
| Standards | ISO 26262 Part 4/5/6, ASPICE SWE.4â€“SWE.6, ASAM XIL, ISO 29119. |
| Problems | W3-001..W3-038 (38 records), W4-018 (HIL scarcity), W4-019 (vECU fidelity), W4-020 (CI/CD suitability), W4-021 (build times), W4-029 (test tool interop)., W7-015, W7-017, W7-025, G5-006, G5-010, G5-014, G5-018, G5-022 |
| Duration | **Dominant cost:** up to ~50% of total software development cost (Boehm/Basili 2001, cited in ISO 26262 context); V&V is the most time-consuming activity (47% of ~600 professionals, Perforce 2024). |

**Key metrics (sourced):**
- Testing verifies/vast complexity; "not enough time to test more" top quality concern (24% overall; 44% for 5â€“10 yrs exp) (Perforce 2024).
- Up to **91% of failed test executions were invalid** in four automotive system-testing projects; **27â€“53%** linked to unreliable test infrastructure (ACM ICSE-SEIP 2022).
- HIL benches ~$50k+ each, ~100â€“200 tests/day, not parallelizable.
- 80% of integration issues surface ~6 months before SOP; feedback to dev 4â€“6 weeks (KPIT 2025).
- OEM SOP delays >2 years due to late software errors (HCLTech 2025).
- 43.6% of benchmark ADAS/ADS scenarios flaky in CARLA (2026 study).

---

### Stage 7 â€” Vehicle-Level Validation

| Field | Content |
|---|---|
| Purpose | Validate software in real vehicles under real-world conditions: prototype, road/track, EMC, climate, durability, ADAS scenarios, fleet. |
| Activities | Prototype vehicle integration, road/track testing, environmental chamber (climate, EMC, vibration), durability, sensor validation, ADAS scenario validation (Euro NCAP, UNECE), real-world scenario coverage, fleet validation, homologation. |
| Inputs | Integrated vehicle (Stage 5/6), test scenarios, regulatory requirements. |
| Outputs | Validation reports, homologation evidence, defect reports, ODD coverage data. |
| Stakeholders | Validation engineers, test drivers, homologation teams, safety engineers, OEMs. |
| Tool categories | Test fleet management (CONiX.tfs), data loggers, scenario databases, track instrumentation. |
| Standards | Euro NCAP, UNECE R10/R155/R156, ISO 26262, SOTIF (ISO 21448), regional type approval. |
| Problems | W3-017 (proving-ground scope explosion), W3-018 (ADAS scenario coverage/completeness), W3-019 (simulation flakiness), W3-020 (scenario reuse unsafe), W3-021 (environmental/EMC/durability physical), W3-022 (test fleet/data inefficiency), W4-017 (SOTIF doesn't scale), W4-015 (UNECE R155)., G5-010, G5-014, G5-015 |
| Duration | Monthsâ€“years; track availability and weather dependent. |

---

### Stage 8 â€” Safety / Cybersecurity / Compliance

| Field | Content |
|---|---|
| Purpose | Produce the evidence and approvals that the software meets functional safety, cybersecurity, and process-quality requirements. |
| Activities | Hazard analysis & risk assessment (HARA), safety requirements, ASIL decomposition, safety case, TARA, cybersecurity requirements, verification/validation evidence assembly, ASPICE assessments, tool qualification, audit preparation. |
| Inputs | All prior artefacts (requirements, architecture, code, tests, validation). |
| Outputs | Safety case, cybersecurity case, compliance evidence packages, assessment reports, type-approval submissions. |
| Stakeholders | Safety engineers, cybersecurity engineers, quality managers, auditors, regulators. |
| Tool categories | Safety tools (medini analyze, PREEvision, SCADE), TARA tools (ThreatGet), ALM traceability, compliance reporting. |
| Standards | ISO 26262, ISO/SAE 21434, ISO 21448 (SOTIF), ISO 24089, ASPICE, UNECE R155/R156/R10, MISRA Compliance:2020, tool qualification (ISO 26262 Part 8). |
| Problems | W1-014 (standards fragmentation), W3-025 (compliance doc burden for testing), W3-034 (safety validation statistical evidence), W4-011 (ISO 26262 overhead), W4-012 (safety case maintenance), W4-013 (ASPICE overhead), W4-014 (ISO/SAE 21434 integration), W4-015 (UNECE R155), W4-026 (standards/regional fragmentation)., W7-001..W7-009, W7-013, W7-024, G5-011, G5-015, G5-026, G5-030 |
| Duration | Continuous; peaks before release. |

---

### Stage 9 â€” Release & Production

| Field | Content |
|---|---|
| Purpose | Baseline, build, flash, and release software to manufacturing and to vehicles; end-of-line (EOL) testing. |
| Activities | Configuration baseline freeze, software versioning, release approval, ECU flashing/programming, bootloader validation, manufacturing EOL testing, production programming, release documentation. |
| Inputs | Validated software baseline, compliance evidence, manufacturing specs. |
| Outputs | Released software baseline, flashed ECUs, EOL test records, release notes. |
| Stakeholders | Release managers, manufacturing engineers, quality, suppliers. |
| Tool categories | Build systems (Jenkins/GitLab/GitHub Actions, safety-qualified variants), flashing tools, EOL test rigs, PLM. |
| Standards | ISO 26262 Part 7 (production), IATF 16949, UNECE R156 (SUMS), ISO 24089. |
| Problems | W2-010 (OTA/FOTA complexity seeds here), W4-016 (UNECE R156/ISO 24089 update engineering), W4-020 (CI/CD unsuitable for safety-critical), W4-021 (long build times), W6-001..W6-030 (bootloader & flash programming: standardisation, secure boot, memory layout, UDS flash, power transients, recovery, testing, manufacturing/EOL, toolchain, OTA orchestration)., W7-024, W7-027, G5-003, G5-007, G5-011, G5-027, G5-031 |
| Duration | Weeksâ€“months per release; multiple releases per program. |

---

### Stage 10 â€” Field / Maintenance / OTA

| Field | Content |
|---|---|
| Purpose | Monitor fielded vehicles, analyse defects, deliver updates, manage recalls and warranty. |
| Activities | Field monitoring, defect analysis, OTA campaign management (background install, safe-state activation, rollback), warranty/recall management, post-production maintenance, obsolescence management. |
| Inputs | Field telemetry, defect reports, released baselines. |
| Outputs | OTA campaigns, defect reports, service bulletins, recalls. |
| Stakeholders | Field service, OTA engineers, quality, cybersecurity, OEMs. |
| Tool categories | OTA platforms, telematics, field data analytics, diagnostics (UDS/DoIP). |
| Standards | UNECE R156, ISO 24089, ISO/SAE 21434, UNECE R155. |
| Problems | G1-007 (lifecycle mismatch / obsolescence), G1-035 (chip shortages delay validation), W2-010 (OTA/FOTA complexity), W4-016 (update engineering complexity)., G5-003, G5-011, G5-015, G5-019, G5-031 |
| Duration | Vehicle lifetime (15+ years) â€” the longest stage. |

**Structural mismatch:** Vehicle service life is 15+ years while semiconductors reach EOL in <7 years (G1-007). Obsolescence, last-time buys, and ECU redesigns dominate this stage.

---

## 3. Cross-stage (horizontal) concerns

| Concern | Affected stages | Key problems |
|---|---|---|
| Traceability (req â†’ code â†’ tests â†’ safety) | 1â€“9 | W1-003, W3-023, W4-007 |
| Variant management | 1, 2, 4, 6, 7, 9, 10 | W1-011, W2-011, W3-011, W4-005, W7-024, G5-003, G5-007, G5-027, G5-031 |
| Change management / late changes | 1â€“10 | W1-005, W1-017, W4-012 |
| Compliance / standards | 1â€“10 | W1-014, W3-025, W4-011, W4-013, W4-014, W4-026 |
| Toolchain fragmentation | 1â€“10 | W4-001, W4-023, W4-028 |
| Human skills | 1â€“10 | W1-015, W2-019, W3-038, W4-022, W5-001..W5-025 |
| Semiconductor/hardware dependency | 0, 2, 3, 5, 6, 10 | G1-001..G1-035 |

---

## 4. Lifecycle effort distribution (indicative, sourced where possible)

| Activity | Share of software dev effort | Source |
|---|---|---|
| Verification & validation | ~47% (most time-consuming) | Perforce 2024 (~600 professionals) |
| Software testing (cost share) | Up to ~50% of total dev cost | Boehm/Basili 2001, cited ISO 26262 context |
| Documentation (compliance) | ~41% (2nd most time-consuming) | Perforce 2024 |
| Data transformation / reconciliation ("glue work") | 10â€“30% (estimate) | W4-001 (hypothesis, not independently quantified) |
| Requirements-driven rework | 35â€“45% of total rework (cross-industry) | Capers Jones / Namcook (W1-012) |

---

## 5. V-model vs. reality

Nominal (V-model): requirements â†’ design â†’ code â†’ unit test â†’ integration test â†’ system test â†’ acceptance, with each left-side artefact mirrored on the right.

Reality (research findings):
- Requirements are ambiguous and incomplete at intake (W1-001, W1-006); suppliers fill gaps with assumptions.
- Architecture is defined late and inconsistently (W1-008); no single source of truth (W1-017).
- Integration becomes discovery (W1-006, W1-007).
- Testing is the dominant cost and is incomplete under time pressure (W3-026).
- Compliance evidence is assembled late and manually (W3-025, W4-011).
- Late changes propagate poorly (W1-005, W4-012).
- Shift-left ambition (33% implemented) lags reality (W3-031).

---

## 6. Sources

- Task.md Â§3D (Complete Software Development Lifecycle) and Â§8 (Time/Cost/Productivity).
- W1 (Requirements & System Engineering), W2 (AUTOSAR & Middleware), W3 (Testing & Validation), W4 (Toolchain & Standards).
- DOMAIN_MAP.md (domains, ECUs, networks, standards, stakeholders).
- Part II book chapters 5â€“13 (Requirements, System Engineering, Software Design, AUTOSAR, Configuration, Integration, Testing, HIL/Vehicle, Release).
- Perforce 2024 State of Automotive Software Development Report; ACM ICSE-SEIP 2022; KPIT 2025; HCLTech 2025; Boehm/Basili 2001 (cited in ISO 26262 context); Capers Jones/Namcook (W1-012).

---

*End of lifecycle_map.md â€” End-to-End Automotive Software Lifecycle Map (Artefact #2).*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

## G6–G11 Wave Lifecycle Mapping

| ID | Primary Lifecycle Stage(s) | Domain Focus |
|---|---|---|
| G6-045 | Stage 2–3 (System Engineering & Software Design) | Semiconductor CDD → BCM/PMIC/Zonal |
| G6-046 | Stage 2–3 (System Engineering & Software Design) | Semiconductor CDD model governance across partitions |
| G6-047 | Stage 3–4 (Configuration & Software Integration) | PMIC power-domain integration in zonal controllers |
| G6-048 | Stage 3–4 (Configuration & Software Integration) | Semiconductor-to-vehicle dependency tracking |
| G6-049 | Stage 4–5 (Software Integration) | Zonal controller software consolidation |
| G6-050 | Stage 4–5 (Software Integration) | Cross-partition FFI evidence generation |
| G6-051 | Stage 5 (Software Integration) | A/B partial update semantic compatibility |
| G6-052 | Stage 5 (Software Integration) | OTA rollback and campaign consistency |
| G6-053 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation |
| G6-054 | Stage 5–6 (Software Integration → Testing) | Semiconductor obsolescence impact assessment |
| G6-055 | Stage 6 (Testing & Validation) | Test coverage for semiconductor-to-vehicle integration |
| G7-010 | Stage 5–7 (Software Integration → Vehicle Validation) | Zonal mixed-criticality FFI evidence |
| G7-011 | Stage 5–7 (Software Integration → Vehicle Validation) | A/B update rollout across zones |
| G7-012 | Stage 6–7 (Testing → Vehicle Validation) | HIL validation for mixed-criticality zonal architectures |
| G7-013 | Stage 6–7 (Testing → Vehicle Validation) | Cabinet-level test for semiconductor-in-the-loop |
| G7-014 | Stage 7 (Vehicle-Level Validation) | Type-approval and certification workflows |
| G7-015 | Stage 7 (Vehicle-Level Validation) | Production validation and field quality |
| G7-016 | Stage 7–8 (Vehicle Validation → Compliance) | Regulatory submission and type-approval |
| G7-017 | Stage 8 (Compliance) | ISO 26262 and ISO/SAE 21434 evidence assembly |
| G7-018 | Stage 8–9 (Compliance → Release) | Package configuration and signing |
| G7-019 | Stage 9 (Release/Prod) | Flash programming and EOL testing |
| G7-020 | Stage 9–10 (Release → Field) | OTA update and field monitoring |
| G8-024 | Stage 3–4 (Software Design & Configuration) | Middleware service version skew across ECUs |
| G8-025 | Stage 3–4 (Software Design & Configuration) | Interface governance stops at single-release |
| G8-026 | Stage 4–5 (Software Integration) | Cross-domain timing and power budget arbitration |
| G8-027 | Stage 4–5 (Software Integration) | Bootloader safety patterns and evidence automation |
| G8-028 | Stage 4–5 (Software Integration) | Universal flash tool interface gaps |
| G8-029 | Stage 5–6 (Software Integration → Testing) | ISO 26262 processes and ASPICE safety case tools |
| G8-030 | Stage 5–6 (Software Integration → Testing) | Bootloader safety patterns and doc automation |
| G8-031 | Stage 5–6 (Software Integration → Testing) | Mandatory safety case documentation |
| G8-032 | Stage 5–6 (Software Integration → Testing) | For interface and toolchain changes |
| G8-033 | Stage 6 (Testing & Validation) | Test coverage for safety-relevant changes |
| G8-034 | Stage 6–7 (Testing → Vehicle Validation) | For safety evidence |
| G9-006 | Stage 6 (Testing & Validation) | Test coverage gaps for semiconductor-to-vehicle integration |
| G9-007 | Stage 6–7 (Testing → Vehicle Validation) | XiL/HIL validation for mixed-criticality zonal architectures |
| G9-008 | Stage 7 (Vehicle-Level Validation) | Type-approval and certification testing |
| G9-009 | Stage 7–8 (Vehicle Validation → Compliance) | Regulatory submission evidence |
| G9-010 | Stage 8 (Compliance) | ISO 26262 and ISO/SAE 21434 evidence assembly |
| G9-011 | Stage 8–9 (Compliance → Release) | Package configuration and signing |
| G9-012 | Stage 8 (Compliance) | FFI evidence tracing and documentation |
| G9-013 | Stage 9 (Release/Prod) | Test fleet management and validation |
| G9-014 | Stage 9–10 (Release → Field) | OTA update monitoring and defect analysis |
| G9-015 | Stage 10 (Field / Maintenance / OTA) | Field callback and recall campaigns |
| G9-016 | Stage 10 (Field / Maintenance / OTA) | End-of-life and last-time-buy coordination |
| G10-015 | Stage 8 (Compliance) | PMIC characterization and semiconductor-to-vehicle certification |
| G10-016 | Stage 8–9 (Compliance → Release) | Regulatory compliance for PMIC and zonal architecture |
| G10-017 | Stage 8 (Compliance) | ISO 26262 and ISO/SAE 21434 evidence gaps |
| G10-018 | Stage 8–9 (Compliance → Release) | Type-approval and certification workflows |
| G10-019 | Stage 9 (Release/Prod) | PMIC field programming and EOL testing |
| G10-020 | Stage 9–10 (Release → Field) | OTA update compliance and monitoring |
| G10-021 | Stage 9 (Release/Prod) | FFI evidence for production configurations |
| G10-022 | Stage 9–10 (Release → Field) | Qualification-scope traceability |
| G10-023 | Stage 10 (Field / Maintenance / OTA) | Field analytics and anomaly detection |
| G10-024 | Stage 10 (Field / Maintenance / OTA) | OTA platform security and update governance |
| G10-025 | Stage 10 (Field / Maintenance / OTA) | End-of-life and regulatory re-certification |
| G11-008 | Stage 2–3 (System Engineering & Software Design) | Semiconductor roadmap alignment with CDD |
| G11-009 | Stage 2–3 (System Engineering & Software Design) | CDD schema governance and versioning |
| G11-010 | Stage 3–4 (Configuration & Software Integration) | PMIC safety/ESD mapping to ISO 26262/21434 |
| G11-011 | Stage 3–4 (Configuration & Software Integration) | Cross-project CDD governance gap |
| G11-012 | Stage 4–5 (Software Integration) | Zonal controller software consolidation gaps |
| G11-013 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility |
| G11-014 | Stage 4–5 (Software Integration) | OTA rollback and campaign consistency |
| G11-015 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps |
| G11-016 | Stage 5–6 (Software Integration → Testing) | Semiconductor obsolescence impact assessment |
| G11-017 | Stage 6 (Testing & Validation) | Test infrastructure for cross-domain validation |
| G11-018 | Stage 7–10 (Vehicle Validation → Field) | Last-time-buy, obsolescence, and EOL coordination |

| G6-001 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-002 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-003 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-004 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-005 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-006 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-007 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-008 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-009 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-010 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-011 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-012 | Stage 4–5 (Software Integration) | A/B partial update semantic incompatibility for semiconductor CDD |
| G6-013 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-014 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-015 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-016 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-017 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-018 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-019 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-020 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality FFI validation gaps for semiconductor CDD |
| G6-021 | Stage 7–10 (Vehicle Validation → Field) | Semiconductor obsolescence impact assessment |
| G6-022 | Stage 7–10 (Vehicle Validation → Field) | Semiconductor obsolescence impact assessment |
| G6-023 | Stage 7–10 (Vehicle Validation → Field) | Semiconductor obsolescence impact assessment |
| G6-024 | Stage 7–10 (Vehicle Validation → Field) | Semiconductor obsolescence impact assessment |
| G6-025 | Stage 7–10 (Vehicle Validation → Field) | Semiconductor obsolescence impact assessment |
| G6-026 | Stage 7–10 (Vehicle Validation → Field) | Semiconductor obsolescence impact assessment |
| G6-027 | Stage 7–10 (Vehicle Validation → Field) | Semiconductor obsolescence impact assessment |
| G7-001 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality partitioning without FFI evidence |
| G7-002 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality partitioning without FFI evidence |
| G7-003 | Stage 5–6 (Software Integration → Testing) | Mixed-criticality partitioning without FFI evidence |
| G7-004 | Stage 7–10 (Vehicle Validation → Field) | Zonal mixed-criticality FFI evidence gaps |
| G7-005 | Stage 7–10 (Vehicle Validation → Field) | Zonal mixed-criticality FFI evidence gaps |
| G7-006 | Stage 7–10 (Vehicle Validation → Field) | Zonal mixed-criticality FFI evidence gaps |
| G8-001 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-002 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-003 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-004 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-005 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-006 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-007 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-008 | Stage 4–5 (Software Integration) | Chip packaging variation impact on ECU configuration |
| G8-009 | Stage 5–6 (Software Integration → Testing) | Semiconductor lot-to-lot electrical variation verification |
| G8-010 | Stage 5–6 (Software Integration → Testing) | Semiconductor lot-to-lot electrical variation verification |
| G8-011 | Stage 5–6 (Software Integration → Testing) | Semiconductor lot-to-lot electrical variation verification |
| G8-012 | Stage 5–6 (Software Integration → Testing) | Semiconductor lot-to-lot electrical variation verification |
| G8-013 | Stage 5–6 (Software Integration → Testing) | Semiconductor lot-to-lot electrical variation verification |
| G8-014 | Stage 5–6 (Software Integration → Testing) | Semiconductor lot-to-lot electrical variation verification |
| G8-015 | Stage 5–6 (Software Integration → Testing) | Semiconductor lot-to-lot electrical variation verification |
| G9-001 | Stage 5–6 (Software Integration → Testing) | Body domain electrification with mixed ASIL levels validation |
| G9-002 | Stage 5–6 (Software Integration → Testing) | Body domain electrification with mixed ASIL levels validation |
| G9-003 | Stage 5–6 (Software Integration → Testing) | Body domain electrification with mixed ASIL levels validation |
| G9-004 | Stage 5–6 (Software Integration → Testing) | Body domain electrification with mixed ASIL levels validation |
| G10-001 | Stage 4–5 (Software Integration) | PMIC regulatory compliance gap analysis |
| G10-002 | Stage 4–5 (Software Integration) | PMIC regulatory compliance gap analysis |
| G10-003 | Stage 4–5 (Software Integration) | PMIC regulatory compliance gap analysis |
| G10-004 | Stage 4–5 (Software Integration) | PMIC regulatory compliance gap analysis |
| G10-005 | Stage 4–5 (Software Integration) | PMIC regulatory compliance gap analysis |
| G10-006 | Stage 7–10 (Vehicle Validation → Field) | AD/ADAS sensor validation under real-world conditions |
| G10-007 | Stage 7–10 (Vehicle Validation → Field) | AD/ADAS sensor validation under real-world conditions |
| G10-008 | Stage 7–10 (Vehicle Validation → Field) | AD/ADAS sensor validation under real-world conditions |
| G10-009 | Stage 7–10 (Vehicle Validation → Field) | AD/ADAS sensor validation under real-world conditions |