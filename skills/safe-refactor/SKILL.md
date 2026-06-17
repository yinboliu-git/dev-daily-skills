---
name: safe-refactor
description: >-
  Guides safe code refactoring with pre-flight checks, incremental changes,
  and verification at each step. Prevents refactoring from introducing bugs.
  Use when the user says: "refactor this", "clean up this code", "extract this",
  "rename this", "move this function", "split this file", "simplify this",
  "improve this code structure", or any request to restructure code without
  changing behavior.
---

# Safe Refactoring

Refactoring means changing structure WITHOUT changing behavior.
If you change behavior, that's a rewrite — use a different workflow.

<HARD-GATE>
Before making ANY change, confirm there are passing tests that cover the code
you're about to refactor. If no tests exist, STOP and tell the user:
"No tests cover this code. Add a characterization test first — it captures
current behavior so we can detect regressions. Want me to write one?"
</HARD-GATE>

## Phase 1: Safety Net

1. **Run existing tests:** `git diff --name-only` to find touched files, then run their tests
2. **Check coverage:** If the code being refactored has no tests, write a characterization test first
3. **Commit or stash** any unrelated changes so the working tree is clean
4. **Confirm:** "Tests pass on baseline. Starting refactor."

## Phase 2: Plan

Announce what you're about to do before doing it.

1. **State the goal:** "I'm going to extract the validation logic from `UserService.create()` into a separate `validateUser()` function."
2. **State the scope:** What files will change? What will NOT change?
3. **Keep it small:** If the plan has more than 5 steps, split it. Refactors are easiest to review when they're small.
4. **Ask:** "This is a {estimated complexity} change. Proceed?"

## Phase 3: Execute Incrementally

One atomic change at a time. After EACH step, run tests.

### Common Refactoring Patterns

**Extract Function:**
1. Create new function with the extracted code
2. Replace original code with call to new function
3. Run tests
4. Commit: `refactor(module): extract <function> from <source>`

**Rename:**
1. Use IDE refactor or search-and-replace (NOT manual — you'll miss something)
2. Run tests
3. Commit: `refactor(module): rename <old> to <new>`

**Move Function:**
1. Copy function to new location
2. Update imports in new location
3. Update callers to use new location
4. Remove from old location
5. Run tests at step 3 AND step 4
6. Commit when done

**Split File:**
1. Create new file
2. Move one export at a time
3. Update imports
4. Run tests after EACH export moved
5. Commit when done

## Phase 4: Verify

After all steps are complete:

1. **Run full test suite** (not just the files you touched)
2. **Diff against baseline:** `git diff main...HEAD --stat` — verify only expected files changed
3. **Self-review the diff:** Look for accidental changes, leftover debug code, commented-out lines
4. **Confirm no behavior changes:** The diff should show structural changes only — renames, extractions, reorganizations. If you see logic changes, flag them.

## Checklist

Before declaring "done":
- [ ] All tests pass (not just related tests — the full suite)
- [ ] No behavior changed (only structure)
- [ ] Each commit compiles and passes tests independently
- [ ] No TODO comments left behind
- [ ] No dead code left behind (old functions that are now unused)
- [ ] New function/class names are clear and follow project conventions

## Anti-Patterns

| Don't | Do |
|-------|-----|
| Refactor + add feature in same PR | Refactor first, feature second — separate PRs |
| Change 10 files in one commit | One atomic change per commit |
| Skip tests because "it's a simple rename" | Even renames can break imports |
| Refactor without tests | If untested, write a characterization test first |

## Example

**User:** "Refactor the payment processing in `checkout.js` — it's doing too much in one function."

**Phase 1 — Safety net:**
- `npm test -- checkout` → 23 tests passing
- Characterization test: capture current output for valid and invalid payments
- Clean working tree confirmed

**Phase 2 — Plan:**
> "I'll extract three functions from `processPayment()`:
> 1. `validatePaymentInput(input)` — input validation
> 2. `calculateTotal(items, discounts)` — price calculation
> 3. `chargeCustomer(paymentMethod, amount)` — payment gateway call
>
> The original `processPayment()` becomes an orchestrator that calls these three.
> No behavior changes. 2 files changed. Proceed?"

**Phase 3 — Execute:**
- Extract `validatePaymentInput` → 23 tests pass → commit
- Extract `calculateTotal` → 23 tests pass → commit
- Extract `chargeCustomer` → 23 tests pass → commit
- Refactor `processPayment` to orchestrator → 23 tests pass → commit

**Phase 4 — Verify:**
- Full suite: 142 tests pass
- `git diff main...HEAD --stat` shows only `checkout.js` and `checkout.test.js`
- No logic changes in diff — only code movement
