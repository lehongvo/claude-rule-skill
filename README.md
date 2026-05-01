# claude-rule-skill

Claude Code configuration repository containing project-level rules and skills
that shape how Claude collaborates on the codebase.

## Layout

```
.claude/
├── settings.local.json     # Local permissions (not for sharing secrets)
├── rules/                  # Project rules — applied on every interaction
│   ├── karpathy-guidelines.md   # 4 LLM coding principles (priority: apply first)
│   ├── commit.md                # Hard rules around git commit/push
│   └── agents.md                # Guide for specialized agents
└── skills/                 # Skills — invoked when triggers match
    ├── karpathy_guidelines.md   # Apply-first checklist for the 4 principles
    ├── plan_before_code.md      # Require an approved plan before any code change
    ├── commit_push_approval.md  # Require explicit approval before commit/push
    ├── review_commit.md         # Comprehensive commit review (security, bugs, quality)
    └── use_plugins.md           # Route review/search work to the right plugin
```

## Order of evaluation

For every request, rules and skills are applied in this order:

1. `karpathy-guidelines` — think before coding, simplicity, surgical changes, goal-driven execution
2. `plan-before-code` — formal plan + user approval
3. Domain rules (e.g. agents)
4. `commit-push-approval` — explicit approval before any commit/push

## Notes

- All rule and skill files are written in professional English.
- `settings.local.json` is local-only and should not contain secrets.
