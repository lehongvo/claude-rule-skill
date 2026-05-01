# Karpathy Guidelines — LLM Coding Discipline (PRIORITY: APPLY FIRST)

Source: https://github.com/forrestchang/andrej-karpathy-skills
Origin: Andrej Karpathy's observations on LLM coding pitfalls (https://x.com/karpathy/status/2015883857489522876)

> **⚠️ This rule is applied FIRST, before any other rule or skill.**
> Order: karpathy-guidelines → plan_before_code → domain rules (migration/testing/agents) → commit_push_approval

These 4 principles override default "just do it" behavior. Biased toward **caution over speed** — for trivial one-liners, use judgment.

---

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State assumptions explicitly. If uncertain, **ask**.
- If multiple interpretations exist, **present them** — don't pick silently.
- If a simpler approach exists, **say so**. Push back when warranted.
- If something is unclear, **stop**. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If 200 lines could be 50, rewrite it.

**Test:** Would a senior engineer call this overcomplicated? If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor what isn't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, **mention** it — don't delete it.

When your changes create orphans:
- Remove imports/vars/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

**Test:** Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform imperative tasks into verifiable goals:

| Instead of... | Transform to... |
|---|---|
| "Add validation" | "Write tests for invalid inputs, then make them pass" |
| "Fix the bug" | "Write a test that reproduces it, then make it pass" |
| "Refactor X" | "Ensure tests pass before and after" |

For multi-step tasks, state a brief plan:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let the model loop independently. Weak criteria ("make it work") require constant clarification.

---

## Signals it's working
- Fewer unnecessary changes in diffs
- Fewer rewrites due to overcomplication
- Clarifying questions come **before** implementation, not after mistakes
- Clean, minimal PRs — no drive-by refactoring

## Project interaction
- Complements `.claude/rules/migration.md`, `testing.md`, `commit.md`, `agents.md`
- Complements `.claude/skill/plan_before_code.md` (Principle 1 + 4 → formal plan)
- When in doubt, user instructions (CLAUDE.md, direct requests) override these guidelines.
