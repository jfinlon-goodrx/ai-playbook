# Task: AI-Assisted Code Review

## When to Use
You're reviewing a PR and want the AI to do a first pass, or you're preparing your own PR and want to catch issues before submitting.

## The Prompt: Review Someone Else's PR

```
I'm reviewing a pull request. Here are the changed files:

@[file1]
@[file2]
@[file3]

The PR description says: "[paste PR description]"

Review these changes for:

1. **Correctness** — Does the code do what the PR description says? Are there logic errors?
2. **Security** — OWASP Top 10 issues: injection, XSS, broken auth, sensitive data exposure.
   For healthcare code: PHI leakage in logs, error messages, or API responses.
3. **Error handling** — Are all failure cases handled? Can exceptions leak sensitive data?
4. **Performance** — N+1 queries, unnecessary allocations, missing async, missing caching.
5. **Testing** — Are the tests meaningful? Do they test behavior or just implementation details?
6. **Standards** — Does the code follow our .cursorrules conventions?

For each issue found, provide:
- File and approximate location
- Severity: Critical / Major / Minor / Nitpick
- Description of the issue
- Suggested fix (code snippet if applicable)

Don't flag formatting issues — the linter handles those.
Focus on things a human reviewer should catch.
```

## The Prompt: Pre-Submit Self-Review

```
I'm about to submit a PR with these changes. Review my own code before I submit.

@[changed file 1]
@[changed file 2]
@[changed file 3]

Look for:
1. Bugs I might have missed
2. Security issues (especially PHI exposure in logs or error responses)
3. Missing validation on inputs
4. Missing null checks
5. Async methods without CancellationToken
6. Missing or weak tests
7. Anything that would get flagged in code review

Be direct. I'd rather fix issues now than get review comments later.
```

## The Prompt: Generate PR Description

```
Generate a pull request description for the following changes.

@[all changed files — or use git diff context]

Format:

## Summary
[2-3 sentences describing what this PR does and why]

## Changes
- [Bulleted list of specific changes]

## Testing
- [How this was tested]
- [What test cases were added]

## Security & Compliance
- [Any security considerations]
- [PHI handling changes, if any]
- [Audit logging changes, if any]

## Screenshots
[Note if UI changes require screenshots]

## Checklist
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated (if applicable)
- [ ] No PHI in logs or error messages
- [ ] Audit logging for data mutations
- [ ] XML doc comments on public APIs
- [ ] Migration tested on dev database
```

## The Prompt: Security-Focused Review (Healthcare)

```
@[changed files]

Perform a HIPAA and OWASP security review of these changes:

**HIPAA Compliance:**
- [ ] No PHI logged (patient names, DOBs, SSNs, drug names, dosages)
- [ ] No PHI in error messages or exception details
- [ ] No PHI in URLs or query parameters
- [ ] Audit logging present for all data mutations
- [ ] Data access properly authorized (users can only access their own data)
- [ ] PHI fields have appropriate encryption annotations

**OWASP Top 10:**
- [ ] SQL Injection: All queries parameterized, no string concatenation
- [ ] XSS: All output encoded (if applicable)
- [ ] Broken Authentication: Tokens validated, sessions managed properly
- [ ] Sensitive Data Exposure: No secrets in code, PHI encrypted in transit and at rest
- [ ] Broken Access Control: Authorization checked on every endpoint
- [ ] Security Misconfiguration: No debug mode, no default credentials
- [ ] Insecure Deserialization: Input deserialization validated
- [ ] Logging & Monitoring: Security events logged, no PHI in logs

For each finding, specify:
- Location (file:line)
- Severity (Critical/High/Medium/Low)
- OWASP or HIPAA category
- Recommended fix
```

## The Prompt: Architectural Review

```
@[changed files]
@docs/ARCHITECTURE.md (if it exists)

Review these changes for architectural concerns:

1. **Layer violations** — Does code in the API layer directly access the database?
   Does the domain layer depend on infrastructure?
2. **Dependency direction** — Do dependencies flow inward (API → Application → Domain)?
3. **Interface segregation** — Are interfaces focused, or do they have methods this consumer doesn't need?
4. **Single responsibility** — Are classes doing too much?
5. **Coupling** — Would changing one component require changing many others?
6. **Naming** — Do class, method, and variable names accurately describe their purpose?

Focus on structural issues, not style. I don't need formatting nitpicks.
```

## Tips
- Use the security review prompt on **every PR that touches patient data**, not just ones that look security-relevant
- The self-review prompt catches 60-70% of the comments you'd get in peer review — use it before submitting to respect your reviewers' time
- For large PRs, break the review into sections: "Review just the controller changes" → "Now review the service layer" → "Now the tests"
- The AI won't catch business logic errors unless you tell it the business rules. Include the Jira story requirements in the prompt for logic-heavy reviews
