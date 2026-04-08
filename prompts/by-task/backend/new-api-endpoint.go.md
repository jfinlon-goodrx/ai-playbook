# Task: New API Endpoint for Go

## Best Fit

Use this with `net/http`, Chi, Gin, Echo, or a similar Go HTTP service.

## Prompt

```text
@[existing handler file]
@[service or use-case file]
@[existing test file]
@[project context or rules file]

Add a new API endpoint with the following specification:

Endpoint:
- Method and route: [HTTP Method] [route]
- Purpose: [business outcome]
- Request body: [payload or "none"]
- Response: [payload and status codes]
- Authorization: [anonymous, authenticated, role or policy rules]

Requirements:
1. Add the route in the correct router setup
2. Add or update the handler, request/response structs, and service layer
3. Keep HTTP concerns in the handler and business logic in services or use cases
4. Validate inputs explicitly
5. Match the repo's logging and error-response pattern
6. Add tests using the repo's current style
   (table-driven tests, `httptest`, testify, or standard library assertions)

Tests should cover:
- happy path
- malformed input
- not found
- forbidden or unauthorized when applicable
- one meaningful business-rule edge case

Prefer explicit, idiomatic Go over framework-heavy abstractions.
```

## Notes

- Use table-driven tests when the repo already favors them.
- Keep JSON encoding and HTTP status handling easy to read in handlers.
- Avoid leaking internal errors directly to clients.
