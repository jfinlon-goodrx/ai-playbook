# Task: Security Audit and HIPAA Compliance

## When to Use
You need to review code for OWASP vulnerabilities, HIPAA compliance, or prepare for a security audit. Critical for healthcare/prescription processing applications.

## The Prompt: Full OWASP Security Scan

```
@[Project root or specific directories]

Perform a comprehensive OWASP Top 10 security audit of this codebase:

**A01:2021 Broken Access Control**
- Are all endpoints protected with [Authorize] or explicitly [AllowAnonymous]?
- Can users access other users' data by manipulating IDs in URLs?
- Are role/permission checks enforced at the service layer (not just the controller)?
- Can users escalate privileges?

**A02:2021 Cryptographic Failures**
- Is sensitive data encrypted at rest (PHI, passwords, tokens)?
- Is TLS enforced for all connections (HTTPS, database, external APIs)?
- Are passwords hashed with a strong algorithm (bcrypt/Argon2, not SHA256)?
- Are API keys or secrets hardcoded anywhere?

**A03:2021 Injection**
- Are all SQL queries parameterized (no string concatenation)?
- Are LINQ queries safe from injection?
- Are any raw SQL queries used, and if so, are they parameterized?
- Is user input sanitized before use in file paths, commands, or regex?

**A04:2021 Insecure Design**
- Is there rate limiting on authentication endpoints?
- Are business logic workflows validated server-side (not trusting client state)?
- Is the principle of least privilege applied to service accounts?

**A05:2021 Security Misconfiguration**
- Is CORS configured restrictively (not wildcard *)?
- Are detailed error messages disabled in production?
- Are default credentials used anywhere?
- Is the Swagger UI disabled in production?

**A06:2021 Vulnerable Components**
- Are there known vulnerabilities in NuGet dependencies?
- Are dependencies pinned to specific versions?

**A07:2021 Authentication Failures**
- Are JWTs validated correctly (signature, expiry, issuer, audience)?
- Is there account lockout after failed login attempts?
- Are sessions properly invalidated on logout?

**A08:2021 Data Integrity Failures**
- Are CI/CD pipeline dependencies verified (package integrity)?
- Is code signing used for deployments?

**A09:2021 Logging & Monitoring**
- Are security events logged (failed logins, permission denials, data access)?
- Is PHI excluded from logs?
- Are logs tamper-resistant (append-only, centralized)?

**A10:2021 SSRF**
- Does the application make requests based on user-supplied URLs?
- If so, is URL validation and allowlisting in place?

For each finding:
- File and line number
- Severity: Critical / High / Medium / Low
- OWASP category
- Description and impact
- Recommended fix with code example
```

## The Prompt: HIPAA PHI Audit

```
Scan this codebase for HIPAA compliance issues related to PHI (Protected Health Information).

PHI includes: patient names, DOBs, SSNs, addresses, phone numbers, email addresses,
medical record numbers, prescription details (drug names, dosages, NDC codes),
insurance information, prescriber information, and any data that could identify a patient.

Check every file for:

1. **Logging** — Is PHI ever passed to ILogger, Console.Write, Debug.Write, or any logging framework?
   Search for: _logger.Log*, Log.*, Console.Write*, Debug.*, Trace.*
   PHI fields should NEVER appear in log messages.

2. **Error Messages** — Does any exception message, ProblemDetails response, or error DTO include PHI?
   Search for: new Exception(*, ProblemDetails, ValidationProblemDetails, error responses

3. **API Responses** — Do any endpoints return more PHI than necessary?
   Search for: over-exposed DTOs that include fields the consumer doesn't need

4. **URLs and Query Strings** — Does any PHI appear in URL paths or query parameters?
   Search for: route parameters or query strings containing patient identifiers
   (PHI in URLs gets logged by web servers, proxies, and browsers)

5. **Client-Side Storage** — Is PHI stored in localStorage, sessionStorage, or cookies?

6. **Audit Logging** — Do all data mutations (create, update, delete) have audit log entries?
   Every mutation of PHI-containing records must log: who, what action, which record, when, from where.

7. **Access Control** — Can users access PHI they shouldn't?
   Every endpoint that returns patient data must verify the requesting user has permission.

8. **Encryption** — Are PHI fields marked for encryption at rest?
   Database columns containing PHI should use column-level encryption or TDE.

9. **Data Retention** — Is there logic to purge PHI after the required retention period?

10. **Minimum Necessary** — Does each endpoint/service request only the PHI it needs?
    Check for SELECT * or .Include() that loads unnecessary PHI fields.

For each finding, provide:
- File:line
- Severity: Critical (active PHI leak) / High (potential leak) / Medium (best practice) / Low (recommendation)
- Category (Logging/ErrorMessages/AccessControl/etc.)
- The specific PHI field(s) at risk
- Recommended fix
```

## The Prompt: Secure a Specific Endpoint

```
@Controllers/[Controller].cs
@Services/[Service].cs

This endpoint handles [PHI type]. Harden it:

1. **Input validation** — Add FluentValidation rules for every input field.
   Reject any input that contains potential SQL injection or XSS patterns.

2. **Authorization** — Verify the authenticated user has permission to access this data.
   Check: is this their own data? Do they have the required role? Is the resource
   assigned to their organization?

3. **Output filtering** — Return only the fields the consumer needs. Create a
   response DTO that excludes sensitive fields not required by this endpoint.

4. **Audit logging** — Log: user ID, action, resource ID, timestamp, IP address.
   Do NOT log the request body or response body (they contain PHI).

5. **Rate limiting** — Add rate limiting if this endpoint could be used for
   enumeration attacks (e.g., searching for patients by partial name).

6. **Error handling** — Ensure error responses don't leak PHI.
   Return generic error messages: "Resource not found" not "Patient John Doe not found"

Show me the before and after for each change.
```

## The Prompt: Pre-Audit Checklist Generator

```
We have a security/HIPAA audit coming up. Read the codebase and generate
a compliance checklist specific to our implementation:

For each item, mark:
- [PASS] — we implement this correctly
- [FAIL] — we have a gap that needs fixing
- [PARTIAL] — we implement this in some places but not others
- [N/A] — not applicable to our system

Include file references for each finding so we can quickly verify and fix.
```

## Tips
- Run the PHI audit prompt on **every project that touches patient data** — not just the ones you think are risky
- The OWASP audit takes a while but catches real issues. Run it quarterly or before major releases.
- For the PHI audit, the AI is searching for patterns (logging calls, error messages). It may have false positives. Verify each finding before creating a remediation ticket.
- If you find a real PHI leak, fix it immediately — don't add it to the backlog. HIPAA violations are not features.
- Keep the audit results confidential — they describe your security weaknesses. Store them in a restricted Confluence space, not the public wiki.
