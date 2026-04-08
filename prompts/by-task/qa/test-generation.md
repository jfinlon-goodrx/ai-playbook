# Task: Test Generation

## When to Use
You need unit tests, integration tests, or both for existing or new code. AI is especially useful for expanding scenario coverage, drafting test cases, and spotting obvious gaps in existing suites.

## Use With

- `.dotnet`: `test-generation.dotnet.md`
- `.python`: `test-generation.python.md`
- `.go`: `test-generation.go.md`

## Prerequisites
- [ ] The code to test exists (or you're doing test-first development)
- [ ] The test stack is already present in the repo
- [ ] Existing tests are available for pattern reference

## The Prompt: Unit Tests for Existing Code

```
@[SourceFile]
@[ExistingTestFile] (for pattern reference)
@[ProjectContextOrRulesFile]

Write comprehensive tests for this code using the repo's current test stack and conventions.

For each public behavior, generate tests for:
1. happy path
2. validation failures
3. not found or dependency failure cases
4. authorization or permission failures if applicable
5. edge cases and boundary values

Follow the patterns in [ExistingTestFile] for:
- Test class structure and naming
- setup and fixture style
- assertion style
- builders or factories if used

Naming convention: MethodName_Scenario_ExpectedResult

Do not test private methods. Do not test trivial getters/setters.
Focus on business logic and error handling.
```

## The Prompt: Integration Tests

```
@[ExistingIntegrationTest]
@[RelevantAppOrRouterFile]
@[RelevantDataSetupFile]

Write integration tests for the [Feature] endpoints or end-to-end service flow.

Use the integration-test pattern from [ExistingIntegrationTest].
Tests should:
1. Seed or arrange necessary reference data
2. Test the full request/response or service flow
3. Verify resulting state after mutations
4. Verify status codes or equivalent outcome signals
5. Test authentication and authorization where applicable

For each endpoint:
- Happy path with valid auth
- 401 without authentication
- 403 with wrong role
- 400 with invalid input
- 404 for non-existent resources
```

## The Prompt: Add Tests to Existing Untested Code

```
@[SourceFile].cs

This file has no tests. Analyze it and generate a comprehensive test suite.

First, tell me:
1. What are the testable behaviors in this class?
2. What are the dependencies that need to be mocked?
3. Are there any design issues that make testing difficult?
   (If yes, suggest a minimal refactor to improve testability)

Then generate the tests following our standard patterns.
```

## The Prompt: Test-First Development

```
I'm about to implement [feature description].

Before I write any production code, generate the test suite that defines
the expected behavior:

1. List the test cases (method names only) — I'll review before you write them
2. After I approve the list, write the full test implementations
3. The tests should compile but fail (no production code exists yet)

I'll implement the production code to make them pass.
```

## The Prompt: Mutation Testing / Test Quality Check

```
@Services/[Service].cs
@Tests/[ServiceTests].cs

Review these tests for quality. Identify:

1. Untested code paths — methods or branches with no test coverage
2. Weak assertions — tests that pass but don't actually verify behavior
   (e.g., Assert.NotNull when the value should be checked specifically)
3. Missing edge cases — inputs that could cause unexpected behavior
4. Tests that would pass even if the code was wrong (false confidence)

Generate additional tests to close any gaps you find.
```

## Tips
- AI-generated tests are one of the safer uses of AI because test failures reveal many bad assumptions quickly.
- Always run the tests after generation.
- Keep test data obviously fake for regulated or sensitive domains.
- If the assistant generates too many tests, narrow it back to behavior, risk, and edge cases.
- Use the language-specific companion files when the test stack matters.
