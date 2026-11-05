# E2E capability specification (solution design)

Maps research problem categories to platform capabilities.

## Capability domains

- Problem discovery & traceability: research IDs -> backlog
- Requirements engineering: templates + traceability graph
- System design: YAML templates -> ARXML -> code
- AUTOSAR config: xsdgen + yaml2arxml
- Testing: test harness + HIL orchestration
- Cloud/fleet: OTA + telemetry pipelines
- AI agents: research agents + verification agents
- Safety/security: safety.yaml + security.yaml + compliance checks

## Capability-to-problem mapping

Each capability references problem IDs from research/ (W*, G*). See CROSS_REFERENCE_INDEX.md.