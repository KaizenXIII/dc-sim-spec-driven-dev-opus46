<!--
  Sync Impact Report
  ===================
  Version change: N/A → 1.0.0 (initial creation)

  Added principles:
    - I. Simulation-First
    - II. Provider Abstraction
    - III. Test-Driven Development (NON-NEGOTIABLE)
    - IV. API-First Architecture
    - V. Incremental Complexity
    - VI. Observability & Reproducibility

  Added sections:
    - Technology Stack & Constraints
    - Development Workflow
    - Governance

  Templates requiring updates:
    ✅ .specify/templates/plan-template.md — no updates needed (Constitution Check section is generic)
    ✅ .specify/templates/spec-template.md — no updates needed (template is technology-agnostic)
    ✅ .specify/templates/tasks-template.md — no updates needed (TDD flow aligns with Principle III)

  Follow-up TODOs: None
-->

# DC Operations Simulator Constitution

## Core Principles

### I. Simulation-First

Docker containers simulate datacenter infrastructure. All features MUST work against simulated hosts before targeting real platforms (VMware, OpenStack, UCS, bare metal).

- The simulation layer is the development and testing backbone — if it cannot be demonstrated in simulation, it is not ready for production.
- The system MUST be fully functional without any real infrastructure connected.
- Simulated environments MUST be reproducible from declarative configuration files (YAML/JSON).

### II. Provider Abstraction

All host interactions MUST go through a provider interface. No direct Docker, VMware, SSH, or platform-specific calls from business logic.

- Providers are swappable without changing the operations layer above them.
- Every provider MUST implement the same interface and return identical data structures.
- Adding a new provider MUST NOT require changes to existing providers or the operations layer.
- The provider interface is the single integration boundary between platform-specific code and business logic.

### III. Test-Driven Development (NON-NEGOTIABLE)

TDD is mandatory for all feature work. Red-Green-Refactor cycle strictly enforced.

- Tests MUST be written first, verified to fail, then implementation proceeds.
- Simulation behavior MUST be verified through deterministic, reproducible test scenarios.
- All provider implementations require both unit tests and integration tests.
- Contract tests MUST verify that all providers return data conforming to the shared interface.

### IV. API-First Architecture

All operations MUST be exposed via a well-defined REST API with structured JSON responses before any frontend or CLI consumes them.

- The API is the contract between the backend operations engine and any client (web dashboard, CLI, scripts, external integrations).
- API endpoints MUST be documented and versioned.
- No operation may exist only in the UI — every UI action corresponds to an API call.

### V. Incremental Complexity

Start with read-only inventory. Add configuration management, operations workflows, and incident simulation incrementally. YAGNI applies.

- Each increment MUST be a working, demonstrable system.
- Do not build capacity planning, failure injection, or advanced scheduling until the base inventory and config management layers are proven.
- Features MUST be delivered as independently testable user stories.
- Resist premature abstraction — extract patterns only when they emerge across multiple concrete implementations.

### VI. Observability & Reproducibility

Structured logging for all operations. Simulation state MUST be reproducible given the same inputs and configuration.

- All provider calls MUST be logged with structured data (host ID, operation, result, duration).
- Errors MUST produce actionable log entries with enough context to diagnose without reproduction.
- Simulation runs with identical configuration and seed values MUST produce identical results.

## Technology Stack & Constraints

- **Backend**: Python 3.11+ with FastAPI for the API layer
- **Frontend**: React + TypeScript (introduced later, not in initial features)
- **Simulation**: Docker containers with SSH as the host management protocol
- **Repository Structure**: Monorepo with `backend/` and `frontend/` directories at the repository root
- **Testing**: pytest for backend, contract tests for provider interface compliance
- **Infrastructure Providers** (planned): Docker (simulation), SSH (generic), VMware vSphere, OpenStack Nova, UCS/IPMI (bare metal)
- **Data Formats**: YAML for configuration, JSON for API responses

## Development Workflow

- **Spec-kit driven**: Every feature starts with `/speckit.specify` → `/speckit.plan` → `/speckit.tasks` → implementation
- **Feature branches**: Follow spec-kit numbering convention (`###-feature-name`)
- **Provider changes**: All provider implementations require both unit tests and integration tests demonstrating correct data structure compliance
- **API changes**: New endpoints require contract tests and updated API documentation
- **Constitution compliance**: Every plan MUST include a Constitution Check gate validating alignment with these principles

## Governance

This constitution supersedes all other development practices for the DC Operations Simulator project.

- **Amendments** require documentation in the Sync Impact Report, version bump following semantic versioning, and propagation to dependent templates.
- **Compliance review**: All implementation plans MUST pass a Constitution Check gate before proceeding. Violations MUST be documented in the Complexity Tracking section with justification.
- **Principle III (TDD) is NON-NEGOTIABLE** and cannot be waived or deferred. All other principles may be temporarily relaxed with documented justification and a remediation plan.

**Version**: 1.0.0 | **Ratified**: 2026-02-22 | **Last Amended**: 2026-02-22
