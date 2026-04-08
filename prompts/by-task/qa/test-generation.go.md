# Task: Test Generation for Go

## Prompt: Unit Tests

```text
@[service or handler file]
@[existing test file]
@[project context or rules file]

Write comprehensive tests for this Go code using the repo's current test style.

For each public behavior, cover:
1. happy path
2. invalid input
3. not found or dependency failures
4. authorization or permission failures when applicable
5. boundary and edge cases

Prefer table-driven tests when the repo already uses them.
Use `httptest`, testify, or the standard library patterns already present in the codebase.
```

## Prompt: Handler Tests

```text
@[handler file]
@[existing handler test]

Write handler or endpoint tests that cover:
- success path
- malformed input
- unauthorized or forbidden access
- not found
- correct status codes and response payloads
```
