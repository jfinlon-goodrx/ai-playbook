# Common Mistakes in AI-Assisted Development

Mistakes you'll make (everyone does) and how to avoid or recover from them.

## Mistake 1: The Vague Prompt

**What happens:**
```
"Add a prescription feature"
```
The AI generates a generic, over-engineered implementation that doesn't match your project's patterns, your business rules, or your data model. You spend more time fixing it than you would have writing it from scratch.

**Fix:** Be specific. Include: endpoint paths, field names, business rules, authorization requirements, which existing code to follow.

**Good version:**
```
Add POST /api/prescriptions/{id}/refill. Require PharmacyStaff role.
Validate remaining refills > 0 and prescription not expired.
Follow the patterns in PrescriptionController.cs.
```

## Mistake 2: Accepting Without Reading

**What happens:** The AI generates 300 lines. It compiles. Tests pass. You commit it.

Two weeks later, a security review finds the endpoint doesn't check authorization, the service logs patient names, and the SQL query is vulnerable to injection.

**Fix:** Read every line before committing. Especially:
- Authorization attributes on controllers
- What's being logged (search for `_logger.Log`)
- How queries are constructed (search for string concatenation in SQL)
- What's included in error responses

## Mistake 3: Starting Over Instead of Iterating

**What happens:** The AI's output is 80% right. You delete everything and try a completely different prompt. The second attempt is also 80% right, but different 80%. You cycle between rewrites.

**Fix:** Iterate. "Fix these 3 specific issues" is faster and more reliable than "try again." See `patterns/iterative-refinement.md`.

## Mistake 4: No .cursorrules File

**What happens:** The AI uses its default patterns: block-scoped namespaces, no async, inconsistent naming, no HIPAA awareness. Every prompt requires repeating the same context.

**Fix:** Add a `.cursorrules` file to your project root. Do it once, benefit forever. See `cursorrules/`.

## Mistake 5: Trusting AI for Security

**What happens:** You prompt "add authentication to this endpoint." The AI adds `[Authorize]`. You assume it's secure. But it didn't check that the user can only access *their own* data, didn't add audit logging, and the error message includes the patient's name.

**Fix:** For security-sensitive code, use the specific security prompts from `prompts/by-task/security-audit.md`. Never assume the AI "knows" your security requirements — state them explicitly every time.

## Mistake 6: Pasting PHI into Prompts

**What happens:** You copy a log line containing a real patient name into the Cursor prompt to debug an issue. That prompt is sent to the AI provider's API.

**Fix:** Always redact PHI before pasting. Replace patient names with `[PATIENT]`, DOBs with `[DOB]`, etc. The AI doesn't need real PHI to debug code issues.

## Mistake 7: Generating Tests That Test Nothing

**What happens:** The AI generates 20 tests. They all pass. You feel confident. But the tests assert that the method was called (interaction testing), not that the behavior is correct (behavior testing).

```csharp
// This test proves nothing:
[Fact]
public async Task GetPrescription_CallsRepository()
{
    await _service.GetPrescriptionAsync(id, ct);
    _mockRepo.Verify(r => r.GetByIdAsync(id, ct), Times.Once);
}
```

**Fix:** Ask the AI to test *behavior and outcomes*, not *interactions*:

```csharp
// This test proves something:
[Fact]
public async Task GetPrescription_ExistingId_ReturnsPrescriptionWithCorrectFields()
{
    var result = await _service.GetPrescriptionAsync(id, ct);
    Assert.True(result.IsSuccess);
    Assert.Equal(expectedStatus, result.Value.Status);
    Assert.Equal(expectedPharmacyId, result.Value.PharmacyId);
}
```

## Mistake 8: Ignoring the AI's Questions

**What happens:** The AI says "I need to see the IPrescriptionRepository interface to implement this correctly. Can you share it?" You ignore this and say "just write it." The AI guesses wrong.

**Fix:** When the AI asks for more context, give it. It's telling you what it needs to produce correct output. Use `@filename` references in Cursor.

## Mistake 9: Using AI for Things You Don't Understand

**What happens:** A junior developer uses AI to implement a complex authorization scheme. The code compiles and seems to work. But the developer can't explain how it works, can't debug it when it breaks, and can't review someone else's code that extends it.

**Fix:** AI amplifies your skills — it doesn't replace understanding. If you don't understand the concept (CQRS, event sourcing, policy-based auth), learn it first (ask the AI to explain it), then use the AI to implement it.

## Mistake 10: One Giant Prompt

**What happens:** You write a 2000-word prompt describing an entire feature with every detail. The AI produces a confused, inconsistent output because it tried to handle too many concerns at once.

**Fix:** Break it into steps. Orient the AI first (read existing code). Then build incrementally (data layer → service → controller → tests). See `patterns/vertical-slice.md` for the right flow.
