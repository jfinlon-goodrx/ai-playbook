# Task: Refactoring

## When to Use
You need to restructure existing code without changing its behavior. AI is excellent at mechanical refactoring across multiple files — it's faster and less error-prone than doing it manually.

## Prerequisites
- [ ] Existing tests that verify current behavior (critical — refactoring without tests is dangerous)
- [ ] Clear understanding of what you want to change and why

## The Prompt: Extract Service from Controller

```
@Controllers/[Controller].cs

This controller has business logic mixed into the action methods.
Extract all business logic into a service class:

1. Create I[Name]Service interface with methods for each action's logic
2. Create [Name]Service implementation
3. Move the business logic from the controller into the service
4. The controller should only handle HTTP concerns (model binding, status codes, response mapping)
5. Register the service in DI
6. Update existing tests (or create them if none exist)

Keep the external behavior (request/response contracts) exactly the same.
The controller actions should call the service and map the result to HTTP responses.
```

## The Prompt: Convert Sync to Async

```
@[TargetFile].cs
@[RelatedFiles].cs

Convert this class from synchronous to asynchronous:

1. All methods that call the database, HTTP clients, or file I/O should be async
2. Add Async suffix to method names (e.g., GetById → GetByIdAsync)
3. Return Task<T> instead of T
4. Add CancellationToken parameter to all async methods
5. Update the interface to match
6. Update all callers to await the async methods
7. Update tests to use async/await

Do NOT change any business logic — only the sync/async pattern.
Run through every caller in the solution to make sure nothing is missed.
```

## The Prompt: Introduce Repository Pattern

```
@Services/[Service].cs
@Data/ApplicationDbContext.cs

This service accesses the DbContext directly. Refactor to use the repository pattern:

1. Create I[Entity]Repository interface with methods matching the data access patterns
   used in the service (don't create methods that aren't needed)
2. Create [Entity]Repository implementation using the existing DbContext
3. Replace direct DbContext usage in the service with repository calls
4. Register the repository in DI
5. Update tests to mock the repository instead of the DbContext

Keep the same data access behavior — same queries, same includes, same filters.
The repository is a seam for testing, not a new abstraction layer.
```

## The Prompt: Apply Result Pattern (Replace Exceptions)

```
@Services/[Service].cs

This service uses exceptions for business logic flow control (e.g., throwing
NotFoundException, ValidationException). Refactor to use the Result<T> pattern:

1. Create a Result<T> type if one doesn't exist, or use the existing one at [path]
2. Replace thrown exceptions with Result.Failure<T>(error) returns
3. Replace successful returns with Result.Success(value)
4. Update the controller to map Result outcomes to HTTP status codes:
   - Success → 200/201
   - NotFound → 404
   - ValidationError → 400
   - Forbidden → 403
   - Failure → 500
5. Update tests to assert on Result.IsSuccess/IsFailure instead of expecting exceptions

Keep the exact same business logic — only change how errors are communicated.
```

## The Prompt: Decompose God Class

```
@[LargeFile].cs

This class has [X] methods and [Y] lines. It's doing too much.
Analyze it and propose how to split it:

1. First, list the distinct responsibilities you see (just the list, don't refactor yet)
2. For each responsibility, name the new class and its methods
3. Identify shared state or dependencies between the responsibilities

Then, after I approve the split plan:
4. Create the new classes with their interfaces
5. Move the relevant methods
6. Update the original class to delegate to the new classes
7. Update all callers
8. Update tests

Show me the plan before making changes. I want to approve the split boundaries.
```

## The Prompt: Rename Across Codebase

```
Rename [OldName] to [NewName] across the entire solution:

1. Class/interface rename
2. File rename to match
3. All references in other files
4. Namespace updates if needed
5. Test file and test class renames
6. XML doc comment references
7. Configuration references (appsettings.json, DI registration)
8. Migration references (if applicable)

List all files that will be changed before making the changes.
```

## The Prompt: Add Audit Logging to Existing Service

```
@Services/[Service].cs
@Services/IAuditLogger.cs (if exists)

Add audit logging to every method in this service that creates, updates, or deletes data.

For each mutation method:
1. Log BEFORE the operation: who is doing it, what they're attempting, timestamp
2. Log AFTER the operation: success/failure, what changed
3. Use the existing IAuditLogger interface (or create one if it doesn't exist)

HIPAA requirement:
- Log the entity ID, user ID, action type, and timestamp
- Do NOT log the entity's PHI fields (patient name, DOB, drug details)
- Do NOT log the full request/response body
- The audit log itself must be append-only (no updates or deletes)
```

## The Prompt: Safe Refactoring with Feature Flag

```
I need to refactor [component] but it's in production and I can't risk breaking it.

Create a feature-flagged refactoring:
1. Create the new implementation alongside the old one
2. Use an IFeatureFlag (or config toggle) to switch between old and new
3. Both implementations share the same interface
4. Add logging that compares old vs. new output (shadow testing)
5. The feature flag defaults to OFF (old implementation)

This lets us:
- Deploy the new code behind the flag
- Enable it in staging for testing
- Enable it in production with instant rollback (toggle flag off)
- Remove the old code after the new implementation is proven
```

## Tips
- **Never refactor without tests.** If the code has no tests, write tests first (see `../qa/test-generation.md`), then refactor.
- Ask the AI to show you the plan before executing — "list the files you'll change and what you'll change in each" before accepting changes
- For large refactors, do it in steps: "First, create the new classes. Then, move the methods. Then, update the callers." Review after each step.
- Run tests after each step to catch issues early
- For healthcare code, always include the audit logging step — regulators care about traceability
