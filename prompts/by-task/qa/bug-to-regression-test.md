# Task: Bug to Regression Test

## When to Use
You have a real defect from Jira, QA, logs, or production and you want to convert it into a test that prevents the same failure from coming back.

## The Prompt

```text
Turn this bug into a regression test plan and, if possible, an automated test.

Bug details:
- Title: [bug title]
- Symptoms: [what users or QA saw]
- Steps to reproduce: [steps]
- Expected behavior: [expected]
- Actual behavior: [actual]
- Environment: [dev, test, staging, prod]

Repo context:
@[relevant implementation file]
@[existing similar test file]

Produce:
1. Root cause hypothesis
2. The smallest regression test that would have caught this bug
3. Whether the regression belongs in:
   - unit tests,
   - integration tests,
   - API tests,
   - UI tests,
   - or a manual regression checklist
4. The exact scenario and assertions the test should cover
5. Any supporting data setup or fixtures needed

If appropriate, generate the test code using the same conventions as the referenced test file.
```

## Follow-Up Prompt

```text
Review the regression test you just generated and tell me whether it proves the bug is fixed, or whether it only proves the current implementation detail.
```

## Tips

- This is one of the highest-value AI workflows for QA maturity.
- Favor the smallest test that proves the behavior, not the biggest test that touches the most layers.
