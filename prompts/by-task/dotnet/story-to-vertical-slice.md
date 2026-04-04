# Task: Story to .NET Vertical Slice

## When to Use
You have a Jira story, acceptance criteria, and a .NET codebase, and you want the AI to turn that into a concrete implementation plan or a complete vertical slice.

## The Prompt: Story to Delivery Plan

```text
Read this Jira story and our repo context, then build a delivery plan for a .NET feature.

Story:
- Title: [story title]
- Description: [story description]
- Acceptance Criteria: [paste criteria]
- Constraints: [security, compliance, rollout, deadlines]

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

Assume a .NET and Azure team. Prefer maintainable architecture, explicit validation, and meaningful tests over shortcut code generation.
```

## The Prompt: Build the Slice

```text
Using the approved plan, implement this feature as a complete .NET vertical slice.

Requirements:
- Follow the patterns in the referenced files
- Keep controllers thin and business logic out of HTTP handlers
- Add validation, authorization, and error handling
- Add or update unit tests and integration tests where appropriate
- Note any migration, configuration, or deployment implications

Do not invent architecture that conflicts with the repo. Match the existing project conventions first.
```

## Tips

- Use this after the story is clear. If the acceptance criteria are weak, refine the story first.
- Pair this with `../qa/test-plan-from-story.md` so implementation and testing stay aligned.
