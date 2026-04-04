# Task: Pipeline Failure Triage

## When to Use
An Azure Pipelines or GitHub Actions run failed and you want to get from red build to likely root cause quickly.

## The Prompt

```text
Help me triage a CI/CD pipeline failure.

Pipeline context:
- Platform: [Azure Pipelines / GitHub Actions]
- Branch or PR: [name]
- Stage or job that failed: [name]
- What changed recently: [summary]

Evidence:
- Failure log excerpt: [paste]
- Pipeline YAML: @[pipeline file]
- Relevant project files: @[solution, Dockerfile, test file, deploy file, etc.]

Analyze:
1. The likely root cause
2. Whether this is a code issue, environment issue, pipeline issue, dependency issue, or flaky test issue
3. The minimum fix to get back to green
4. The follow-up hardening work that would prevent recurrence
5. Whether this should block the team immediately or be routed as advisory

If the log is noisy, identify which lines matter most and which ones are probably downstream noise.
```

## Follow-Up Prompt

```text
Based on the diagnosis, propose:
- the code or YAML change,
- the validation steps to confirm the fix,
- and one improvement to make future failures easier to diagnose.
```

## Tips

- This is best when you include the exact failing step, not the whole pipeline history.
- Ask the AI to separate first failure from cascade failures.
