# End-to-End Enterprise AI Delivery Platform

## Purpose

This guide explains how to connect your internal knowledge system, work management tools, documentation, source control, and AI hosts into one delivery platform.

For a Glean-specific implementation guide, use `../guides-by-platform/Glean-Guide.md`.
For the GoodRx MCP standard and rollout baseline, use `../guides-by-platform/Enterprise-MCP-Baseline.md`.

It is written for teams using:

- Gleen as the corporate RAG or knowledge-search system
- Jira for planning and work tracking
- Confluence for durable documentation and knowledge capture
- GitHub for source control, pull requests, CI, and release workflows
- MCP and APIs to connect those systems to AI tools safely

The goal is not to "add AI everywhere."

The goal is to create a governed, high-speed delivery system where AI helps with:

- grounding and discovery
- story shaping
- implementation planning
- coding
- testing
- review
- release communication
- knowledge capture

## The Core System Model

Treat each system as a distinct part of the delivery loop.

### Gleen

Use Gleen as the search and grounding layer for:

- internal policies
- architecture docs
- runbooks
- domain vocabulary
- ownership information
- previous decisions
- historical incidents and rollout notes

Gleen should reduce time spent hunting for context. It should not become the only source of truth.

### Jira

Use Jira as the operational record for:

- what is planned
- who owns it
- what is blocked
- what is in progress
- what is ready for review or release

Jira should carry intent, scope, risk, and workflow state.

### Confluence

Use Confluence as the durable human-readable knowledge layer for:

- requirements and functional context
- ADRs
- rollout and support notes
- handoff pages
- platform standards
- onboarding material

Confluence should hold the explanation people need later, not just the output of one AI session.

### GitHub

Use GitHub as the delivery execution layer for:

- code
- pull requests
- review
- Actions or CI evidence
- releases
- issues where relevant

GitHub should remain the source of truth for the actual change and the evidence that it passed validation.

### MCP and APIs

Use MCP and APIs as the connective tissue that lets AI hosts:

- search and retrieve context
- draft or update work items
- write handoff pages
- inspect code and CI status
- orchestrate repeatable workflows

MCP is most valuable when it replaces manual copy-paste loops with controlled, auditable access to the right systems.

## One Source Of Truth Per Data Type

Do not let the same fact drift across four systems.

Use this operating rule:

- Jira is the source of truth for workflow state
- Confluence is the source of truth for durable documentation
- GitHub is the source of truth for code, PR review, and build evidence
- Gleen is the discovery layer across trusted internal sources

AI assistants should pull from those systems and write back the right summary to the right home.

## The End-to-End Flow

## 1. Discovery And Grounding

### Inputs

- Gleen search results
- linked Confluence pages
- related Jira tickets
- relevant GitHub code or PR history

### What AI Should Do

- summarize the business and technical context
- identify the most relevant internal documents
- separate policy constraints from implementation detail
- show what is still ambiguous

### Output

- a short context pack for the current task
- links to the most relevant sources
- missing-information list

### Best Practice

Start read-only. Make discovery cheap and broad before you automate any write actions.

## 2. Story Shaping And Planning

### Inputs

- Jira issue or epic
- Gleen-discovered policy or domain context
- Confluence specs, ADRs, or handoff pages
- repo context and similar GitHub code

### What AI Should Do

- rewrite vague stories into delivery-ready language
- separate product ambiguity from technical ambiguity
- classify risk
- propose implementation, QA, rollout, and documentation tasks
- link the right supporting docs

### Output

- refined Jira story
- sub-task proposal
- implementation plan
- test and rollout notes

### Best Practice

Require the AI to identify what is missing before it starts decomposing work. Bad stories scaled by AI become faster bad work.

## 3. Implementation

### Inputs

- approved Jira story
- supporting Confluence and Gleen context
- repo rules and agent context files
- similar GitHub files

### What AI Should Do

- generate a reviewable vertical slice
- match repo conventions
- propose tests and rollout implications
- draft PR summaries and reviewer context

### Output

- code changes
- tests
- documentation updates
- PR summary

### Best Practice

Keep repo-local context strong. Jira and Confluence explain intent, but the codebase still defines reality.

## 4. Testing And Validation

### Inputs

- generated or modified code
- QA prompts and workflows
- CI configuration
- Jira acceptance criteria

### What AI Should Do

- derive test cases from the story and code
- expand edge-case coverage
- highlight risky paths and gaps
- summarize CI and test evidence for reviewers

### Output

- unit and integration tests
- QA checklist
- summarized validation evidence

### Best Practice

Treat testing as a first-class generation target, not a clean-up step after code generation.

## 5. Review, Release, And Publishing

### Inputs

- GitHub PR
- CI evidence
- Jira ticket
- rollout expectations
- Confluence handoff template

### What AI Should Do

- draft reviewer notes and release notes
- update Jira status and attach PR context
- create Confluence handoff or ADR pages
- summarize rollback and monitoring expectations

### Output

- PR description
- Jira update
- Confluence handoff page
- release communication draft

### Best Practice

Use AI to reduce coordination friction, not to hide risk. High-risk changes still need explicit human review.

## 6. Feedback And Knowledge Capture

### Inputs

- production telemetry
- incident notes
- retro outcomes
- support feedback
- merged PR history

### What AI Should Do

- summarize what was learned
- update runbooks or handoff pages
- turn incidents into follow-up work
- improve prompts, skills, and playbook assets

### Output

- Confluence updates
- Jira follow-up tickets
- repo prompt or skill improvements

### Best Practice

If a useful workflow lives only in chat history, it is not part of the platform yet.

## MCP and API Strategy

Use a phased approach.

## Phase 1: Read-Only Context

Allow AI to:

- search Gleen
- read Jira tickets
- search Confluence
- read GitHub repo, PR, and CI data

This phase creates trust and improves planning quality.

## Phase 2: Draft Writes

Allow AI to:

- draft Jira comments
- propose sub-tasks
- draft Confluence pages
- draft PR descriptions

Keep a human approval step before submission.

## Phase 3: Controlled Operational Writes

Allow AI to:

- update ticket states
- create or update handoff pages
- post structured release notes
- open or update low-risk follow-up tasks

This only works well once permissions, templates, and auditability are clear.

## Data Management Rules

## 1. Keep Retrieval Grounded

Only index or retrieve from sources that:

- are trustworthy
- are current enough for the task
- have clear ownership

## 2. Preserve Links And IDs

Always keep:

- Jira IDs
- Confluence page links
- GitHub PR or commit links
- service or system names

This makes the generated artifacts traceable.

## 3. Avoid Prompting With Raw Sensitive Data

Do not copy large amounts of sensitive operational or customer data into prompts if the system does not require it.

Prefer:

- identifiers
- links
- summaries
- sanitized examples

## 4. Write Back Structured Knowledge

When an AI session produces something durable, save it as:

- a Jira update
- a Confluence page
- a repo prompt
- a repo skill
- a runbook update

Not as an orphaned chat transcript.

## 5. Manage Freshness

Knowledge systems decay unless ownership is explicit.

Assign owners for:

- story templates
- Confluence standards
- prompt libraries
- skills
- MCP tool definitions

## Recommended MCP and Integration Surface

At minimum, aim for:

- Gleen search and document retrieval
- Jira issue read, comment, and controlled status update
- Confluence page read, search, and draft publishing
- GitHub repo read, PR read, CI status, and controlled PR drafting

Prefer small, high-value capabilities over broad tool access with weak controls.

## Skillset The Team Should Build

The platform only works if the team develops shared skills, not just tool access.

### Product and Delivery

- writing intent-rich stories
- classifying risk
- linking the right supporting context

### Developers

- grounding work in repo and system context
- generating coherent slices instead of isolated snippets
- using AI for tests, review support, and documentation

### QA

- converting acceptance criteria into risk-based validation
- using AI to expand scenario coverage
- reviewing test evidence and coverage gaps

### DevOps and Platform

- designing safe MCP and API access
- making CI and release evidence readable to humans and AI
- controlling secrets, permissions, and auditability

### Tech Leads and Managers

- standardizing workflows and templates
- measuring system outcomes instead of raw AI activity
- capturing successful patterns back into the platform

## The Minimum Viable High-Speed AI Delivery Platform

If you want the smallest useful version of this system, build this first:

1. strong repo context files
2. read-only Gleen, Jira, Confluence, and GitHub access
3. one story-refinement workflow
4. one story-to-code vertical-slice workflow
5. one QA and test-generation workflow
6. one PR, release, and handoff workflow
7. one feedback loop that writes useful knowledge back into Confluence or the repo

## Recommended Next Assets In This Repo

- more prompts that span Jira to GitHub to Confluence
- skills for end-to-end orchestration
- templates for release handoff and production follow-up
- examples showing how one feature moved across all systems

## Related Reading

- `../Agile-AI/04-AI-Native-Adoption-Roadmap.md`
- `../guides-by-platform/Enterprise-MCP-Baseline.md`
- `../guides-by-platform/Glean-Guide.md`
- `../guides-by-platform/Glean-API-vs-MCP-Decision-Guide.md`
- `../prompts/by-task/atlassian/jira-confluence-mcp.md`
- `../prompts/by-task/atlassian/jira-story-to-delivery-plan.md`
- `../guides-by-platform/01-Skills-Prompts-And-MCP-Playbook.md`
