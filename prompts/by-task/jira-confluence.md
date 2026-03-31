# Task: Jira and Confluence with MCP

## When to Use
You have MCP (Model Context Protocol) access to Atlassian tools and want the AI to read/update Jira tickets, search Confluence, or automate project management tasks.

## Prerequisites
- [ ] MCP connection to Atlassian configured in Cursor
- [ ] Appropriate Jira/Confluence permissions for your account

## The Prompt: Understand a Jira Story Before Implementation

```
Read Jira ticket [PROJ-1234] using MCP.

Summarize:
1. What is being requested (in technical terms)?
2. What are the acceptance criteria?
3. Are there linked tickets (blockers, related, parent epic)?
4. Are there any comments with additional context or clarifications?
5. What's the priority and sprint assignment?

Then, based on the ticket and our codebase, outline:
- Which files will need to change
- What new files need to be created
- Estimated complexity (trivial/small/medium/large)
- Any risks or open questions I should clarify before starting
```

## The Prompt: Create Sub-Tasks from a Story

```
Read Jira story [PROJ-1234].

Break it into technical sub-tasks using this format:

For each sub-task:
- Title: [concise, technical]
- Description: [what to implement]
- Acceptance criteria: [how to verify it's done]
- Estimated effort: [hours]
- Dependencies: [which sub-tasks must complete first]

Group into: Data Layer / API Layer / UI Layer / Tests / Documentation

Create these as sub-tasks on the story using MCP.
```

## The Prompt: Update Ticket After Completion

```
I've completed the work for [PROJ-1234]. Using MCP:

1. Update the ticket status to "In Review"
2. Add a comment with:
   - What was implemented (summary of changes)
   - PR link: [paste PR URL]
   - Files changed: [or "see PR"]
   - Testing performed: [what you tested]
   - Notes for reviewer: [any context that helps the reviewer]
3. Log time spent: [X hours]
```

## The Prompt: Search Confluence for Context

```
Search Confluence for documentation related to [topic/feature].

Look for:
- Architecture decision records
- Technical specifications
- Business requirements or process flows
- Previous implementation notes or runbooks

Summarize what you find and identify which pages are most relevant
to the work I'm doing on [PROJ-1234].
```

## The Prompt: Write Confluence Technical Documentation

```
Write a Confluence page documenting the [feature/system] I just built.

Structure:
## Overview
[What it does, why it exists]

## Architecture
[How it works, key components, data flow diagram description]

## API Reference
[Endpoints, request/response schemas]

## Configuration
[Environment variables, feature flags, settings]

## Security & Compliance
[HIPAA considerations, access control, audit logging]

## Troubleshooting
[Common issues and resolutions]

## Change History
| Date | Author | Change |
|---|---|---|

Publish to the [Space/Section] in Confluence using MCP.
```

## The Prompt: Sprint Reporting

```
Using MCP, read all tickets in the current sprint for project [PROJ].

Generate a sprint status report:

## Sprint Progress
- Total stories: X
- Completed: X
- In Progress: X
- Blocked: X
- Not Started: X

## Completed This Sprint
[List with ticket ID, title, and assignee]

## In Progress
[List with ticket ID, title, assignee, and any blockers noted in comments]

## Risks & Blockers
[Any blocked tickets with the blocking reason]

## Carry-Over Candidates
[Tickets that are unlikely to complete this sprint based on status and remaining time]
```

## Tips
- MCP access to Jira/Confluence means the AI can read and write to your project management tools — be explicit about what you want it to modify vs. just read
- Always verify MCP-created sub-tasks before the team sees them — the AI may misunderstand story scope
- The Confluence search prompt is excellent for onboarding — new team members can quickly find relevant documentation
- For sprint reporting, the AI provides a snapshot — it can't predict blockers it hasn't seen in the comments
