# GitHub Copilot Setup

This repository is a GitHub Copilot workspace setup for a structured multi-agent development workflow with one standard orchestration path.

The repository is intended to serve as a reusable starting point for projects that want:

- A single orchestration entry point
- Role-based agents for architecture, design, development, and QA
- Docs-first handoffs and predictable delivery artifacts
- Shared skills, review standards, and evidence expectations

## Core Workflow Model

The setup is organized around a Copilot agent system with a clear separation of responsibilities and a tighter operating contract:

- `project-manager` is the single entry point for work
- All work follows the same standard flow; task risk is documented but does not create alternate workflow branches
- Canonical agent ids are kebab-case and used in docs and mentions, for example `@project-manager`, `@developer`, and `@tester`
- `tasks.md` is the source of truth and includes the plan summary for the feature
- Full-stack tasks use a `contract.md` owned by `developer`
- Task evidence is stored under `.github/docs/{feature}/{task}/artifacts/`
- QA consolidates findings with a shared severity model: `Critical`, `High`, `Medium`, `Low`

## Architecture Baseline

This setup assumes a deliberately simpler architecture baseline:

- Frontend work uses `Feature-Sliced Design (FSD)`
- Backend work uses `Clean Architecture`
- The `DDD` and `Hexagonal Architecture` skills are optional reference skills, not default workflow requirements

The primary conventions live in:

- `AGENTS.md`
- `.github/copilot-instructions.md`
- `.github/agents/`
- `.github/skills/`
- `.github/instructions/`

## Repository Structure

```
AGENTS.md
README.md
.github/
├── copilot-instructions.md
├── agents/
├── docs/
│   └── {feature}/
│       ├── architecture.md
│       ├── tasks.md
│       └── {task}/
│           ├── ui-spec.md
│           ├── contract.md
│           ├── dev-summary.md
│           ├── qa-report.md
│           └── artifacts/
├── instructions/
└── skills/
```

## Standard Workflow

The intended workflow starts with `@project-manager`.

From there, the system is designed to:

1. Clarify scope and constraints.
2. Produce `architecture.md` when architecture guidance is needed.
3. Write `.github/docs/{feature}/tasks.md` with a top-level `Plan Summary` and ordered tasks.
4. For each task, optionally produce `ui-spec.md` for user-facing work.
5. Have `developer` implement the task, own `contract.md` for full-stack tasks, and provide code, tests, and `dev-summary.md`.
6. Have `tester` review developer evidence, apply the required QA lenses, and issue the final `PASS` or `FAIL`.
7. Record outputs and evidence under `.github/docs/`.

If QA fails twice on the same task, the flow escalates back to `architect` before a third implementation attempt.

## Key Files

### `AGENTS.md`

Documents the agent roster, canonical agent ids, delegation model, workflow, evidence expectations, severity model, and documentation structure.

### `.github/copilot-instructions.md`

Defines the global project context, including stack assumptions, architectural rules, and coding conventions.

### `.github/agents/`

Contains the role definitions for each agent, including responsibilities, handoff expectations, evidence handling, and output formats.

### `.github/skills/`

Contains the reusable standards each agent is expected to load and follow for specific tasks such as:

- Feature-Sliced Design for frontend
- Clean Architecture for backend
- Frontend and backend patterns
- Testing
- Code review
- Security and performance review

### `.github/instructions/`

Contains targeted instructions that apply to specific file patterns or workflow checkpoints.

One important workflow rule already included in this repo: markdown written under `.github/docs/` is treated as part of the orchestration trail, and task-level artifacts are stored alongside those docs.

## How To Use This Setup

1. Open this folder in an IDE with GitHub Copilot enabled.
2. Keep the `AGENTS.md` file and `.github/` folder at the workspace root so Copilot can discover the instructions, agents, and skills.
3. Start work through `@project-manager` rather than asking implementation agents directly.
4. Use the canonical kebab-case agent ids when referring to agents in prompts or documentation.
5. Review generated docs and artifacts under `.github/docs/` as the workflow progresses.

## Customizing The System

You can adapt the setup by editing:

- Edit `AGENTS.md` to change the agent roster, workflow policy, evidence rules, or documentation conventions
- Edit `.github/copilot-instructions.md` for project-specific context and rules
- Edit `.github/agents/*.agent.md` for agent behavior, responsibilities, and output formats
- Edit `.github/skills/*/SKILL.md` for reusable standards and workflow templates
- Edit `.github/instructions/*.instructions.md` for path-targeted rules and gates
