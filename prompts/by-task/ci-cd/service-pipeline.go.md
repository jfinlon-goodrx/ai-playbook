# Task: Service Pipeline for Go

## Prompt

```text
Generate or update a CI/CD pipeline for this Go service.

Repo context:
@[go.mod]
@[existing pipeline file]
@[Dockerfile if present]

Requirements:
- restore modules and verify dependency state
- run formatting or linting tools used by the repo
- run unit tests, and race tests where appropriate
- publish test results where the platform supports it
- build the service binary and container image if applicable
- deploy to [target platform]
- use placeholders for secrets, connections, and environment references
- keep secrets out of logs and inline config

Also include:
1. assumptions
2. placeholders I must replace manually
3. security review notes
4. rollback considerations
```
