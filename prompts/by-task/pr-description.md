# Task: PR Description and Commit Messages

## When to Use
You've finished a feature and need to create a clear, informative PR description or meaningful commit messages.

## The Prompt: Full PR Description

```
Generate a pull request description for the changes in this branch.

Read all modified files and produce:

## Summary
[2-3 sentences: what this PR does and why]

## Changes
[Bulleted list organized by area: API / Service / Data / UI / Tests]

## Architecture Decisions
[Any design choices made and why — especially if there were alternatives]

## Database Changes
[Migrations added, schema changes, data seeds — or "None"]

## Security & Compliance
[HIPAA considerations: PHI handling, audit logging, access control changes]
[OWASP considerations: input validation, auth changes, query safety]
[Or "No security-relevant changes" if truly none]

## Testing
[What was tested and how: unit tests, integration tests, manual testing steps]

## How to Review
[Suggested order to read the code, key files to focus on]

## Related
[Jira ticket: PROJ-1234]
[Related PRs, if any]
```

## The Prompt: Conventional Commits

```
Generate conventional commit messages for my staged changes.
Group related changes into logical commits.

Format: type(scope): description

Types:
- feat: new feature
- fix: bug fix
- refactor: code restructuring (no behavior change)
- test: adding or updating tests
- docs: documentation changes
- chore: build, config, dependency updates

Rules:
- Subject line max 72 characters
- Use imperative mood ("add" not "added")
- Include body for non-obvious changes
- Reference Jira ticket if applicable: "Refs PROJ-1234"

Example:
feat(prescriptions): add refill request endpoint

Add POST /api/prescriptions/{id}/refill for submitting refill requests.
Includes validation for remaining refills, expiry, and authorization.
Audit logging added per HIPAA requirements.

Refs PROJ-1234
```

## The Prompt: Commit Splitting

```
I have a large set of uncommitted changes that should be multiple commits.
Analyze the changes and suggest how to split them into logical, atomic commits.

For each proposed commit:
1. List the files to include
2. Write the commit message
3. Explain why these changes belong together

Order the commits so each one leaves the codebase in a buildable, testable state.
```

## The Prompt: Changelog Entry

```
Based on the changes in this PR, write a changelog entry:

Format:
### [Category]
- [Change description in user-facing language, not technical implementation details]

Categories: Added, Changed, Fixed, Removed, Security, Deprecated

Write it for someone who uses the product, not someone who reads the code.
```

## Tips
- Run the PR description prompt **after** your self-review (see `code-review-assist.md`) — the AI should describe the final state, not intermediate changes
- For healthcare PRs, always include the Security & Compliance section even if empty — it shows reviewers you considered it
- Good PR descriptions make reviews faster — a reviewer who understands the intent catches issues more effectively than one reading code cold
