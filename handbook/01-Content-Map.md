# Content Map and Consolidation Guide

This document maps the major source material in this repo and shows how it fits into a more coherent handbook.

## Primary Source Clusters

### Strategy and Organizational Change

- `../Agile-AI/AI-Adoption-Playbook-From-Kanban-to-AI-Driven-Development.md`
- `../Agile-AI/AI-Assisted-Development-and-the-Future-of-Agile.md`
- `../Agile-AI/`

These documents explain the delivery-model shift: AI changes throughput, shifts bottlenecks toward validation and review, and demands stronger guardrails plus lighter, faster coordination loops.

### Developer Enablement and Working Patterns

- `../getting-started/`
- `../prompts/`
- `../anti-patterns/`
- `../cursorrules/`
- `../developer-workflow/Building Skills Into Your Dev Environment.md`
- `../developer-workflow/Git-Worktrees-Guide.md`

These are the most practical “how to work with AI day to day” sources. They are the foundation for the quick-start, patterns, and guardrails portions of the handbook.

### Curriculum and Shared Education

- `../0-to-60-ai/`
- `../handbook/02-Production-AI-Quality-And-Operations.md`

This is the curated learning system carried in the canonical repo. It preserves the strongest practical modules and role-based appendices without bringing over the entire education workspace.

### Tools and Platform Guides

- `../guides-by-platform/`
- `../developer-workflow/`

These provide the setup and comparison layer: which tools to use, how to structure day-to-day work, and how to get productive quickly.

### Local, Private, and Enterprise AI

- `../local-private-ai/`

These materials support internal enablement, privacy-conscious deployment, and internal knowledge patterns.

## Important Overlaps To Be Aware Of

### The Repo Now Includes More Than Prompts

This repo started as a prompt-and-pattern library. It now also carries strategic adoption guidance, curated education content, workflow docs, and platform guides so a team can go from first use to broader operating-model change in one place.

### Agile Guidance Was Previously Split

The longer essays provide the strongest business and organizational framing. The repo’s `Agile-AI` folder provides the clearest operating-model language. Together they give teams both the why and the how of AI-native delivery.

### Prompting, Skills, and Tool Use Were Fragmented

Prompting, skills, and MCP still span multiple folders by design. The repo now keeps task prompts in `prompts/`, reusable workflows in `skills/`, workflow discipline in `developer-workflow/`, and the practical education subset in `0-to-60-ai/`.

## Target Reader Journeys

### A Developer

1. `../quick-starts/README.md`
2. `../0-to-60-ai/18-AI-for-Developers.md`
3. `../developer-workflow/README.md`
4. `../local-private-ai/README.md`

### A Tech Lead or Engineering Manager

1. `../Agile-AI/README.md`
2. `../0-to-60-ai/17-AI-for-Dev-Managers.md`
3. `../quick-starts/README.md`
4. `README.md`

### An AI Coach or Transformation Lead

1. `README.md`
2. `../Agile-AI/README.md`
3. `../0-to-60-ai/README.md`
4. `02-Production-AI-Quality-And-Operations.md`

## Editorial Rules Going Forward

- Add new content to the handbook only when it advances a clear reader journey.
- Prefer linking to one canonical source instead of maintaining parallel copies of the same guidance.
- Put broad principles in the handbook and curriculum; put narrowly reusable prompts and templates in the workflow and quick-start layers.
- Keep the repo biased toward material a delivery team can use directly, rather than turning it into a general research archive.
