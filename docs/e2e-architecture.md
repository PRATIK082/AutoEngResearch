# E2E architecture (solution design)

## Layers

1. Authoring layer: YAML/JSON/TOML templates
2. Query layer: SQLite/DuckDB + graph DB
3. AI layer: embeddings + RAG
4. Generation layer: YAML -> ARXML -> Rust/C
5. Validation layer: schema + semantic checks
6. Orchestration layer: multi-agent research + verification

## Components

- Template engine
- ARXML generator
- Code generator
- Test harness
- Cloud/fleet connectors
- AI agent orchestrator

## Non-goals

No implementation until research and verification complete.