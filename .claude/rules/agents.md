# Specialized Agents Guide

## When to Use Each Agent

### `bug-fix-workflow` (model: opus, color: red)
**Trigger:** Any bug, error, crash, stack trace, failing test
**Workflow:** 8-step systematic process
1. Analyze root cause (use gopls-lsp + context7)
2. Write reproducing test (red)
3. Fix (minimal, focused)
4. 6-agent parallel review (security, logic, quality, test, Go, CodeRabbit)
5. Refactor cleanup
6. Run bug test (must be green)
7. Run full suite (no regressions)
8. Delete test artifacts

### `code-review-commit-parallel` (model: opus, color: varies)
**Trigger:** Large commits (50+ files) needing comprehensive review
**Workflow:** Splits files into 4 groups → parallel sub-agents → synthesized report
- Catches: logic errors, duplication, edge cases, validation gaps, dead code

## Agent Memory
Each agent accumulates institutional knowledge in:
`.claude/agent-memory/<agent-name>/MEMORY.md`

Read these before starting complex work — agents record patterns, gotchas, and decisions across sessions.
