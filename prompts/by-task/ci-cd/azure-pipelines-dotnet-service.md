# Task: Azure Pipelines for a .NET Service

## When to Use
You need an Azure Pipelines YAML for a .NET service, or you want to improve an existing one without losing environment, security, or deployment discipline.

## The Prompt

```text
Generate or update an Azure Pipelines YAML for this .NET service.

Repo context:
@[solution file]
@[existing pipeline file if present]
@[Dockerfile if present]

Requirements:
- Restore, build, and test the solution
- Publish test results and code coverage
- Build and push a container image if the service is containerized
- Deploy to [App Service / AKS / Container Apps / staging only / staging + prod]
- Use environment-based deployment jobs where appropriate
- Use placeholders for service connections, variable groups, and secret references
- Keep secrets out of logs and inline YAML

Also include:
1. Assumptions the pipeline is making
2. Placeholders I must replace manually
3. Security review notes
4. Rollback considerations
```

## Follow-Up Prompt: Review This Pipeline

```text
Review this Azure Pipelines YAML for:
- missing test or coverage publication,
- unsafe secret handling,
- bad service connection assumptions,
- poor stage boundaries,
- missing deployment approval points,
- weak rollback or environment controls.

Explain what should change before the team relies on it.
```

## Tips

- AI is good at the YAML shape, but bad at knowing your real Azure DevOps project names.
- Use this to create a first draft, then verify every environment-specific reference.
