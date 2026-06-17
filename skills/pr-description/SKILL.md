---
name: pr-description
description: >-
  Generates structured pull request descriptions from git branch diff. Reads
  commit history and diff against the base branch, then produces a PR description
  with summary, changes, testing notes, and screenshots placeholder. Use when
  the user says: "create a PR", "write PR description", "generate pull request",
  "open a PR", "describe my changes for PR", "summarize this branch for review",
  or any request to prepare changes for code review on GitHub.
---

# Pull Request Description Generator

Analyze the current branch's commits and diff against the base branch,
then generate a structured PR description. **Generate first — do not ask
clarifying questions.** Make assumptions and flag them.

## Analysis Steps

1. **Identify base branch:** Usually `main` or `master`. Check with:
   `git remote show origin | grep "HEAD branch"` or `git branch -a | grep main`
2. **Get commits:** `git log base..HEAD --oneline`
3. **Get changed files:** `git diff base..HEAD --stat`
4. **Get the full diff:** `git diff base..HEAD`
5. **Synthesize:** Turn the above into a PR description

## Output Format

```markdown
## Summary
{2-3 sentences: what this PR does and why. Focus on the problem solved,
not a file-by-file list.}

## Changes
- **{area}**: {what changed in this area and why}
- **{area}**: {what changed in this area and why}

## Test Plan
- [ ] {specific test that should pass}
- [ ] {edge case to verify}
- [ ] {manual test to run}

## Screenshots
<!-- If this PR changes the UI, add before/after screenshots here -->

## Notes for Reviewers
<!-- Any context reviewers need: known limitations, follow-up work, decisions made -->
{Call out anything surprising, risky, or worth extra scrutiny}

## Post-Merge
<!-- Migration steps, config changes, or cleanup needed after merge -->
{List anything that needs to happen after merging, or write "None"}
```

## Quality Rules

### Summary
- **Start with the problem**, not the diff. "Users couldn't reset passwords because..." not "Changed the password reset handler..."
- **No file list in the summary.** The "Changes" section handles that.
- **If the branch fixes an issue, mention it:** "Fixes #123 — users couldn't reset passwords..."

### Changes Section
- **Group by logical area**, not by file. "Authentication" not "auth.js, login.js, session.js"
- **Explain WHY** each area changed, not just WHAT
- **One bullet per area**, not one per file
- **If an area is a refactor with no behavior change, say so**

### Test Plan
- **Be specific.** Not "Test the feature" — "Log in as user `test@example.com`, click 'Forgot Password', verify email arrives within 10 seconds, click link, set new password, confirm login works."
- **Include manual tests** for things automated tests can't cover (UI, email, third-party APIs)
- **Include edge cases** you thought about

## Special Cases

### Single Commit
If the branch has only one commit, use that commit message as the PR title and expand for the body.

### Large PR (>20 files, >500 lines)
Add a warning at the top:
> ⚠ **This is a large PR** ({N} files, {N} lines). The changes are grouped by
> logical area below. Reviewing commit-by-commit may be easier.

### Breaking Changes
Add a prominent callout:
> ⚠ **BREAKING CHANGE**: {what breaks and how to migrate}.

## Post-Generation

After outputting the description:
1. Ask: "Create the PR with this description?" (yes/edit/skip)
2. If yes and `gh` CLI is available:
   - Run `gh pr create --title "<title>" --body "<description>"`
3. If yes and `gh` is not available:
   - Output the title and body for manual pasting into GitHub
4. If edit → ask which section, regenerate, go back to step 1

## Checklist

Before delivering:
- [ ] Summary says what and why, not a file list
- [ ] Changes grouped by logical area
- [ ] Each area explains WHY
- [ ] Test plan has specific, actionable steps
- [ ] Breaking changes flagged if present
- [ ] PR title is under 72 characters
- [ ] Mentioned related issues (#123, Closes #456)

## Example

```markdown
## Summary
Fixes #342 — the search page would time out when users searched with
more than 3 filters. The root cause was a missing database index
on the combined filter columns, plus the query was loading all
results into memory instead of using pagination.

## Changes
- **Search indexing**: Added composite index on `(category, price_range, rating)`
  to support multi-filter queries efficiently
- **Query pagination**: Search results now use cursor-based pagination
  (20 results per page) instead of loading everything
- **Frontend**: Added "Load More" button and loading states to search page

## Test Plan
- [ ] Search with 0, 1, 3, and 5 filters — each returns in <500ms
- [ ] Click "Load More" — next page appends, no duplicates
- [ ] Search with no results — shows empty state, not error
- [ ] Manual: Test on staging with production-size dataset (5M products)

## Notes for Reviewers
- The index migration (`0043_add_search_indexes.sql`) will lock the
  products table for ~2 seconds on production. We need to run this
  during low-traffic window.
- Cursor-based pagination was chosen over offset because new products
  are added frequently and offset causes duplicates/misses.
```
