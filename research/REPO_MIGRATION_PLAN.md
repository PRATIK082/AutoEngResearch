# Repository migration plan — AutoEngResearch

## 1. Repository audit

- Current repo: PRATIK082/selfdata (main)
- Remote: https://github.com/PRATIK082/selfdata.git
- Top-level folders: crates, docs, drivers, platforms, research, schemas, stacks, templates, tools
- File count: ~144
- Primary content currently mixed: Rust AUTOSAR tooling + research corpus.
- Research corpus is the dominant content by volume (research/book 45 chapters, research/artefacts 14, workstreams W1-W5, G1-G4, stakeholders, AI governance, cross-references).

## 2. Proposed folder structure

```
AutoEngResearch/
├── README.md (research landing page)
├── COPYRIGHT.md
├── LICENSE (if chosen)
├── research/
│   ├── book/
│   ├── artefacts/
│   ├── W*.md, G*.md
│   ├── stakeholders.md
│   ├── ai_governance.md
│   └── ...
├── engineering/ (optional move of crates, tools, schemas, templates, platforms, stacks, drivers)
├── docs/
└── .github/
```

## 3. Copyright and licensing assessment

- Copyright holder: Pratik Patil (pratik.patil082@gmail.com)
- All research original; permission required for reuse.
- COPYRIGHT.md added.
- No open-source license selected yet; recommend adding a clear license or "All rights reserved" statement.

## 4. Research execution plan

- INSPECT: audit complete.
- RESEARCH: workstreams W1-W5, G1-G4, stakeholders, AI governance complete.
- VALIDATE: data verification done; AI governance rechecked.
- DOCUMENT: README, copyright, migration plan.
- REVIEW: cross-linking done; final review pending.
- PUBLISH: commit/push after approval.

## 5. AI governance subagent plan

- Expand research/ai_governance.md (done).
- Verify sources and citations.
- Cross-link to W5, W3, W4, G3.
- Record unresolved questions.

## 6. Repository rename impact assessment

- Proposed name: AutoEngResearch
- Target: PRATIK082/AutoEngResearch
- Need confirm no existing repo with that name (gh auth required).
- Impacts: remote URL, documentation, badges, GitHub Actions/Pages.
- Rollback: rename back via GitHub settings.

## Approval needed

Proceed with:
1. Rename repository to AutoEngResearch?
2. Realign folders (move engineering code into engineering/)? 
3. Add license file?