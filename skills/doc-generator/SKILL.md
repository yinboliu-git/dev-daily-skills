---
name: doc-generator
description: >-
  Generates concise, useful documentation for functions, classes, and modules.
  Focuses on WHAT and WHY, not HOW (the code already shows how). Produces
  docstrings, README sections, or API docs. Use when the user says:
  "document this", "add docs", "generate documentation", "write docstrings",
  "add JSDoc", "document this function", "what does this module need documented",
  "write API docs", or points at code and asks for documentation.
---

# Documentation Generator

Generate documentation that helps the next developer understand the code
in 30 seconds. Focus on **WHAT** the code does and **WHY** it exists —
the code already shows **HOW**.

<HARD-GATE>
Do NOT write documentation for code you haven't read. Read the entire function,
class, or module before writing a single line of docs.
</HARD-GATE>

## When to Document

- **Always:** Public APIs, exported functions, complex logic, non-obvious behavior
- **Sometimes:** Internal helpers with unclear purpose, workarounds for bugs
- **Never:** Self-explanatory getters/setters, trivial one-liners, code that reads like English

## Format by Language

### Python (Google-style docstring)
```python
def function_name(param1: str, param2: int) -> bool:
    """One-line summary. Use imperative mood: "Compute the..." not "Computes the..."

    Extended description (only if the one-liner isn't enough). Explain the
    algorithm, edge cases handled, and any non-obvious behavior.

    Args:
        param1: Description of param1. Mention type constraints.
        param2: Description of param2.

    Returns:
        Description of return value. Include type info if non-obvious.

    Raises:
        ValueError: When param1 is empty.
        IOError: When the config file is missing.

    Example:
        >>> function_name("hello", 42)
        True
    """
```

### JavaScript/TypeScript (JSDoc)
```typescript
/**
 * One-line summary. Imperative mood.
 *
 * @param param1 - Description. Mention type constraints.
 * @param param2 - Description.
 * @returns Description of return value.
 * @throws {Error} When the input is invalid.
 *
 * @example
 * ```ts
 * const result = functionName("hello", 42);
 * ```
 */
```

### Go
```go
// FunctionName one-line summary. Imperative mood.
//
// Extended description for non-obvious behavior, algorithm choice,
// or important caveats. Keep it under 4 lines when possible.
//
// If there's a workaround for a bug, link the issue:
// Workaround for https://github.com/org/repo/issues/123
func FunctionName(param1 string, param2 int) (bool, error) {
```

## Content Rules

1. **Lead with the most important information.** The reader might not read past line 2.
2. **One-liner first.** Every docstring starts with a single-line summary.
3. **Document the contract, not the implementation.** What does this function promise to do? What must callers guarantee? The implementation can change; the contract shouldn't.
4. **Mention side effects.** If the function modifies state, writes to disk, makes network calls — say so upfront.
5. **Include units.** "Delay in seconds" not "The delay." "Returns size in bytes."
6. **Document thrown errors.** Every exception/error the caller might need to catch.
7. **Provide an example** for non-trivial functions. One example is worth 10 sentences.

## Quality Checklist

Before delivering:
- [ ] One-liner is present and uses imperative mood
- [ ] All parameters documented (or marked intentionally undocumented with a reason)
- [ ] Return value documented
- [ ] Exceptions/errors documented
- [ ] Non-obvious behavior explained (WHY, not WHAT)
- [ ] No documentation for trivial code (don't document `getX()/setX()`)
- [ ] Example provided for non-trivial functions
- [ ] No "This function..." prefix (the reader knows it's a function)

## Anti-Patterns

| Don't | Do |
|-------|-----|
| `// This function adds two numbers` | `// Add two integers; returns the sum` |
| `/** @param x The x parameter */` | `/** @param x - The user's age in years. Must be >= 0. */` |
| Document every line of a 3-line function | Skip trivial functions entirely |
| `// TODO: document this later` | Document now, or don't add the comment |
| Explain HOW in prose | Explain WHAT and WHY; the code shows HOW |

## Troubleshooting

**"This code is too complex to document concisely":**
The documentation isn't the problem — the code is. Suggest splitting the function:
> "This function is {N} lines and handles {X, Y, Z}. Consider extracting
> {X} and {Y} into separate functions first — then each will be easy to document."

**"I can't tell what this function does":**
> "I need to understand this code better before documenting it. Can you explain
> what problem this function solves and when it's called?"
