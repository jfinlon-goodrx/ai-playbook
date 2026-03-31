# Hallucination Recovery

How to detect, handle, and prevent AI hallucinations — when the AI generates plausible-looking code that is factually wrong.

## What Hallucinations Look Like in Code

### Invented APIs
The AI uses a method, class, or NuGet package that doesn't exist:

```csharp
// The AI generated this, but EF Core has no .SafeDeleteAsync() method
await _context.Prescriptions.SafeDeleteAsync(prescription);
```

**Detection:** The build fails with `CS1061 does not contain a definition for`.
**Fix:** Paste the error. The AI will replace it with the real API.

### Wrong Method Signatures
The AI calls a real method with the wrong parameters:

```csharp
// Real signature: AddAsync(TEntity entity, CancellationToken ct)
// AI used: AddAsync(TEntity entity, bool saveChanges, CancellationToken ct)
await _context.Prescriptions.AddAsync(prescription, true, cancellationToken);
```

**Detection:** Build error with parameter mismatch.
**Fix:** Paste the error and the actual method signature from IntelliSense.

### Plausible but Wrong Business Logic
The most dangerous hallucination — the code compiles and passes tests but implements the wrong business rule:

```csharp
// AI wrote: allow refill if refills remaining > 0
if (prescription.RemainingRefills > 0)

// Actual requirement: allow refill if remaining > 0 AND not expired AND not on hold
if (prescription.RemainingRefills > 0
    && prescription.ExpirationDate > DateTimeOffset.UtcNow
    && prescription.Status != PrescriptionStatus.OnHold)
```

**Detection:** Only caught by human review against the business requirements.
**Fix:** Always include business rules explicitly in the prompt. Don't assume the AI infers them.

### Confident Incorrect Security Advice
The AI states a security practice as fact when it's wrong or outdated:

```
"You should use SHA-256 to hash passwords for HIPAA compliance."
```

SHA-256 is not appropriate for password hashing (use bcrypt/Argon2). The AI presented this confidently.

**Detection:** Security knowledge and review. This is why we never trust AI for security decisions without verification.
**Fix:** For security-critical code, verify against OWASP documentation, not just the AI's assertion.

### Invented NuGet Packages

```csharp
// The AI suggests installing a package that doesn't exist
// dotnet add package Microsoft.AspNetCore.HipaaCompliance
```

**Detection:** `dotnet restore` fails.
**Fix:** Tell the AI the package doesn't exist and ask for the real alternative.

## Prevention Strategies

### 1. Provide Context, Don't Rely on AI Knowledge

```
// BAD: relies on AI's knowledge of your codebase
"Use our standard audit logging"

// GOOD: shows the AI exactly what to use
@Infrastructure/AuditLogger.cs
"Use the AuditLogger.LogAsync method as shown in the referenced file"
```

### 2. Reference Real Files, Not Descriptions

```
// BAD: AI may hallucinate the interface
"Implement the IPrescriptionRepository interface"

// GOOD: AI reads the actual interface
@Repositories/IPrescriptionRepository.cs
"Implement every method in this interface"
```

### 3. Verify Claims About Libraries

When the AI suggests using a library feature, verify it exists:
- Check IntelliSense in your IDE
- Check the library's documentation
- Check the NuGet package version you have installed

### 4. Test-Driven Verification

Write tests first (or have the AI write them from requirements), then implement. Tests catch logic hallucinations because they verify behavior against expectations, not just compilation.

### 5. Ask the AI to Verify Itself

```
You just generated code that uses [method/class/package].
Verify: does this actually exist in [library] version [version]?
If you're not sure, tell me and I'll check.
```

Surprisingly, this often works — the AI will admit uncertainty when directly asked.

## Recovery Workflow

When you suspect a hallucination:

```
I think [specific thing] might be a hallucination. Verify:

1. Does [method/class/package] actually exist?
2. If not, what's the correct alternative?
3. Show me the corrected code.

If you're not sure, say so — I'd rather check documentation than deploy wrong code.
```

## The Rule of Thumb

**If the AI generates something you've never seen before and can't verify from your existing codebase or official documentation, treat it as suspicious until proven real.**

This doesn't mean the AI is wrong — it might be introducing you to a real API you didn't know about. But verify before trusting, especially for:
- Security patterns
- Healthcare/HIPAA compliance claims
- New library features
- Complex LINQ or SQL operations
- Cryptographic operations
