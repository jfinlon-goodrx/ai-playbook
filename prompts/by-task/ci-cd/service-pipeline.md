# Task: Service Pipeline

## When to Use

You need a CI/CD pipeline for a service or you want to upgrade an existing one without losing test, security, or deployment discipline.

## Use With

- `.dotnet`: `service-pipeline.dotnet.md`
- `.python`: `service-pipeline.python.md`
- `.go`: `service-pipeline.go.md`

## Prompt

```text
Generate or update a CI/CD pipeline for this service.

Repo context:
@[project file or manifest]
@[existing pipeline file if present]
@[Dockerfile if present]
@[deployment config if present]

Requirements:
- restore or install dependencies
- build the service
- run linting, tests, and coverage where appropriate
- publish test evidence
- build and publish an image or artifact if applicable
- deploy to [environment target]
- keep secrets out of logs and inline config
- call out placeholders that must be replaced manually

Also include:
1. assumptions
2. security review notes
3. rollback considerations
4. environment and approval assumptions
```

## Review Checklist

- Are tests and evidence publication included?
- Are secrets referenced safely?
- Are deployment stages explicit and reviewable?
- Is rollback or safe recovery addressed?
- Does the pipeline match the actual service packaging and runtime model?
