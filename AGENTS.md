# AGENTS.md

## Purpose

This repository is a shared playbook for AI-assisted software delivery.

It should help an Agile SDLC team get productive with AI across the full delivery lifecycle:

- developers
- QA and test automation engineers
- tech leads and architects
- engineering managers
- product and delivery partners
- DevOps and platform engineers

The repo started with a strong .NET/Azure bias. Going forward, it must remain useful to:

- .NET teams
- Python teams
- Go teams

When content is stack-specific and still valuable, keep it. When a concept applies across stacks, write a cross-platform canonical version first and add language-specific variants only where the implementation details truly differ.

## What Good Content Looks Like

Prefer content that is:

- practical
- reusable
- reviewable
- grounded in real delivery work
- clear about risks, guardrails, and verification

Favor:

- prompts tied to real tasks
- skills that encode repeatable workflows
- guides that reduce onboarding time
- examples that show how to adapt a pattern across languages
- role-based material that helps an Agile team work together better

Avoid:

- generic AI hype
- duplicate guidance with slightly different wording
- stack-specific advice presented as if it is universal
- examples with no clear audience or outcome

## Repository Intent

This repo should become a clean handbook plus asset library with three layers:

1. Core concepts and team operating model
2. Reusable workflows, prompts, and examples
3. Language and platform adaptations

The repo is not just a prompt dump. It is a working reference system for AI-assisted delivery.

## Organization Rules

Use these rules when adding or revising content:

1. Every folder should have a clear reason to exist.
2. Every folder that holds more than a few files should have a `README.md`.
3. Each document should have one primary audience and one primary purpose.
4. Cross-link instead of duplicating when the same guidance already exists elsewhere.
5. Prefer one canonical concept page plus small language-specific companion files over three large duplicated documents.

## Language Coverage Rules

When a document includes implementation examples, decide which of these applies:

### Cross-platform guidance only

Use one file when the guidance is mostly language-agnostic:

- prompting patterns
- review habits
- planning workflows
- Agile operating model changes
- testing strategy principles

### Cross-platform guidance with language variants

Use one canonical page plus language-specific companion files when the workflow is shared but code differs.

Recommended naming pattern:

- `topic.md` for the canonical workflow or explanation
- `topic.dotnet.md` for .NET examples
- `topic.python.md` for Python examples
- `topic.go.md` for Go examples

Examples:

- `new-api-endpoint.md`
- `new-api-endpoint.dotnet.md`
- `new-api-endpoint.python.md`
- `new-api-endpoint.go.md`

### Language-specific only

Use a language-only file when the content is genuinely tied to a stack and not worth forcing into a shared template.

Examples:

- Entity Framework migration guidance
- FastAPI dependency injection examples
- Go project layout or interface-focused service wiring

In those cases, make the language explicit in the filename and link to sibling language content where it exists.

## Preferred Expansion Strategy

When converting existing .NET-heavy assets:

1. Identify the shared workflow underneath the current file.
2. Extract the shared workflow into a neutral canonical page.
3. Move .NET details into a `.dotnet.md` companion where needed.
4. Add equivalent `.python.md` and `.go.md` variants for the same workflow.
5. Add a small comparison table if it helps readers pick the right variant quickly.

Do not create Python and Go files by mechanically translating C# examples. Adapt the workflow to idiomatic tooling and conventions for each ecosystem.

## Audience Model

Assume readers may enter the repo from different angles:

- "I am new and need a safe starting point."
- "I need a prompt or workflow for today."
- "I need to standardize team practices."
- "I need language-specific examples for my stack."
- "I need to explain AI-native delivery to leadership."

New content should support one of those journeys clearly.

## Contribution Standards

Before adding a new file, ask:

- Is there already a better home for this?
- Is this a concept, a workflow, an example, or a reference asset?
- Is this cross-platform or language-specific?
- Who will use it and at what point in their journey?
- What should this link to?

If you add stack-specific examples, include:

- the target language in the filename
- the expected frameworks or tools
- verification guidance
- notes about what is portable versus stack-bound

## Current Structural Direction

The long-term target structure should distinguish between:

- onboarding and learning
- team operating model and Agile guidance
- prompts and reusable workflows
- language and tool variants
- case studies and examples

Contributors should preserve existing material when useful, but organize new work toward that clearer model instead of adding more parallel categories.

## Maintenance Expectations

When editing the repo:

- tighten naming
- remove avoidable duplication
- add missing README navigation pages
- add cross-links
- note when content is outdated or too vendor-specific
- prefer incremental cleanup over large unreviewable rewrites

If a major reorganization is proposed, document the mapping from old paths to new paths so links and reader journeys remain understandable during the transition.
