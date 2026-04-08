# Task: Test Generation for .NET

## Prompt: Unit Tests for a Service

```text
@Services/[ServiceName].cs
@Services/I[ServiceName].cs
@Tests/[ExistingTestFile].cs

Write comprehensive unit tests for [ServiceName] using xUnit and Moq.

For each public method, generate tests for:
1. happy path
2. validation failures
3. not found
4. authorization if applicable
5. boundary and edge cases

Follow the referenced file for:
- naming
- mock setup
- assertion style
- builders or fixture patterns

Naming convention: MethodName_Scenario_ExpectedResult

Do not test trivial getters or private methods.
Focus on business behavior and error handling.
```

## Prompt: Integration Tests

```text
@Tests/Integration/[ExistingIntegrationTest].cs
@Data/[DbContextFile].cs

Write integration tests for the [Feature] endpoints.

Use the WebApplicationFactory pattern from the referenced test.
Cover:
- happy path
- 401 without auth
- 403 with wrong role
- 400 invalid input
- 404 missing resource
- resulting database state after mutations
```
