# Task: New API Endpoint

## When to Use

You need to add or extend an HTTP API endpoint in an existing service and want the assistant to match the repo's current architecture, validation, security, and testing patterns.

## Use With

- `.dotnet`: `new-api-endpoint.dotnet.md`
- `.python`: `new-api-endpoint.python.md`
- `.go`: `new-api-endpoint.go.md`

## Prerequisites

- the project has a clear routing pattern
- at least one existing endpoint can be referenced
- you know the request, response, authorization, and error expectations

## Prompt: Plan the Endpoint

```text
Read the referenced implementation files first, then plan a new API endpoint.

Repo context:
@[existing endpoint or handler]
@[service or business-logic file]
@[existing test file]
@[project rules or context file]

Endpoint spec:
- Method and route: [GET/POST/etc] [route]
- Purpose: [business outcome]
- Request shape: [body, params, query]
- Response shape: [payload and status codes]
- Authorization: [who can call it]
- Constraints: [validation, compliance, performance, rollout]

Produce:
1. The files likely to change
2. Any new files needed
3. The request/response contract
4. Validation and authorization rules
5. Risks, assumptions, and open questions
6. Test cases to add before implementation

Match the existing repo patterns instead of inventing a new architecture.
```

## Prompt: Build the Endpoint

```text
Using the approved plan, implement the endpoint.

Requirements:
- keep transport concerns separate from business logic
- follow the repo's existing validation, logging, and error-handling conventions
- enforce authorization at the correct boundary
- include tests for happy path, invalid input, not found, and forbidden scenarios where applicable
- note any migration, config, or rollout implications

Do not expose sensitive data in logs or error messages.
```

## Review Checklist

- Does the endpoint match existing route and response conventions?
- Is validation explicit and close to the input boundary?
- Is authorization enforced in a way that matches the domain?
- Are tests focused on behavior rather than implementation details?
- Are rollout and observability needs called out when the change is user-facing or operationally meaningful?
