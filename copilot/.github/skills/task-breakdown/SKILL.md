name: task-breakdown
description: Defines how any project goal is broken down into a structured tasks.md file. This file is the single source of truth for all work in the project.

---

# Skill: Task Breakdown

## Purpose

This skill defines how any project goal is broken down into a structured `tasks.md` file. This file is the single source of truth for all work in the project, including the top-level plan summary.

## When to Use

Invoke this skill before any development begins. No agent should be delegated work until `tasks.md` exists and is complete.

## Process

### Step 1 — Understand the Goal

Restate the user's goal in one sentence. If it's ambiguous, ask clarifying questions before proceeding.

### Step 2 — Write The Plan Summary

Capture the overall approach in `tasks.md` before task-level breakdown. Keep it short and concrete:

- What is being delivered
- The expected sequencing
- The key dependencies or risks to watch

### Step 3 — Identify Epics

Group related work into epics (high-level themes). Typical epics: Auth, Core Feature, UI, API, Infrastructure, Testing.

### Step 4 — Break Into Tasks

Each epic contains tasks. A task must be:

- Independently implementable by one developer in one session
- Testable — has a clear definition of done
- Specific enough that an agent knows exactly what to build

**Full-stack tasks:** If a task requires both frontend and backend work, it may remain a single full-stack task when one `developer` can complete and validate it within one implementation and QA cycle. Split it into linked tasks only when the scope is too large or when dependencies require sequencing. Add a `contract.md` reference for the relevant task folder and note that `developer` owns the contract gate before implementation starts.

### Step 5 — Add Subtasks

Each task can have subtasks for fine-grained tracking.

### Step 6 — Declare MCP Inputs, Evidence, And Artifact Location

Each task must explicitly record the MCP inputs and evidence it requires. Do not leave this implicit.

- Browser-backed UI validation -> declare the required `Playwright MCP` evidence
- Runtime performance or browser security inspection -> declare the required `Chrome DevTools MCP` evidence
- External framework or library uncertainty -> declare the required `Context7` topic or package
- Figma design review -> declare `Figma MCP` only if the user actually supplied a Figma link or explicitly asked to work from Figma

If an MCP is not relevant, mark it `N/A`. If Figma was not provided, say `Not provided by user`.

Also record:

- The task-scoped artifact folder under `.github/docs/{feature}/{task}/artifacts/`
- The current evidence collection status (`Pending`, collected artifact paths, or unavailable with reason)

### Step 7 — Assign Agents

For each task, note which agents will be involved.

### Step 8 — Order by Dependency

Sequence tasks so dependencies are respected. Mark blockers explicitly.

---

## Rules

- Tasks must not be so large that they can't be reviewed in a single QA cycle.
- Each task must have a definition of done (reference `.github/skills/definition-of-done/SKILL.md`).
- Each task must declare MCP inputs and evidence, even when the answer is `N/A`.
- Each task must declare the current evidence collection status and the artifact location.
- Figma MCP must only be required when the user provided a Figma link or explicitly asked to use Figma.
- Record task risk for visibility, but do not use it to bypass the standard orchestration flow.
- Full-stack tasks must reference `contract.md` and note that `developer` owns it.
- Never start development on a task before its upstream dependencies are complete.
- Update `tasks.md` status as work progresses — it is a living document.

---

## Usage

See `TEMPLATE.md` in this folder for the exact format to use when writing `tasks.md`.
