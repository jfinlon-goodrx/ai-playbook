# AI Development Playbook

**Repository:** [github.com/jfinlon-goodrx/ai-playbook](https://github.com/jfinlon-goodrx/ai-playbook)

A shared library of prompts, patterns, and guides for AI-assisted development. Built for a .NET/C#/MSSQL team using Cursor, GitHub, Azure DevOps, Jira, and Confluence.

## How to Use This

1. **New to AI-assisted development?** Start with `getting-started/` — install your tools, learn the basics, build your first feature with AI.
2. **Need a prompt for a specific task?** Browse `prompts/by-task/` — copy the prompt, adapt it to your story, paste it into Cursor.
3. **Want to see how experienced developers use AI?** Read `case-studies/` — real examples from our codebase.
4. **Setting up a new repo for AI?** Grab a `.cursorrules` file from `cursorrules/` — this is the single most important thing you can do to improve AI output quality.

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
├── getting-started/          # Setup guides and first steps
├── prompts/
│   ├── by-task/              # Prompts organized by development task
│   └── patterns/             # Reusable prompt patterns for complex work
├── cursorrules/              # .cursorrules files for different project types
├── case-studies/             # Real examples of AI-assisted development
└── anti-patterns/            # What to avoid and how to recover
```

## Contributing

Add your prompts and case studies via PR. Use the templates in each folder. The most valuable contributions are:

- **Prompts that worked** on our actual codebase (not generic examples)
- **Case studies** showing before/after timing ("I built X in Y hours using this approach")
- **Anti-patterns** you discovered the hard way

## Ground Rules

1. **Never paste secrets, API keys, or PII into prompts.** The AI tool may send your prompt to a cloud API.
2. **Always review AI-generated code before committing.** The AI is a fast junior developer with no judgment.
3. **Attribute AI assistance in your PR descriptions.** Not because it's required, but because it helps the team learn what works.
4. **Share what works.** If a prompt saved you hours, add it here.
