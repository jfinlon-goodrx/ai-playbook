# Task: Test Generation

## When to Use
You need unit tests, integration tests, or both for existing or new code. AI excels at generating comprehensive test cases — it's one of the highest-ROI uses of AI-assisted development.

## Prerequisites
- [ ] The code to test exists (or you're doing test-first development)
- [ ] Test project is set up with xUnit and Moq
- [ ] Existing tests available for pattern reference

## The Prompt: Unit Tests for a Service

```
@Services/[ServiceName].cs
@Services/I[ServiceName].cs
@Tests/[ExistingTestFile].cs (for pattern reference)

Write comprehensive unit tests for [ServiceName] using xUnit and Moq.

For each public method, generate tests for:
1. Happy path — valid input produces expected output
2. Validation failures — invalid inputs are rejected with appropriate errors
3. Not found — referenced entities don't exist
4. Authorization — user doesn't have permission (if applicable)
5. Edge cases — nulls, empty collections, boundary values, concurrent access

Follow the patterns in [ExistingTestFile] for:
- Test class structure and naming
- Mock setup conventions
- Assertion style
- Test data builders (if used)

Naming convention: MethodName_Scenario_ExpectedResult

Do not test private methods. Do not test trivial getters/setters.
Focus on business logic and error handling.
```

## Example: Testing Prescription Refill Service

```
@Services/PrescriptionRefillService.cs
@Services/IPrescriptionRefillService.cs
@Repositories/IPrescriptionRepository.cs
@Tests/Services/PrescriptionServiceTests.cs

Write unit tests for PrescriptionRefillService. This service handles
prescription refill requests in a healthcare application.

Test each public method. For SubmitRefillRequest, specifically test:

Happy path:
- Valid refill request for an active prescription with remaining refills → returns success with refill ID
- Request with no requestedQuantity → defaults to original prescription quantity

Validation:
- Null or empty pharmacyId → returns validation error
- Notes exceeding 500 characters → returns validation error
- RequestedQuantity <= 0 → returns validation error

Business rules:
- Prescription not found → returns NotFound error
- Prescription expired → returns error "Prescription has expired"
- No remaining refills → returns error "No refills remaining"
- Prescription on hold → returns error "Prescription is on hold, contact prescriber"
- Duplicate pending refill request → returns error "A refill request is already pending"

Authorization:
- Patient requesting refill for someone else's prescription → returns Forbidden
- PharmacyStaff requesting for a prescription not assigned to their pharmacy → returns Forbidden

Mock setup:
- Use Moq for IPrescriptionRepository, IAuditLogger, ICurrentUserService
- Use a builder pattern for test Prescription objects if one exists

IMPORTANT: Do not use real patient data in test fixtures. Use clearly fake data:
- Patient names: "Test Patient Alpha", "Test Patient Beta"
- Prescription IDs: use deterministic GUIDs
- Drug names: "TestDrug-100mg", "TestDrug-200mg"
```

## The Prompt: Integration Tests

```
@Tests/Integration/[ExistingIntegrationTest].cs
@Data/ApplicationDbContext.cs

Write integration tests for the [Feature] endpoints.

Use the WebApplicationFactory pattern from [ExistingIntegrationTest].
Tests should:
1. Use an in-memory database (or test database if the project uses one)
2. Seed necessary reference data before each test
3. Test the full HTTP request/response cycle
4. Verify database state after mutations
5. Verify response headers (status code, content type)
6. Test authentication/authorization (call without token, call with wrong role)

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
- AI-generated tests are one of the safest uses of AI because **the tests verify themselves** — if the test is wrong, it either fails or you catch it in review
- Always run the tests after generation — the AI sometimes references methods that don't exist or uses wrong signatures
- For healthcare code, verify that test data doesn't contain real PHI patterns (real drug NDC codes, real insurance BIN numbers, etc.)
- If the AI generates 30+ tests for a simple service, it's probably testing implementation details. Ask it to focus on behavior, not implementation
- Request test data builders early — they save massive time on future tests
