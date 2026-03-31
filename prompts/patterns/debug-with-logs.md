# Pattern: Debug with Container Logs

## When to Use
Your application is running in Docker containers (or locally) and something is broken. You feed the logs directly to the AI and let it trace the issue across services.

## The Pattern

### Step 1: Capture the Logs

```bash
# Docker Compose — all services
docker compose logs --tail=100 > /tmp/debug-logs.txt

# Specific service
docker compose logs api --tail=200 > /tmp/debug-logs.txt

# Follow live (paste snippets as they happen)
docker compose logs -f api
```

For local development:
```bash
# .NET console output
dotnet run 2>&1 | tee /tmp/debug-logs.txt
```

### Step 2: Feed the AI

```
I'm debugging an issue in our running application. Here are the recent logs:

```
[Paste the relevant log section — REDACT PHI FIRST]
```

The problem is: [describe what's wrong — what you expected vs. what happened]

Analyze these logs and:
1. Identify the error or unexpected behavior
2. Trace the request flow through the logs
3. Tell me which source file and method is causing the issue
4. Suggest a fix

If you need to see specific source files to diagnose further, tell me which ones.
```

### Step 3: Iterative Debugging

The AI may need more context. It will ask for specific files:

```
You asked to see PrescriptionService.cs. Here it is:
@Services/PrescriptionService.cs

And here are the updated logs after I added the logging you suggested:

```
[New logs]
```

What's the diagnosis now?
```

### Step 4: Fix and Verify

```
Apply the fix you suggested. After I restart the service, I'll paste the new logs
to confirm it's resolved.

[After restart]

Here are the logs after the fix:

```
[New logs]
```

Is the issue resolved? Are there any other concerns in these logs?
```

## Example: API Returning 500

```
Our prescription API is returning 500 errors. Here are the logs (PHI redacted):

```
[2026-03-31 14:23:15 ERR] An unhandled exception occurred while processing the request.
System.InvalidOperationException: Sequence contains no matching element
   at System.Linq.ThrowHelper.ThrowNoMatchException()
   at System.Linq.Enumerable.Single[TSource](IEnumerable`1 source, Func`2 predicate)
   at PrescriptionService.GetActivePrescription(Guid patientId, Guid prescriptionId)
   at PrescriptionController.GetPrescription(Guid patientId, Guid prescriptionId)
[2026-03-31 14:23:15 INF] Request finished HTTP/1.1 GET /api/patients/[PATIENT_ID]/prescriptions/[RX_ID] - 500 - 45ms
```

The endpoint should return 404 when the prescription doesn't exist, not 500.

@Services/PrescriptionService.cs
@Controllers/PrescriptionController.cs

Find the bug and fix it. The service is using .Single() which throws when
no match is found. Change the approach to handle the not-found case gracefully
and return a proper 404.
```

## Example: Database Connection Failure

```
The API container won't start. Here are the logs:

```
[2026-03-31 14:00:01 ERR] An error occurred using the connection to database 'prescriptions' on server '192.168.5.31'.
Microsoft.Data.SqlClient.SqlException: Cannot open database "prescriptions" requested by the login. The login failed.
Login failed for user 'ai'.
```

@docker-compose.yml
@appsettings.json
@appsettings.Development.json

Check:
1. Is the connection string correct?
2. Does the database exist?
3. Is the username/password correct?
4. Is there an environment variable override that might be wrong?
```

## Tips for Healthcare Log Debugging

**Before pasting logs into the AI:**
1. Search-and-replace patient names with `[PATIENT]`
2. Replace DOBs with `[DOB]`
3. Replace SSNs with `[SSN]`
4. Replace drug names with `[DRUG]` (if they appear in logs, that's a finding — PHI shouldn't be logged)
5. Replace prescription IDs only if they're linkable to real patients in your context

**If you find PHI in the logs during debugging, that's a bug itself:**

```
I found PHI in our log output. The patient name "[PATIENT]" appeared in this log line:

```
[log line with PHI redacted]
```

@[file that generates this log line]

1. Fix the logging to exclude PHI
2. Check if this same logging pattern exists elsewhere in the codebase
3. Add a code review comment or Jira ticket for any other instances
```

## Tips
- Docker Compose logs with `--tail=100` is usually enough. Don't paste 10,000 lines — the AI works better with focused context.
- Always include the timestamp range around the failure, not just the error line. The AI can trace the request flow through sequential log entries.
- If your services use structured logging (Serilog JSON), paste the JSON format — the AI parses it better than formatted text.
- For multi-service debugging, include logs from ALL involved services. The bug might be in Service A but the error appears in Service B.
