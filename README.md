# AI Development Playbook

**Repository:** [github.com/jfinlon-goodrx/ai-playbook](https://github.com/jfinlon-goodrx/ai-playbook)

A shared library of prompts, patterns, guides, and adoption material for AI-assisted development. Built for a .NET/C#/MSSQL team using Cursor, GitHub, Azure DevOps, Jira, and Confluence.

## How to Use This

1. **New to AI-assisted development?** Start with `getting-started/` — install your tools, learn the basics, build your first feature with AI.
2. **Need a prompt for a specific task?** Browse `prompts/by-task/` — copy the prompt, adapt it to your story, paste it into Cursor.
3. **Rolling AI out to a team?** Start with `Agile-AI/`, `quick-starts/`, and `handbook/`.
4. **Need a curated learning track?** Read `0-to-60-ai/` for the practical subset of the larger AI curriculum.
5. **Want to see how experienced developers use AI?** Read `case-studies/` — real examples from our codebase.
6. **Setting up a new repo for AI?** Grab a `.cursorrules` file from `cursorrules/` — this is the single most important thing you can do to improve AI output quality.
7. **Need a reusable workflow?** Browse `skills/` — these are portable skill definitions for recurring delivery tasks.

## Quick Start (5 minutes)

If you're in a hurry and just want to try one thing:

1. Copy `cursorrules/dotnet-api.cursorrules` into the root of your .NET project as `.cursorrules`
2. Open the project in Cursor
3. Open Composer (Ctrl+Shift+I) and paste this:

```
Read the .cursorrules file, then read the Program.cs and any existing controllers.
Add a new API endpoint: GET /api/health that returns { "status": "ok", "timestamp": "<utc now>" }.
Include a unit test.
```

4. Watch what happens. Then come back and read the rest of this playbook.

## Structure

```
ai-playbook/
├── Agile-AI/                 # AI-native delivery and adoption guidance
├── 0-to-60-ai/               # Curated practical education subset
├── handbook/                 # Higher-level map and production guidance
├── quick-starts/             # Fast practical team adoption guides
├── developer-workflow/       # Day-to-day engineering workflow help
├── guides-by-platform/       # Tool-specific guides
├── local-private-ai/         # Private AI, knowledge, and onboarding workflows
├── getting-started/          # Setup guides and first steps
├── prompts/
│   ├── by-task/              # Prompts organized by functional area and task
│   └── patterns/             # Reusable prompt patterns for complex work
├── skills/                   # Portable workflow skills for recurring team tasks
├── cursorrules/              # .cursorrules files for different project types
├── case-studies/             # Real examples of AI-assisted development
└── anti-patterns/            # What to avoid and how to recover
```

## Prompt Areas

- `prompts/by-task/dotnet/` — implementation planning and .NET feature delivery.
- `prompts/by-task/frontend/` — React and Blazor component work.
- `prompts/by-task/atlassian/` — Jira and Confluence workflows.
- `prompts/by-task/documentation/` — architecture docs, runbooks, and repo documentation prompts.
- `prompts/by-task/qa/` — test planning, automation, and regression work.
- `prompts/by-task/review/` — review, security, and PR-quality prompts.
- `prompts/by-task/ci-cd/` — Azure Pipelines and GitHub Actions support.
- `prompts/by-task/environments/` — ephemeral and preview environment planning.

## Skills

The `skills/` folder fills a gap in the original playbook: repeatable team workflows that are too structured for a one-off prompt but lighter than full project rules.

## Best Team Wins

If you want the highest-value starting points for a team:

- Delivery model change: `Agile-AI/README.md`
- Practical team rollout: `quick-starts/01-AI-Assisted-Engineering-Starter-Kit.md`
- Manager/developer/QA/DevOps onboarding: `0-to-60-ai/README.md`
- Faster QA feedback: `prompts/by-task/qa/test-plan-from-story.md`
- Regression discipline: `prompts/by-task/qa/bug-to-regression-test.md`
- Flaky test cleanup: `prompts/by-task/qa/flaky-test-triage.md`
- Faster pipeline recovery: `prompts/by-task/ci-cd/pipeline-failure-triage.md`
- Better incident response: `prompts/by-task/ci-cd/production-incident-triage.md`
- Confidence-building onboarding: `getting-started/05-first-week-with-ai.md`
- Reusable team workflow skills: `skills/README.md`

## Contributing

Add your prompts and case studies via PR. Use the templates in each folder. The most valuable contributions are:

- **Prompts that worked** on our actual codebase (not generic examples)
- **Skills that encode repeatable workflows** the team wants to reuse
- **Case studies** showing before/after timing ("I built X in Y hours using this approach")
- **Anti-patterns** you discovered the hard way

## Ground Rules

1. **Never paste secrets, API keys, or PII into prompts.** The AI tool may send your prompt to a cloud API.
2. **Always review AI-generated code before committing.** The AI is a fast junior developer with no judgment.
3. **Attribute AI assistance in your PR descriptions.** Not because it's required, but because it helps the team learn what works.
4. **Share what works.** If a prompt saved you hours, add it here.
