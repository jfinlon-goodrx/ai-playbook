# AI-Assisted Engineering Starter Kit

This is the fastest practical path for a technical team that wants useful AI adoption without a lot of ceremony.

Assume a team centered on dotnet, Azure, APIs, delivery quality, and enterprise guardrails.

## Week 1 Outcome

By the end of the first week, the team should have:

- one sanctioned AI coding tool in active use,
- one shared project-context pattern,
- one shared prompt/pattern library,
- one review or testing workflow that uses AI safely,
- one visible example of real work done faster.

## Step 1: Pick One Tool And Standardize The First Experience

Start with one of these:

- Cursor for deep IDE-integrated work,
- Claude Code for terminal-heavy or agentic workflows,
- Copilot if the team already has licenses and wants lower friction.

Do not begin with “everyone may choose anything.” Early adoption goes better when the team can compare notes on one shared workflow.

Read next:

- `../getting-started/01-installing-cursor.md`
- `../guides-by-platform/Coding Tools/Cursor-Guide.md`
- `../guides-by-platform/Coding Tools/Claude-Code-Guide.md`

## Step 2: Add Project Context Before Chasing Magic

The biggest quality multiplier is not a clever prompt. It is usable context.

Create:

- a project rule file,
- a short architecture/context document,
- examples of the patterns the AI should follow,
- a small list of high-risk constraints.

Read next:

- `../getting-started/03-project-context.md`
- `../developer-workflow/Building Skills Into Your Dev Environment.md`
- `../cursorrules/`

## Step 3: Adopt Two Core Prompt Patterns

Start with:

### Vertical Slice

Use when building a feature across multiple layers.

Source:

- `../prompts/patterns/vertical-slice.md`

### Iterative Refinement

Use when the AI output is mostly right and needs precise correction rather than a restart.

Source:

- `../prompts/patterns/iterative-refinement.md`

## Step 4: Add One Guardrail Workflow Immediately

Choose one:

- AI-generated PR summaries,
- AI pre-review for security and quality,
- AI-assisted test generation with human verification.

Recommended sources:

- `../prompts/by-task/review/pr-description.md`
- `../prompts/by-task/qa/test-generation.md`
- `../prompts/by-task/review/security-audit.md`
- `../prompts/patterns/ai-testing-and-review-pipeline.md`

## Step 5: Teach The Team What Not To Do

Make these norms explicit:

- never paste secrets or regulated data,
- never commit code you cannot explain,
- never assume generated tests are meaningful,
- never trust AI to infer your security model,
- never skip project context.

Read next:

- `../anti-patterns/common-mistakes.md`
- `../anti-patterns/hallucination-recovery.md`

## Step 6: Produce One Internal Demo

The best adoption artifact is not a slide deck. It is a real example:

- the task,
- the prompts or workflow used,
- what the AI got right,
- what needed correction,
- time saved,
- and what guardrails made it safe.

This becomes the seed of your internal playbook and the first case study for wider adoption.

## What Success Looks Like

By the end of the first month:

- a skeptical but curious engineer can copy a known-good workflow,
- project context exists in reusable form,
- review and testing have adapted at least a little,
- the team has examples of both wins and failure modes,
- AI use feels like an engineering practice, not a novelty.
