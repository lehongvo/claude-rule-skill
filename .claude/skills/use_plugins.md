---
name: use-plugins
description: >
  Requires using purpose-built plugins for code review and codebase search
  instead of doing it manually. Activate this skill when the user asks to
  review code, review a commit, review a PR, review a diff, audit code quality,
  or when they need to search/explore the codebase, look up an API/library, or
  read framework documentation.
  Trigger phrases: "review", "check code", "audit", "code review", "search",
  "find", "look up", "ask about library X", "docs for", "how does X work".
---

# Use Plugins Skill

Route tasks to the right plugin instead of handling them manually with Read/Grep.

---

## Routing Rules

### Code review → use a review plugin

When the user requests a review (commit, PR, diff, changed files), prefer one of the following plugins:

- `code-review` — general-purpose PR review
- `coderabbit:review` or `coderabbit:code-review` — in-depth AI review with auto-fix
- `feature-dev:code-reviewer` — feature review with confidence-based filtering
- `pr-review-toolkit:review-pr` — comprehensive review using multiple specialized agents

Choose the plugin based on context:
- Large PR with many concerns → `pr-review-toolkit:review-pr`
- Review + automated fixes desired → `coderabbit:autofix`
- New feature review → `feature-dev:code-reviewer`
- Quick single-commit/diff review → `code-review` or `coderabbit:review`

Do NOT manually read the diff and write your own review when the user asks for one. Always invoke a plugin first.

---

### Codebase search & library docs → use context7

When deeper codebase understanding or library/framework lookup is needed:

- Broad codebase search across many files → use `context7` (MCP)
- API / library documentation lookup → use `context7`
- Questions like "how does X work" within the codebase → use `context7`

Only use Grep/Glob directly when:
- The exact file/symbol is already known
- The task is very small (1–2 queries are enough)
- No external context beyond the codebase is needed

---

## Important Rules

- **Plugin first, manual second** — If a suitable plugin exists, use it. Fall back to Read/Grep only when no plugin applies.
- **Do not reinvent** — Don't write your own review logic when a specialized agent exists.
- **Announce on use** — State which plugin is being used so the user knows.
- **If the plugin fails or doesn't fit** — Inform the user and propose a manual alternative.
