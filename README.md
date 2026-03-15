# DC Operations Simulator — Spec-Driven Development Kit

A spec-driven development (SDD) workflow toolkit built on top of Claude Code (and other AI coding agents), combined with a project constitution for building a **DC Operations Simulator** — a Python/FastAPI backend that models datacenter infrastructure using Docker containers as simulated hosts.

The repository ships two interlocking layers:

1. **Spec-kit** — a set of Claude slash-commands and supporting Bash scripts that enforce a rigorous Specify > Plan > Tasks > Implement > Analyze lifecycle for every feature.
2. **Project constitution** — an opinionated, versioned document that encodes non-negotiable architectural principles.

---

## Features

- **Structured feature workflow** — `/speckit.specify`, `/speckit.clarify`, `/speckit.plan`, `/speckit.tasks`, `/speckit.implement`, `/speckit.analyze`, `/speckit.checklist`, `/speckit.taskstoissues`
- **Constitution-gated planning** — every implementation plan must pass a Constitution Check gate
- **Mandatory TDD** — the constitution declares Test-Driven Development non-negotiable
- **Provider abstraction** — all datacenter host interactions flow through a swappable provider interface
- **API-first** — FastAPI REST backend is the contract between the operations engine and any client
- **Simulation-first** — Docker containers simulate datacenter infrastructure
- **Multi-agent context management** — `update-agent-context.sh` keeps AI agent instruction files in sync

---

## Development Workflow

Every feature follows this pipeline:

```
/speckit.specify  >  /speckit.clarify  >  /speckit.plan  >  /speckit.tasks  >  /speckit.analyze  >  /speckit.implement
```

1. **Specify** — describe the feature; auto-generates a numbered branch and spec template
2. **Clarify** — resolve ambiguities interactively
3. **Plan** — research, tech stack, architecture decisions, Constitution Check gate
4. **Tasks** — dependency-ordered task list with parallelism markers
5. **Analyze** — read-only consistency scan across spec, plan, and tasks
6. **Implement** — execute tasks phase-by-phase with TDD

---

## Project Constitution (v1.0.0)

| # | Principle | Summary |
|---|---|---|
| I | Simulation-First | Fully functional against Docker-simulated hosts before targeting real platforms |
| II | Provider Abstraction | All host interactions through a swappable provider interface |
| III | Test-Driven Development | **NON-NEGOTIABLE.** Tests written first, verified to fail, then implementation |
| IV | API-First Architecture | Every operation exposed via versioned REST API before any UI |
| V | Incremental Complexity | Start read-only, add complexity only when previous layer is proven |
| VI | Observability & Reproducibility | Structured logging; identical config + seed = identical results |

---

## Repository Layout

```
.
├── .claude/commands/               # Claude slash-command definitions
│   ├── speckit.specify.md
│   ├── speckit.clarify.md
│   ├── speckit.plan.md
│   ├── speckit.tasks.md
│   ├── speckit.implement.md
│   ├── speckit.analyze.md
│   ├── speckit.checklist.md
│   └── speckit.taskstoissues.md
├── .specify/
│   ├── memory/constitution.md      # Versioned project constitution
│   ├── scripts/bash/               # Helper scripts
│   └── templates/                  # Spec, plan, tasks, checklist templates
└── specs/                          # Feature specifications
```

---

## Prerequisites

- Git, Bash 4+
- Claude Code (or another supported AI agent)
- Python 3.11+ and Docker (for implementation)
- GitHub CLI (`gh`) for `/speckit.taskstoissues`

---

## Supported AI Agents

`update-agent-context.sh` supports: Claude, Gemini, Copilot, Cursor, Qwen, OpenCode, Codex, Windsurf, Kilocode, Auggie, Roo, CodeBuddy, Amp, Shai, Q, Agy, Bob, QoderCLI.

---

## License

No license file is present in this repository. All rights reserved unless otherwise stated.
