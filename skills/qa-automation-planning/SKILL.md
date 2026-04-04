---
name: qa-automation-planning
description: Builds QA plans, automation candidates, and regression guidance from stories and existing test patterns. Use when generating test plans, converting manual tests to automation, or prioritizing regression coverage.
---
# QA Automation Planning

## Quick Start

When this skill applies:

1. Read the story, acceptance criteria, and known risks.
2. Identify the smallest test strategy that still protects quality.
3. Separate what should be manual, exploratory, and automated.
4. Match the project's actual test framework and conventions.

## Workflow

### 1. Build the test model

Group scenarios into:
- happy path,
- negative,
- boundary,
- edge case,
- auth and permissions,
- regression risk.

### 2. Choose automation targets

Prefer automation for:
- repetitive checks,
- critical regressions,
- high-risk integrations,
- and stable user flows.

Do not force automation onto unstable or low-value scenarios.

### 3. Generate in project style

Before proposing automation:
- read one or more existing tests,
- match fixture and naming patterns,
- respect pipeline execution constraints,
- avoid flaky waits and weak assertions.

### 4. Report gaps clearly

Call out:
- missing requirements,
- missing data setup,
- testability problems,
- and what still needs human exploratory coverage.

## Good Output

Good output includes:
- a practical test plan,
- a prioritized automation slice,
- clear reasons for what is not automated yet,
- and CI notes when relevant.

## Related Prompts

- `../../prompts/by-task/qa/test-plan-from-story.md`
- `../../prompts/by-task/qa/playwright-and-api-automation.md`
