# Task: Test Plan from Story

## When to Use
QA needs to turn a Jira story, epic, or requirements doc into a practical test plan and a targeted test case set.

## The Prompt

```text
You are a senior QA lead. Build a test plan from this feature description.

Inputs:
- Story or epic: [paste Jira content]
- Acceptance criteria: [paste]
- Architecture notes: [paste if available]
- Stack: [e.g. .NET 8 API, React, Azure SQL, Service Bus]
- Test tools: [e.g. Azure Test Plans, Playwright, REST Assured, Postman]

Produce:
1. Objective
2. Scope and out-of-scope
3. Test strategy by level:
   - Unit
   - Integration
   - API
   - UI
   - Regression
   - Security
4. Environment and test data needs
5. Risks and failure modes
6. Entry and exit criteria
7. Suggested test cases grouped by:
   - Happy path
   - Negative
   - Boundary
   - Edge case
   - Authorization / permissions
   - Operational or rollout risk

Format it so it can be pasted into Confluence or Azure Test Plans.
```

## Follow-Up Prompt: Prioritize the Regression Slice

```text
From the test plan you just created, identify the smallest high-value regression suite we should automate first.

Explain why each selected test belongs in the first automation pass.
```

## Tips

- This is best used before coding completes so QA and development can align early.
- Ask for authorization and data-state scenarios explicitly; they are often missed.
