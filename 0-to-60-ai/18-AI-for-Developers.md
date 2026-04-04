---
title: "Appendix: AI for Developers"
status: first-draft
include_in_compile: true
synopsis: "AI-assisted coding, debugging, refactoring, testing, and the repeatable workflows that make developers faster without lowering standards."
---

# AI for Developers

> Your quick-start guide to using AI as a force multiplier in your daily development work.

---

## AI in Your Day

You already know how to write code. AI does not change that -- it changes how fast you get from intent to working implementation. Here is where AI fits into a typical developer's day:

| Daily Task | How AI Helps | Time Saved |
|------------|-------------|------------|
| Writing boilerplate and scaffolding | Generates CRUD endpoints, data models, DTOs, mappers, and configuration classes from a brief description of what you need | 30-60 min per feature |
| Debugging errors and stack traces | Explains cryptic error messages, identifies root causes from code context, and suggests targeted fixes | 15-45 min per bug |
| Writing unit and integration tests | Generates test suites from function signatures and requirements, including edge cases, boundary values, and mock setups | 30-45 min per test suite |
| Understanding unfamiliar code | Explains what a function or module does, traces call chains, and summarizes design patterns in play | 20-40 min per investigation |
| Writing commit messages and PR descriptions | Analyzes diffs and generates conventional commit messages and structured PR summaries | 5-10 min per PR |
| Learning new frameworks or APIs | Provides working examples in the target framework, answers "how do I do X in Y" with production-quality code | 30-60 min per learning task |

The pattern across all of these: **you provide context and intent, AI provides a first draft, you review, refine, and own the result.** AI is your pair programmer, not your replacement.

---

## 30-Minute Quick Start

> Get productive with an AI coding assistant in one sitting. This guide focuses on setting up and using your first tool.

### Step 1: Pick Your Tool (5 minutes)

You have three main options. Pick the one that fits your workflow:

- **GitHub Copilot** -- If you work primarily in VS Code or a JetBrains IDE. Install the GitHub Copilot extension from the marketplace. Sign in with your GitHub account. Copilot provides inline completions as you type and a chat panel for longer requests. If your team has Copilot Business licenses, use those for enterprise data protections.
- **Cursor** -- If you want a full IDE with AI built into every interaction. Download Cursor (it is a fork of VS Code, so your extensions and settings carry over). Cursor excels at multi-file edits and understands your entire codebase via its indexing feature.
- **Claude Code** -- If you prefer the terminal. Install with `npm install -g @anthropic-ai/claude-code`, then run `claude` in your project directory. Claude Code reads your repository structure, understands your codebase, and executes file edits and shell commands with your approval.

### Step 2: Generate Your First Function (10 minutes)

Pick a function you need to write today. Open the AI chat panel (Copilot or Cursor) or your terminal (Claude Code) and describe it:

```
Write a C# method that takes an order ID (Guid), queries the database
using Entity Framework Core with our AppDbContext, and returns an
OrderDetailDto with line items. Throw an OrderNotFoundException
if the order does not exist. We use the repository pattern -- follow
the existing IOrderRepository interface.
```

Review what comes back. Check: Does it match your project's naming conventions? Does it handle nulls correctly? Does it use async/await properly? Make corrections and move on.

### Step 3: Debug Something With AI (10 minutes)

Take a recent error from your application. Paste the full stack trace and the relevant code into the chat. Ask:

```
Here is the stack trace and the code that triggers it. Explain the root
cause and suggest a fix. I am using .NET 8, Entity Framework Core 8,
and SQL Server.

[paste stack trace]
[paste relevant code]
```

Compare the AI's diagnosis to your own instinct. Apply the fix if it makes sense. If not, ask follow-up questions -- "Could this also be caused by a race condition in the connection pool?" The back-and-forth is where the real value is.

### Step 4: Generate Tests (5 minutes)

Copy a function from your codebase and ask:

```
Write xUnit tests for this method. Use Moq for dependencies. Cover:
happy path, null inputs, not-found scenarios, and the case where the
database throws a timeout exception. Use Arrange-Act-Assert and
descriptive test names.

[paste the method]
```

Adapt the generated tests to your project's conventions, run them, and verify they actually assert meaningful behavior.

---

## The AI-Assisted Development Loop

The most reliable engineering workflow with AI is not “ask once, accept output, commit.” It is a loop:

1. **Orient** the assistant with project context, nearby files, and constraints.
2. **Generate** a first draft or candidate implementation.
3. **Review** the code for architecture, security, edge cases, and correctness.
4. **Verify** with builds, tests, and runtime checks.
5. **Refine** with targeted follow-up prompts instead of starting over.
6. **Capture** the prompt, rule, or pattern if it worked especially well.

This loop is what turns AI from a novelty into a force multiplier.

### The Minimum Context To Provide

Before asking an AI coding tool to implement anything meaningful, give it:

- the relevant files or symbols to read first,
- the architecture or pattern it should follow,
- the error-handling and validation expectations,
- the auth or security constraints,
- the testing expectation,
- and what a successful result looks like.

This is why project context files such as `.cursorrules`, `CLAUDE.md`, and saved prompt patterns matter so much. They reduce repetition and improve output quality across the entire team.

### What To Save For Reuse

When a workflow works well, save:

- the prompt,
- the pattern,
- the project rule,
- the before-and-after example,
- or the test/review checklist.

Personal prompting skill is useful. Shared reusable assets are better.

---

## Concrete Examples

### Example 1: Feature Implementation with Proper Context

**Scenario:** You need to add a new endpoint to your .NET Web API for retrieving warehouse locations with filtering and pagination.

**Prompt:**
```
You are a senior C# developer. Write a complete implementation for a
paginated GET /api/locations endpoint in our .NET 8 Web API.

Existing patterns in this project:
- We use MediatR for CQRS (queries return DTOs via IRequestHandler)
- Repository pattern with ILocationRepository
- FluentValidation for request validation
- All endpoints return ActionResult<ApiResponse<T>>
- Pagination uses our PaginatedRequest base class with Page and PageSize

Requirements:
- Filter by IsActive (optional bool query param)
- Filter by Name (optional, case-insensitive contains)
- Sort by Name or CreatedDate (default: Name ascending)
- Return LocationDto with: Id, Name, Address, Capacity, CurrentOccupancy, IsActive
- Page size max 100, default 25

Produce: Controller endpoint, MediatR query + handler, FluentValidation
validator, and LocationDto.
```

**What to expect:** The AI generates four files that follow your specified patterns. Review each one: verify the query handler uses `IQueryable` for efficient filtering, check that the validator enforces PageSize bounds, and confirm the DTO mapping does not leak entity details. Wire the files into your project structure and adjust naming to match your conventions.

### Example 2: Debugging Workflow -- Stack Trace to Fix

**Scenario:** Your API intermittently returns 500 errors under load. You have a stack trace and the relevant code.

**Prompt:**
```
I have an intermittent 500 error in production. It happens under load
(~100 concurrent users) but not in dev with single requests.

Stack trace:
System.InvalidOperationException: Timeout expired. The timeout period
elapsed prior to obtaining a connection from the pool. This may have
occurred because all pooled connections were in use and max pool size
was reached.

Relevant code:
```csharp
public async Task<OrderDto> CreateOrderAsync(CreateOrderCommand cmd)
{
    using var connection = new SqlConnection(_connectionString);
    await connection.OpenAsync();

    var order = await _orderRepo.InsertAsync(connection, cmd);

    // Send confirmation email via external API
    await _emailService.SendOrderConfirmationAsync(order);

    // Update analytics dashboard
    await _analyticsService.RecordOrderAsync(connection, order);

    return _mapper.Map<OrderDto>(order);
}
```

Environment: .NET 8, Dapper, SQL Server on Azure, running in
Azure App Service with 3 instances.

Analyze: root cause, why it works in dev but fails under load,
specific code fix, and prevention recommendations.
```

**What to expect:** The AI identifies that the `_emailService.SendOrderConfirmationAsync` call holds the database connection open while waiting for an external HTTP call. Under load, connections pile up waiting for slow email API responses, exhausting the pool. The fix: restructure so the connection is closed before the external call, or use separate connections for the insert and analytics steps. The AI should also recommend connection pool tuning and circuit breakers for external services.

### Example 3: Writing Unit Tests from Existing Implementation

**Scenario:** You have a discount calculation method that needs test coverage before you refactor it.

**Prompt:**
```
Write xUnit tests with Moq for this C# method. Cover all code paths
including edge cases.

```csharp
public decimal CalculateDiscount(Order order, Customer customer)
{
    if (order.Items.Count == 0)
        throw new InvalidOperationException("Cannot discount empty order");

    decimal discount = 0m;

    if (customer.LoyaltyTier == LoyaltyTier.Gold)
        discount = 0.10m;
    else if (customer.LoyaltyTier == LoyaltyTier.Platinum)
        discount = 0.15m;

    if (order.Total > 500m)
        discount += 0.05m;

    return Math.Min(discount, 0.20m); // Cap at 20%
}
```

Test naming convention: MethodName_Scenario_ExpectedResult
```

**What to expect:** The AI generates tests for: empty order throws exception, no loyalty tier gives zero discount, Gold tier gives 10%, Platinum tier gives 15%, order over 500 adds 5%, Platinum with order over 500 caps at 20% (not 0.20), order at exactly 500 boundary, and order at 500.01. Review that the assertions are correct and the boundary at exactly 500 is handled as your business rules intend.

### Example 4: Refactoring Legacy Code

**Scenario:** You inherited a 120-line method that handles order processing, validation, persistence, and notification all in one block.

**Prompt:**
```
Refactor this method into smaller, single-responsibility methods.
Preserve existing behavior exactly. This code is in production with
no tests, so the refactoring must be safe and mechanical.

Explain each extracted method's purpose and any design patterns applied.
Flag any bugs or code smells you notice but do NOT fix them -- I will
address those separately after tests are in place.

```csharp
public async Task ProcessOrder(OrderRequest request)
{
    // [paste the 120-line method]
}
```
```

**What to expect:** The AI breaks the method into `ValidateOrderRequest`, `MapToOrderEntity`, `ApplyDiscountRules`, `PersistOrder`, and `SendNotifications`. Each method has a single responsibility. The AI flags potential issues (like swallowed exceptions or missing null checks) but does not fix them, respecting your request for a safe mechanical refactor. You run existing integration tests to verify behavior is preserved, then add unit tests for each extracted method.

---

## Code Examples

This section shows before/after code to illustrate what AI-assisted development looks like in practice.

### Before/After: Raw SQL to Repository Pattern

**Your prompt:** "Refactor this inline SQL into a proper repository using Dapper with parameterized queries. Follow the async repository pattern."

**Before:**
```csharp
// Inline SQL scattered through the controller
public IActionResult GetUser(int id)
{
    var conn = new SqlConnection(_connStr);
    conn.Open();
    var cmd = new SqlCommand("SELECT * FROM Users WHERE Id = " + id, conn);
    var reader = cmd.ExecuteReader();
    // ... manual mapping, no error handling, SQL injection risk
    conn.Close();
    return Ok(user);
}
```

**After (AI-generated, reviewed and adapted):**
```csharp
// Repository with parameterized queries and proper resource management
public class UserRepository : IUserRepository
{
    private readonly IDbConnectionFactory _connectionFactory;

    public async Task<User?> GetByIdAsync(int id)
    {
        using var connection = _connectionFactory.Create();
        return await connection.QuerySingleOrDefaultAsync<User>(
            "SELECT Id, Email, DisplayName, CreatedAt FROM Users WHERE Id = @Id",
            new { Id = id });
    }
}
```

The AI eliminated the SQL injection vulnerability, added parameterized queries, introduced proper async disposal with `using`, and selected specific columns instead of `SELECT *`. You still need to verify the column names match your schema and add the `IUserRepository` interface to your DI container.

### Before/After: Nested Conditionals to Guard Clauses

**Your prompt:** "Refactor this deeply nested method to use guard clauses and early returns. Keep the same behavior."

**Before:**
```csharp
public async Task<Result> SubmitExpenseReport(ExpenseReport report)
{
    if (report != null)
    {
        if (report.Items.Any())
        {
            if (report.Items.All(i => i.Amount > 0))
            {
                if (report.Total <= _policy.MaxAmount)
                {
                    var saved = await _repo.SaveAsync(report);
                    await _approvalService.RequestApprovalAsync(saved);
                    return Result.Success(saved.Id);
                }
                else { return Result.Failure("Exceeds policy limit"); }
            }
            else { return Result.Failure("All amounts must be positive"); }
        }
        else { return Result.Failure("Report must have at least one item"); }
    }
    else { return Result.Failure("Report is required"); }
}
```

**After (AI-generated, reviewed and adapted):**
```csharp
public async Task<Result> SubmitExpenseReport(ExpenseReport report)
{
    if (report is null)
        return Result.Failure("Report is required");

    if (!report.Items.Any())
        return Result.Failure("Report must have at least one item");

    if (report.Items.Any(i => i.Amount <= 0))
        return Result.Failure("All amounts must be positive");

    if (report.Total > _policy.MaxAmount)
        return Result.Failure("Exceeds policy limit");

    var saved = await _repo.SaveAsync(report);
    await _approvalService.RequestApprovalAsync(saved);
    return Result.Success(saved.Id);
}
```

Same behavior, half the nesting, twice the readability. The AI flattened the pyramid of doom into a linear sequence of guard clauses. Verify the logic inversion is correct (e.g., `All(i => i.Amount > 0)` becomes `Any(i => i.Amount <= 0)`).

### Before/After: Manual Mapping to Source Generator Pattern

**Your prompt:** "Replace this manual DTO mapping with a Mapperly source-generated mapper. We use Mapperly in this project."

**Before:**
```csharp
public static OrderDto ToDto(Order order)
{
    return new OrderDto
    {
        Id = order.Id,
        CustomerName = order.Customer.FullName,
        Items = order.Items.Select(i => new OrderItemDto
        {
            ProductName = i.Product.Name,
            Quantity = i.Quantity,
            UnitPrice = i.UnitPrice,
            LineTotal = i.Quantity * i.UnitPrice
        }).ToList(),
        Total = order.Items.Sum(i => i.Quantity * i.UnitPrice),
        Status = order.Status.ToString()
    };
}
```

**After (AI-generated, reviewed and adapted):**
```csharp
[Mapper]
public partial class OrderMapper
{
    [MapProperty(nameof(Order.Customer.FullName), nameof(OrderDto.CustomerName))]
    public partial OrderDto ToDto(Order order);

    [MapProperty(nameof(OrderItem.Product.Name), nameof(OrderItemDto.ProductName))]
    [MapDerived(nameof(OrderItemDto.LineTotal), nameof(MapLineTotal))]
    public partial OrderItemDto ToItemDto(OrderItem item);

    private decimal MapLineTotal(OrderItem item) => item.Quantity * item.UnitPrice;
}
```

The AI replaced manual mapping with Mapperly's compile-time source generation, eliminating runtime reflection overhead. Note that you need to verify the `MapDerived` attribute syntax against your Mapperly version -- AI sometimes generates attributes for the wrong library version.

---

## Prompt Templates

### 1. Feature Implementation with Context

```
You are a senior [LANGUAGE] developer. Implement the following feature
for our [FRAMEWORK] application.

PROJECT CONTEXT:
- Architecture: [e.g., Clean Architecture with MediatR, repository pattern]
- Database: [e.g., SQL Server with EF Core / PostgreSQL with Dapper]
- Existing conventions: [e.g., "All services are registered via Scrutor
  assembly scanning", "We use FluentValidation for all request objects"]

FEATURE:
[Describe the feature in 3-5 sentences]

REQUIREMENTS:
- [Specific requirement 1]
- [Specific requirement 2]
- [Error handling expectation]
- [Performance constraint, if any]

PRODUCE:
- [List the specific files/classes you want generated]

Follow existing project conventions. Include XML doc comments on public
methods. Do not use deprecated APIs.
```

### 2. Debug This Error

```
I need help diagnosing a bug in my [LANGUAGE/FRAMEWORK] application.

SYMPTOM: [What is happening -- be specific]
EXPECTED BEHAVIOR: [What should happen instead]
FREQUENCY: [Always / intermittent / only under load / only in production]

STACK TRACE:
[Paste the full stack trace]

RELEVANT CODE:
[Paste the code where the error occurs and any related methods]

ENVIRONMENT:
- Runtime: [e.g., .NET 8, Node 20, Python 3.12]
- Database: [e.g., SQL Server 2022, PostgreSQL 16]
- Hosting: [e.g., Azure App Service, Kubernetes, local Docker]
- Other relevant context: [e.g., "behind a load balancer", "uses Redis for caching"]

Provide:
1. Root cause diagnosis
2. Why this happens under [specific condition] but not otherwise
3. Code fix with explanation
4. Recommendations to prevent similar issues
```

### 3. Generate Tests for This Code

```
Write [TESTING FRAMEWORK] tests for the following [LANGUAGE] code.
Use [MOCKING LIBRARY] for dependencies.

CODE UNDER TEST:
[Paste the function or class]

COVER THESE SCENARIOS:
1. Happy path -- normal expected inputs produce correct output
2. Edge cases -- boundary values, empty collections, max/min values
3. Invalid inputs -- null, empty strings, negative numbers, wrong types
4. Error conditions -- exceptions from dependencies, timeouts, invalid state
5. Dependency interactions -- verify correct calls to mocked services

CONVENTIONS:
- Test naming: [e.g., MethodName_Scenario_ExpectedResult]
- Pattern: Arrange-Act-Assert
- One assertion per test where practical
- Descriptive failure messages on assertions
```

### 4. Code Review Request

```
Review the following code changes as a senior developer. Focus on issues
that automated linters would miss: logic errors, security risks,
performance problems, and maintainability concerns.

CONTEXT:
- Language: [LANGUAGE]
- Framework: [FRAMEWORK]
- This code is for: [Brief description of the feature/fix]
- Related ticket: [Jira/ADO ticket ID and summary]

CODE TO REVIEW:
[Paste the diff or the new code]

For each finding, provide:
1. Severity: Critical / Major / Minor / Nit
2. The specific line or block
3. What is wrong and why it matters
4. Suggested fix with code

Also provide:
- Overall assessment: Approve / Request Changes / Needs Discussion
- Missing test coverage: what scenarios are not tested
- Security: any input validation, auth, or data exposure concerns
```

---

## AI Coding Tools: A Quick Comparison

Three tools dominate the AI-assisted coding space. Each takes a different approach, and the best choice depends on how you work.

### GitHub Copilot

**Approach:** IDE extension that provides inline code completions and a chat panel. Agent mode (available since early 2025) can make multi-file edits from a natural language description.

**Best for:** Developers who want AI woven into their existing VS Code or JetBrains workflow with minimal disruption. The inline tab-completion is the fastest path from thought to code for small to medium tasks.

**Key strength:** Seamless integration with GitHub -- generates PR descriptions, performs automated code review, and connects Azure DevOps work items directly to coding tasks.

**Watch out for:** Inline completions can create a "tab-tab-tab" habit where you accept suggestions without fully reading them. Stay engaged.

### Cursor

**Approach:** A standalone IDE (VS Code fork) with AI capabilities built into every layer -- code completion, multi-file editing, codebase-wide Q&A, and a Composer mode for complex changes across files.

**Best for:** Developers who want the AI to understand their full codebase. Cursor indexes your project and uses that context to generate more accurate code. Multi-file edits in Composer mode are especially strong for refactoring.

**Key strength:** Codebase awareness. When you ask Cursor to implement a feature, it references your existing patterns, types, and conventions because it has indexed them.

**Watch out for:** It is a separate IDE, not a plugin. If your team has standardized on a specific VS Code configuration or JetBrains IDE, adopting Cursor means maintaining a different editor.

### Claude Code

**Approach:** A terminal-based agent that reads your repository, understands its structure, and makes changes by editing files and running commands -- with your approval at each step.

**Best for:** Developers who prefer working in the terminal and want an AI that can autonomously execute multi-step workflows (implement a feature, run tests, fix failures, commit). Especially powerful for large refactors and codebase exploration.

**Key strength:** Autonomy with oversight. Claude Code can plan and execute complex tasks (create files, run tests, iterate on failures) while asking for your approval before destructive operations. Its understanding of entire repositories makes it strong for "explain this codebase" tasks.

**Watch out for:** Usage-based pricing means costs scale with how much you use it. Monitor your usage, especially during long autonomous sessions.

### Which Should You Start With?

If your team already has GitHub Copilot licenses, start there -- it is the lowest-friction option. If you want deeper codebase understanding and multi-file editing, try Cursor. If you live in the terminal and want an AI that can execute end-to-end workflows, try Claude Code. They are not mutually exclusive; many developers use Copilot for inline completions and Claude Code for larger tasks.

---

## Pitfalls and Anti-Patterns

### 1. Blindly Accepting Generated Code

The most dangerous anti-pattern. AI-generated code can contain subtle bugs, use deprecated APIs, introduce security vulnerabilities, or implement patterns that are technically correct but wrong for your architecture. If you cannot explain every line to a colleague, do not commit it. Review AI output with the same rigor you apply to a junior developer's pull request.

**The fix:** Read every line. Run the code. Write tests for it. If the AI generated something you do not understand, ask it to explain -- or look it up yourself. Understanding is not optional.

### 2. Providing Insufficient Context

A prompt like "write a login function" produces generic, unusable code. The AI does not know your framework, authentication method, error handling conventions, or how the function integrates with your existing codebase. Garbage context in, garbage code out.

**The fix:** Always include: language and version, framework, existing patterns and conventions, specific requirements, and how the code fits into the broader system. The five minutes you spend writing a detailed prompt saves thirty minutes of fixing a bad first draft.

### 3. Skipping Tests Because "The AI Wrote It"

AI-generated code needs tests just as much as human-written code -- arguably more, because you did not reason through the logic yourself. "It looks right" is not a test strategy.

**The fix:** Generate the tests with AI if you want, but run them and verify they assert meaningful behavior. A test that just verifies the function does not throw is not testing anything useful. Check that edge cases and error paths are covered.

### 4. Using AI as a Substitute for Learning

If you use AI to generate code in a framework you do not understand, you are accumulating technical debt you cannot service. When the code breaks at 2 AM, you need to understand it well enough to diagnose and fix it without AI assistance.

**The fix:** When AI generates code using patterns you are unfamiliar with, take the time to understand them. Ask the AI to explain the pattern. Read the documentation. Use AI as a tutor, not a crutch.

### 5. Leaking Sensitive Data Into AI Prompts

Pasting production connection strings, API keys, customer data, or proprietary business logic into a public AI tool is a security incident waiting to happen. Consumer-tier AI tools may use your input for training.

**The fix:** Use enterprise AI deployments (GitHub Copilot Business, Azure OpenAI) that provide data isolation. Strip sensitive values before pasting code. Never paste credentials, PII, or production data into any AI chat. When in doubt, check your organization's AI usage policy.

### 6. Treating passing tests as proof that the generated code is good

AI-generated code can compile and pass a shallow test suite while still being wrong in production-relevant ways. Weak tests often give false confidence because they only validate the happy path or the implementation mechanics.

**The fix:** Ask for tests that verify behavior, error paths, and edge cases. Read the generated tests carefully. A passing test suite is evidence, not certainty.

### 7. Forgetting to capture the workflow that worked

Developers often discover a great prompt or a clean workflow, use it once, and then lose it. The team gets no compounding value from the experience.

**The fix:** Save strong prompts, patterns, and project rules. The real leverage comes from making good workflows reusable for the next task and the next engineer.

---

*[Back to Curriculum Overview](00-Curriculum-Overview.md)*
