# Glean API vs Glean MCP Decision Guide

## Purpose

This guide explains when a GoodRx team should use:

- Glean UI and chat
- Glean Client API
- Glean Indexing API
- Glean MCP

## Short Version

Use:

- **Glean UI/chat** for immediate human discovery
- **Client API** for approved programmatic retrieval before MCP is enabled
- **Indexing API** only when you need to ingest new owned data into Glean
- **Glean MCP** once enabled, for standard AI-host integration across tools

## Decision Table

| Need | Best Choice | Why |
|---|---|---|
| A person needs quick context now | Glean UI/chat | fastest, lowest setup |
| An internal service or app needs grounded retrieval | Client API | explicit app integration, works before MCP |
| An MCP-capable host needs search and read tools | Glean MCP | standard tool interface across AI hosts |
| You need to add internal documents or datasets into Glean | Indexing API | correct path for ingestion and permissions |
| You need company-wide AI-host standardization | Glean MCP | one shared integration model across hosts |

## Use Glean UI When

- the workflow is mostly human-driven
- someone is investigating context manually
- the team is not ready to build or standardize an integration
- MCP is not yet enabled

## Use Client API When

- you need a platform-owned integration before MCP is enabled
- you want a service, web app, or approved internal automation to call Glean
- you want tighter control over retrieval, filtering, and logging

Good examples:

- internal story-shaping helpers
- knowledge-grounding services
- support or incident context panels
- internal AI apps that need retrieval but not full MCP rollout yet

## Use Indexing API When

- the data is not already in Glean
- the source has a clear owner
- freshness and permission mapping will be maintained
- the data materially improves retrieval quality

Do not use the Indexing API as a dumping ground for every possible internal dataset.

## Use Glean MCP When

- tenant enablement is complete
- the security review is complete
- you want one standard integration across:
  - Cursor
  - Codex
  - Claude
  - Antigravity
  - other MCP-capable hosts

This is the best long-term operating model because it gives teams one shared tool contract instead of multiple custom wrappers.

## Current GoodRx Recommendation

As of **April 8, 2026**:

- use Glean UI/chat now
- use Client API for approved programmatic integrations if needed
- plan for Glean MCP soon, but do not assume it is part of the default baseline yet
- use Indexing API only for intentional platform-owned ingestion work

## Migration Path

### Stage 1

- human workflows in Glean UI
- read-only experimentation

### Stage 2

- platform-owned Client API wrappers for specific use cases
- shared retrieval patterns and prompt templates

### Stage 3

- Glean MCP enabled
- read-only MCP standardization

### Stage 4

- richer grounded workflows across Jira, Confluence, GitHub, and repo context

## Default Decision Rule

If there is any doubt:

1. use Glean UI first
2. use Client API for shared internal apps
3. use Glean MCP when tenant enablement is ready
4. use Indexing API only with ownership and governance

## Related Reading

- `Glean-Guide.md`
- `Enterprise-MCP-Baseline.md`
- `../handbook/04-End-to-End-Enterprise-AI-Delivery-Platform.md`
