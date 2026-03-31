# Pattern: Iterative Refinement — Getting from 80% to 100%

## When to Use
The AI generated code that's mostly right but needs adjustments. Instead of starting over, you refine incrementally. This is the normal workflow — expecting perfect output on the first prompt is unrealistic. The skill is in fast, effective iteration.

## The Anti-Pattern (What NOT to Do)

Don't do this:
```
That's wrong. Try again.
```

This throws away the AI's context and often produces worse output the second time. The AI doesn't know what was wrong or what you want differently.

## The Pattern

### Targeted Corrections

Be specific about what's wrong and what you want instead:

```
The code is mostly good. Fix these specific issues:

1. In TransferService.cs line 45: change .Single() to .FirstOrDefault()
   and handle the null case by returning Result.NotFound()

2. In TransferController.cs: the POST endpoint should return 201 Created
   with a Location header, not 200 OK

3. In TransferServiceTests.cs: the mock setup for IPharmacyRepository
   should return null for the "pharmacy not found" test case, not throw

Don't change anything else.
```

### Incremental Addition

Add features one at a time, not all at once:

```
The basic endpoint works. Now add:
1. Input validation using FluentValidation
2. Validation error responses as ProblemDetails with field-level errors

Don't change the existing controller or service logic — only add validation.
```

Then:

```
Good. Now add caching:
1. Cache the GET endpoint response for 5 minutes
2. Invalidate the cache when any mutation endpoint is called
3. Use IMemoryCache (or IDistributedCache if the project uses one)

Don't change validation or business logic — only add caching.
```

### Style and Convention Corrections

```
The logic is correct but the code doesn't match our conventions:

1. We use file-scoped namespaces, not block-scoped
2. Private fields should have underscore prefix (_logger not logger)
3. Use var for obviously-typed assignments (var service = new Service())
4. XML doc comments use present tense ("Gets" not "Get")
5. Async methods must have Async suffix and accept CancellationToken

Apply these conventions throughout the files you generated.
Don't change any logic.
```

### Error and Build Fix Loop

When the AI's code doesn't compile:

```
The build failed with these errors:

CS1061: 'ITransferService' does not contain a definition for 'ApproveTransferAsync'
CS0246: The type or namespace 'TransferResponseDto' could not be found

Fix both. The likely cause:
1. The interface is missing the new method — add it
2. The DTO class is probably missing — check if you created it

Show me only the fixes, don't regenerate entire files.
```

### Test Failure Loop

When tests fail:

```
Two tests are failing:

1. ApproveTransfer_ValidRequest_ReturnsSuccess
   Expected: Result.IsSuccess = true
   Actual: Result.IsSuccess = false, Error = "Prescription not found"

   The mock for IPrescriptionRepository.GetByIdAsync isn't returning
   the test prescription. Check the mock setup.

2. DenyTransfer_AlreadyDenied_ReturnsError
   Expected: "Transfer has already been processed"
   Actual: "Transfer not found"

   The test is setting up a transfer with Status = "Denied" but the
   service is filtering for Status = "Requested" only. The test is
   correct — fix the service to check for any non-terminal status.

Fix both issues. Run through the other tests mentally to make sure
the fixes don't break them.
```

### "Almost Right" Pattern

When the code is 90% right but the approach needs adjustment:

```
This is close but I want a different approach for the authorization check.

Current: You're checking the user's pharmacy in the controller with an if statement.
Wanted: Create an authorization handler using ASP.NET Core's policy-based authorization.

Specifically:
1. Create a TransferAuthorizationHandler that implements IAuthorizationHandler
2. It should verify the user's PharmacyId claim matches the transfer's FromPharmacyId or ToPharmacyId
3. Register it as a policy called "TransferParticipant"
4. Use [Authorize(Policy = "TransferParticipant")] on the controller actions
5. Remove the manual if-check from the controller

Keep everything else the same — just change the authorization approach.
```

## The Refinement Conversation Flow

A typical session looks like:

```
Prompt 1: "Build the feature" → AI generates 80% correct code
Prompt 2: "Fix these 3 issues" → Code is 95% correct
Prompt 3: "Build fails, fix these errors" → Code compiles
Prompt 4: "These 2 tests fail, here's why" → Tests pass
Prompt 5: "Add audit logging" → Feature complete
Prompt 6: "Apply our conventions to all files" → Code review ready
```

Six prompts, 30 minutes, feature done. Each prompt builds on the previous context.

## Tips
- **Be specific.** "Fix the error handling" is bad. "In line 45, return Result.NotFound() instead of throwing NotFoundException" is good.
- **One concern per iteration.** Don't say "fix the bugs AND add caching AND update the docs." Do them sequentially so you can verify each step.
- **Don't start over** unless the approach is fundamentally wrong. If the AI chose the right architecture but got implementation details wrong, refine — don't regenerate.
- **Tell the AI what NOT to change.** "Fix the service but don't touch the controller or tests" prevents unnecessary churn.
- **Trust the build.** If `dotnet build` and `dotnet test` pass, the mechanical errors are gone. Focus your review on logic, security, and architecture.
