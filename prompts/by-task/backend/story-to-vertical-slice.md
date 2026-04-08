# Task: Story to Vertical Slice

## When to Use

You have a story, acceptance criteria, and a real codebase. You want the assistant to turn that into a coherent implementation plan or an end-to-end slice instead of generating disconnected code fragments.

## Use With

- `.dotnet`: `story-to-vertical-slice.dotnet.md`
- `.python`: `story-to-vertical-slice.python.md`
- `.go`: `story-to-vertical-slice.go.md`

## Prompt: Story to Delivery Plan

```text
Read this story and the referenced repo context, then build a vertical-slice delivery plan.

Story:
- Title: [story title]
- User problem: [problem statement]
- Business outcome: [expected value]
- Acceptance criteria: [paste criteria]
- Constraints: [security, compliance, rollout, deadlines]
- Risk tier: [R1, R2, R3]

Repo context:
@[similar endpoint or workflow]
@[similar service or domain logic]
@[similar test file]
@[project context or rules file]

Produce:
1. A delivery plan grouped by architectural layer or workflow step
2. Files likely to change
3. New files likely to be needed
4. Risks, assumptions, and open questions
5. Test strategy and rollout notes
6. Recommended review order for the final PR

Optimize for a coherent, reviewable change that matches the repo's actual conventions.
```

## Prompt: Build the Slice

```text
Using the approved plan, implement this feature as one coherent vertical slice.

Requirements:
- match the repo's existing architecture
- keep cross-layer changes aligned to the same business outcome
- add validation, authorization, and tests where appropriate
- call out migration, configuration, deployment, analytics, or documentation implications
- avoid speculative abstractions that the repo does not already use
```

## Good Outcome

A strong result should leave the team with:

- one clear feature path
- one reviewable PR shape
- one aligned test strategy
- one documented rollout approach
