# Task: Service Pipeline for Python

## Prompt

```text
Generate or update a CI/CD pipeline for this Python service.

Repo context:
@[pyproject.toml or requirements file]
@[existing pipeline file]
@[Dockerfile if present]

Requirements:
- install dependencies using the repo's preferred tool (uv, pip, poetry, etc.)
- run formatting, linting, type checks, and tests where configured
- publish test results and coverage
- build and publish an image or deployable artifact if applicable
- deploy to [target platform]
- use placeholders for secrets, connections, and environment references
- keep secrets out of logs and inline config

Also include:
1. assumptions
2. placeholders I must replace manually
3. security review notes
4. rollback considerations
```
