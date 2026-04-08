# Repo Reorganization and Expansion Plan

This document captures the strategic structure direction for the repo.

For the implementation-oriented version that an AI agent can execute step by step, use [REPO-ENHANCEMENT-PLAN.md](/Users/jfinlon/repos/ai-playbook/REPO-ENHANCEMENT-PLAN.md).

## Why This Plan Exists

This repository already contains strong material, but the current structure reflects how content was accumulated rather than how a mixed Agile team will discover and use it.

Right now the repo mixes:

- audience-based groupings
- task-based groupings
- maturity-based learning tracks
- tool-specific guidance
- stack-specific examples

That is workable for a seed library, but it will get harder to maintain as Python and Go content are added unless we define a cleaner information architecture first.

## Current State Assessment

### What is already working

- The repo has useful content clusters with readable `README.md` entry points.
- The practical delivery focus is strong.
- The material already spans strategy, onboarding, prompts, workflows, and operational guardrails.
- The existing `.cursorrules` assets show a good pattern for stack-specific guidance.

### Current friction points

- The root `README.md` still describes the repo primarily as a .NET/C#/MSSQL team resource.
- The folder model mixes several organizing principles at once.
- Prompt coverage is uneven across languages.
- Some sections are role-oriented, others are tool-oriented, and others are concept-oriented.
- There is no repo-level agent/contributor guidance to keep future additions consistent.
- There is not yet a durable convention for cross-language companion files.

### Important repo reality

- There is currently no `.cursor/rules` folder in this checkout.
- There is currently no `CLAUDE.md` in this checkout.
- The closest equivalent source material is the `cursorrules/` directory plus the existing repo readmes and handbook content.

## Target Outcome

The repo should become a clean, cross-platform AI engineering playbook that supports:

- onboarding new team members
- repeatable AI-assisted delivery workflows
- language-specific implementation examples for .NET, Python, and Go
- team-level Agile operating model change
- role-based enablement across the full SDLC

## Proposed Information Architecture

Keep the current content, but gradually reorganize toward these stable top-level purposes:

### 1. `handbook/`

Use for canonical concepts, team operating model, guardrails, and reader journeys.

Examples:

- AI-native delivery model
- evaluation, safety, quality, observability
- content map
- repo organization plan

### 2. `learning-paths/` or keep `0-to-60-ai/`

Use for sequenced education and role-based onboarding.

Recommendation:

- keep `0-to-60-ai/` for now to avoid churn
- later rename only if the team wants a more obvious path-based label

### 3. `quick-starts/`

Use for fast entry points by role or need.

Examples:

- first day with AI
- first team rollout week
- manager starter kit
- QA starter kit

### 4. `workflows/`

This is the biggest structural improvement to make next.

Use it to unify:

- task prompts
- repeatable skills
- reusable delivery patterns

Recommended substructure:

- `workflows/prompts/`
- `workflows/skills/`
- `workflows/patterns/`

For now, the current `prompts/` and `skills/` folders can remain in place while content is normalized and mapped.

### 5. `platforms/`

Use for tool and environment guidance.

Recommended substructure:

- `platforms/editors/`
- `platforms/rules/`
- `platforms/private-ai/`

This would eventually absorb:

- `guides-by-platform/`
- `cursorrules/`
- selected `local-private-ai/` content

### 6. `examples/`

Use for case studies, worked examples, and language variants.

Recommended substructure:

- `examples/case-studies/`
- `examples/by-language/dotnet/`
- `examples/by-language/python/`
- `examples/by-language/go/`

### 7. `anti-patterns/`

Keep as a standalone section.

It is already clear and useful.

## Language Strategy

The repo should adopt a simple rule:

Write once at the workflow level, specialize only when needed at the implementation level.

### Naming convention

For shared workflow topics with language-specific examples:

- `topic.md`
- `topic.dotnet.md`
- `topic.python.md`
- `topic.go.md`

This is better than splitting every topic into three separate folders because:

- related variants stay discoverable together
- canonical guidance is easier to maintain
- future languages can be added without restructuring folders again

### When to create all three variants

Create `.dotnet`, `.python`, and `.go` companions when a file includes:

- code generation examples
- API patterns
- persistence patterns
- testing examples
- CI/CD examples tied to ecosystem tooling

### When not to create all three variants

Do not force language copies for:

- Agile operating model content
- prompt hygiene
- hallucination recovery
- review discipline
- AI rollout strategy
- tool-comparison guidance unless the tooling differs materially by stack

## Content Refactoring Rules

Use these rules as the repo is cleaned up:

1. Keep a canonical page for the shared idea.
2. Move stack-specific detail into companion files.
3. Add a short "Who this is for" section near the top of each file.
4. Add a short "See also" section so related assets form a path.
5. Avoid maintaining duplicate versions of the same guidance in multiple folders.

## Recommended Phases

## Phase 1: Foundation and Navigation

Goal: make the repo safer to extend without breaking the current reader experience.

Tasks:

- add a root `AGENTS.md`
- update the root `README.md` to describe the repo as cross-platform, not .NET-only
- add a short language-support policy to the root `README.md`
- add a repo map that explains content by audience and by purpose
- identify duplicate or near-duplicate docs and mark canonical sources

Deliverables:

- repo-level contribution rules
- clearer top-level narrative
- language expansion policy

## Phase 2: Normalize Structure

Goal: reduce navigational ambiguity.

Tasks:

- map current folders to the target architecture
- decide whether to keep current folder names as-is with better README navigation or rename them gradually
- create index pages that connect prompts, skills, and patterns into one workflow story
- create a migration table for any moved or renamed files

Deliverables:

- path mapping document
- improved section readmes
- reduced overlap between handbook, quick-starts, and workflow assets

## Phase 3: Cross-Language Expansion

Goal: make the most valuable practical assets usable by .NET, Python, and Go teams.

Start with the highest-value workflow assets:

1. new API endpoint
2. story to vertical slice
3. database migration
4. test generation
5. pipeline setup
6. production incident triage

For each one:

- extract the shared workflow
- keep or create the `.dotnet.md` version
- add `.python.md`
- add `.go.md`

Suggested first targets from the current repo:

- `prompts/by-task/dotnet/new-api-endpoint.md`
- `prompts/by-task/dotnet/story-to-vertical-slice.md`
- `prompts/by-task/dotnet/database-migration.md`
- `prompts/by-task/ci-cd/azure-pipelines-dotnet-service.md`
- `prompts/by-task/qa/test-generation.md`

Deliverables:

- three-language examples for the highest-traffic tasks
- better reuse for mixed-language teams

## Phase 4: Role-Based Completeness

Goal: ensure the full Agile team can find relevant material quickly.

Add or strengthen role-specific entry points for:

- developers
- QA
- DevOps/platform
- engineering managers
- tech leads/architects
- product or delivery managers

Each role page should point to:

- learning path
- daily workflows
- prompts and skills
- guardrails
- case studies

## Phase 5: Example and Proof Layer

Goal: make the repo easier to trust and adopt.

Tasks:

- add real case studies in `case-studies/`
- add worked examples with before-and-after prompt evolution
- add stack-specific examples that show idiomatic differences between .NET, Python, and Go
- add review checklists and verification expectations for generated artifacts

## Recommended Content Backlog

These additions would materially improve usefulness:

### Cross-platform workflow assets

- `story-to-delivery-plan.md`
- `new-api-endpoint.md`
- `service-integration.md`
- `test-strategy-from-story.md`
- `pr-review-checklist.md`

### Language companions

- `new-api-endpoint.dotnet.md`
- `new-api-endpoint.python.md`
- `new-api-endpoint.go.md`
- `database-migration.dotnet.md`
- `database-migration.python.md`
- `database-migration.go.md`

### Role guides

- `quick-starts/developer-starter.md`
- `quick-starts/qa-starter.md`
- `quick-starts/devops-starter.md`
- `quick-starts/manager-starter.md`

### Governance and maintenance

- contribution checklist
- content lifecycle and review cadence
- metadata convention for audience, stack, and task type

## Suggested Metadata Pattern

To keep the repo searchable and maintainable, add a lightweight front-matter convention to new or heavily revised files:

```md
---
title: New API Endpoint
audience: [developer, tech-lead]
stacks: [dotnet, python, go]
type: prompt
stage: implementation
status: active
---
```

This is optional at first, but it will help if the repo grows quickly.

## Success Criteria

The reorganization is working if:

- a new engineer can find a safe starting point in under five minutes
- a mixed-language team can find equivalent guidance for .NET, Python, and Go
- contributors know where to add new content without guessing
- duplicate material shrinks over time instead of growing
- prompts, skills, and team guidance feel like one system rather than separate collections

## Immediate Next Moves

The most effective next sequence is:

1. keep the current folders in place for now
2. update the root navigation and positioning language
3. standardize the language-variant naming pattern
4. convert the top five .NET-heavy workflow assets into shared-plus-variant format
5. add role-based quick starts for the rest of the Agile team

This gives the repo a clearer shape without forcing a risky big-bang reorganization.
