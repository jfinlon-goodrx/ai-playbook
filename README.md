# AI Engineering Playbook

**Repository:** [github.com/jfinlon-goodrx/ai-playbook](https://github.com/jfinlon-goodrx/ai-playbook)

A shared library of prompts, patterns, guides, rules, and operating-model material for AI-assisted software delivery.

This repo is for Agile delivery teams, not just individual developers. It is intended to help:

- developers
- QA and test automation engineers
- DevOps and platform engineers
- tech leads and architects
- engineering managers
- product and delivery partners

The repo started with a strong .NET and Azure bias. It is now being expanded to stay useful for .NET, Python, and Go teams.

## Start Here

### If you are new to AI-assisted development

Start with:

- `getting-started/`
- `quick-starts/README.md`
- `0-to-60-ai/README.md`

### If you need a task workflow today

Start with:

- `prompts/by-task/README.md`
- `prompts/patterns/`
- `skills/README.md`

### If you are leading team adoption

Start with:

- `Agile-AI/README.md`
- `handbook/README.md`
- `handbook/04-End-to-End-Enterprise-AI-Delivery-Platform.md`
- `REPO-ENHANCEMENT-PLAN.md`

### If you are setting up agent or editor context

Start with:

- `AGENTS.md`
- `CLAUDE.md`
- `repo-context.yaml`
- `.cursor/rules/`
- `cursorrules/`

## Entry Points By Role

- Developers: `quick-starts/developer-starter.md`
- Frontend and UI: `quick-starts/frontend-ui-starter.md`
- QA and test automation: `quick-starts/qa-starter.md`
- DevOps and platform: `quick-starts/devops-platform-starter.md`
- Tech leads and architects: `quick-starts/tech-lead-starter.md`
- Engineering managers: `quick-starts/manager-starter.md`
- Product and delivery: `quick-starts/product-delivery-starter.md`

## Entry Points By Stack

- .NET: `prompts/by-task/backend/new-api-endpoint.dotnet.md`, `prompts/by-task/backend/story-to-vertical-slice.dotnet.md`, `prompts/by-task/backend/database-migration.dotnet.md`, `cursorrules/dotnet-api.cursorrules`
- Python: `prompts/by-task/backend/new-api-endpoint.python.md`, `prompts/by-task/backend/story-to-vertical-slice.python.md`, `prompts/by-task/backend/database-migration.python.md`, `prompts/by-task/qa/test-generation.python.md`
- Go: `prompts/by-task/backend/new-api-endpoint.go.md`, `prompts/by-task/backend/story-to-vertical-slice.go.md`, `prompts/by-task/backend/database-migration.go.md`, `prompts/by-task/qa/test-generation.go.md`

## Quick Start (5 minutes)

If you want one fast, concrete first win:

1. Pick the project rules closest to your stack from `cursorrules/`.
2. Add repo context using `.cursorrules`, `CLAUDE.md`, or `.cursor/rules/*.mdc`.
3. Open the closest workflow prompt in `prompts/by-task/`.
4. Point the assistant at real files in your repo.
5. Ask for a reviewable artifact, not a magic solution.

Example starter prompt:

```text
Read the project rules and the closest existing implementation pattern first.

Then add a new GET /api/health endpoint that returns:
{
  "status": "ok",
  "timestamp": "<utc now>"
}

Match the repo's existing architecture, validation, error handling, and test style.
Include tests and call out any assumptions before implementation.
```

## Repository Shape

```text
ai-playbook/
├── Agile-AI/                 # AI-native delivery and adoption guidance
├── 0-to-60-ai/               # Curated practical education subset
├── handbook/                 # Canonical maps, quality, and structure guidance
├── quick-starts/             # Role-based and fast-start guides
├── developer-workflow/       # Day-to-day engineering workflow help
├── guides-by-platform/       # Editor, tool, and integration guidance
├── local-private-ai/         # Private AI and internal knowledge workflows
├── getting-started/          # Setup guides and first steps
├── prompts/
│   ├── by-task/              # Task workflows and stack variants
│   └── patterns/             # Reusable prompt patterns for complex work
├── skills/                   # Portable workflow skills for recurring tasks
├── cursorrules/              # Reusable `.cursorrules` examples
├── .cursor/rules/            # Cursor rule files for this repo itself
├── case-studies/             # Real examples of AI-assisted development
├── anti-patterns/            # What to avoid and how to recover
├── AGENTS.md                 # Repo guidance for coding agents
├── CLAUDE.md                 # Repo guidance for Claude-style hosts
└── repo-context.yaml         # Machine-readable repo context
```

## Prompt Areas

- `prompts/by-task/backend/` for backend workflows with shared plus language-specific variants
- `prompts/by-task/frontend/` for React, Blazor, Razor, and server-rendered UI work
- `guides-by-platform/UI/` for Figma workflow, stack choices, and frontend process guidance
- `prompts/by-task/atlassian/` for Jira and Confluence workflows
- `handbook/04-End-to-End-Enterprise-AI-Delivery-Platform.md` for Gleen, Jira, Confluence, GitHub, MCP, and end-to-end AI delivery design
- `prompts/by-task/documentation/` for repo docs, runbooks, and architecture writing
- `prompts/by-task/qa/` for test planning, automation, and regression work
- `prompts/by-task/review/` for review, security, and PR-quality prompts
- `prompts/by-task/ci-cd/` for pipelines, deployment workflows, and incident support
- `prompts/by-task/environments/` for ephemeral and preview environment planning

## Language Variant Convention

When a workflow applies across stacks, this repo now uses:

- `topic.md` for the shared workflow
- `topic.dotnet.md` for .NET examples
- `topic.python.md` for Python examples
- `topic.go.md` for Go examples

This keeps the shared reasoning in one place while letting code and tool examples stay idiomatic.

## Best Team Wins

If you want the highest-value starting points for a team:

- Delivery model change: `Agile-AI/README.md`
- Practical team rollout: `quick-starts/01-AI-Assisted-Engineering-Starter-Kit.md`
- Role-based onboarding: `0-to-60-ai/README.md`
- Enterprise AI delivery platform: `handbook/04-End-to-End-Enterprise-AI-Delivery-Platform.md`
- Cross-platform backend workflows: `prompts/by-task/backend/README.md`
- UI implementation and design handoff: `quick-starts/frontend-ui-starter.md`
- Faster QA feedback: `prompts/by-task/qa/test-plan-from-story.md`
- Pipeline recovery: `prompts/by-task/ci-cd/pipeline-failure-triage.md`
- Better incident response: `prompts/by-task/ci-cd/production-incident-triage.md`
- Reusable team workflow skills: `skills/README.md`

## Contributing

The most valuable contributions are:

- prompts that worked on a real codebase
- skills that encode repeatable workflows
- case studies showing what changed in speed or quality
- cross-language variants for shared workflows
- anti-patterns and recovery patterns learned the hard way

Before adding a new file, check:

1. Is there already a canonical page for this idea?
2. Is this cross-platform or stack-specific?
3. Should it be a shared file plus `.dotnet`, `.python`, and `.go` companions?
4. What role should discover this first?

## Ground Rules

1. Never paste secrets, API keys, or PII into prompts.
2. Always review AI-generated code before committing.
3. Keep stack-specific advice clearly labeled.
4. Share what worked so the next teammate starts faster.
