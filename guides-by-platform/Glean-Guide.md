# Glean Guide

## Purpose

This guide explains how GoodRx teams should use Glean as part of an AI-assisted delivery platform.

It covers:

- current-state usage through the Glean product and REST APIs
- likely future-state usage through Glean MCP
- how Glean fits with Jira, Confluence, GitHub, and repo-local AI workflows
- data-management and governance rules
- role-based baseline guidance
- rollout guidance for GoodRx

## Current GoodRx Status

As of **April 8, 2026**, based on the current GoodRx internal status shared for this repo:

- Glean MCP is **not yet enabled** in the GoodRx tenant
- tenant enablement is tracked in `EBS-2252`
- near-term usage should assume:
  - Glean UI and chat are available
  - Glean REST APIs may be available through approved admin setup
  - Glean MCP should be treated as a target-state capability, not a required current baseline

That means teams should plan for Glean to become a first-class MCP knowledge layer soon, but should not depend on it being available in every host today.

For the concrete GoodRx MCP standard, use `Enterprise-MCP-Baseline.md`.

For a focused integration choice guide, use `Glean-API-vs-MCP-Decision-Guide.md`.

## What Glean Should Do In The Platform

Use Glean as the **discovery and grounding layer** for:

- internal policies
- architecture docs
- runbooks
- onboarding material
- service ownership
- prior incidents
- domain vocabulary
- previous decisions and historical project context

Do not use Glean as the source of truth for workflow state, code, or release evidence.

## Source Of Truth Rules

Use this split consistently:

- Glean: discovery and retrieval across trusted internal sources
- Jira: work state, scope, risk, and acceptance criteria
- Confluence: durable human-readable documentation and decisions
- GitHub: code, PR review, CI evidence, and release history

Glean helps the team find the right information quickly. It should not replace the systems that own that information.

## Current-State Usage

Until MCP is enabled, teams should use Glean in two main ways.

## 1. Glean Product UI And Chat

This is the easiest way to use Glean today for:

- finding relevant design or architecture docs
- finding policy or compliance references
- locating runbooks and support context
- gathering background before story refinement or implementation

Best use cases:

- preparing to refine a Jira story
- gathering system context before coding
- finding incident or rollout history
- helping new team members discover relevant internal docs faster

## 2. Glean REST APIs

### Client API

Use the Client API when you want an internal app, service, or approved automation to call Glean programmatically for:

- search
- chat
- answers
- documents
- tools
- entities

This is the right path when:

- Glean MCP is not yet enabled
- a platform team wants to build a shared integration
- you want read-oriented retrieval in a controlled workflow

### Indexing API

Use the Indexing API when you want to push your own data into Glean from:

- internal tools
- custom databases
- service catalogs
- support systems
- internal knowledge stores not already indexed

Use this deliberately. Poorly curated indexing creates noisy retrieval and weak grounding.

## Future-State Usage With Glean MCP

Once Glean MCP is enabled in the GoodRx tenant, Glean should become part of the standard technical AI toolchain.

## Recommended Glean MCP Baseline

### For Most Technical Roles

- read-only search
- read document
- grounded chat over trusted internal knowledge

### For Delivery And Management Roles

- search
- read document
- lightweight summarization of linked internal documentation

## Recommended MCP Server Structure

When MCP is enabled, create at least two Glean servers:

### `default`

Purpose:

- broad company knowledge discovery

Suggested tools:

- `search`
- `read_document`
- `chat`

### `engineering-context`

Purpose:

- engineering and delivery context only

Suggested content focus:

- architecture docs
- ADRs
- platform standards
- runbooks
- security guidance
- delivery playbooks
- incident and rollout history

Suggested tools:

- `search`
- `read_document`
- `chat`

Start read-only. Add higher-autonomy behavior only if there is a clear governance need and a clear audit model.

## Recommended Role Usage

### Developers

Use Glean to:

- find architecture and service context
- find prior implementation or rollout notes
- identify ownership and system boundaries before coding

### QA

Use Glean to:

- find behavior specs and policy expectations
- locate prior bugs, incidents, or support issues
- gather operational context for regression planning

### Frontend And UI

Use Glean to:

- find design-system usage notes
- locate rollout constraints or compliance guidance
- find product and support context for user flows

### DevOps And Platform

Use Glean to:

- find runbooks and incident history
- find service ownership and operational standards
- gather release or rollback context before change execution

### Product, Delivery, And Managers

Use Glean to:

- retrieve business and policy context quickly
- support better story shaping
- find prior decisions before restarting the same debate

## Best Workflow Patterns

## Story Shaping

Use Glean before or alongside Jira refinement to:

- find business and technical context
- identify missing requirements
- surface policies and prior decisions
- link the most relevant Confluence and operational docs

## Implementation Planning

Use Glean with Confluence and GitHub to:

- ground the feature in real system context
- find architecture constraints
- locate prior migration or rollout patterns
- reduce guesswork before code generation starts

## Testing And Validation

Use Glean to:

- find support history, incident patterns, and known failure modes
- improve test-plan coverage with operational context
- connect acceptance criteria to historical risk

## Release And Handoff

Use Glean to:

- locate the right runbooks and rollout notes
- find related systems and ownership information
- make handoff pages and release communication more grounded

## Data-Management Rules

## 1. Prefer Retrieval Over Copy-Paste

Do not paste large blocks of internal content into prompts if retrieval plus citation can do the job.

Prefer:

- search results
- document links
- short summaries
- explicit source references

## 2. Keep Sensitive Data Out Of Ad Hoc Prompts

Even with approved systems, do not paste unnecessary secrets, customer data, or sensitive records into prompts.

Prefer:

- identifiers
- sanitized examples
- links to the controlled source

## 3. Index Carefully

Only push new data into Glean through the Indexing API when:

- the source has a clear owner
- freshness is maintained
- permissions are correct
- the content adds real retrieval value

## 4. Preserve Traceability

When Glean helps produce a useful output, keep the linked source references where possible:

- Jira ID
- Confluence page
- runbook link
- system name
- GitHub PR or repo reference

## Governance Recommendations

## Start With Read-Only Access

For both MCP and API usage, begin with:

- search
- read
- summarize

This creates trust and keeps the blast radius small.

## Keep Glean Out Of Workflow Ownership

Do not let Glean become the system that stores:

- final acceptance criteria
- final workflow state
- code review status
- release approvals

Those belong in Jira, Confluence, and GitHub.

## Use Shared Integrations Before Ad Hoc Personal Ones

If platform teams build a Glean Client API wrapper or future Glean MCP templates, standardize them across:

- Cursor
- Codex
- Claude
- Antigravity
- approved OpenAI-based apps

That creates one security and operations model instead of five disconnected personal setups.

## Recommended GoodRx Rollout Path

## Phase 1: Today

- use Glean UI and chat for discovery
- use Jira and Confluence MCP for structured work
- use GitHub MCP for code and PR workflows
- build read-only Glean Client API integrations where needed

## Phase 2: When Glean MCP Is Enabled

- add Glean MCP as a standard read-only technical baseline
- standardize one or two approved server profiles
- update prompts and skills to assume Glean retrieval is available

## Phase 3: Mature Platform Usage

- connect Glean discovery, Jira planning, GitHub delivery, and Confluence handoff into one repeatable workflow
- measure whether Glean is reducing time-to-context and improving story and implementation quality
- add more structured shared skills and automations around grounded retrieval

## Good Prompt Pattern

```text
Before planning this work, search Glean for:
- architecture docs for [system]
- runbooks or incident notes related to [feature]
- policy or security guidance relevant to [change]

Then summarize:
1. the most relevant findings
2. what they imply for implementation or testing
3. which linked Confluence pages or owners I should review next

Do not invent missing context. Call out uncertainty explicitly.
```

## Related Reading

- `Enterprise-MCP-Baseline.md`
- `Glean-API-vs-MCP-Decision-Guide.md`
- `../handbook/04-End-to-End-Enterprise-AI-Delivery-Platform.md`
- `../prompts/by-task/atlassian/end-to-end-ai-delivery-orchestration.md`
- `../quick-starts/enterprise-ai-platform-starter.md`
