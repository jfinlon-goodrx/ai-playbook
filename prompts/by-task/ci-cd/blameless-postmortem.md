# Task: Blameless Postmortem

## When to Use
An incident or major production issue has been mitigated and the team needs a useful postmortem that improves systems instead of just retelling the outage.

## The Prompt

```text
Write a blameless postmortem for this incident.

Inputs:
- Incident summary: [paste]
- Timeline notes: [paste]
- Root cause analysis: [paste]
- User impact: [paste]
- Mitigation and rollback steps: [paste]
- Follow-up ideas: [paste]

Output structure:
1. Summary
2. Impact
3. Timeline
4. Detection
5. Root Cause
6. Contributing Factors
7. What Went Well
8. What Was Hard
9. Action Items
10. Owners and target dates

Rules:
- Keep it blameless and factual
- Separate root cause from contributing factors
- Turn vague lessons into specific actions
- Call out missing observability, weak runbooks, or pipeline gaps when they mattered
```

## Follow-Up Prompt

```text
Review this postmortem and tell me:
- which action items are strongest,
- which are vague,
- which should be engineering work vs. process work,
- and which one or two changes would most reduce recurrence risk.
```

## Tips

- The best postmortems produce fewer, stronger actions rather than a long wish list.
- Ask for timeline gaps explicitly when the evidence is incomplete.
