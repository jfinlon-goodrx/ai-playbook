# Task: GitHub Actions for PR Quality Gates

## When to Use
You want GitHub Actions to support code review, testing, packaging, or deployment checks for a .NET or web repo.

## The Prompt

```text
Generate or improve a GitHub Actions workflow for this repository.

Repo context:
@[solution or package manifest]
@[existing workflow if present]
@[test project or example test file]

Workflow goals:
- Trigger on pull requests and/or pushes to main
- Restore dependencies, build, and run tests
- Publish artifacts or coverage if useful
- Enforce quality gates before merge
- Keep steps modular and readable
- Use GitHub Environments or approvals if deployment is involved

Return:
1. The workflow YAML
2. Required secrets or environment settings as placeholders
3. What the workflow protects us from
4. What still requires human review
```

## Follow-Up Prompt: PR Automation Review

```text
Review this GitHub Actions workflow for:
- wasted runtime,
- missing cache opportunities,
- unsafe secret handling,
- weak branch protections,
- missing artifact or test result publication,
- deploy steps that should be gated by environment approvals.
```

## Tips

- Pair this with `../../patterns/ai-testing-and-review-pipeline.md` when you want AI-assisted review plus CI.
- Keep deployment workflows separate from simple PR validation when possible.
