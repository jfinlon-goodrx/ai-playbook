# Skills, Prompts, and MCP Playbook

This page connects three ideas that are often taught separately but work best together:

- prompts define the immediate task,
- skills define the reusable workflow,
- tools and MCP define the execution surface.

## A Useful Mental Model

### Prompts

Use prompts for:

- one specific request,
- one implementation task,
- one review pass,
- one refinement loop.

Examples:

- build this endpoint,
- review this diff,
- generate tests for this service.

### Skills

Use skills for:

- repeatable reasoning patterns,
- standardized review procedures,
- refactoring workflows,
- documentation pipelines,
- multi-step domain guidance.

Skills should encode how the assistant should think and sequence the work.

### Tools and MCP

Use tools and MCP for:

- reading and writing files,
- querying systems,
- calling APIs,
- searching documentation,
- interacting with external platforms.

MCP matters because it makes tool integrations portable across hosts instead of hard-wired to one vendor.

## The Stack

1. The user defines the objective.
2. A prompt frames the current task.
3. A skill or rule provides the workflow.
4. Tools execute the actions.
5. Guardrails constrain what is allowed.

If one layer is weak, the whole system degrades:

- no context means bad prompts,
- no skills means repeated manual orchestration,
- no tools means the assistant can only talk,
- no guardrails means the assistant can act in unsafe ways.

## Recommended Workspace Pattern

### Keep Project Facts In Context Files

Use:

- `CLAUDE.md`,
- `AGENTS.md`,
- `repo-context.yaml`,
- `.cursorrules`,
- `.cursor/rules/*.mdc`,
- architecture overviews,
- repo-specific constraints.

These should answer:

- what stack is this,
- what patterns must be followed,
- what commands matter,
- what areas are sensitive,
- what should never be changed casually.

### Keep Task Reuse In Prompt Libraries

Use task prompts for:

- new endpoints,
- refactors,
- bug investigation,
- test generation,
- review,
- documentation,
- Jira/Confluence support.

Main source:

- `../prompts/by-task/`
- `../prompts/patterns/`

### Keep Repeatable Procedures In Skills And Rules

Use skills and rules for:

- code review workflows,
- security review,
- architecture review,
- refactoring discipline,
- documentation generation,
- testing strategy.

Main source:

- `../developer-workflow/Building Skills Into Your Dev Environment.md`

## MCP Adoption Advice

Start with MCP when:

- the team needs repeatable access to real systems,
- the same tool should work across multiple AI hosts,
- governance and auditability matter,
- or browser automation is becoming a fragile workaround.

Do not start by wiring every internal system to AI. Start with a small, high-value, low-risk tool surface:

- read-only documentation search,
- read-only issue lookup,
- read-only repo or file access,
- tightly scoped developer helper tools.

Then expand to confirm-write or policy-based workflows only after trust and observability exist.

## The Most Important Safety Rule

Do not confuse “the assistant can call a tool” with “the assistant understands your business or security context.”

Even with strong tools:

- state the risk model,
- scope the permissions,
- review the outputs,
- log the actions,
- and prefer least privilege by default.

## Best Next Reads

- `../developer-workflow/Building Skills Into Your Dev Environment.md`
- `../0-to-60-ai/10-Skills-And-Tools.md`
- `../0-to-60-ai/11-MCP-And-Integrations.md`
- `../getting-started/03-project-context.md`
