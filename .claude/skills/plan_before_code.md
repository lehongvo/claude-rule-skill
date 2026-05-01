---
name: plan-before-code
description: >
  Requires a detailed plan before any code change. Activate this skill when the
  user requests: adding a feature, fixing a bug, refactoring, migrations, logic
  changes, code updates, implementation, or any task that involves writing or
  modifying code. Do NOT begin writing code until the plan is approved.
  Trigger phrases: "add", "fix", "implement", "refactor", "update", "migrate",
  "add feature", "change", "modify", "convert", "create", "build".
---

# Plan Before Code Skill

Every code change MUST have a detailed, user-approved plan before implementation begins.

---

## Core Rules

1. **No code is written before the plan is approved.**
2. The plan must be detailed enough for the user to understand WHAT, WHERE, WHY, and HOW.
3. If the plan is rejected or needs adjustments → revise the plan → request approval again.
4. Only after the user says "ok", "approve", "agreed", "go", or "proceed" → start coding.

---

## Step 1 — Analyze the request

Before drafting the plan, gather context:

1. **Read related code** — Use Read/Grep/Glob to understand the current implementation.
2. **Define scope** — Which files/modules will the task affect?
3. **Identify existing patterns** — What conventions does the surrounding code follow?
4. **Surface risks** — Any breaking changes, dependencies, or side effects?

---

## Step 2 — Build the plan

Present the plan in the following format:

```
## Plan: [Short task name]

### Goal
[1–2 sentences describing what the task does and why]

### Current state
- **Existing code**: [brief description of how it works today]
- **Problem / gap**: [why a change is needed]

### Detailed changes

#### 1. `path/to/file.go` — [change summary]
- **Add / Modify / Remove**: [specific description]
- **Reason**: [why this change is needed]
- **Logic**: [explain new logic if non-trivial]

#### 2. `path/to/another_file.go` — [change summary]
- ...

### Execution order
1. [Step 1 — what to do first]
2. [Step 2 — what comes next]
3. ...

### Risks & considerations
- [Breaking changes, if any]
- [Side effects to verify]
- [Tests to run after completion]

### Out of scope
- [List what will NOT be touched — reassures the user about scope]
```

---

## Step 3 — Wait for approval

After presenting the plan, ask:

> **Does this plan look good? Any adjustments needed?**

Wait for the user's response:
- **Approve** ("ok", "approved", "go", "proceed", "agreed", "lgtm") → Step 4
- **Reject / revise** → Return to Step 2 and update the plan based on feedback
- **Follow-up question** → Answer it, then update the plan if needed

---

## Step 4 — Implement per plan

Once approved:

1. **Follow the plan in order** — Execute each step in the "Execution order".
2. **Do not expand scope** — Only do what the plan specifies; no unrequested additions.
3. **Report progress** — After each major step, give a brief update.
4. **If a new issue surfaces** — Stop, notify the user, and propose a plan adjustment.

---

## Exceptions — When a formal plan is not required

For tasks that are too trivial to warrant a full plan:
- Typo fixes / variable renames
- Adding 1–2 lines of log/comment
- Running commands (build, test, deploy)
- Reading or searching code (research only, no modifications)

For these, confirm intent quickly with the user instead of writing a full plan.

---

## Important Rules

- **The plan is a contract** — Once approved, it defines the agreed scope. Do not deviate.
- **Concise but complete** — A small task (1–2 files) only needs 5–10 lines. Reserve detail for larger tasks.
- **File paths must be accurate** — Verify each file exists before listing it in the plan.
- **Match the user's language** — If the user writes in Vietnamese, plan in Vietnamese; if English, plan in English.
- **Do not over-engineer the plan** — The goal is alignment, not documentation.
