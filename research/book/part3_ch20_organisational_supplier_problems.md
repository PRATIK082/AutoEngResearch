# Part III — Chapter 20: Organisational and Supplier Problems

**Book:** *The State of Automotive Software Engineering*  
**Part:** III — Industry Problem Investigation  
**Chapter:** 20 — Organisational and Supplier Problems  
**Source workstreams:** W1, W4, W5, G1, G2, G3  

---

## 20.0 Chapter purpose and problem map

This chapter collects problems that arise from organisational structure, supply-chain boundaries, and stakeholder misalignment rather than from technology alone.

| Group | Problem IDs | Count |
|---|---|---|
| Requirements/supplier handoff | W1-002, W1-006 | 2 |
| Architecture consistency across suppliers | W1-008 | 1 |
| Supplier/OEM toolchain & data exchange | W4-023 | 1 |
| Communication gaps | W5-014 | 1 |
| Supply-chain opacity/shortage | G1-004, G1-005, G1-006 | 3 |
| Semiconductor talent | G1-034 | 1 |
| Vehicle-domain variants | G2-029, G2-030 | 2 |
| Fleet/platform business model | G3-010, G3-027 | 2 |
| Data privacy/consent economics | G3-008, G3-022 | 2 |
| Cloud/security attack surface | G3-009 | 1 |
| Cybersecurity supply chain | W4-014, W4-015 | 2 |
| Compliance overhead (organisational) | W1-014, W4-013 | 2 |

---

## 20.1 Requirements and supplier handoff (W1-002, W1-006)

- **W1-002 — Unconnected abstraction levels across OEM/Tier-1/Tier-2:** requirements are refined manually across organisational boundaries; granularity mismatches.
- **W1-006 — Incomplete requirements handed to suppliers (assumption-driven development):** OEM capacity and time pressure; unclear system-requirements ownership.

**Root cause:** system-level requirements ownership is unclear between OEM and suppliers; OEMs deliver high-level specs and suppliers fill gaps with assumptions; integration then becomes discovery (Chapter 16).

---

## 20.2 Architecture consistency across suppliers (W1-008)

- **W1-008 — Inconsistent architecture across suppliers/OEM:** each supplier implements its own interpretation; no shared architecture governance.

**Root cause:** architecture is defined at OEM level but implemented at supplier level; the interface between the two is a document, not a governed model.

---

## 20.3 Supplier/OEM toolchain and data exchange (W4-023)

- **W4-023 — Supplier/OEM toolchain mismatch and data exchange friction.**

**Root cause:** toolchains are chosen per organisation for local optimisation; data exchange is manual and error-prone; ARXML fragmentation (W4-002) compounds the problem.

---

## 20.4 Communication gaps (W5-014)

- **W5-014 — Communication gaps across disciplines and organisations.**

**Root cause:** software, hardware, and system teams use different vocabularies and tools; OEM/supplier communication is contract-driven, not engineering-driven.

---

## 20.5 Supply-chain opacity and semiconductor shortage (G1-004, G1-005, G1-006)

- **G1-004 — Multi-tier supply-chain opacity beyond Tier-1.**
- **G1-005 — Chip shortages caused massive production loss and feature deletion.**
- **G1-006 — Geopolitical concentration of semiconductor manufacturing.**

**Root cause:** OEMs lack visibility beyond Tier-1; semiconductor supply is concentrated geographically; shortages cascade into feature deletion and production loss.

---

## 20.6 Semiconductor talent and organisational capability (G1-034)

- **G1-034 — Semiconductor talent shortage (design, process, test).**

**Root cause:** semiconductor expertise is concentrated in few regions and companies; automotive is a secondary customer.

---

## 20.7 Vehicle-domain variants and cost constraints (G2-029, G2-030)

- **G2-029 — Commercial vehicles and off-highway: harsh environment and diverse variants.**
- **G2-030 — Two-/three-wheeler electronics: cost constraints and feature scaling.**

**Root cause:** commercial/off-highway and two-/three-wheeler markets have extreme variant diversity and cost constraints that the passenger-car toolchain does not serve.

---

## 20.8 Fleet, platform, and business-model problems (G3-010, G3-027)

- **G3-010 — Fleet-management platform fragmentation across mixed-OEM fleets.**
- **G3-027 — OEM business-model shift to platform operator.**

**Root cause:** OEMs are becoming platform operators but lack the software and data capabilities; fleet platforms are fragmented across OEMs.

---

## 20.9 Data privacy, consent, and economics (G3-008, G3-022)

- **G3-008 — Data privacy, consent and regulatory compliance.**
- **G3-022 — Data monetisation and customer consent economics.**

**Root cause:** vehicle data monetisation conflicts with privacy regulation and customer consent; the economics are unresolved.

---

## 20.10 Cloud security and attack surface (G3-009)

- **G3-009 — Cloud/backend cybersecurity attack surface.**

**Root cause:** cloud backends add a new attack surface beyond the vehicle; automotive security tooling was designed for in-vehicle networks.

---

## 20.11 Cybersecurity supply-chain burden (W4-014, W4-015)

- **W4-014 — ISO/SAE 21434 cybersecurity engineering integration cost.**
- **W4-015 — UNECE R155 type-approval and CSMS supply-chain burden.**

**Root cause:** cybersecurity management systems must span the supply chain, which most OEMs cannot fully observe or control.

---

## 20.12 Compliance overhead at organisational level (W1-014, W4-013)

- **W1-014 — Compliance overhead and standards fragmentation.**
- **W4-013 — Automotive SPICE assessment overhead and tailoring burden.**

**Root cause:** compliance activities are organised per-project and per-supplier; the boundary between genuine safety work and avoidable overhead is organisation-specific and poorly measured.

---

## 20.13 Cross-cutting organisational root causes

1. **System-level requirements ownership is unclear** (W1-006).
2. **Supply-chain visibility stops at Tier-1** (G1-004).
3. **Toolchains are chosen per organisation** (W4-023, W4-001).
4. **Compliance spans organisations that cannot observe each other** (W4-015).
5. **Business models are shifting faster than capabilities** (G3-027).

---

## 20.14 Evidence limitations

- Supply-chain and shortage impact figures are industry estimates.
- Business-model claims (G3-027) are strategic observations, not measured data.

---

## 20.15 Cross-references

- **Chapter 16 (Integration):** supplier integration problems.
- **Chapter 18 (Toolchain):** toolchain mismatch (W4-023).
- **Chapter 19 (Human Skills):** communication gaps (W5-014).
- **Chapter 22 (Quality/Safety/Cyber):** cybersecurity supply chain (W4-014, W4-015).

---

*End of Chapter 20.*

---
See also: [CROSS_REFERENCE_INDEX.md](../CROSS_REFERENCE_INDEX.md) | [KNOWLEDGE_INDEX.md](../KNOWLEDGE_INDEX.md)

