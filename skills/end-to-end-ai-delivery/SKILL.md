---
name: end-to-end-ai-delivery
description: Orchestrates AI-assisted work across knowledge retrieval, story shaping, implementation planning, coding support, testing, and publication using systems such as Gleen, Jira, Confluence, GitHub, MCP, and repo context.
---
# End-to-End AI Delivery

## When To Use

Use this skill when the work spans multiple systems and you want one guided workflow from context gathering through handoff.

## Workflow

### 1. Ground The Work

Gather:

- Jira issue context
- relevant Confluence pages
- Gleen results for policy, architecture, and historical context
- related GitHub code or PR references
- repo-local rules and conventions

Separate:

- business context
- technical context
- risk and compliance constraints
- unresolved ambiguity

### 2. Decide Readiness

Before planning or coding, determine:

- is the story clear enough
- is the risk level obvious
- are dependencies visible
- is there enough repo context to proceed safely

If not, produce clarification questions before generating implementation work.

### 3. Build The Delivery Shape

Produce:

- implementation plan
- QA and testing plan
- rollout notes
- documentation updates
- reviewer guidance

Keep the output role-aware for:

- product
- development
- QA
- platform
- reviewers

### 4. Support Implementation

When coding begins:

- point the assistant at real code
- prefer vertical slices over disconnected snippets
- generate tests and reviewer context with the implementation
- keep rollout and observability implications visible

### 5. Close The Loop

When work completes, draft or update:

- Jira comments or status notes
- Confluence handoff or ADR pages
- PR description or release notes
- follow-up tasks

## Good Output

Good output is:

- grounded in real systems
- explicit about uncertainty
- traceable through links and IDs
- useful across the full delivery lifecycle

## Related Assets

- `../../handbook/04-End-to-End-Enterprise-AI-Delivery-Platform.md`
- `../../prompts/by-task/atlassian/end-to-end-ai-delivery-orchestration.md`
- `../../prompts/by-task/atlassian/jira-confluence-mcp.md`
