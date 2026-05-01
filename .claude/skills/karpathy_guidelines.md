---
name: karpathy_guidelines
priority: 1
description: FIRST SKILL TO APPLY — always invoke before any other skill or action. Andrej Karpathy's 4 LLM coding principles (think before coding, simplicity first, surgical changes, goal-driven execution) to prevent wrong-assumption bugs, over-engineering, orthogonal edits, and weak success criteria.
triggers:
  - EVERY user request (before any other skill/rule)
  - Any feature implementation
  - Any refactor or edit
  - Any ambiguous user request where multiple interpretations exist
  - Before claiming "task complete"
---

# Karpathy Guidelines Skill — APPLY FIRST

Source: https://github.com/forrestchang/andrej-karpathy-skills

## Priority
**THIS SKILL IS APPLIED FIRST, BEFORE ANY OTHER SKILL OR RULE.**

Order of evaluation on every user request:
1. `karpathy_guidelines` (this file) — check 4 principles
2. `plan_before_code` — formal plan + approval
3. Domain rules (`migration.md`, `testing.md`, etc.)
4. `commit_push_approval` — before any commit/push

## When to invoke
Invoke on **every** user request. Only skip the full checklist for obvious one-liners (typo fixes, trivial renames) — but still mentally pass through principle #1 (assumptions stated).

## The 4 Principles (checklist)

### [ ] 1. Think Before Coding
- [ ] Assumptions stated explicitly
- [ ] If ambiguity exists → presented options, didn't pick silently
- [ ] Asked for clarification if anything unclear
- [ ] Considered if a simpler approach exists; pushed back if warranted

### [ ] 2. Simplicity First
- [ ] No features beyond what was asked
- [ ] No abstractions for single-use code
- [ ] No speculative "flexibility" or "configurability"
- [ ] No error handling for impossible scenarios
- [ ] If code feels long → asked "would a senior engineer call this overcomplicated?"

### [ ] 3. Surgical Changes
- [ ] Every changed line traces directly to user's request
- [ ] No "improvements" to adjacent code/comments/formatting
- [ ] Matched existing style even if I'd do it differently
- [ ] Orphans from my changes removed (imports/vars/funcs)
- [ ] Pre-existing dead code mentioned but NOT deleted

### [ ] 4. Goal-Driven Execution
- [ ] Transformed imperative task → verifiable goal
- [ ] Stated brief plan with verify steps (for multi-step work)
- [ ] Success criteria strong enough to loop independently

Plan format:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

## Interaction with other skills
- **Precedes** `plan_before_code` — principles 1+4 feed into the formal plan
- **Complements** `commit_push_approval` — surgical changes → cleaner diffs → easier approval
- **Does not override** user instructions (CLAUDE.md, direct requests always win)

## Anti-patterns to block
- "Let me also clean up this nearby function while I'm here" → violates #3
- "I'll add error handling just in case" (for internal code) → violates #2
- Silently picking one of multiple valid interpretations → violates #1
- "Make it work" as the only success criterion → violates #4
- 200-line implementation where 50 would do → violates #2

## Verify on completion
Before claiming done:
- Diff review: every line maps to the request (#3)
- Line count vs minimum viable (#2)
- Assumptions surfaced up front (#1)
- Success criteria met and verified (#4)
