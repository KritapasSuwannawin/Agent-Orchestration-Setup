---
name: agent-documentaion-logging
description: "Use when: You are about to write or modify .md files in the .github/docs/ folder. Agents write documentation files and continue automatically without waiting for user approval."
applyTo: "**/.github/docs/**/*.md"
---

# Agent Documentation Logging

## Documentation Rule

Whenever you write or modify **any** `.md` file in the `.github/docs/` folder, you **MUST**:

1. **List all files created/modified** in this session
2. **Report the paths** relative to the workspace root (e.g., `.github/docs/feature-name/tasks.md`)
3. **Continue automatically** to the next step without waiting for user approval

## Exception: Non-Doc Outputs

This rule applies **only** to `.md` files in `.github/docs/`. Other outputs (code, config files, etc.) follow normal workflow.

---

**Purpose:** This logging step ensures all agent-produced documentation is trackable and visible as work progresses.
