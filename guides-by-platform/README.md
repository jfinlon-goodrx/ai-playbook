# Guides by Platform

This section organizes the platform-specific guidance that was previously scattered across the workspace.

## Read This First

- `01-Skills-Prompts-And-MCP-Playbook.md`

Use this page before diving into individual tool docs if you want the big picture on how prompts, skills, rules, and MCP fit together.

## Core AI Coding Tools

- `Coding Tools/Cursor-Guide.md`
- `Coding Tools/Claude-Code-Guide.md`
- `Enterprise-MCP-Baseline.md`
- `Glean-Guide.md`
- `Glean-API-vs-MCP-Decision-Guide.md`
- `UI/README.md`
- `../0-to-60-ai/18-AI-for-Developers.md`

These are the best starting points for engineers doing real implementation work.

## Scope In This Repo

This repo currently carries the coding-tool guidance most directly useful for daily engineering work. If you are standardizing team usage, start with Cursor or Claude Code and pair those with the prompt, skill, and MCP material below.

## Foundation Context Files

Before standardizing team usage, review the repo-level context assets:

- `../AGENTS.md`
- `../CLAUDE.md`
- `../repo-context.yaml`
- `../.cursor/rules/`
- `../cursorrules/`

## Skills, Rules, and Agent Guidance

- `../developer-workflow/Building Skills Into Your Dev Environment.md`
- `../0-to-60-ai/10-Skills-And-Tools.md`
- `../0-to-60-ai/11-MCP-And-Integrations.md`
- `../cursorrules/`

This is the right cluster when your team needs reusable reasoning workflows, project rules, or safe tool integration patterns.

## Developer Environment Helpers

- `../developer-workflow/Git-Worktrees-Guide.md`
- `../developer-workflow/README.md`
- `../local-private-ai/README.md`

These support the operational side of AI-assisted development: cleaner parallel work, better local ergonomics, and hardware-aware setups.

## How To Use This Section

### If You Need An Editor

Start with Cursor or Claude Code, then read the skills/rules material.

### If You Need An Integration Strategy

Start with Skills, Tools, and MCP. Then decide whether the workflow needs built-in tools, MCP servers, browser automation, or internal APIs.

### If You Need Enterprise Knowledge Grounding

Start with `Glean-Guide.md`, then connect it to the enterprise platform and Atlassian workflow material.

### If You Need A GoodRx MCP Standard

Start with `Enterprise-MCP-Baseline.md`, then use `Glean-API-vs-MCP-Decision-Guide.md` for the Glean-specific integration choice.

### If You Need Team Standards

Start with `../developer-workflow/Building Skills Into Your Dev Environment.md` and the repo `cursorrules`, then define a minimal reusable setup before adding more automation.
