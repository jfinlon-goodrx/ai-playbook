# Task: End-to-End AI Delivery Orchestration

## When to Use

You want the assistant to use Jira, Confluence, Gleen, GitHub, and repo context together to move work from discovery through implementation and handoff.

## Prompt

```text
Use our connected systems to prepare an end-to-end delivery plan for [Jira ticket or epic].

Systems to use:
- Gleen for internal knowledge and policy context
- Jira for the work item, linked tickets, and workflow state
- Confluence for specs, ADRs, runbooks, and prior handoff pages
- GitHub for similar code, PR history, and CI patterns
- repo context files for implementation conventions

Return:
1. Business and technical context summary
2. Missing information or ambiguities
3. Relevant Confluence, Gleen, Jira, and GitHub links
4. Proposed delivery plan grouped into:
   - story clarification
   - implementation
   - testing
   - rollout
   - documentation
5. Proposed sub-tasks with owner role and definition of done
6. Risks, dependencies, and release considerations
7. Suggested artifacts to write back to Jira, Confluence, and GitHub

If the work is not ready for implementation, say so clearly before generating the rest.
```

## Follow-Up Prompt: Close The Loop

```text
The implementation for [ticket] is complete.

Using our connected systems:
1. Summarize what changed from the GitHub PR and test evidence
2. Draft the Jira update and status transition notes
3. Draft a Confluence handoff page
4. Draft release or rollout notes
5. List follow-up work or unresolved risks

Keep the outputs concise, traceable, and useful to developers, QA, product, and support.
```
