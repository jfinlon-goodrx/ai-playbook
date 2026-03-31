# Pattern: Session Handoff — Transferring Context Between AI Sessions

## When to Use
You're ending a Cursor session but the work isn't finished. You need to capture the current state so you (or a teammate) can pick up where you left off in a new session. Also useful for handing work between Cursor and Claude Code, or between developers.

## Why This Matters
AI sessions don't persist context. When you start a new Composer window, the AI has no memory of what you were doing. A handoff prompt captures the essential context so the new session starts at full speed instead of cold.

## The Pattern

### Step 1: Generate the Handoff (End of Current Session)

```
I'm ending this session. Generate a handoff document that captures everything
a new AI session (or a teammate) needs to continue this work.

Include:

## Current State
- What feature/task we're working on
- What's been completed so far
- What files were created or modified (list all)
- The current state of each file (working? broken? partial?)

## What's Left To Do
- Remaining tasks in priority order
- Known issues or bugs that exist in the current code
- Design decisions that were made (and why)
- Design decisions that are still open

## Key Context
- Architecture patterns being followed (reference .cursorrules)
- Business rules that aren't obvious from the code
- PHI/security considerations specific to this feature
- Test status (what passes, what fails, what's missing)

## How to Continue
- Which files to read first to get oriented
- The next immediate task
- Any commands to run (build, test, migrate)

## Open Questions
- Things we weren't sure about and deferred
- Things that need product/architecture input
```

### Step 2: Start the New Session

Open a new Composer window and paste:

```
I'm picking up work that was started in a previous session. Here's the handoff:

[Paste the handoff document]

Before writing any code:
1. Read the files listed in the handoff
2. Confirm you understand the current state
3. Tell me what you think the next step should be
4. Only proceed after I confirm
```

## Example: Mid-Feature Handoff

```
## Current State
Working on prescription transfer feature (PROJ-1234).

Completed:
- Entity: PrescriptionTransfer.cs ✅
- Migration: 20260331_AddPrescriptionTransfers ✅ (applied to dev DB)
- Service interface: ITransferService.cs ✅
- Service implementation: TransferService.cs ⚠️ (approve/deny methods not yet implemented)
- Controller: TransferController.cs ⚠️ (POST and GET endpoints working, PUT endpoints stubbed)
- DTOs: TransferRequestDto, TransferResponseDto ✅
- Tests: TransferServiceTests.cs ⚠️ (happy path tests pass, error cases not yet written)

## What's Left To Do
1. Implement TransferService.ApproveTransfer() — should update prescription's CurrentPharmacyId
2. Implement TransferService.DenyTransfer() — should set status and denial reason
3. Complete PUT /api/transfers/{id}/approve endpoint
4. Complete PUT /api/transfers/{id}/deny endpoint
5. Add audit logging to all transfer status changes
6. Write error case tests (duplicate transfer, wrong pharmacy, expired prescription)
7. Add XML doc comments

## Key Context
- Using Result<T> pattern for business logic errors (not exceptions)
- Pharmacy authorization: verify user's PharmacyId claim matches the transfer's from/to pharmacy
- Notes field is PHI — do not log its contents
- Audit log format: AuditLogger.Log(userId, "TransferApproved", transferId, timestamp)

## How to Continue
1. Read: TransferService.cs, TransferController.cs, TransferServiceTests.cs
2. Next task: implement ApproveTransfer() in TransferService.cs
3. Run: dotnet test to see current state
```

## Handoff Between Developers

When handing work to a teammate, add:

```
## For [Teammate Name]
- PR is at: [URL] (draft, don't review yet)
- Branch: feature/prescription-transfers
- I'll be available on Slack for questions until [time]
- The hardest part remaining is [what's tricky]
- Watch out for [known gotcha]
```

## Tips
- Generate the handoff **before** you close the session — you can't get it back after
- The handoff doesn't need to be perfect. It needs to be good enough that the next session doesn't start from zero.
- Store handoffs in a predictable location: `docs/handoffs/` or as a comment on the Jira ticket
- If you're handing off to yourself tomorrow, the handoff is still valuable — you'll forget the context overnight
- For sensitive features, redact PHI from the handoff document (use generic descriptions instead of real data examples)
