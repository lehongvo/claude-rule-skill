# Commit Rules

## Hard Rule: ALWAYS Confirm Before Commit

- **NEVER run `git commit` without explicit user approval first**
- Before committing, show the user:
  1. Files being committed (`git status`)
  2. Summary of changes
  3. Proposed commit message
- Wait for the user to say "ok", "commit", "go", "approved", or equivalent before executing
- This applies to ALL commits — no exceptions, even for "obvious" changes

## Push Rules
- **NEVER push without explicit user instruction**
- `unittest/` repo: commit only, NEVER push
- Other repos: push only when user explicitly says "push"
