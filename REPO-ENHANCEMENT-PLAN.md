# Repository Enhancement Plan

## Objective

Turn this repository into a clean, cross-platform AI engineering playbook that helps an Agile SDLC team adopt AI-assisted development across:

- development
- QA
- DevOps and platform engineering
- technical leadership
- engineering management
- product and delivery collaboration

The repo should remain practical, example-driven, and safe for enterprise use, while no longer reading like it is only for a .NET team.

## Intended Audience

This plan is written so a human contributor, Cursor, Claude, or another coding agent can implement it incrementally.

## Current State Summary

The repository already has strong content. The main issue is not lack of material, but lack of a single organizing model.

### What exists today

- strategic AI and Agile adoption guidance
- role-based education content
- getting-started material
- prompt libraries
- reusable skills
- Cursor rules examples
- workflow and tool guides
- anti-patterns
- initial frontend prompt coverage
- Atlassian and MCP guidance that can be extended into fuller enterprise orchestration

### What is not yet aligned

- the repo is still described primarily as a .NET/C#/MSSQL team resource
- language-specific guidance is uneven
- content is grouped by several different mental models at once
- there is no consistent naming convention for cross-language examples
- there was no repo-level agent guidance before this pass
- frontend and UI guidance has been much thinner than backend and delivery guidance
- enterprise system integration guidance needed a clearer end-to-end operating model

## Constraints and Assumptions

- Preserve existing useful content.
- Prefer incremental refactoring over a big-bang restructure.
- Keep links understandable during transition.
- Bias toward reusable canonical docs with language-specific companions.
- Support .NET, Python, and Go as first-class implementation languages.

## Guiding Principles

1. Keep one canonical explanation for each shared concept.
2. Add language-specific companion files only when implementation details differ.
3. Optimize for fast discovery by role, task, and stack.
4. Prefer examples grounded in real delivery work over generic AI advice.
5. Keep review, verification, and guardrails close to every acceleration workflow.

## Target Repository Shape

The long-term shape should distinguish between:

### Canonical concepts

Home for:

- operating model
- governance
- quality and safety
- evaluation
- observability
- content maps

Current best home:

- `handbook/`
- `Agile-AI/`

### Learning and onboarding

Home for:

- role-based curricula
- first-week onboarding
- starter paths

Current best home:

- `0-to-60-ai/`
- `getting-started/`
- `quick-starts/`

### Reusable workflows

Home for:

- prompts
- patterns
- skills
- workflow templates

Current best home:

- `prompts/`
- `skills/`
- parts of `developer-workflow/`

### Platform and tool guidance

Home for:

- editor guidance
- project rules
- local/private AI
- environment setup
- frontend and design-tool guidance

Current best home:

- `guides-by-platform/`
- `cursorrules/`
- `local-private-ai/`

### Example and proof layer

Home for:

- case studies
- worked examples
- language adaptations

Current best home:

- `case-studies/`

## Language Support Model

Use one of these three patterns for every new or revised file.

### Pattern 1: shared-only

Use a single file when the guidance is language-agnostic.

Examples:

- AI adoption guidance
- story-writing guidance
- review discipline
- hallucination recovery
- prompt hygiene

### Pattern 2: shared plus language companions

Use a canonical file plus language-specific variants when the workflow is shared but implementation differs.

Naming standard:

- `topic.md`
- `topic.dotnet.md`
- `topic.python.md`
- `topic.go.md`

Examples:

- `new-api-endpoint.md`
- `new-api-endpoint.dotnet.md`
- `new-api-endpoint.python.md`
- `new-api-endpoint.go.md`

### Pattern 3: language-only

Use a language-only file when the topic is fundamentally ecosystem-specific.

Examples:

- EF Core migrations
- FastAPI dependency wiring
- Go project layout and interface boundaries

## File and Metadata Standards

Every significant new doc should include:

- a clear title
- who it is for
- when to use it
- related links

Recommended optional front matter for new or heavily revised documents:

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

## Workstreams

Implement this plan in eight workstreams.

## Workstream 1: Foundation Files

### Goal

Make the repo understandable to AI tools and contributors before deeper refactoring begins.

### Tasks

- add root `AGENTS.md`
- add root `CLAUDE.md`
- add `.cursor/rules/` files for repo behavior
- add a machine-readable repo catalog or metadata file if helpful
- ensure root `README.md` reflects the current and intended mission

### Acceptance criteria

- a new agent can infer repo purpose without reading many files
- Cursor and Claude have explicit repo guidance
- the repo is positioned as cross-platform rather than .NET-only

## Workstream 2: Navigation and Reader Journeys

### Goal

Make it obvious where each audience should start.

### Tasks

- update root `README.md`
- add role-based entry points
- add task-based entry points
- add stack-based entry points
- cross-link major sections from every section `README.md`

### Acceptance criteria

- a developer, QA engineer, manager, or platform engineer can each find a recommended start path in under five minutes

## Workstream 3: Content Normalization

### Goal

Reduce duplication and create cleaner canonical sources.

### Tasks

- inventory duplicate or overlapping docs
- identify canonical versions
- merge or cross-link overlapping content
- add short intros to older docs that currently assume too much context
- standardize headings and file intros

### Acceptance criteria

- overlapping content is intentional and linked, not accidental
- every major section has a clear purpose

## Workstream 4: Cross-Language Expansion

### Goal

Make the most important implementation workflows useful to .NET, Python, and Go teams.

### Priority conversion targets

1. `prompts/by-task/dotnet/new-api-endpoint.md`
2. `prompts/by-task/dotnet/story-to-vertical-slice.md`
3. `prompts/by-task/dotnet/database-migration.md`
4. `prompts/by-task/ci-cd/azure-pipelines-dotnet-service.md`
5. `prompts/by-task/qa/test-generation.md`

### Conversion method

For each target:

1. extract the shared workflow into a neutral file
2. preserve or create the `.dotnet.md` version
3. create `.python.md`
4. create `.go.md`
5. link all siblings together

### Acceptance criteria

- top practical workflows exist for all three supported languages
- examples are idiomatic, not mechanically translated

## Workstream 5: Role Completeness

### Goal

Support the full Agile SDLC team, not only developers.

### Priority roles

- developer
- QA/test automation
- DevOps/platform
- tech lead/architect
- engineering manager
- product or delivery manager

### Tasks

- create or improve quick-starts per role
- ensure each role has recommended prompts, skills, and guardrails
- connect role guides to learning paths and example assets

### Acceptance criteria

- each role has an obvious landing page and next steps

## Workstream 6: Example and Proof Layer

### Goal

Increase trust and practical reuse.

### Tasks

- add real case studies
- add worked prompt examples
- add before-and-after examples of improving context and results
- add verification checklists for generated outputs

### Acceptance criteria

- contributors can see how the playbook works in practice, not only in theory

## Workstream 7: Frontend and UI Enablement

### Goal

Close the repo's biggest practical gap around design handoff, UI implementation, component systems, accessibility, and frontend workflow.

### Tasks

- add frontend quick-start guidance
- add Figma-to-implementation workflow guidance
- recommend pragmatic UI toolsets by stack
- add prompts for turning Figma or design handoff into reviewable implementation plans
- connect UI work to accessibility, responsive review, and Playwright-style testing

### Acceptance criteria

- frontend teams have an obvious starting point
- Figma and design handoff are treated as structured inputs, not screenshots alone
- React, Blazor, and server-rendered UI teams all have actionable guidance

## Workstream 8: Enterprise AI Delivery Integrations

### Goal

Show how knowledge systems, work-management tools, source control, and MCP or APIs combine into one governed delivery platform.

### Tasks

- add an end-to-end enterprise AI delivery guide
- document the role of Gleen, Jira, Confluence, GitHub, MCP, and APIs
- add orchestration prompts that span multiple systems
- add a reusable skill for full-lifecycle AI-assisted delivery
- define data-management and source-of-truth rules

### Acceptance criteria

- teams can see how to connect discovery, planning, coding, testing, and publication
- enterprise tools have clear roles instead of overlapping vaguely
- read-only, draft-write, and controlled-write automation stages are explicit

## Initial Backlog

Use this as the first implementation queue.

### P0

- create `CLAUDE.md`
- create `.cursor/rules/` repo guidance
- update root `README.md` mission statement
- add role-based and stack-based navigation to root `README.md`
- define the language-variant naming convention in the root docs

### P1

- normalize the highest-value prompt assets into shared-plus-variant format
- add Python and Go companion examples for the top implementation prompts
- strengthen `quick-starts/` for QA, DevOps, and management
- add a section-level index for `prompts/` and `skills/` that reflects the long-term workflow model
- add frontend/UI workflow guidance and Figma-driven prompts
- add enterprise integration guidance for Gleen, Jira, Confluence, GitHub, MCP, and APIs

### P2

- introduce lightweight front matter on new or revised docs
- reorganize or remap folders if the navigation cleanup proves insufficient
- add more case studies and example bundles

## Suggested File Moves or Convergence Areas

Do not move everything immediately. Use these as gradual convergence targets.

### Likely convergence area: workflow assets

Current:

- `prompts/`
- `skills/`
- `developer-workflow/`

Long-term intention:

- one clearer workflow story with prompts, patterns, and skills connected

### Likely convergence area: platform/tool guidance

Current:

- `guides-by-platform/`
- `cursorrules/`
- `local-private-ai/`

Long-term intention:

- one clearer platform/tool area with editor, rules, and private AI guidance

## Implementation Notes for Agents

When working this plan:

1. prefer editing existing docs over creating new overlapping docs
2. add redirects or link notes when paths change
3. keep diffs reviewable
4. preserve useful examples even when reorganizing
5. make repo-wide wording progressively less vendor- and stack-narrow

## Definition of Done

This repository is in a good state when:

- the root experience clearly serves mixed-language Agile teams
- major workflows are available for .NET, Python, and Go
- new contributors know where to place content
- section readmes form coherent reader journeys
- prompts, skills, and operating-model guidance feel like one system

## Recommended First Execution Sequence

Implement in this order:

1. foundation files
2. root navigation rewrite
3. language-variant naming standard
4. first five cross-language workflow conversions
5. role-based quick starts
6. frontend/UI enablement
7. proof and case-study layer

## Related Documents

- [AGENTS.md](/Users/jfinlon/repos/ai-playbook/AGENTS.md)
- [03-Repo-Reorganization-And-Expansion-Plan.md](/Users/jfinlon/repos/ai-playbook/handbook/03-Repo-Reorganization-And-Expansion-Plan.md)
