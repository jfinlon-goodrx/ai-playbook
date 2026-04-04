---
name: pipeline-failure-triage
description: Diagnoses CI/CD failures and separates root cause from noisy downstream errors. Use when Azure Pipelines or GitHub Actions fail and the team needs a quick path from red build to likely fix and hardening steps.
---
# Pipeline Failure Triage

## Quick Start

When this skill applies:

1. Read the failing stage, job, and exact error output.
2. Identify the first meaningful failure, not the last noisy line.
3. Classify the issue as code, pipeline, environment, dependency, or flaky test.
4. Recommend the smallest fix that gets signal back quickly.

## Workflow

### 1. Find the real failure

Prioritize:
- the first broken task,
- the exact command or step,
- and the nearest actionable error.

Do not confuse cascade failures with the root cause.

### 2. Map the failure type

Check whether the break is caused by:
- source code changes,
- test instability,
- missing environment configuration,
- pipeline YAML drift,
- dependency or package issues,
- or deploy-time assumptions.

### 3. Propose two levels of response

Always give:
- the immediate fix to restore flow,
- and the hardening step that prevents recurrence.

### 4. Improve signal quality

Recommend one or more of:
- better logging,
- clearer artifact publication,
- more focused stage boundaries,
- failure summaries,
- or earlier validation steps.

## Good Output

Good output includes:
- the likely root cause,
- what evidence supports it,
- what change should happen now,
- and one operational improvement for the pipeline.

## Related Prompts

- `../../prompts/by-task/ci-cd/pipeline-failure-triage.md`
- `../../prompts/by-task/qa/flaky-test-triage.md`
