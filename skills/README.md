# Skills

This folder contains reusable skill definitions for common team workflows.

These are written as portable `SKILL.md` assets so they can be adapted into project skills, personal skills, or other agent guidance systems later.

The current skills skew toward .NET and enterprise delivery patterns because that is how the repo began. As the repo expands, prefer cross-platform workflow framing and make stack assumptions explicit inside each skill.

## Current Skills

- `dotnet-feature-delivery/`: move from Jira story to implementation-ready .NET delivery work.
- `end-to-end-ai-delivery/`: connect knowledge retrieval, work management, code, testing, and handoff across enterprise systems.
- `jira-confluence-delivery/`: shape stories, hand off technical changes, and keep project knowledge current.
- `qa-automation-planning/`: create test plans, convert manual tests into automation, and tighten regression coverage.
- `ci-ephemeral-environments/`: generate safer CI/CD, preview environment, and deployment workflow plans.
- `pipeline-failure-triage/`: shorten time-to-diagnosis when CI/CD breaks.
- `production-incident-response/`: guide structured incident triage, containment, and postmortem follow-up.
- `newbie-ai-pairing/`: help newer teammates build safe habits and confidence with LLM-assisted work.

## Guidance

- Use a prompt when you need a one-off artifact.
- Use a skill when you want a repeatable workflow with guardrails and expected output shape.
- Keep skills concise and procedural. Link to prompts or reference docs when deeper detail is needed.
