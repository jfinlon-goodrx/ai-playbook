# Task: Bug Investigation

## When to Use
You have a bug report, an error in logs, or a failing test and need to find the root cause. AI is exceptionally good at tracing errors across layers when given the right context.

## Prerequisites
- [ ] Error message, stack trace, or reproduction steps available
- [ ] Access to the relevant source code in Cursor

## The Prompt: Stack Trace Analysis

```
I'm investigating a bug. Here's the error:

**Error message:**
[Paste the full error message]

**Stack trace:**
[Paste the full stack trace]

**Context:**
- This happens when [describe the user action or trigger]
- It started [when — after a deployment? after a data change? randomly?]
- It happens [always / intermittently / only for specific users/data]

Analyze the stack trace and:
1. Identify the root cause — which line of our code is the actual failure point?
2. Trace the call chain — how did we get to this point?
3. Suggest the most likely fix
4. Identify if this could affect other code paths (blast radius)

Read the source files referenced in the stack trace to confirm your analysis.
```

## The Prompt: Log-Based Investigation

```
I'm debugging an issue in [service/feature]. Here are the relevant log entries:

```
[Paste log entries — REDACT any PHI before pasting]
```

The expected behavior was: [what should have happened]
The actual behavior was: [what actually happened]

Based on these logs:
1. What went wrong and at what point in the flow?
2. What's missing from the logs that would help diagnose this? (suggest additional logging)
3. What's the most likely root cause?
4. Read the relevant source files and suggest a fix.
```

## The Prompt: Container/Docker Log Analysis

```
I'm running the application in Docker. The service is failing with this behavior:
[Describe the symptom]

Here are the container logs:

```
[Paste container logs — REDACT any PHI]
```

And here's the docker-compose.yml for reference:
@docker-compose.yml

Diagnose the issue. Common causes to check:
- Connection string or environment variable misconfiguration
- Database migration not applied
- Port conflict or networking issue
- Missing dependency or DI registration
- Startup ordering (service starting before database is ready)
```

## The Prompt: Reproduce and Fix

```
Bug report from Jira:

**Title:** [Bug title]
**Steps to reproduce:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Expected:** [What should happen]
**Actual:** [What actually happens]
**Environment:** [Dev/Staging/Production, browser, OS]

Read the relevant code and:
1. Trace the code path for these reproduction steps
2. Identify where the behavior diverges from expected
3. Propose a fix
4. Write a regression test that would catch this bug

If you need to see specific files, tell me which ones.
```

## The Prompt: Performance Issue

```
We're seeing slow response times on [endpoint/feature]:

**Endpoint:** [Method] /api/[path]
**Normal response time:** [expected, e.g., < 200ms]
**Current response time:** [actual, e.g., 3-5 seconds]
**Load:** [requests per minute, concurrent users]

@Controllers/[Controller].cs
@Services/[Service].cs
@Repositories/[Repository].cs

Analyze the code path and identify potential performance issues:
1. N+1 queries (loading related data in loops)
2. Missing indexes (check the queries against the schema)
3. Unnecessary data loading (SELECT * when only 2 columns are needed)
4. Missing caching opportunities
5. Synchronous blocking calls
6. Large object allocations

Suggest specific fixes with code changes.
```

## The Prompt: Intermittent Failure

```
We have an intermittent bug that's hard to reproduce:

**Symptom:** [What happens]
**Frequency:** [How often — 1 in 100 requests? Only on Mondays? Only with certain data?]
**No stack trace available** — the error is swallowed somewhere

@[Relevant files]

Analyze the code for:
1. Race conditions (shared mutable state, missing locks, concurrent access)
2. Resource exhaustion (connection pool, thread pool, memory leaks)
3. Timing-dependent behavior (timeouts, cache expiry, token expiry)
4. Data-dependent behavior (specific input values that trigger edge cases)
5. External dependency failures (database, API, message queue)

For each potential cause you find, suggest:
- How to confirm it's the cause (what to log, what to monitor)
- How to fix it
- How to write a test that catches it
```

## Tips
- **Always redact PHI before pasting logs into Cursor.** Replace patient names with "[PATIENT]", prescription details with "[RX_DATA]", etc. The AI doesn't need real PHI to diagnose code issues.
- Paste the **full** stack trace, not just the top line. The root cause is often buried in an inner exception.
- Include the **context** (when it started, what changed) — this is the information the AI can't get from the code alone.
- For intermittent bugs, the AI's analysis of race conditions and timing issues is surprisingly good — it reads the code more carefully than most humans scan for concurrency bugs.
- After fixing a bug with AI, always ask: "Write a regression test that would have caught this bug before this fix existed."
