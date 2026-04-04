# AI Onboarding Prompts

Starter prompts for kicking off AI-assisted delivery work in a corporate engineering environment.

These are designed for a dotnet- and Azure-oriented team and assume that the best workflow has three clear stages:

1. shape the requirements and working documents,
2. build the plan,
3. execute the work.

Do not jump straight to code when the docs are weak. The highest-leverage use of AI is often improving the task definition first.

## Recommended Workflow

### Stage 1: Requirements and Working Docs

Use this first. The goal is to make sure the team has enough clarity to start safely.

```
I would like to build or improve an application that {{purpose of application or feature}}.

Our environment is primarily dotnet, Azure, and enterprise delivery. Assume we care about maintainability, observability, performance, security, and practical team adoption across developers, QA, product, and managers.

Please read the documents in {{docs path}} and tell me:

1. whether the requirements and supporting documents are complete enough to start,
2. what is ambiguous, missing, or contradictory,
3. what additional acceptance criteria, risks, architectural notes, QA scenarios, rollout constraints, or operational guardrails should be added,
4. what should be clarified before implementation begins.

Please improve the documentation set first before planning implementation. If useful, propose updates for:

- requirements,
- architecture notes,
- API or data contracts,
- acceptance criteria,
- test strategy,
- rollout notes,
- security and compliance constraints.

Create or update any lightweight project-memory files that would help future sessions resume cleanly, such as task-list, session-state, lessons-learned, or decision-log files.
```

### Stage 2: Planning

Use this after the docs are strong enough. The goal is to create an implementation plan that a real team can execute and review.

```
Using the approved requirements and project documents in {{docs path}}, build an implementation plan for a production-quality solution.

Assume a corporate delivery team using dotnet, Azure, Git, PR review, and normal QA/release controls.

Please produce:

1. a phased implementation plan,
2. the recommended solution shape and project structure,
3. key risks and unknowns,
4. testing strategy across unit, integration, QA, and operational validation,
5. rollout considerations,
6. suggested prompt, rules, or reusable workflow assets the team should save for repeat use.

Prefer vertical slices over isolated layer-by-layer work when that improves learning and delivery speed.

Call out what product, QA, engineering, and managers each need to align on before execution starts.
```

### Stage 3: Execution

Use this when the requirements and plan are already in place.

```
Use the approved requirements in {{docs path}} and the implementation plan in {{plan path}} to execute the work.

Follow strong software engineering practices for a dotnet and Azure environment. Prefer maintainable architecture, explicit trade-offs, clear tests, and safe incremental changes.

While executing:

- keep task-list, session-state, lessons-learned, and decision-log files current,
- update project documentation when implementation clarifies the design,
- add or update repo rules and ignore files when new project types or tooling require them,
- surface risks early instead of guessing,
- keep QA, rollout, and observability implications visible as part of the work.

If the requirements or plan prove incomplete, pause and propose document updates before continuing.
```

## Variations

### Corporate RAG or Internal Knowledge Assistant

Use the same three-stage flow, but make sure Stage 1 explicitly asks for:

- document ownership and freshness,
- retrieval scope,
- access-control requirements,
- hallucination and source-citation expectations,
- evaluation and observability requirements.

### Agentic or MCP-Enabled Business Workflow

Use the same three-stage flow, but make sure Stage 1 and Stage 2 explicitly cover:

- tool permissions,
- approval boundaries,
- failure handling,
- auditability,
- abuse and prompt-injection risks,
- what actions must remain human-approved.

## Rule Of Thumb

For enterprise AI work, the best sequence is:

1. improve the docs,
2. improve the plan,
3. then improve the implementation.
