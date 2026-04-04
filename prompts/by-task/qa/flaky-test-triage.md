# Task: Flaky Test Triage

## When to Use
Tests pass locally but fail in CI, fail intermittently, or create noise that slows review and release flow.

## The Prompt

```text
Help me triage a flaky test.

Context:
- Test name: [test name]
- Framework: [xUnit / Playwright / NUnit / Jest / etc.]
- Where it fails: [local / CI / both]
- Frequency: [e.g. 1 in 10 runs]
- Symptoms: [timeout, selector not found, race condition, data mismatch]

Evidence:
- Recent failure output: [paste]
- Relevant logs: [paste]
- Test code: @[test file]
- Supporting fixture or helper: @[fixture/helper file]
- Pipeline file if relevant: @[pipeline/workflow file]

Analyze:
1. The most likely causes of flakiness
2. What evidence points to each cause
3. What additional logging or diagnostics would narrow it down
4. The safest fix
5. Whether this test should be stabilized, rewritten, quarantined temporarily, or demoted from the critical path

Prefer practical fixes over idealized rewrites.
```

## Follow-Up Prompt

```text
Now propose a hardening plan for this test:
- code changes,
- fixture or data changes,
- environment changes,
- retry or timeout policy,
- and whether the pipeline should treat this as blocking or advisory while we stabilize it.
```

## Tips

- Ask the AI to distinguish product bugs from test bugs.
- Intermittent failures often come from environment assumptions, async timing, shared data, or weak assertions.
