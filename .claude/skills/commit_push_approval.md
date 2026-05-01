---
name: commit-push-approval
description: >
  REQUIRES explicit user approval before running git commit or git push.
  Activate this skill whenever a task involves committing, pushing, or any
  action that will produce a commit/push: "commit", "push", "create commit",
  "send to remote", "sync code", "merge to main", or similar.
  Trigger phrases: "commit", "push", "create commit", "send to remote",
  "git commit", "git push", "sync", "publish".
---

# Commit & Push Approval Skill

Every `git commit` and `git push` MUST receive explicit user approval before execution.

---

## Core Rules

1. **Never run `git commit` autonomously** — Even after a code plan is approved, commits require their own approval.
2. **Never run `git push` autonomously** — Even after a successful commit, push requires its own approval.
3. **Approving a commit ≠ approving a push** — They are two distinct actions, each requiring its own approval.
4. **Approval phrases**: "ok", "approved", "commit it", "push it", "go", "lgtm", "agreed".

---

## Step 1 — Before committing

After coding is done and tests pass, do NOT commit immediately. Present:

```
## Ready to commit

### Changed files
- `path/to/file1.go` — [short description]
- `path/to/file2.go` — [short description]

### Proposed commit message
```
[type]: [short subject]

[body explaining the why, if needed]
```

### Checklist
- [x] Code is tested
- [x] No sensitive files (.env, credentials)
- [x] No leftover debug code / console.log

**Approve commit with the message above?**
```

Wait for the user's response:
- **Approve** → Step 2 (run commit)
- **Edit message** → Update → re-request approval
- **Reject** → Ask why, adjust accordingly

---

## Step 2 — Run the commit

After user approval:

1. `git status` — review staged changes
2. `git add <specific files>` — never use `git add .` or `-A`
3. `git commit -m "..."` — use a HEREDOC for multi-line messages
4. `git status` — verify the commit succeeded

Report the resulting short hash to the user.

---

## Step 3 — Before pushing

After committing, do NOT push immediately. Present:

```
## Ready to push

### Commits to be pushed
- `abc1234` — [subject]
- `def5678` — [subject] (if multiple)

### Target
- Branch: `feature/xyz`
- Remote: `origin`
- Base: `main` (ahead by X commits)

**Approve push to `origin/feature/xyz`?**
```

Wait for approval:
- **Approve** → Step 4 (run push)
- **Reject / defer** → Stop, do not push

---

## Step 4 — Run the push

After user approval:

1. `git push -u origin <branch>` (if branch is not yet tracking)
2. `git push` (if already tracking)
3. Report success and the remote branch URL if available

Do NOT use `--force` or `--force-with-lease` unless the user explicitly requests it.
Do NOT push directly to `main`/`master` unless the user explicitly requests it.

---

## Special cases

### Pre-commit hook fails
- Do NOT bypass with `--no-verify`.
- Fix the issue the hook reports → restage → create a NEW commit (do not use `--amend`).

### Multiple commits to push
- List every commit that will be pushed so the user can review.
- The user may request squash/reorder before pushing.

### User asks to commit and push together
If the user says "commit and push", present both the commit message and the push target, and request a single approval covering both actions.

---

## Important Rules

- **Two approvals by default** — Commit and push are separate checkpoints. The user may want to commit but defer pushing.
- **No auto-execute** — Even in "plan mode", commit/push still require runtime approval.
- **Stage exact files** — Add only the files relevant to the task; never stage indiscriminately.
- **Follow the repo's commit-message convention** — Inspect `git log` to learn the style before proposing a message.
- **Do not skip hooks or disable signing** — Unless the user explicitly requests it.
- **Do not force-push** — Unless the user explicitly requests it, and never on main/master.
