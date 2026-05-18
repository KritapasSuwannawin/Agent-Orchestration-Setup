# Agent Orchestration Setup

This repository is a workspace setup for structured multi-agent development workflow. It provides one standard orchestration path, role-based agents, and shared skills—adaptable to different AI coding assistants.

Use it as a starting point when you want:

- A single orchestration entry point
- Role-based agents for architecture, design, development, and QA
- Docs-first handoffs and predictable delivery artifacts
- Shared skills, review standards, and evidence expectations

## Supported Platforms

The same workflow and agent roles are packaged for each platform under its own folder. Copy the contents of the folder you need into your project workspace root.

| Platform       | Folder     | Global instructions               | Agents            | Skills            | Docs & artifacts |
| -------------- | ---------- | --------------------------------- | ----------------- | ----------------- | ---------------- |
| GitHub Copilot | `copilot/` | `.github/copilot-instructions.md` | `.github/agents/` | `.github/skills/` | `.github/docs/`  |
| Cursor         | `cursor/`  | `.cursor/rules/`                  | `.cursor/agents/` | `.cursor/skills/` | `.cursor/docs/`  |

Each platform folder includes its own `AGENTS.md` with platform-specific paths. The orchestration model, agent roster, and documentation conventions are the same across platforms.

## Core Workflow Model

The setup uses a multi-agent system with clear separation of responsibilities:

- `project-manager` is the single entry point for work
- All work follows the same standard flow; task risk is documented but does not create alternate workflow branches
- Canonical agent ids are kebab-case and used in docs and mentions, for example `@project-manager`, `@developer`, and `@tester`
- `tasks.md` is the source of truth and includes the plan summary for the feature
- Full-stack tasks use a `contract.md` owned by `developer`
- Task evidence is stored under `{docs-root}/{feature}/{task}/artifacts/` (see platform table above)
- QA consolidates findings with a shared severity model: `Critical`, `High`, `Medium`, `Low`

## Architecture Baseline

This setup assumes a deliberately simpler architecture baseline:

- Frontend work uses `Feature-Sliced Design (FSD)`
- Backend work uses `Clean Architecture`
- The `DDD` and `Hexagonal Architecture` skills are optional reference skills, not default workflow requirements

Platform-specific convention files (for example `copilot-instructions.md` or `agent-instructions.mdc`) define stack assumptions, architectural rules, and coding standards.

## Repository Structure

```
README.md
copilot/                          # GitHub Copilot template
├── AGENTS.md
└── .github/
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
cursor/                           # Cursor template
├── AGENTS.md
└── .cursor/
    ├── rules/
    ├── agents/
    ├── docs/                     # same {feature}/{task}/ layout as above
    └── skills/
```

## Standard Workflow

The intended workflow starts with `@project-manager`.

From there, the system is designed to:

1. Clarify scope and constraints.
2. Produce `architecture.md` when architecture guidance is needed.
3. Write `{docs-root}/{feature}/tasks.md` with a top-level `Plan Summary` and ordered tasks.
4. For each task, optionally produce `ui-spec.md` for user-facing work.
5. Have `developer` implement the task, own `contract.md` for full-stack tasks, and provide code, tests, and `dev-summary.md`.
6. Have `tester` review developer evidence, apply the required QA lenses, and issue the final `PASS` or `FAIL`.
7. Record outputs and evidence under the platform docs folder.

If QA fails twice on the same task, the flow escalates back to `architect` before a third implementation attempt.

## Key Files

### `AGENTS.md`

Documents the agent roster, canonical agent ids, delegation model, workflow, evidence expectations, severity model, and documentation structure. Lives at the workspace root after you copy a platform template.

### Global instructions

Define project context, stack assumptions, architectural rules, and coding conventions:

- **GitHub Copilot:** `.github/copilot-instructions.md`
- **Cursor:** `.cursor/rules/agent-instructions.mdc` (and related rules)

### Agent definitions

Role definitions for each agent, including responsibilities, handoff expectations, evidence handling, and output formats:

- **GitHub Copilot:** `.github/agents/*.agent.md`
- **Cursor:** `.cursor/agents/*.agent.md`

### Skills

Reusable standards each agent loads for specific tasks, such as:

- Feature-Sliced Design for frontend
- Clean Architecture for backend
- Frontend and backend patterns
- Testing
- Code review
- Security and performance review

Paths: `.github/skills/` (Copilot) or `.cursor/skills/` (Cursor).

### Targeted instructions

Path-specific or checkpoint rules:

- **GitHub Copilot:** `.github/instructions/*.instructions.md`
- **Cursor:** `.cursor/rules/*.mdc` (for example documentation logging)

Markdown written under the platform docs folder is part of the orchestration trail; task-level artifacts live alongside those docs.

## How To Use This Setup

1. Choose your platform folder (`copilot/` or `cursor/`).
2. Copy its contents into your project workspace root so `AGENTS.md` and the platform config folder (`.github/` or `.cursor/`) sit at the root.
3. Open the project in an IDE with your chosen AI assistant enabled.
4. Start work through `@project-manager` rather than asking implementation agents directly.
5. Use the canonical kebab-case agent ids when referring to agents in prompts or documentation.
6. Review generated docs and artifacts under the platform docs folder as the workflow progresses.

## Customizing The System

Adapt the setup by editing files in your workspace root after copying a template:

- `AGENTS.md` — agent roster, workflow policy, evidence rules, documentation conventions
- Global instructions — project-specific context and rules
- `agents/*.agent.md` — agent behavior, responsibilities, and output formats
- `skills/*/SKILL.md` — reusable standards and workflow templates
- Platform-specific instruction files — path-targeted rules and gates

When adding a new platform, mirror the same agent roles, skills, docs layout, and `AGENTS.md` content using that tool's native configuration paths.
