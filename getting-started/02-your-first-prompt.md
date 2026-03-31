# Your First Prompt: From Zero to Working Code

This guide walks you through your first AI-assisted development task on a real .NET project. By the end, you'll have added a working API endpoint with tests in under 15 minutes.

## Before You Start

- [ ] Cursor is installed and configured (see `01-installing-cursor.md`)
- [ ] You have a .NET project open in Cursor
- [ ] The project builds successfully (`dotnet build`)
- [ ] There's a `.cursorrules` file in the project root (grab one from `cursorrules/`)

## Step 1: Give the AI Context

Open Composer (`Ctrl+Shift+I`) and start by orienting the AI:

```
Read the project structure. Look at:
- Program.cs (startup and DI configuration)
- Any existing controllers
- Any existing service interfaces and implementations
- The .cursorrules file

Tell me what patterns this project uses for:
1. Controller structure
2. Dependency injection
3. Error handling
4. Response types
```

**Why this works:** You're not asking the AI to write code yet. You're asking it to understand the project first. This prevents it from generating code that doesn't match your existing patterns.

**Read the response.** The AI should describe your project's architecture accurately. If it gets something wrong, correct it — "No, we use MediatR for CQRS, not direct service injection." This correction stays in the conversation context and improves all subsequent output.

## Step 2: Describe Your Task

Now give it a real task. Here's an example:

```
Add a new endpoint to manage user notifications. Specifically:

1. Create a NotificationsController at /api/notifications
2. Add a GET endpoint that returns all notifications for the authenticated user
3. Add a POST endpoint to mark a notification as read
4. Create an INotificationService interface and implementation
5. Register the service in DI
6. Add unit tests for the service

Follow the same patterns you identified in the existing code.
The authenticated user ID comes from the JWT claims (ClaimTypes.NameIdentifier).
Use the existing database context for data access.
```

**What makes this prompt effective:**

- **Specific endpoints** — not "add notification stuff"
- **Clear data flow** — where the user ID comes from
- **Explicit architecture** — interface + implementation + DI registration
- **Reference to existing patterns** — "Follow the same patterns you identified"
- **Includes tests** — AI-generated tests catch AI-generated bugs

## Step 3: Review the Diff

Cursor will show you the proposed changes as diffs. **Read every line.** Look for:

- Does the controller follow the same structure as existing controllers?
- Are the DI registrations in the right place?
- Does the service interface make sense?
- Do the tests actually test meaningful behavior?

If something is wrong, don't start over. Refine:

```
The controller looks good, but:
1. We use IActionResult return types, not ActionResult<T>
2. Add XML doc comments matching the style in UserController
3. The test should use Moq for the service mock, not a manual stub
```

## Step 4: Test It

```bash
dotnet build
dotnet test
dotnet run
```

Try the endpoint:

```bash
curl https://localhost:5001/api/notifications -H "Authorization: Bearer <your-token>"
```

If it doesn't work, paste the error into Composer:

```
I'm getting this error when I call the endpoint:

[paste the full error/stack trace]

The request I'm making is:
curl https://localhost:5001/api/notifications -H "Authorization: Bearer <token>"
```

The AI will diagnose and fix. This iterate-until-it-works loop is where the speed comes from — you're not manually debugging, you're describing the problem and letting the AI solve it.

## Step 5: Commit

Once everything works:

```
Write a conventional commit message for these changes.
Format: feat(notifications): <description>
Include a body that explains the architectural decisions.
```

## What You Just Did

You built a feature that would normally take 2-4 hours in about 15-30 minutes:

- Controller with two endpoints
- Service interface and implementation
- DI registration
- Unit tests
- Conventional commit message

You did it by being **specific about what you wanted** and **letting the AI handle the implementation details** while you focused on reviewing the output.

## Next Steps

- Read `03-project-context.md` to learn how `.cursorrules` files make this even faster
- Browse `prompts/by-task/` for ready-made prompts for common tasks
- Try `prompts/patterns/vertical-slice.md` to build a complete feature across all layers
