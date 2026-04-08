# Task: Service Pipeline for .NET

## Prompt

```text
Generate or update a CI/CD pipeline for this .NET service.

Repo context:
@[solution file]
@[existing pipeline file]
@[Dockerfile if present]

Requirements:
- restore, build, and test the solution
- publish test results and code coverage
- run linting or analyzers if the repo already uses them
- build and publish a container image if the service is containerized
- deploy to [App Service / AKS / Container Apps / staging / prod]
- use environment-based deployment jobs where appropriate
- use placeholders for service connections, variable groups, and secret references
- keep secrets out of logs and inline YAML

Also include:
1. assumptions
2. placeholders I must replace manually
3. security review notes
4. rollback considerations
```
