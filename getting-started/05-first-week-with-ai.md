# First Week with AI

This guide is for teammates who are new to AI-assisted work and want a safe way to build skill and confidence.

The goal of week one is not to become an expert prompt engineer. The goal is to learn one or two workflows that are useful, reviewable, and low-risk.

## What to Aim For

By the end of the first week, you should be able to:

- give the AI enough project context to get useful output,
- use AI for one implementation task,
- use AI for one review or testing task,
- and tell the difference between a good first draft and an answer you should not trust.

## Day 1: Set Up and Observe

1. Read `03-project-context.md`.
2. Read `04-when-to-use-ai.md`.
3. Open one real file from your project and ask the AI to explain:
   - what it does,
   - what its dependencies are,
   - and what nearby files are likely related.

Do not ask it to change code yet. First, learn how it reads your project.

## Day 2: Use AI for a Safe Win

Pick one low-risk task:

- generate unit tests,
- write XML docs,
- draft a PR description,
- summarize a Jira story into implementation steps.

Good first prompt shape:

```text
Read these files first:
@[relevant file]
@[similar test or example file]
@.cursorrules

Then help me [specific task].

Match the patterns in the referenced files.
```

## Day 3: Practice Review, Not Just Generation

Take something the AI generated and review it for:

- correctness,
- missing edge cases,
- project convention mismatches,
- and hidden assumptions.

This is the habit that builds trust. Confidence comes from reviewing well, not from hoping the AI is right.

## Day 4: Use AI on a Bug or Test Problem

Choose one:

- `../prompts/by-task/qa/bug-to-regression-test.md`
- `../prompts/by-task/qa/flaky-test-triage.md`
- `../prompts/by-task/ci-cd/pipeline-failure-triage.md`

These are great confidence-builders because the outcome is observable: the bug is better understood, the test is stronger, or the pipeline gets fixed faster.

## Day 5: Save Something for Reuse

At the end of the week, save one thing:

- a prompt that worked,
- a checklist,
- a skill idea,
- or a before/after example.

This is how a team moves from isolated prompting to shared capability.

## Rules for New Users

- Start with tasks you can review confidently.
- Prefer asking the AI to explain, plan, summarize, and draft before asking it to transform many files.
- Use real repo context every time.
- Never paste secrets or sensitive production data.
- If the output feels plausible but you cannot verify it, slow down.

## A Simple Confidence Test

Before you accept AI output, ask:

1. Do I understand what it changed or suggested?
2. Do I know how I would verify it?
3. Would I be comfortable explaining this to a teammate in code review?

If the answer is no, the next step is not “trust harder.” The next step is to ask better follow-up questions or narrow the task.
