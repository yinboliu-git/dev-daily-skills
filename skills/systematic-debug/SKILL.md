---
name: systematic-debug
description: >-
  Structured four-phase debugging workflow: Observe → Hypothesize → Test → Fix.
  Replaces random guessing with methodical fault isolation. Use when the user says:
  "debug this", "fix this bug", "find the root cause", "why is this failing",
  "something is broken", "investigate the error", "help me debug", or describes
  unexpected behavior like "it's not working", "this should return X but returns Y".
---

# Systematic Debugging

Follow four phases in strict order. Never jump to "Fix" before completing
the first three phases. Random changes waste time and introduce new bugs.

<HARD-GATE>
Do NOT edit any code until you have completed Observe, Hypothesize, and Test phases.
A fix without a confirmed hypothesis is gambling.
</HARD-GATE>

## Phase 1: Observe

Gather facts before forming opinions.

1. **Reproduce the bug.** Run the failing command or test. Confirm the error is reproducible.
2. **Read the error message completely.** Most debugging time is wasted ignoring what the error already tells you.
3. **Collect context:**
   - What is the expected behavior vs. actual behavior?
   - When did it start? (check recent changes with `git log --oneline -10`)
   - What inputs trigger it? What inputs don't?
   - Is it consistent or intermittent?
4. **Read the relevant code.** Trace the code path from entry point to failure site.

Output a concise observation summary before moving on.

## Phase 2: Hypothesize

Form **one** specific, falsifiable hypothesis.

A good hypothesis:
- Names a specific line or function
- States what it should do vs. what it actually does
- Can be proven wrong with a single test

Bad: "Maybe it's a race condition somewhere in the auth module."
Good: "Line 47 of auth.go reads the session before middleware sets it, so `session` is always nil."

If you have multiple hypotheses, list them all and rank by likelihood.
Test the most likely one first.

## Phase 3: Test

Design a minimal test to confirm or refute your hypothesis.

1. **Add a log/print/assert** at the suspected failure point
2. **Run the reproduction case** and check output
3. **Interpret results:**
   - Confirmed → move to Phase 4
   - Refuted → go back to Phase 2 with new information
   - Inconclusive → refine the test, or try the next hypothesis

Ask the user to run the diagnostic if you cannot run it yourself.

## Phase 4: Fix

Only now do you write code.

1. **Write the minimal fix.** Change the smallest amount of code that fixes the root cause.
2. **Verify the fix** by running the reproduction case again.
3. **Run existing tests** to check for regressions: `git diff --name-only | grep test`
4. **If the original reproducer has no test**, write one. The bug that isn't tested comes back.
5. **Output a root cause summary:**
   - What the bug was
   - Why it happened
   - How the fix addresses the root cause
   - Why this fix is minimal

## Checklist

Before declaring "fixed":
- [ ] The reproduction case now passes
- [ ] Existing tests still pass
- [ ] A regression test exists (or I explained why not)
- [ ] I can explain the root cause in one sentence
- [ ] The fix addresses the root cause, not a symptom

## Anti-Patterns

| Don't | Do |
|-------|-----|
| Change multiple things at once | Change one thing, test, repeat |
| "I don't know why it works now" | Confirm the hypothesis before fixing |
| Add a sleep/workaround | Fix the root cause |
| Skip the regression test | Add a test — or the bug returns next month |
| Assume the error message is wrong | Read it carefully first |

## Example

**User:** "The login page accepts the form but doesn't redirect me."

**Phase 1 — Observe:**
- Expected: POST /login → 302 redirect to /dashboard
- Actual: POST /login → 200 OK, stays on login page
- Reproduced with valid credentials
- `git log --oneline -5` shows `a1b2c3d refactor: extract auth middleware`

**Phase 2 — Hypothesize:**
- Hypothesis: The extracted middleware calls `next()` after setting the session, but the login handler checks `req.session` before `next()` returns, so the session cookie isn't set yet when the redirect check runs.
- File: `middleware/auth.js:12`

**Phase 3 — Test:**
- Add `console.log(req.session)` in the login handler before the redirect check
- Run → session is `undefined`
- Hypothesis confirmed

**Phase 4 — Fix:**
- Move session check to after `await next()` in the middleware chain
- Reproduction case now redirects correctly
- All existing auth tests pass
- Added regression test for session availability in handler
