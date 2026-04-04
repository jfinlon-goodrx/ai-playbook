---
name: newbie-ai-pairing
description: Guides newer team members through safe, confidence-building AI usage. Use when onboarding developers, QA, or leads who are new to LLM-driven tools and need low-risk workflows, review habits, and reuse patterns.
---
# Newbie AI Pairing

## Quick Start

When this skill applies:

1. Start with a task the user can review confidently.
2. Prefer explanation, planning, testing, or documentation before large code generation.
3. Use explicit repo context and one or two example files.
4. Build confidence by choosing observable outcomes.

## Workflow

### 1. Pick a safe first task

Good first tasks:
- explain unfamiliar code,
- generate unit tests,
- draft a PR description,
- summarize a Jira story,
- turn a bug into a regression test.

Avoid high-blast-radius tasks until the user has stronger review habits.

### 2. Teach the prompt shape

Encourage this pattern:
- reference relevant files,
- state the desired artifact,
- state project conventions,
- state what success looks like.

### 3. Require review and verification

After generation, have the user check:
- correctness,
- convention match,
- missing edge cases,
- and how to verify the result.

### 4. Capture what worked

At the end of a good session, save:
- the prompt,
- a checklist,
- a useful example,
- or a candidate reusable skill.

## Good Output

Good output helps the user feel:
- oriented,
- in control,
- and able to explain what the AI produced.

It should lower anxiety, not increase blind dependence.

## Related Guides

- `../../getting-started/05-first-week-with-ai.md`
- `../../getting-started/03-project-context.md`
- `../../getting-started/04-when-to-use-ai.md`
