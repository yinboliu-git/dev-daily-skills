---
name: test-first
description: >-
  Enforces Test-Driven Development: RED (write a failing test) → GREEN (minimal
  code to pass) → REFACTOR (clean up without breaking tests). Prevents writing
  implementation before tests exist. Use when the user says: "write tests",
  "add tests for this", "I need tests", "test this function", "add test coverage",
  "TDD this", "test-driven", or any request to add tests to existing or new code.
---

# Test-First Development (TDD)

Follow the RED → GREEN → REFACTOR cycle strictly. Never write implementation
code before a failing test exists for it.

<HARD-GATE>
If the user asks you to implement a feature or fix a bug without tests:
STOP. Write the test first. If you write implementation before a
failing test, you are NOT doing TDD.
</HARD-GATE>

## The Cycle

```
RED → GREEN → REFACTOR → (repeat)
```

Each cycle should take 2-5 minutes. If it takes longer, the test is too big —
split it into smaller tests.

## Phase 1: RED — Write a Failing Test

1. **Identify the smallest testable behavior.** Not "the whole feature" — one specific case.
2. **Write the test.** Name it clearly: `test_<function>_<scenario>_<expected_result>`
3. **Run it and watch it FAIL.** A test that passes before you write code is useless — it might be testing nothing.

```python
# Good test name
def test_parse_date_rejects_empty_string():
    with pytest.raises(ValueError):
        parse_date("")

# Bad test name
def test_parse_date_1():  # What does this test?
    ...
```

## Phase 2: GREEN — Write Minimal Code

1. **Write the simplest code that makes the test pass.** This is often one line.
2. **No gold-plating.** Don't add error handling, optimization, or edge cases that aren't tested yet.
3. **Run the test.** It must pass. All other tests must still pass too.
4. **Commit** when green.

If the test still fails after writing code:
- Is the test correct? (expected value wrong?)
- Is the code being called? (import missing?)
- Debug before writing more code.

## Phase 3: REFACTOR — Clean Up

1. **Remove duplication.** Same logic in two places? Extract it.
2. **Improve names.** Now that you understand the code, can the name be clearer?
3. **Simplify.** Is there a simpler way to express this? (But don't change behavior.)
4. **Run tests.** They must still pass. If they don't, undo the refactor and try again.

## Triangulation

When you don't know the right implementation, use triangulation:

1. Write test for case A → make it pass with simple code
2. Write test for case B → generalize the code to handle both
3. Write test for case C → the pattern is now clear

Don't jump to the general solution after one test. You need at least two cases
to see the pattern.

## Test Template by Language

### Python (pytest)
```python
import pytest
from module import function_name

def test_function_name_normal_case():
    """Happy path with typical input."""
    result = function_name("typical_input")
    assert result == expected_output

def test_function_name_empty_input():
    """Edge case: empty string."""
    result = function_name("")
    assert result == default_value

def test_function_name_invalid_input():
    """Invalid input should raise."""
    with pytest.raises(ValueError, match="specific message"):
        function_name(None)

def test_function_name_boundary():
    """Right at the limit."""
    result = function_name("a" * 255)  # max length
    assert result is not None
```

### JavaScript/TypeScript (vitest/jest)
```typescript
import { describe, it, expect } from 'vitest';
import { functionName } from './module';

describe('functionName', () => {
  it('returns expected output for typical input', () => {
    expect(functionName('typical')).toBe(expectedOutput);
  });

  it('handles empty input gracefully', () => {
    expect(functionName('')).toBe(defaultValue);
  });

  it('throws on invalid input', () => {
    expect(() => functionName(null)).toThrow('specific message');
  });
});
```

## Test Selection Heuristic

When adding tests to existing code, prioritize:
1. **Happy path** — does the main use case work?
2. **Edge cases** — empty, null, zero, negative, max, min
3. **Error cases** — what happens when things go wrong?
4. **Boundary values** — just inside/outside the limit
5. **Combinations** — interactions between features

## Checklist

Before declaring a feature tested:
- [ ] Happy path covered
- [ ] Each error case covered
- [ ] Edge cases covered (empty, null, boundary)
- [ ] Tests are independent (no shared mutable state between tests)
- [ ] Each test has a clear name that describes the scenario
- [ ] No implementation code written before its test
- [ ] All tests pass

## When NOT to TDD

TDD is not appropriate for:
- **Exploratory/spike code** that you'll throw away
- **Configuration files** (YAML, JSON, TOML)
- **Generated code** (protobuf stubs, GraphQL types)
- **UI layout** (test behavior, not markup)

State clearly if you're skipping TDD and why.

## Troubleshooting

**"This is too simple to test"**:
If it's truly trivial (one-line delegation), skip it. But a "trivial" function
with an if-statement has 2 paths and needs 2 tests.

**"Testing this requires mocking too many things"**:
The code is too coupled. Refactor first: extract the pure logic from the side effects,
then test the pure logic without mocks.

**"Tests take too long to write"**:
Each RED-GREEN-REFACTOR cycle should be 2-5 minutes. If tests take longer,
the test is too big — test a smaller behavior.
