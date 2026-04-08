# Task: Story to Python Vertical Slice

## Prompt: Story to Delivery Plan

```text
Read this story and our repo context, then build a delivery plan for a Python feature.

Story:
- Title: [story title]
- User problem: [problem statement]
- Business outcome: [expected value]
- Acceptance criteria: [paste criteria]
- Constraints: [security, compliance, rollout, deadlines]
- Risk Tier: [R1, R2, R3]

Repo context:
@[similar endpoint, router, or view]
@[similar service or domain logic]
@[similar test file]
@[project context or rules file]

Produce:
1. A vertical-slice implementation plan grouped by API / Application / Domain / Data / Tests / Docs
2. Files likely to change
3. New files likely to be needed
4. Risks, assumptions, and open questions
5. Suggested review order

Assume a maintainable Python service. Prefer explicit validation, clear module boundaries, and good tests over shortcut code generation.
```

## Prompt: Build the Slice

```text
Using the approved plan, implement this feature as a complete Python vertical slice.

Requirements:
- keep API transport logic separate from business logic
- match the repo's framework and project layout
- add validation, authorization, and error handling
- add or update pytest coverage
- note migration, settings, deployment, analytics, or documentation implications

Do not restructure the application unless the story requires it.
```
