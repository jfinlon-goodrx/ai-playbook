# Task: New API Endpoint for .NET

## Best Fit

Use this with ASP.NET Core APIs that follow controller, minimal API, service, or handler-based patterns.

## Prompt

```text
@Controllers/[ExistingController].cs
@Services/I[ExistingService].cs
@Program.cs

Add a new API endpoint with the following specification:

Endpoint:
- Method and route: [HTTP Method] /api/[resource]/[action]
- Purpose: [what this does in business terms]
- Request body: [describe the expected input, or "none" for GET]
- Response: [describe the expected output]
- Authorization: [anonymous, authenticated, or specific roles/policies]

Requirements:
1. Add to the correct controller or minimal API module
2. Create request and response DTOs
3. Add validation using FluentValidation or the repo's existing pattern
4. Add or update the service or handler layer
5. Register dependencies in DI
6. Match the repo's error handling, logging, and response conventions
7. Add unit tests using xUnit and Moq
8. Add integration tests if the repo already uses WebApplicationFactory or endpoint integration tests

Follow the exact same patterns as the referenced files for:
- return types
- authorization
- logging
- response wrapping
- result/error handling

Do not invent a new architectural pattern if the repo already has one.
```

## Notes

- Use `CancellationToken` on async methods where the repo expects it.
- Keep controllers or minimal API handlers thin.
- Use ProblemDetails or the repo's existing error envelope if present.
- If PHI or regulated data is involved, avoid logging payload details.
