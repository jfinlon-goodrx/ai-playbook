# When to Use AI (and When Not To)

AI-assisted development is not "use AI for everything." It's knowing when AI accelerates you and when it slows you down or introduces risk.

## High-Value Tasks (Use AI)

| Task | Why AI Excels |
|---|---|
| **New CRUD endpoints** | Repetitive, pattern-based. AI matches existing patterns perfectly. |
| **Unit and integration tests** | AI generates comprehensive test cases faster than you can think of them. |
| **Database migrations** | Describe the schema change; AI writes the EF migration. |
| **Boilerplate code** | DTOs, mapping profiles, DI registration, middleware setup. |
| **Bug investigation** | Paste the stack trace and logs. AI traces the cause across layers. |
| **Refactoring** | "Extract this into a service" or "Convert this to async" — mechanical, multi-file changes. |
| **Documentation** | AI reads the code and writes accurate XML docs, README sections, architecture docs. |
| **Code review prep** | "Summarize these changes and identify potential issues" before you submit the PR. |
| **Converting patterns** | "Convert all repository classes from sync to async" — tedious but mechanical. |
| **Prototyping** | Get a working version fast, then refine. AI is the best prototyping tool ever built. |

## Medium-Value Tasks (Use with Caution)

| Task | Caveat |
|---|---|
| **Complex business logic** | AI will write plausible-looking logic that may not match requirements. You must verify against the spec, not just check that it compiles. |
| **Database queries** | AI writes good SQL/LINQ for common patterns but can miss edge cases in complex joins or window functions. Always test with real data. |
| **Security-sensitive code** | Authentication, authorization, encryption, PHI handling. AI can scaffold these but **you must review every line** against OWASP and HIPAA requirements. |
| **Performance-critical code** | AI writes correct code but not always optimal code. Profile first, optimize manually or with AI guidance. |

## Low-Value or Dangerous Tasks (Don't Use AI Blindly)

| Task | Why |
|---|---|
| **Architecture decisions** | AI can propose architectures but can't weigh organizational context, team skills, or long-term maintenance costs. Use Chat to explore options, but decide yourself. |
| **Cryptography implementation** | Never let AI write custom crypto. Use established libraries (ASP.NET Data Protection, Azure Key Vault). |
| **HIPAA compliance logic** | AI doesn't understand your specific BAA, data flow, or regulatory context. It can scaffold audit logging, but compliance decisions are human decisions. |
| **Novel algorithms** | AI is trained on existing code. If you need something genuinely novel, AI will produce something that looks right but may be subtly wrong. |
| **Deleting or refactoring code you don't understand** | AI will happily delete code that looks unused but is called via reflection, convention, or configuration. Understand it first. |

## The Decision Framework

Before starting a task with AI, ask:

```
1. Is this task pattern-based or novel?
   Pattern-based → AI is great
   Novel → AI can help explore, but verify everything

2. What's the blast radius if the AI gets it wrong?
   Low (internal tool, reversible) → Let AI move fast
   High (patient data, financial, security) → AI scaffolds, human verifies every line

3. Do I understand the expected output?
   Yes → I can review effectively → Use AI
   No → I can't review effectively → Learn first, then use AI
```

## The Healthcare Guardrail

In our domain (prescription processing, healthcare), apply this additional check:

**Does this code touch PHI (Protected Health Information)?**

- Patient names, DOBs, SSNs, addresses
- Prescription data (drug names, dosages, prescriber info)
- Insurance/billing information
- Any data that could identify a patient

**If yes:** AI can write the code, but a human must:
1. Verify PHI is never logged (check every `_logger.Log*` call)
2. Verify PHI is never included in error responses
3. Verify audit logging captures mutations correctly
4. Verify data access is properly authorized (the user can only see their own data, or has explicit permission)
5. Verify PHI fields are encrypted at rest if required by policy

**Add this to your prompt when working with PHI:**

```
IMPORTANT: This code handles PHI (Protected Health Information).
- Never log PHI fields (patient name, DOB, SSN, prescription details)
- Never include PHI in error messages or exception details
- All data mutations must be audit logged (who, what, when, from what IP)
- All endpoints must validate that the authenticated user has permission to access the requested patient data
- Use parameterized queries only — no string concatenation for SQL
```
