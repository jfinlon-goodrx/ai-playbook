# Task: Test Generation for Python

## Prompt: Unit Tests

```text
@[service, view, or domain file]
@[existing test file]
@[project context or rules file]

Write comprehensive tests for this Python code using pytest and the repo's current mocking style.

For each public behavior, cover:
1. happy path
2. validation failures
3. not found or missing dependency cases
4. authorization or permission failures when applicable
5. boundary and edge cases

Match the repo's existing fixture, factory, and assertion patterns.
Do not test private helpers directly unless the repo already does that by convention.
```

## Prompt: API Integration Tests

```text
@[existing API test]
@[router or view file]

Write API integration tests for this feature using the repo's current test client pattern.

Cover:
- success path
- invalid input
- unauthenticated
- forbidden
- not found
- response shape and important side effects
```
