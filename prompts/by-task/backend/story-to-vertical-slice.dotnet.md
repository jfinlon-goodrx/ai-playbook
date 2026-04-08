# Task: Story to .NET Vertical Slice

## Prompt: Story to Delivery Plan

```text
Read this Jira story and our repo context, then build a delivery plan for a .NET feature.

Story:
- Title: [story title]
- Description: [story description]
- Acceptance Criteria: [paste criteria]
- Constraints: [security, compliance, rollout, deadlines]
- Risk Tier: [R1, R2, R3]

Repo context:
@[similar controller or endpoint]
@[similar service or handler]
@[similar test file]
@.cursorrules

Produce:
1. A vertical-slice implementation plan grouped by API / Application / Domain / Infrastructure / Tests / Docs
2. The files likely to change
3. New files likely to be needed
4. Risks, assumptions, and open questions
5. Recommended review order for the final PR

Assume a maintainable .NET codebase. Prefer explicit validation, meaningful tests, and matching existing patterns over shortcut scaffolding.
```

## Prompt: Build the Slice

```text
Using the approved plan, implement this feature as a complete .NET vertical slice.

Requirements:
- keep controllers thin and business logic out of HTTP handlers
- add validation, authorization, and error handling
- add or update unit tests and integration tests where appropriate
- note any migration, configuration, or deployment implications

Do not invent architecture that conflicts with the repo. Match the existing project conventions first.
```

## Tip

Pair this with `../qa/test-plan-from-story.md` so implementation and testing stay aligned.
