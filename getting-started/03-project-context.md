# Project Context: .cursorrules and Architecture Docs

The single biggest difference between "AI that writes generic code" and "AI that writes code that fits our project" is **context**. This guide explains how to give the AI the context it needs.

## Why Context Matters

Without context, the AI:
- Uses its own conventions instead of yours
- Generates Entity Framework code when you use Dapper
- Returns `ActionResult<T>` when your team uses `IActionResult`
- Ignores your error handling patterns
- Doesn't know about HIPAA compliance requirements
- Misses security patterns that are mandatory in your codebase

With context, the AI matches your existing code as if a senior developer on your team wrote it.

## The .cursorrules File

This is a plain text file at the root of your repository that Cursor reads automatically. Every prompt you write is silently prefixed with the contents of this file.

### What to Put In It

```
# Project: [Name]

## Tech Stack
- .NET 10 / C# / ASP.NET Core Minimal APIs (or MVC — specify which)
- Entity Framework Core with MSSQL (or Dapper — specify which)
- xUnit + Moq for testing
- Azure DevOps for CI/CD
- GitHub for source control

## Architecture
- Clean Architecture: API → Application → Domain → Infrastructure
- CQRS with MediatR
- Repository pattern for data access
- Result<T> pattern for error handling (not exceptions for business logic)

## Conventions
- File-scoped namespaces
- Nullable reference types enabled
- Async/await everywhere — never .Result or .Wait()
- CancellationToken on all async methods
- XML doc comments on all public APIs
- Private fields with underscore prefix (_logger, _repository)

## Security & Compliance
- HIPAA compliant — never log PHI/PII (patient names, DOBs, SSNs, prescription data)
- OWASP Top 10 aware — validate all inputs, parameterize all queries, no SQL concatenation
- All endpoints require authentication unless explicitly marked [AllowAnonymous]
- Audit logging on all data mutations (who changed what, when)
- Data at rest encryption for PHI columns
- PHI must never appear in error messages, logs, or API responses beyond what's necessary

## Error Handling
- Use Result<T> for business logic outcomes
- Use middleware for unhandled exceptions
- Return ProblemDetails for HTTP errors
- Log errors with structured logging (Serilog) — NEVER log PHI fields

## Testing
- Unit tests for all service/handler logic
- Integration tests for database operations
- Name tests: MethodName_Scenario_ExpectedResult
- Use builder patterns for test data

## Project-Specific Notes
[Add anything unique to this specific project]
```

### Where to Get One

The `cursorrules/` folder in this playbook has ready-made templates:
- `dotnet-api.cursorrules` — ASP.NET Core API projects
- `dotnet-fullstack.cursorrules` — API + Blazor/React frontend
- `healthcare-api.cursorrules` — HIPAA-compliant healthcare API (prescription processing, PHI handling)

Copy the closest match and customize for your project.

## Beyond .cursorrules: Architecture Decision Records

For complex projects, reference your architecture docs directly in prompts:

```
Read docs/ARCHITECTURE.md and docs/DATA_MODEL.md before making any changes.

Now add a new endpoint for prescription refill requests following the patterns
described in those documents.
```

If your project doesn't have architecture docs, that's your first task. Ask the AI:

```
Read every file in this solution. Analyze the architecture, patterns, and conventions.
Write an ARCHITECTURE.md that describes:
1. Project structure and layer responsibilities
2. Data access patterns
3. Authentication and authorization approach
4. Error handling conventions
5. Testing patterns
6. HIPAA compliance patterns (PHI handling, audit logging)

Make it accurate to what the code actually does, not aspirational.
```

This is one of the highest-value tasks you can give the AI: documenting existing code so that future AI sessions (and future developers) understand the project.

## The Context Hierarchy

Cursor uses context from multiple sources, in this priority order:

1. **Your prompt** — what you type in Composer or Chat
2. **.cursorrules** — automatically included in every prompt
3. **Open files** — the AI can see files you have open in tabs
4. **Codebase index** — if enabled, the AI can search your entire project
5. **@ references** — you can explicitly reference files with `@filename`

### Pro tip: @ references

In Composer, type `@` to reference specific files:

```
@Controllers/PrescriptionController.cs
@Services/IPrescriptionService.cs
@Models/PrescriptionRefill.cs

Add a new endpoint to PrescriptionController for canceling a refill request.
Follow the same patterns used in the existing SubmitRefill endpoint.
Ensure the audit log captures the cancellation reason and the user who canceled.
```

This is more targeted than letting the AI search the whole codebase. When you know which files matter, reference them explicitly.
