---
name: dotnet-feature-delivery
description: Plans and executes .NET feature delivery from story to vertical slice. Use when working from Jira stories, implementing API or service features, or shaping a .NET delivery plan with tests and review steps.
---
# .NET Feature Delivery

## Quick Start

When this skill applies:

1. Read the story, acceptance criteria, and any linked docs.
2. Read similar implementation files in the repo before proposing changes.
3. Build a vertical-slice plan across API, application, domain, infrastructure, tests, and docs.
4. Surface ambiguities and risks before coding.
5. Keep output aligned to existing repo patterns, not generic clean-room architecture.

## Workflow

### 1. Validate the input

Confirm:
- the business goal,
- the acceptance criteria,
- auth and security constraints,
- rollout or migration implications,
- what QA will need to verify.

If the story is weak, stop and list what needs clarification.

### 2. Plan the slice

Break work into:
- entry points and contracts,
- business logic,
- persistence or integration changes,
- tests,
- documentation and release notes.

Prefer one coherent slice over disconnected layer-by-layer output.

### 3. Execute safely

- Match existing controller, handler, service, and test patterns.
- Keep HTTP concerns at the edge.
- Add validation and failure handling explicitly.
- Flag any migration, background job, config, or environment change.

### 4. Verify

Check:
- build impact,
- test coverage impact,
- review order,
- security or compliance concerns,
- operational follow-up items.

## Good Output

Good output includes:
- a clear change plan,
- likely files to touch,
- open questions,
- test expectations,
- and review guidance.

## Related Prompts

- `../../prompts/by-task/dotnet/story-to-vertical-slice.md`
- `../../prompts/by-task/qa/test-plan-from-story.md`
