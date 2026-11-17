# Master Prompt — W7 (ASIL / Mixed-Criticality / Multicore) + G5 (Domain Deep-Dive)

**Purpose:** Single detailed execution prompt for the next research wave, acting as project architect. Produces lesson-learnt + AI-ready guideline data in existing repo formats.
**Branch:** `research/asil-mixedcriticality-domains` (cut from `research/bootloader-flash` @ `10f96d8`, verified clean + pushed before cut)
**Status:** Active — prompt frozen; execution started (W7 seed + G5 seed)
**Date:** 2026-11-16

---

## 1. Architect's analysis (why this shape)

1. **Current corpus state:** 161 records (W1:18, W2:20, W3:38, W4:30, W5:25, W6:30) + G1–G4 + 45-chapter book + 14 artefacts, all integrated on `research/bootloader-flash` (`research/QUALITY_REVIEW.md:44` verdict PASS, 0 broken links, 8/8 artefacts W6-aware). New work must not regress this.
2. **What's missing:** (a) systematic ASIL A/B/C/D + QM treatment — today ASIL appears scattered (W2-004 FFI, W2-018 ASIL-B gap `research/book/part2_ch8_autosar_middleware.md:70`, W6-024 bootloader V&V, G2 safety gradient `research/G2_vehicle_domains.md:16`); no per-ASIL implication/testing matrix, no cross-ASIL/QM interference catalogue, no SWC/multicore/partition design rules; (b) BSW/MCAL integration is fragmented across W2-005/W2-012/G1-020/G1-021/W6-017/W6-021 — needs one canonical integration topic; (c) domain deep-dive (Middleware, Body, Powertrain, AD/ADAS, IVI/Infotainment, Cockpit, Zonal, HPC) exists only as G2 survey + DOMAIN_MAP taxonomy — needs per-domain software problem records with lesson-learnt.
3. **Design decision — two files, one wave:**
   - **W7 `research/W7_asil_mixed_criticality_multicore.md`** — Topic 1 (one topic): ASIL A/B/C/D implications + testing + cross-ASIL/QM + SWC design + multicore + multi-partition + application/BSW/MCAL integration. Planned 30 records (W7-001..W7-030). Owner of ASIL/multicore truth; other workstreams cross-ref it.
   - **G5 `research/G5_domain_deep_dive.md`** — Topic 2: per-domain deep dive (Middleware, Body, Powertrain, AD/ADAS, IVI/Infotainment, Cockpit, Zonal/HPC). Planned 32 records (G5-001..G5-032, 4 per domain). Owner of domain truth; cross-refs W7 for safety/multicore, W2 for AUTOSAR, W3 for testing, G1 for silicon, G3 for cloud, G4 for energy.
4. **Format discipline:** Both files follow the exact W/G schema (header block, §1 summary, §2 problem catalogue, §3 cross-cutting, §4 evidence index, §5 unanswered questions, §6 limitations) **plus two additive fields per record**: `Lesson learnt` and `AI-ready guideline`. No existing field renamed/removed — backward compatible with matrices, indices, and checks. New fields are appended at the end of each record.
5. **AI-ready meaning:** each `AI-ready guideline` is written as a tool-consumable rule: condition → required action → evidence artefact → human-approval gate. Lesson-learnt is a 15-year field note (what actually fails, what actually works).

---

## 2. Scope

### 2.1 W7 — ASIL / mixed-criticality / multicore (30 records)

| Block | IDs | Coverage |
|---|---|---|
| ASIL implications A→D | W7-001..W7-004 | Per-ASIL design/test/evidence implication delta (QM as W7-005) |
| Cross-ASIL + QM interference | W7-006..W7-009 | FFI memory/timing/execution, QM↔ASIL coupling, cascade failure, dependent-failure analysis |
| SWC design | W7-010..W7-013 | Partitioning by ASIL, runnable mapping, data consistency, end-to-end protection |
| Multicore | W7-014..W7-018 | Core allocation, scheduling, spinlocks, inter-core comm, timing analysis on multicore |
| Multi-partition / separation | W7-019..W7-022 | OS-Application/partitions, hypervisor guests, gateway between partitions, mode management |
| Application integration | W7-023..W7-025 | SWC→RTE→OS integration, COM/signal-service mapping, variant/calibration binding |
| BSW stack integration | W7-026..W7-028 | BSW module integration, configuration consistency, BSW scheduler/RTE contract |
| MCAL challenges | W7-029..W7-030 | MCAL portability/verification, MCU driver safety (canonical, cross-refs G1-020/021, W6-017) |

### 2.2 G5 — Domain deep-dive (32 records, 4 per domain)

Domains: **Middleware** (SOME/IP/DDS/gateway/time-sync), **Body** (BCM/access/comfort), **Powertrain** (EMS/TCM/aftertreatment), **AD/ADAS** (perception/fusion/planning), **IVI/Infotainment** (head unit, audio, nav), **Cockpit** (cluster/HUD/driver monitoring), **Zonal** (zonal controllers, E/E consolidation), **HPC** (central compute, virtualization, SOA).
Each domain: 1× architecture/integration record, 1× safety/timing record, 1× update/calibration record, 1× lesson-learnt/cross-domain record.

### 2.3 Explicit non-goals (this wave)

- No changes to W1–W6/G1–G4 record IDs or text (cross-ref only).
- No book chapter rewrites beyond cross-ref lines + problem-ID index rows (full prose deferred to integration wave).
- No quantitative invention: figures only where sourced; otherwise mark unquantified (DATA_VERIFY discipline).

---

## 3. Record schema (frozen)

Problem records use the W6 field order verbatim, then append the two new fields:

`- Domain: / Lifecycle stage: / Problem: / Who: / Root cause: / Contributing factors: / Technical limitations: / Tool limitations: / Process limitations: / Frequency/severity: / Time/cost impact: / Downstream effects: / Existing solutions: / Why insufficient: / Automation feasibility: / AI applicability: / Human approval: / Evidence/sources: / Confidence: / Unanswered questions:` then **`Lesson learnt:`** (field note, 1–3 sentences) then **`AI-ready guideline:`** (IF <condition> THEN <rule> WITH <artefact> GATE <approval>).

---

## 4. Method / evidence tiers

- Tier 2 Verify default: ISO 26262 (all parts relevant to software), ISO 21448, ISO/SAE 21434, AUTOSAR SWS/RST (OS, RTE, BSW, MCAL, FFI), MCU safety manuals, plus repo-internal cross-refs.
- Confidence High/Medium/Low per record. Stale anchors flagged (e.g. NHTSA 2020 already flagged in CROSS_CHECK C4).
- No invented numbers; no guessed URLs.

---

## 5. File map + integration checklist

New: `research/W7_asil_mixed_criticality_multicore.md`, `research/G5_domain_deep_dive.md`, this prompt file.
Later (integration wave, separate commits): KNOWLEDGE_INDEX (W7+G5 counts), CROSS_REFERENCE_INDEX, DOMAIN_MAP, TASK_COVERAGE, problem_to_root_cause_matrix, solution_vs_gap_matrix, bottleneck_catalogue, lifecycle_map, toolchain_map, glossary, CROSS_QUESTIONS, ISSUE_TRACKER, book ch7/ch8/ch15/ch16/ch22 problem-ID rows, QUALITY_REVIEW rerun (counts/links/IDs).

---

## 6. Phased execution

- **Phase A (this turn):** freeze prompt + seed W7 §1 + W7-001..W7-010 + G5 §1 + skeleton; commit + push.
- **Phase B:** W7-011..W7-030.
- **Phase C:** G5-001..G5-032 (4/domain).
- **Phase D:** index/artefact/book integration + overlap analysis + issue tracker + quality review + PR.

---

## 7. Acceptance criteria (Wave complete)

- W7 30/30 + G5 32/32 records, IDs unique, schema-conformant, each with Lesson learnt + AI-ready guideline.
- 0 broken links; all new IDs resolvable in KNOWLEDGE_INDEX; artefact totals updated; book INDEX intact at 45 chapters + new rows.
- QUALITY_REVIEW rerun PASS; branch pushed; PR opened to `research/bootloader-flash` (or main per orchestrator).

---
See also: [W7_asil_mixed_criticality_multicore.md](./W7_asil_mixed_criticality_multicore.md) | [G5_domain_deep_dive.md](./G5_domain_deep_dive.md) | [INTEGRATION_PLAN.md](./INTEGRATION_PLAN.md)
