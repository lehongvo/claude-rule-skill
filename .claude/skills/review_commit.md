---
name: commit-reviewer
description: >
  Automatically reviews git commits to surface bugs, security issues, code
  smells, performance problems, and improvement suggestions. Activate this skill
  when the user mentions "review commit", "check commit", "review diff",
  "review PR", "review pull request", "check code", "audit code", pastes a git
  diff, pastes a code change for review, or asks "is there anything wrong with
  this code?". Always engage this skill whenever code changes need to be
  reviewed, even if the user does not explicitly say "commit".
---

# Commit Reviewer Skill

Comprehensive git-commit review agent: detects bugs, security issues, code smells, and performance problems.

---

## Step 1 — Collect the diff

Determine the input source in this order of priority:

1. **User pastes the diff directly** → use it as-is, no extra steps.
2. **User provides a commit hash or branch** → run:
   ```bash
   git diff <base>..<commit>   # or
   git show <commit_hash>
   ```
3. **User uploads a file** → read it via the file-reading skill.
4. **No input available** → ask the user to paste the diff or provide a commit hash.

If the diff is very long (>500 lines), ask whether to review the entire diff or focus on specific files.

---

## Step 2 — Analyze the commit message (if available)

Check:
- Is the commit message clear and accurately describing the change?
- Does it follow a convention (feat/fix/chore/refactor/...)?
- Do the actual code changes match the message?

---

## Step 3 — Comprehensive review by category

Analyze the diff across **6 categories**. For each category, list findings with **specific locations** (file name + line number when possible).

### 🔴 CRITICAL — Security & dangerous bugs
Stop and flag clearly if any of these appear:
- **Hardcoded secrets / credentials**: API keys, passwords, tokens, private keys in code
- **Injection vulnerabilities**: SQL injection, command injection, XSS, SSTI
- **Path traversal / file inclusion**: user input used directly in file paths
- **Insecure deserialization**: `pickle`, `yaml.load(...)`, or `eval(...)` on untrusted input
- **Weak cryptography**: MD5/SHA1 for passwords, ECB mode, hardcoded keys
- **Authentication / authorization flaws**: bypass logic, missing auth checks
- **Sensitive data exposure**: logging passwords/PII, returning stack traces to the user
- **Unsafe dependencies**: unfamiliar libraries, versions with known CVEs

### 🟠 HIGH — Bugs & logic errors
- Race conditions, deadlocks
- Null/nil pointer dereferences
- Off-by-one errors
- Resource leaks (files, connections, goroutines not closed/cancelled)
- Incorrect error handling: ignored errors, overly broad catches
- Clear logic errors: inverted conditions, infinite loops
- Breaking changes without a migration path

### 🟡 MEDIUM — Code quality & maintainability
- Functions that are too long (>50 lines) or do too much (SRP violation)
- Magic numbers/strings without named constants
- Duplicated code (DRY violation)
- Vague variable/function names
- Deeply nested conditionals (>3 levels)
- Dead code, commented-out code
- Missing or incorrect unit tests for important logic

### 🔵 LOW — Performance
- N+1 queries inside loops
- Unnecessary object allocation in hot paths
- Missing index hints / inefficient queries
- Blocking I/O in async contexts
- Unnecessary deep copy / clone

### ⚪ STYLE — Coding conventions
- Deviation from the project's style guide (when patterns are visible in code)
- Unused imports
- Inconsistent formatting

### ✅ STRENGTHS
Acknowledge what is done well: test coverage, clear error handling, good abstractions, etc.

---

## Step 4 — Output format

Return the review in this structure:

```
## 📋 Commit overview
- **Description**: [summary of the change]
- **Files changed**: X files, +Y lines, -Z lines
- **Overall verdict**: [Safe to merge / Needs fixes before merge / Do NOT merge]

---

## 🚨 Findings

### 🔴 CRITICAL (X issues)
**[Issue name]** — `file.py:42`
> [Offending code]
Explanation: [why it is dangerous]
Suggested fix: [code or guidance]

### 🟠 HIGH (X issues)
...

### 🟡 MEDIUM (X issues)
...

### 🔵 LOW (X issues)
...

---

## ✅ Strengths
- ...

---

## 📌 Conclusion & action items
[List the items that MUST be addressed before merge, in priority order]
```

---

## Important Rules

- **Always cite locations**: file name + line number, never vague references.
- **Do not fabricate**: only report issues actually visible in the diff. When unsure, say "this could be an issue if...".
- **Sort by severity**: CRITICAL findings come first and must be unmistakable.
- **Hardcoded secret = STOP**: any leaked secret/credential is flagged CRITICAL immediately, with a clear risk explanation.
- **Provide concrete fixes**: don't just identify issues; suggest how to fix them.
- **Concise but complete**: skip trivial nits. Focus on what truly matters.
- **Match the user's language**: respond in the language the user is using (Vietnamese / English).
