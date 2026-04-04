# Task: Ephemeral Environment Plan

## When to Use
You want to design or improve preview environments, short-lived test stacks, or story-specific deployment environments for faster validation.

## The Prompt

```text
Design an ephemeral environment workflow for this application and delivery model.

Context:
- App type: [API / web app / microservice / full stack]
- Stack: [e.g. .NET 8, React, Azure SQL, Redis, Service Bus]
- Current CI/CD: [Azure Pipelines / GitHub Actions / mixed]
- Current pain: [slow QA handoff, shared test environment collisions, hard-to-review changes]

Goals:
- Create short-lived environments per PR, branch, or story
- Keep cost and cleanup under control
- Make QA and product review easier
- Support safe teardown and rollback

Produce:
1. The recommended environment lifecycle:
   - create
   - seed
   - validate
   - review
   - tear down
2. Trigger options and naming conventions
3. Required infrastructure components
4. Data strategy for safe test data
5. CI/CD integration points
6. Risks, cost controls, and cleanup guardrails
7. A phased adoption plan for the team
```

## Follow-Up Prompt: Review Our Current Approach

```text
Review our current preview or ephemeral environment approach and tell me:
- where the friction is,
- what can be automated,
- what should stay manual,
- and what would make QA, product, and engineering reviews faster without creating environment sprawl.
```

## Tips

- This is most valuable when shared test environments are a bottleneck.
- Ask the AI to include cleanup and TTL rules explicitly, not as an afterthought.
