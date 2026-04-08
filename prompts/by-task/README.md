# By-Task Prompts

This folder contains copy-paste prompts organized by functional area.

## Areas

- `backend/`: shared backend workflows with `.dotnet`, `.python`, and `.go` companions where needed.
- `dotnet/`: legacy entry points and .NET-specific pointers kept for compatibility.
- `frontend/`: React and Blazor component work for teams with UI responsibilities.
- `atlassian/`: Jira and Confluence workflows for story shaping, handoff, and documentation.
- `documentation/`: repo docs, runbooks, and architecture-documentation prompts.
- `qa/`: test planning, automation generation, defect analysis, and regression planning.
- `review/`: PR review, security review, and description-writing prompts.
- `ci-cd/`: Azure Pipelines, GitHub Actions, and deployment automation prompts.
- `environments/`: ephemeral environments, preview deployments, and lifecycle workflow prompts.

## Best Starting Points

- Newer team members: `../../getting-started/05-first-week-with-ai.md`
- Cross-platform backend work: `backend/README.md`
- QA acceleration: `qa/test-plan-from-story.md` and `qa/bug-to-regression-test.md`
- Safer review: `review/code-review-assist.md` and `review/security-audit.md`
- Pipeline speed: `ci-cd/pipeline-failure-triage.md`
- Production response: `ci-cd/production-incident-triage.md`
- Safer team handoff: `atlassian/jira-story-to-delivery-plan.md`

## How To Use These

1. Start with the prompt closest to the artifact you need.
2. Replace placeholders with your repo, story, environment, and tool details.
3. Point the AI at real project files so it matches your conventions.
4. Review the result like any other engineering artifact before using it.

## Naming Pattern

When a workflow exists for multiple stacks, the repo now prefers:

- `topic.md` for the shared workflow
- `topic.dotnet.md`
- `topic.python.md`
- `topic.go.md`

This keeps the shared task framing in one place while letting implementation examples stay idiomatic.
