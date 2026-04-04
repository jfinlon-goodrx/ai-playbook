# Task: Playwright and API Automation

## When to Use
You want AI to turn manual test cases into automation that fits the team's existing Playwright, API, or pipeline conventions.

## The Prompt

```text
Convert these manual tests into automation that matches our project patterns.

Manual test cases:
[paste test cases]

Project context:
@[existing Playwright test or API test]
@[existing page object or fixture file]
@[pipeline file if relevant]

Conventions:
- Framework: [Playwright / REST Assured / xUnit API integration tests]
- Language: [TypeScript / C# / Java]
- Selector strategy: [data-testid, aria roles, CSS, etc.]
- Test naming style: [describe]
- Retry / wait conventions: [describe]
- Environment setup: [describe]

Generate:
1. The automation files needed
2. Any page objects, fixtures, or helpers
3. Notes on what needs human verification
4. A short explanation of how to run these in CI

Do not invent patterns that are not present in the project. Match the existing style first.
```

## Follow-Up Prompt: Review the Generated Tests

```text
Review the generated automation for:
- flaky waits,
- weak assertions,
- missing cleanup,
- poor selector choices,
- hidden environment assumptions,
- gaps between the original manual test intent and the automated coverage.
```

## Tips

- Always provide one real example test from the repo.
- Ask for CI notes so the automation does not stop at “works on my machine.”
