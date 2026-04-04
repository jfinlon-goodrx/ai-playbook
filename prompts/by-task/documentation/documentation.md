# Task: Documentation Generation

## When to Use
You need XML doc comments, README files, architecture docs, API documentation, or runbooks. AI reads code more thoroughly than most humans and produces accurate documentation when pointed at the right files.

## The Prompt: XML Doc Comments for a Class

```
@[TargetFile].cs

Add XML doc comments to every public member in this file:
- Classes and interfaces: summary of purpose and usage
- Methods: summary, param descriptions, returns, exceptions thrown
- Properties: summary of what the value represents

Style:
- Concise but informative
- Use <see cref=""/> for cross-references to other types
- Include <example> blocks for non-obvious usage
- Match the existing doc comment style in this project

Do NOT add comments to private members.
Do NOT add obvious comments ("Gets or sets the Name" on a Name property).
Focus on why and how, not what.
```

## The Prompt: Architecture Documentation

```
Read the entire solution structure. Analyze every project and its dependencies.

Write an ARCHITECTURE.md that covers:

1. **Solution Overview** — what the system does in 2-3 sentences
2. **Project Structure** — table of each project, its responsibility, and what it depends on
3. **Layer Architecture** — how API, Application, Domain, and Infrastructure layers interact
4. **Data Flow** — how a request flows from HTTP to database and back
5. **Authentication & Authorization** — how users are authenticated, how roles/permissions work
6. **Data Access** — which ORM/library, how queries are structured, connection management
7. **Error Handling** — the error handling strategy (exceptions, Result<T>, ProblemDetails)
8. **Security & Compliance** — HIPAA compliance patterns, PHI handling, audit logging
9. **Testing Strategy** — what's tested, how, and where tests live
10. **Configuration** — how the app is configured (appsettings, env vars, feature flags)
11. **Deployment** — how the app is built and deployed (CI/CD, Docker, Azure)
12. **Key Domain Concepts** — the core business entities and their relationships

Make it accurate to what the code actually does, not aspirational.
Include file path references so readers can navigate to the relevant code.
```

## The Prompt: API Documentation

```
@Controllers/

Read all controllers and generate API documentation:

For each endpoint:
| Method | Route | Auth | Request | Response | Description |
|---|---|---|---|---|---|

Group by controller/resource.

For complex endpoints, include:
- Full request body schema with field descriptions
- Full response body schema
- Error responses (400, 401, 403, 404, 500) with example bodies
- Example curl commands

If the project has Swagger/OpenAPI, verify the generated docs match.
```

## The Prompt: Runbook / Operations Guide

```
Read the deployment configuration (Dockerfile, docker-compose, CI/CD pipeline,
appsettings) and write an operations runbook:

1. **How to build** — commands to build locally and in CI
2. **How to run** — local development setup, Docker, staging, production
3. **Environment variables** — every env var the app uses, with descriptions and example values
4. **Health checks** — what endpoints to monitor, expected responses
5. **Common issues** — startup failures, database connection issues, auth problems
6. **How to debug** — enabling debug logging, attaching a debugger, reading logs
7. **How to deploy** — step-by-step deployment process
8. **How to rollback** — what to do if a deployment goes wrong
9. **Monitoring** — what metrics matter, where to find dashboards
10. **Emergency contacts** — who to page for what (leave as placeholders)

Write for an on-call engineer who has never seen this project before.
```

## The Prompt: Jira Story from Code Changes

```
I've made these code changes. Write a Jira story description that captures
what was implemented, for our documentation and traceability.

Format:
**Summary:** [one-line title]

**Description:**
As a [user role], I want [capability] so that [business value].

**Acceptance Criteria:**
- [ ] [Criterion 1]
- [ ] [Criterion 2]

**Technical Notes:**
[Key implementation details, architectural decisions, PHI handling]

**Test Evidence:**
[What was tested and the results]
```

## Tips
- The architecture doc prompt is one of the highest-value things you can generate — it pays dividends for every future developer and AI session on the project
- Run the API documentation prompt periodically to catch undocumented endpoints
- The runbook prompt is invaluable for on-call rotations — generate it once, keep it updated
- For healthcare projects, always request that documentation note which endpoints handle PHI and what compliance measures are in place
