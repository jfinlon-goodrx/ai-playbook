# Task: New API Endpoint for Python

## Best Fit

Use this with FastAPI, Django REST Framework, Flask with blueprints, or a similar Python service stack.

## Prompt

```text
@[existing router, view, or endpoint file]
@[service or domain file]
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
1. Add the endpoint in the correct router/view module
2. Define request and response schemas using the repo's existing pattern
   (Pydantic, DRF serializers, dataclasses, or Marshmallow)
3. Keep transport concerns separate from business logic
4. Add validation at the API boundary
5. Add or update the service layer if the repo uses one
6. Match the repo's logging, exception handling, and response conventions
7. Add tests using pytest and the repo's current test style

For tests, include:
- happy path
- invalid input
- not found
- forbidden or unauthorized when applicable
- one meaningful edge case

Prefer the repo's existing framework conventions over generic examples.
```

## Notes

- FastAPI projects should usually use Pydantic models and dependency injection patterns already present in the repo.
- Django projects should usually keep business rules out of views and serializers when the repo already has service or domain modules.
- Avoid returning raw exception details in responses.
