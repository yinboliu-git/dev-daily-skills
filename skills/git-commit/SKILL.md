---
name: git-commit
description: >-
  Generates structured, descriptive commit messages following Conventional Commits
  format. Reads staged changes and produces a commit message that is specific,
  imperative, and under 72 characters for the summary. Use when the user says:
  "commit my changes", "write a commit message", "what should my commit say",
  "generate commit", "create a commit", or any request to document staged changes
  for version control.
---

# Git Commit Message Generator

Read staged changes with `git diff --staged`, then generate a Conventional Commits
message. **Generate first — do not ask clarifying questions.** Make assumptions
and flag them after output.

## Output Format

```
type(scope): short imperative summary under 72 characters

Body (if non-trivial):
- What changed and why
- One bullet per logical change
- No bullet for "how" — the diff already shows that

Footer (if applicable):
BREAKING CHANGE: description of what breaks and migration path
Closes #123
Refs #456
```

## Commit Types

| Type | When to use |
|------|------------|
| `feat` | New feature or functionality |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Performance improvement |
| `test` | Adding or correcting tests |
| `chore` | Build, CI, dependencies, tooling |
| `style` | Formatting, whitespace, semicolons (no code change) |

## Quality Rules

- **Imperative mood**: "add" not "added", "fix" not "fixed"
- **No period** at end of summary line
- **Be specific**: "fix race condition in user auth" not "fix bug"
- **Scope is optional** but include it when the change is scoped to a module
- **Never** use vague verbs: "update", "change", "modify", "improve"
- **Flag** if more than 3 unrelated files changed: append "⚠ Split suggestion: this touches {N} unrelated areas; consider separate commits."

## Post-Generation

After outputting the commit message:
1. Ask: "Commit with this message?" (yes/edit/skip)
2. If yes → run `git commit -m "<message>"`
3. If edit → ask which part to change, regenerate, go back to step 1
4. If skip → stop

## Examples

**Simple fix:**
```
fix(api): handle null response in user lookup
```

**Feature with body:**
```
feat(search): add fuzzy matching for product names

- Match products even when query has typos or partial words
- Uses Levenshtein distance with max threshold of 3
- Falls back to exact match if fuzzy index not built yet

Closes #342
```

**Breaking change:**
```
refactor(api)!: replace /v1/users with /v2/users endpoint

- New endpoint returns paginated results by default
- Removed deprecated `include_deleted` query param

BREAKING CHANGE: /v1/users is removed. Migrate to /v2/users which
returns `{ data: [...], pagination: {...} }` instead of a plain array.
```

## Troubleshooting

**No staged changes:** "Nothing staged. Run `git add <files>` first, then I'll generate the message."

**Too many files:** If >20 files are staged with no clear theme, suggest splitting into multiple commits and ask which logical group to commit first.
