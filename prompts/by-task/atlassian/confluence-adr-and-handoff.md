# Task: Confluence ADR and Team Handoff

## When to Use
You need to turn a technical discussion, implementation change, or release outcome into a Confluence page that others can actually use.

## The Prompt: Confluence ADR

```text
Turn this design discussion into a Confluence-ready architecture decision record.

Inputs:
- Decision topic: [short title]
- Context: [problem being solved]
- Discussion notes: [paste notes, comments, or meeting transcript]
- Final decision: [what was chosen]
- Alternatives considered: [list]
- Risks and trade-offs: [list]

Output format:
1. TL;DR
2. Context
3. Decision
4. Alternatives Considered
5. Consequences
6. Open Questions
7. Action Items

Write it in a style suitable for Confluence. Keep it concise enough for engineers, QA, product, and managers to read quickly.
```

## The Prompt: Implementation Handoff Page

```text
Create a Confluence handoff page for a feature that has just been built.

Include:
1. Feature summary
2. Jira story and related ticket links
3. What changed in the codebase
4. Key test evidence
5. Rollout and rollback notes
6. Known limitations
7. Monitoring or support notes
8. Follow-up tasks

Audience:
- Developers
- QA
- Product
- Support or operations

Make the page useful for someone who did not attend the implementation discussions.
```

## Tips

- Use this for real knowledge transfer, not just documentation compliance.
- Ask for a short TL;DR at the top so leadership can skim it.
