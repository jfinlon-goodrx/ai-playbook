---
name: production-incident-response
description: Guides production incident triage, containment, evidence gathering, and postmortem follow-up. Use when a live or recent incident needs structured diagnosis, rollback thinking, stakeholder-safe summaries, or blameless follow-up actions.
---
# Production Incident Response

## Quick Start

When this skill applies:

1. Start with impact, not just technical error text.
2. Separate evidence, hypotheses, and decisions.
3. Optimize first for safe containment and recovery.
4. Capture what will matter later for debugging and postmortem quality.

## Workflow

### 1. Establish the situation

Clarify:
- affected service or workflow,
- user impact,
- incident start window,
- recent deploy or config changes,
- current severity and business risk.

### 2. Triage by fault domain

Sort the issue into likely domains:
- code,
- config,
- dependency,
- environment,
- data,
- capacity or performance.

Do not collapse all symptoms into one guessed root cause too early.

### 3. Recommend immediate response

Always consider:
- containment,
- rollback,
- feature flag disablement,
- traffic reduction,
- and evidence preservation.

### 4. Improve decision quality

Call out:
- what is confirmed,
- what is still hypothesis,
- what evidence should be gathered next,
- and what would justify escalation or rollback.

### 5. Close the learning loop

After mitigation, convert the incident into:
- a short summary,
- a clean timeline,
- a root cause statement,
- and a small set of strong follow-up actions.

## Good Output

Good output includes:
- a calm plain-language summary,
- likely root-cause categories,
- practical next steps,
- rollback thinking,
- and better postmortem inputs.

## Related Prompts

- `../../prompts/by-task/ci-cd/production-incident-triage.md`
- `../../prompts/by-task/ci-cd/blameless-postmortem.md`
- `../../prompts/by-task/ci-cd/pipeline-failure-triage.md`
