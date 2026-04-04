# Task: Production Incident Triage

## When to Use
A production issue is active or recently occurred, and the team needs a fast, structured way to understand impact, likely cause, and next actions.

## The Prompt

```text
Help me triage a production incident.

Incident context:
- Service or system: [name]
- User impact: [what users are seeing]
- Start time: [UTC if known]
- Severity: [SEV-1 / SEV-2 / etc.]
- Recent changes: [deployments, config changes, data changes]

Evidence:
- Error logs or trace excerpts: [paste]
- Metrics or alerts: [paste]
- Relevant KQL or monitoring output: [paste if available]
- Pipeline or deploy info: @[pipeline or workflow file if relevant]
- Key source files: @[relevant code files]

Return:
1. Incident summary in plain language
2. Most likely fault domains:
   - code defect,
   - dependency failure,
   - configuration issue,
   - environment drift,
   - data issue,
   - capacity/performance issue
3. Immediate containment options
4. Fastest safe path to mitigation or rollback
5. What evidence is still missing
6. What to capture for the postmortem

Separate confirmed evidence from hypotheses. Do not overstate certainty.
```

## Follow-Up Prompt

```text
Based on the evidence so far, propose:
- the next 3 diagnostic steps,
- the decision point for rollback,
- and the safest communication summary for stakeholders.
```

## Tips

- Use this when minutes matter and the logs are noisy.
- Ask the AI to distinguish user-facing impact from technical symptoms.
