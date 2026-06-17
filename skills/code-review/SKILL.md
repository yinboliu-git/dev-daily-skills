---
name: code-review
description: >-
  Reviews code changes for correctness, security, performance, and maintainability.
  Runs a structured review checklist and outputs findings organized by severity.
  Use when the user says: "review this code", "review my changes", "code review",
  "check this PR", "find issues in this code", "check for bugs", "is this safe",
  "review the diff", or presents code and asks for feedback on quality.
---

# Code Review

Review code changes systematically. Focus on what matters — correctness and
safety first, style last (linters handle style). Always output findings
organized by severity.

## Review Scope

Determine what to review:
1. If in a git repo: review `git diff` (unstaged + staged) by default
2. If user points to specific files: review those files
3. If user pastes code: review the pasted code

## Review Dimensions

Check each dimension in order. Stop and flag critical issues immediately.

### 1. Correctness (CRITICAL)
- Does the code do what it claims to do?
- Are edge cases handled? (null/undefined, empty collections, zero values, negative numbers)
- Are conditions correct? (off-by-one, inverted booleans, missing else)
- Is error handling present for operations that can fail? (I/O, network, parsing)
- Are async operations properly awaited?

### 2. Security (CRITICAL)
- User input: is it validated and sanitized?
- SQL/NoSQL: any risk of injection? (parameterized queries used?)
- Secrets: any hardcoded keys, tokens, passwords?
- Authentication/authorization: are checks present on new endpoints?
- File paths: any path traversal risk?

### 3. Data Integrity (HIGH)
- Are database operations wrapped in transactions where needed?
- Is there risk of data loss or corruption?
- Are migrations reversible where possible?
- Are constraints enforced at the database level, not just application?

### 4. Performance (MEDIUM)
- N+1 queries?
- Unbounded loops or recursion?
- Large data loaded into memory unnecessarily?
- Missing indexes on new query patterns?
- Unnecessary re-renders (frontend)?

### 5. Error Handling (MEDIUM)
- Are errors surfaced with enough context to debug?
- Are error messages user-friendly (no stack traces to users)?
- Is the system left in a valid state after errors?
- Are retry strategies appropriate for transient failures?

### 6. Maintainability (LOW)
- Are names clear and consistent with project conventions?
- Is there duplicate code that should be extracted?
- Are functions small enough to understand at a glance? (>50 lines — flag it)
- Are complex sections commented with WHY, not WHAT?

## Output Format

```markdown
## Code Review: {scope}

### Critical
- [ ] **{file:line}** — {issue}. Fix: {one-sentence suggestion}

### High
- [ ] **{file:line}** — {issue}. Fix: {one-sentence suggestion}

### Medium
- [ ] **{file:line}** — {issue}. Fix: {one-sentence suggestion}

### Low
- [ ] **{file:line}** — {issue}. Fix: {one-sentence suggestion}

### Summary
- Files reviewed: {N}
- Critical: {N}, High: {N}, Medium: {N}, Low: {N}
- Overall: {LGTM / Changes Requested / Needs Discussion}
```

## What NOT to Flag

Skip these — they're linter territory:
- Formatting/whitespace (unless it obscures logic)
- Import order
- Semicolons, trailing commas
- Variable naming preferences that are consistent within the file
- Minor style differences covered by the project's formatter config

## Checklist

Before delivering the review:
- [ ] Every Critical and High finding has a concrete fix suggestion
- [ ] I did NOT flag style issues a linter would catch
- [ ] I checked the diff, not just the file (avoid reviewing stale code)
- [ ] Security-sensitive paths (auth, payment, data export) got extra scrutiny
- [ ] I'm not blocking on personal preference — only on real problems

## Example

**Good finding:**
> **Critical: `src/auth.js:42`** — Session token stored in localStorage.
> localStorage is accessible to any JS on the page (XSS risk). Use an
> httpOnly cookie or the Credential Management API instead.

**Bad finding:**
> **Low: `src/auth.js:42`** — Consider using a different variable name.
> (No: name is fine, this is personal preference.)
