# Task: Story to Go Vertical Slice

## Prompt: Story to Delivery Plan

```text
Read this story and our repo context, then build a delivery plan for a Go feature.

Story:
- Title: [story title]
- User problem: [problem statement]
- Business outcome: [expected value]
- Acceptance criteria: [paste criteria]
- Constraints: [security, compliance, rollout, deadlines]
- Risk Tier: [R1, R2, R3]

Repo context:
@[similar handler or route]
@[similar service or use-case file]
@[similar test file]
@[project context or rules file]

Produce:
1. A vertical-slice implementation plan grouped by Handler / Service / Data / Tests / Docs / Ops
2. Files likely to change
3. New files likely to be needed
4. Risks, assumptions, and open questions
5. Suggested review order

Assume a maintainable Go service. Prefer explicit flows, readable boundaries, and focused tests.
```

## Prompt: Build the Slice

```text
Using the approved plan, implement this feature as a complete Go vertical slice.

Requirements:
- keep HTTP concerns in handlers and business rules in services or use cases
- match the repo's current router, package layout, and dependency wiring
- add validation, authorization, and tests
- call out schema, config, deployment, and observability implications

Prefer idiomatic, explicit Go over heavy abstractions.
```
