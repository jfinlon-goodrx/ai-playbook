# Enterprise MCP Baseline

## Purpose

This guide defines a practical GoodRx-oriented MCP baseline for:

- Cursor
- Codex
- Claude and other MCP-capable hosts
- Antigravity
- approved internal OpenAPI-backed integrations

It is meant to answer five concrete questions:

1. Which MCP servers should each role have installed?
2. What is the current-state baseline?
3. What is the target-state baseline?
4. How should Glean fit into that standard?
5. What phased rollout makes sense for GoodRx?

## Baseline Rule

Only standardize MCP servers that are:

- security reviewed
- actively maintained
- high-value across repeated workflows
- scoped with least privilege

If a server is not on the approved allowlist, treat it as an evaluation candidate, not a team baseline.

## Current-State Versus Target-State

| Capability Area | Current State | Target State |
|---|---|---|
| Knowledge grounding | Glean UI/chat and approved Client API integrations | Glean MCP becomes part of the standard read-only baseline |
| Planning and docs | Atlassian Rovo MCP for Jira and Confluence | same, with more structured draft and controlled-write workflows |
| Code and PR workflows | GitHub MCP | same, with more mature PR, CI, and release orchestration |
| Frontend and design | Figma MCP for UI roles | same, potentially broader adoption for product-facing teams |
| API and contract workflows | Postman MCP for API-heavy teams | same, with stronger contract-aware testing workflows |
| Operations and incidents | Datadog MCP for platform and SRE | same, with more structured release and incident support |

## Current GoodRx Recommendation

As of **April 8, 2026**:

- standardize **Atlassian Rovo MCP** and **GitHub MCP** for most technical roles
- use **Glean UI/chat** today as the default knowledge layer
- use approved **Glean Client API** integrations when programmatic retrieval is needed
- prepare to add **Glean MCP** as soon as tenant enablement is complete

## Role-By-Role Install Matrix

| Role | Jira/Confluence MCP | GitHub MCP | Glean | Figma MCP | Postman MCP | Datadog MCP | Slack MCP |
|---|---|---|---|---|---|---|---|
| Developer | Required | Required | Use UI now, MCP later | Optional | Optional | Optional | No |
| QA / Test Automation | Required | Required | Use UI now, MCP later | No | Required | Optional | No |
| Frontend / UI | Required | Required | Use UI now, MCP later | Required | Optional | No | No |
| Platform / SRE | Required | Required | Use UI now, MCP later | No | Optional | Required | Optional read-only |
| Tech Lead / Architect | Required | Required | Use UI now, MCP later | Optional | Optional | Optional | No |
| Product / Delivery / Manager | Required | Optional read-only | Use UI now, optional MCP later | Optional | No | No | Optional read-only |

## Recommended Baseline By Role

## Everyone Technical

Install:

- Atlassian Rovo MCP
- GitHub MCP

Then add role-specific servers only where they materially improve daily work.

## Developers

Install:

- Atlassian Rovo MCP
- GitHub MCP

Add when needed:

- Postman MCP for API-heavy work
- Glean MCP once tenant enablement is complete
- optional Sourcegraph if cross-repo search is a real bottleneck

## QA And Test Automation

Install:

- Atlassian Rovo MCP
- GitHub MCP
- Postman MCP

Add when needed:

- Datadog MCP for validation and incident-heavy workflows
- Glean MCP once tenant enablement is complete

## Frontend And UI

Install:

- Atlassian Rovo MCP
- GitHub MCP
- Figma MCP

Add when needed:

- Glean MCP once tenant enablement is complete

## Platform, DevOps, And SRE

Install:

- Atlassian Rovo MCP
- GitHub MCP
- Datadog MCP

Add when needed:

- Glean MCP once tenant enablement is complete
- optional Slack MCP with read-first posture

## Tech Leads And Architects

Install:

- Atlassian Rovo MCP
- GitHub MCP

Add when needed:

- Glean MCP once tenant enablement is complete
- Figma MCP when design oversight is common
- Datadog MCP when operational review is part of the role

## Product, Delivery, And Managers

Install:

- Atlassian Rovo MCP

Use today:

- Glean UI/chat

Add later:

- optional GitHub read-only access
- Glean MCP once tenant enablement is complete

## Permission Model

Use three simple permission tiers.

## Tier 1: Read-Only

Allow:

- search
- read
- summarize
- inspect status

Examples:

- Glean search
- Jira read
- Confluence read
- GitHub PR and CI read

This should be the default baseline.

## Tier 2: Draft Writes

Allow:

- draft PR descriptions
- draft Jira comments
- draft Confluence pages
- draft release notes

Require human review before submission.

## Tier 3: Controlled Writes

Allow:

- ticket status changes
- page publishing
- low-risk PR metadata updates
- low-risk workflow automation

Only introduce this after templates, ownership, auditability, and security review are stable.

## Host Strategy

Do not create a different MCP policy for every AI host.

Use the same approved server catalog and permission model across:

- Cursor
- Codex
- Claude
- Antigravity
- approved OpenAI-based internal applications

That makes security review, onboarding, and workflow documentation much simpler.

## Phased Rollout Plan For GoodRx

## Phase 1: Baseline Read Access

Standardize:

- Atlassian Rovo MCP
- GitHub MCP
- role-based Figma, Postman, and Datadog

Use Glean through:

- Glean UI/chat
- approved Client API integrations where needed

## Phase 2: Structured Draft Workflows

Standardize templates for:

- story shaping
- implementation planning
- PR summaries
- Confluence handoffs
- release notes

Keep write actions human-reviewed.

## Phase 3: Glean MCP Enablement

When tenant enablement is complete:

- add Glean MCP as a standard read-only knowledge layer
- standardize one or two approved Glean server profiles
- update prompts and skills to assume grounded retrieval is available

## Phase 4: Controlled Automation

Introduce:

- low-risk ticket updates
- low-risk documentation publishing
- richer end-to-end AI workflows across systems

Only after:

- auditability
- ownership
- templates
- security review

## Minimum Useful Standard Today

If the team wants a small, useful standard now:

1. Atlassian Rovo MCP for almost everyone
2. GitHub MCP for most technical roles
3. Figma MCP for frontend roles
4. Postman MCP for API and QA roles
5. Datadog MCP for platform and SRE
6. Glean UI today, Glean MCP later

## Related Reading

- `Glean-Guide.md`
- `Glean-API-vs-MCP-Decision-Guide.md`
- `../handbook/04-End-to-End-Enterprise-AI-Delivery-Platform.md`
- `../quick-starts/enterprise-ai-platform-starter.md`
