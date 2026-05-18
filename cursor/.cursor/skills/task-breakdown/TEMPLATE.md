# tasks.md — {Project Name}

**Goal:** {One sentence description of what we are building}
**Created:** {Date}
**Last Updated:** {Date}

## Plan Summary

- **Outcome:** {What the work delivers}
- **Sequencing:** {High-level implementation order}
- **Key dependencies / risks:** {Important context to monitor without changing the workflow path}

---

## Legend

- `[ ]` Not started
- `[~]` In progress
- `[x]` Complete
- `[!]` Blocked

---

## Epic 1: {Epic Name}

### Task 1.1 — {Task Title}

**Status:** `[ ]`
**Agents:** {e.g. designer-ui-ux, developer, tester}
**Depends on:** {Task ID or "none"}
**Risk:** {e.g. requires schema migration | touches auth flow | none} (documentation only; does not change the workflow path)
**Contract:** {`.cursor/docs/{feature}/{task}/contract.md` or `N/A`}

**MCP Inputs & Required Evidence:**

- `Playwright MCP:` {screenshots, traces, video, or `N/A`}
- `Chrome DevTools MCP:` {trace, network, headers, memory, or `N/A`}
- `Context7:` {library/topic to verify, or `N/A`}
- `Figma MCP:` {Figma link / frame reference, or `Not provided by user`}

**Evidence Collected:**

- `Artifacts folder:` `.cursor/docs/{feature}/{task}/artifacts/`
- `Current status:` {`Pending` | collected artifact paths | unavailable with reason}

**Description:**
{What needs to be done. Be specific enough that an agent can act on it.}

**Subtasks:**

- [ ] {Subtask A}
- [ ] {Subtask B}
- [ ] {Subtask C}

**Definition of Done:**

- [ ] {Specific criterion 1}
- [ ] {Specific criterion 2}
- [ ] Required MCP evidence stored under `artifacts/` and linked, or unavailability documented
- [ ] `contract.md` implemented and verified (if applicable)
- [ ] Unit tests written and passing
- [ ] Tester: PASS

**Upstream Outputs Needed:**

- The `architecture.md` file — section: {relevant section}
- The `ui-spec.md` file — screen: {relevant screen} (if UI task)
- The `contract.md` file — section: {relevant section} (if full-stack task)

---

### Task 1.2 — {Task Title}

**Status:** `[ ]`
**Agents:** {agents}
**Depends on:** Task 1.1
**Risk:** {risk} (documentation only; does not change the workflow path)
**Contract:** {`.cursor/docs/{feature}/{task}/contract.md` or `N/A`}

**MCP Inputs & Required Evidence:**

- `Playwright MCP:` {screenshots, traces, video, or `N/A`}
- `Chrome DevTools MCP:` {trace, network, headers, memory, or `N/A`}
- `Context7:` {library/topic to verify, or `N/A`}
- `Figma MCP:` {Figma link / frame reference, or `Not provided by user`}

**Evidence Collected:**

- `Artifacts folder:` `.cursor/docs/{feature}/{task}/artifacts/`
- `Current status:` {`Pending` | collected artifact paths | unavailable with reason}

**Description:**
{Description}

**Subtasks:**

- [ ] {Subtask A}

**Definition of Done:**

- [ ] {Criterion}
- [ ] Required MCP evidence stored under `artifacts/` and linked, or unavailability documented
- [ ] `contract.md` implemented and verified (if applicable)
- [ ] Unit tests written and passing
- [ ] Tester: PASS

---

## Epic 2: {Epic Name}

### Task 2.1 — {Task Title}

**Status:** `[ ]`
**Agents:** {agents}
**Depends on:** {Task ID or "none"}
**Risk:** {risk} (documentation only; does not change the workflow path)
**Contract:** {`.cursor/docs/{feature}/{task}/contract.md` or `N/A`}

**MCP Inputs & Required Evidence:**

- `Playwright MCP:` {screenshots, traces, video, or `N/A`}
- `Chrome DevTools MCP:` {trace, network, headers, memory, or `N/A`}
- `Context7:` {library/topic to verify, or `N/A`}
- `Figma MCP:` {Figma link / frame reference, or `Not provided by user`}

**Evidence Collected:**

- `Artifacts folder:` `.cursor/docs/{feature}/{task}/artifacts/`
- `Current status:` {`Pending` | collected artifact paths | unavailable with reason}

**Description:**
{Description}

**Subtasks:**

- [ ] {Subtask A}

**Definition of Done:**

- [ ] {Criterion}
- [ ] Required MCP evidence stored under `artifacts/` and linked, or unavailability documented
- [ ] `contract.md` implemented and verified (if applicable)
- [ ] Unit tests written and passing
- [ ] Tester: PASS

---

## Progress Summary

| Task          | Status | QA Verdict |
| ------------- | ------ | ---------- |
| 1.1 — {Title} | `[ ]`  | —          |
| 1.2 — {Title} | `[ ]`  | —          |
| 2.1 — {Title} | `[ ]`  | —          |

---

## Blocked Items

{List any blocked tasks and what is needed to unblock them}

## Tech Debt Log

{Items noted during development that should be addressed later}
