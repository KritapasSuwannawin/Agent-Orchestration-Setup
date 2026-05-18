# AGENTS.md — Agent System Reference

> For global coding conventions and stack info, see `.github/copilot-instructions.md`.
> This file is the reference manual for the agent orchestration system.

## Agents

Agents are available at `.github/agents/`. Use kebab-case ids as the canonical identifiers in docs, handoffs, and `@...` mentions. Display names remain human-readable labels only.

| Canonical id      | Role                                                                                           |
| ----------------- | ---------------------------------------------------------------------------------------------- |
| `project-manager` | Entry point. Coordinates all agents. Owns the task breakdown.                                  |
| `architect`       | Decides which architecture principles apply and produces architecture docs.                    |
| `designer-ui-ux`  | Produces UI/UX designs, wireframes, and component specs.                                       |
| `developer`       | Implements frontend and backend work, owns full-stack contracts, and performs dev self-review. |
| `tester`          | Reviews developer evidence, performs QA across all test lenses, and issues the final verdict.  |

## MCP Usage Matrix

| MCP                   | Use when                                                                                                                             | Required evidence                                                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| `Playwright MCP`      | A task needs browser-backed validation of user flows, UI states, or reproducible issue replay                                        | Screenshots for key states, traces on failure, and video for hard-to-reproduce or long-running flows when requested in `tasks.md` |
| `Chrome DevTools MCP` | A running application needs runtime inspection for rendering, networking, console, performance, or browser-visible security behavior | Performance trace, network or header evidence, console snapshot, and memory or Lighthouse/CWV snapshot when relevant              |
| `Context7`            | A task depends on external framework or library APIs, new dependencies, or ambiguous vendor guidance                                 | Library, version or docs ID, and topic consulted in `architecture.md`, `dev-summary.md`, or review notes                          |
| `Figma MCP`           | The user provides a Figma link or explicitly asks to work from Figma                                                                 | Cited frame, component, or token references in `ui-spec.md`, plus comparison evidence when design parity is reviewed              |

## MCP Evidence Policy

- The `project-manager` records required MCP inputs and evidence per task in `tasks.md`.
- Downstream agents must either attach the required evidence or explicitly document why the input was unavailable.
- The `tester` must publish an `Evidence Index` in every QA report so reviewers can trace findings back to artifacts or source references.
- Missing inputs are not a reason to guess. If the app is not running, the Figma link was not provided, or the required environment is unavailable, the agent must say so clearly.

## Artifact Storage Conventions

- Store task-scoped evidence under `.github/docs/{feature}/{task}/artifacts/`.
- Reference artifacts from `tasks.md`, `dev-summary.md`, and `qa-report.md` using workspace-relative paths.
- Use descriptive kebab-case filenames such as `playwright-login-error.png`, `chrome-trace-checkout.json`, or `security-headers.txt`.
- If required evidence is unavailable, record the reason in the owning markdown document instead of leaving the artifact implicit.

## Orchestration Workflow

### Standard Workflow

```
User
 └─▶ project-manager
   ├─▶ architect                            → .github/docs/{feature}/architecture.md
   ├─▶ [plan summary + task breakdown]      → .github/docs/{feature}/tasks.md
   └─▶ per task:
     ├─▶ designer-ui-ux (skip if task has no user-facing UI)    → .github/docs/{feature}/{task}/ui-spec.md
     ├─▶ developer
     │     ├─▶ [contract gate for full-stack tasks]             → .github/docs/{feature}/{task}/contract.md
     │     ├─▶ implementation + unit/integration/E2E tests
     │     └─▶ [code review + test review by developer]         → .github/docs/{feature}/{task}/dev-summary.md
     └─▶ tester
       └─▶ [functional + security + performance + usability review, QA verdict] → .github/docs/{feature}/{task}/qa-report.md
         └─▶ PASS → next task
         └─▶ FAIL (cycle 1-2)   → back to developer
         └─▶ FAIL (cycle 3+)    → escalate to architect
```

## Docs Folder Convention

All agent-produced documents are stored in `.github/docs/` following this structure:

```
.github/docs/
└── {feature}/                      ← one-line summary of the user's input to project-manager
    ├── architecture.md             ← produced by architect (once per feature)
    ├── tasks.md                    ← produced by project-manager (once per feature)
    └── {task}/                     ← short slug of the task title from tasks.md
        ├── ui-spec.md              ← produced by designer-ui-ux
        ├── contract.md             ← optional; required for full-stack tasks
        ├── dev-summary.md          ← produced by developer
        ├── qa-report.md            ← produced by tester
        └── artifacts/              ← evidence files referenced from task, dev, and QA docs
```

### Naming Rules

- The `{feature}` name — Kebab-case slug of the user's original request to `project-manager`
  - Example: `"Add user authentication"` → `user-authentication`
- The `{task}` name — Kebab-case slug of the task title from `tasks.md`
  - Example: `"Task 1.2 — Login form UI"` → `task-1-2-login-form-ui`

### Usage Rules

- The `project-manager` creates the `{feature}/` folder and writes `tasks.md` before any delegation. The `tasks.md` file includes a top-level `Plan Summary` section and remains the source of truth for sequencing.
- Each agent writes its output to the correct path before handing off.
- Downstream agents always read upstream docs from this folder — not from memory or inline summaries.
- The `architecture.md` file is written once per feature and referenced by all tasks within it.
- If a task is full-stack, `developer` owns `contract.md` and keeps it current before implementation proceeds.
- All task-scoped evidence files live under `artifacts/` and must be linked from the relevant markdown documents.
- The `architecture.md`, `dev-summary.md`, and `qa-report.md` files are updated in-place if work iterates, and each keeps a `Change History` section current.

## Context Window Guidance

Large documents (`tasks.md`, `architecture.md`, `contract.md`) can exceed an agent's context window. Follow these rules:

- Agents must reference specific **sections** of upstream docs, not include entire files inline.
- For `architecture.md`, reference only the relevant layer section (e.g. "Backend Architecture" section).
- For `tasks.md` or `contract.md`, reference only the specific task or contract section being worked on.
- If a document is too large to pass in full, summarise the relevant parts in the delegation message.
- Paths are **always relative to the workspace root**. The `.github/` folder path must never be changed without updating every agent file and every skill file that references it.

## Context Handoff Protocol

When delegating, agents use canonical agent ids and pass context by referencing docs paths explicitly. Docs are the source of truth; the optional summary is only a convenience layer.

```
@{agent-id}

**Task ID:** {task id}
**Task:** {task title}
**Feature:** {feature slug}
**Summary:** {optional one-line summary}
**Docs:**
- Tasks: `.github/docs/{feature}/tasks.md` → Task {id}
- Architecture: `.github/docs/{feature}/architecture.md`
- UI Spec: `.github/docs/{feature}/{task}/ui-spec.md` or `N/A`
- Contract: `.github/docs/{feature}/{task}/contract.md` or `N/A`
- Dev Summary: `.github/docs/{feature}/{task}/dev-summary.md` or `N/A`
**MCP Inputs:** {running app URL, Figma link if provided, external libraries to verify, or `none`}
**Required Evidence:** {screenshots, traces, performance profile, docs consulted, or `none`}
**Goal:** {what you need from this agent}
**Open Questions:** {list or `none`}
**Blocking Constraints:** {list or `none`}
```

## Skills Library

All agents reference skills from `.github/skills/` in explicit workflow steps rather than optional top-level lists. Keep this matrix aligned with the agent files.

| Skill                   | Description                                                       |
| ----------------------- | ----------------------------------------------------------------- |
| `task-breakdown`        | Defines how any project goal is broken down.                      |
| `definition-of-done`    | Shared standard that determines when a task is truly complete.    |
| `clean-architecture`    | Clean Architecture guidance for backend.                          |
| `feature-sliced-design` | Feature-Sliced Design (FSD) for frontend architecture.            |
| `api-design`            | REST API design conventions.                                      |
| `database-design`       | Database design principles.                                       |
| `coding-standards`      | Universal coding standards for all TypeScript code.               |
| `frontend-patterns`     | React and TypeScript patterns and conventions.                    |
| `backend-patterns`      | NestJS and TypeScript patterns.                                   |
| `unit-testing`          | Standards and patterns for writing unit tests in TypeScript.      |
| `integration-testing`   | Standards for integration tests.                                  |
| `e2e-testing`           | Standards and patterns for end-to-end testing with Playwright.    |
| `code-review`           | Structured checklist and process for self-reviewing changes.      |
| `security-review`       | Structured security checklist.                                    |
| `performance-review`    | Performance review checklist.                                     |
| `usability-review`      | Usability evaluation framework .                                  |
| `accessibility`         | Accessibility standards and implementation patterns.              |
| `ui-design`             | UI design principles .                                            |
| `design-system`         | Guidelines for building and consuming a consistent design system. |
| `state-management`      | Redux + Redux Toolkit state management patterns.                  |
| `migration`             | Database migration standards                                      |
