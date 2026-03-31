# Pattern: Vertical Slice — Build a Complete Feature Across All Layers

## When to Use
You're implementing a feature that touches database, API, service logic, and optionally UI. Instead of building layer by layer, you build the entire slice at once. This is the most powerful AI-assisted development pattern — it's where the 5-10x speed multiplier comes from.

## The Pattern

### Step 1: Orient the AI

```
I'm implementing a new feature. Before writing any code, read these files
to understand the project's architecture and patterns:

@Program.cs
@[an existing controller that's similar to what I'm building]
@[the service interface for that controller]
@[the service implementation]
@[the entity/model class]
@[the DbContext]
@[an existing test file]
@.cursorrules

Tell me what patterns you see for:
1. Controller → Service → Repository flow
2. DTO structure and mapping
3. Validation approach
4. Error handling (exceptions vs. Result<T>)
5. DI registration
6. Test structure
```

**Review the AI's response.** Correct anything it gets wrong. This calibration step ensures everything that follows matches your project.

### Step 2: Describe the Full Feature

```
Now implement this feature end-to-end:

**Feature:** [Name and business description]
**User story:** As a [role], I want to [action] so that [value]

**Data layer:**
- [New tables/columns needed]
- [EF migration required]
- [Seed data if any]

**API layer:**
- [Endpoints: Method, Route, Request, Response]
- [Authorization requirements]
- [Validation rules]

**Service layer:**
- [Business logic: what decisions does the service make?]
- [External dependencies: other services, APIs, queues]
- [Audit logging requirements]

**UI layer (if applicable):**
- [Pages/components needed]
- [User flow description]

**Tests:**
- Unit tests for service logic
- Integration tests for API endpoints (if the project has them)

**Security (healthcare):**
- [Which fields are PHI]
- [Audit logging specifics]
- [Access control rules]

Build ALL of this. Follow the exact patterns from Step 1.
```

### Step 3: Review and Iterate

The AI will generate many files. Review them in this order:

1. **Entity/model** — Is the data model correct?
2. **Migration** — Will this create the right schema?
3. **Service interface** — Does the contract make sense?
4. **Service implementation** — Is the business logic correct?
5. **Controller** — Does it handle HTTP concerns properly?
6. **DTOs** — Are request/response shapes correct?
7. **Tests** — Do they test meaningful behavior?
8. **UI (if applicable)** — Does it render and function correctly?

If anything is wrong:

```
Good start. Fix these issues:
1. [Specific issue in specific file]
2. [Specific issue in specific file]
3. [Specific issue in specific file]

Don't regenerate everything — just fix the issues I listed.
```

### Step 4: Run and Verify

```bash
dotnet build          # Does it compile?
dotnet test           # Do tests pass?
dotnet run            # Does it start?
curl <endpoint>       # Does the API work?
```

If errors occur, paste them:

```
The build failed with these errors:

[paste errors]

Fix them. The most likely cause is [your guess if you have one].
```

### Step 5: Polish

```
The feature works. Now polish:
1. Add XML doc comments to all public APIs
2. Verify the migration has a clean Down() method
3. Add a PR description summarizing all changes
4. Add a Jira comment documenting what was built (use MCP if available)
```

## Example: Complete Prescription Transfer Feature

```
I'm implementing prescription transfer between pharmacies.

**Feature:** A pharmacist at Pharmacy B requests to transfer a prescription
from Pharmacy A. The system validates the transfer is allowed, creates a
transfer record, and notifies Pharmacy A.

**Data layer:**
New table: PrescriptionTransfers
- Id (Guid PK)
- PrescriptionId (FK to Prescriptions)
- FromPharmacyId (FK to Pharmacies)
- ToPharmacyId (FK to Pharmacies)
- Status: Requested, Approved, Denied, Completed, Cancelled
- RequestedAt, ProcessedAt
- RequestedByUserId (FK to Users)
- ProcessedByUserId (nullable FK to Users)
- DenialReason (nullable, max 500)
- Notes (nullable, max 500) — PHI: may contain clinical notes

Modify Prescriptions: add CurrentPharmacyId (FK to Pharmacies)

**API layer:**
POST /api/prescriptions/{id}/transfer — request a transfer
  Body: { toPharmacyId, notes }
  Auth: PharmacyStaff role, must work at the requesting pharmacy

GET /api/pharmacies/{id}/transfers — list transfers for a pharmacy
  Auth: PharmacyStaff role, must work at this pharmacy
  Supports: pagination, status filter

PUT /api/transfers/{id}/approve — Pharmacy A approves
  Auth: PharmacyStaff at the FROM pharmacy

PUT /api/transfers/{id}/deny — Pharmacy A denies
  Body: { denialReason }
  Auth: PharmacyStaff at the FROM pharmacy

**Service layer:**
- Validate prescription exists and is active
- Validate prescription belongs to the FROM pharmacy
- Validate no pending transfer already exists
- Validate FROM and TO pharmacies are different
- On approval: update prescription's CurrentPharmacyId, mark transfer Completed
- On denial: mark transfer Denied with reason

**Tests:**
- Happy path: request, approve, verify pharmacy changed
- Deny flow: request, deny, verify prescription unchanged
- Duplicate transfer: second request returns error
- Wrong pharmacy: requesting from pharmacy that doesn't own the prescription
- Authorization: staff at wrong pharmacy can't approve/deny

**Security:**
- PHI fields: prescription details, patient info (don't log these)
- Audit log every status change with: user, action, transfer ID, timestamp
- Notes field may contain PHI — encrypt at rest
- Transfer endpoint must verify the authenticated user works at the requesting pharmacy
```

## Why This Pattern Works

Traditional development:
1. Write migration → PR → Review → Merge (Day 1)
2. Write service → PR → Review → Merge (Day 2)
3. Write controller → PR → Review → Merge (Day 3)
4. Write tests → PR → Review → Merge (Day 4)
5. Fix integration issues discovered in staging (Day 5)

Vertical slice with AI:
1. Build everything in one session (2-4 hours)
2. Fix issues immediately because you can see all layers at once
3. One PR, one review, one merge (same day)
4. No integration issues because it was integrated from the start

**Total time saved:** 4 days → 4 hours. That's the force multiplier.

## Tips
- The orientation step (Step 1) is not optional. Skip it and the AI generates generic code that doesn't match your project.
- The more specific your feature description, the better the output. Vague requirements produce vague code.
- Don't try to build 10 features at once. One vertical slice per session. Commit, then start the next one.
- For healthcare features, always include the Security block — the AI won't add HIPAA compliance unless explicitly told.
- If the generated code is 80% right, iterate — don't start over. The AI retains context within the session and fixes are fast.
