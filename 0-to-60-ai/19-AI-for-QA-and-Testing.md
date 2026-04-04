---
title: "Appendix: AI for QA & Testing"
status: first-draft
include_in_compile: true
synopsis: "AI for test case generation, test plans, and exploratory testing strategies."
---

# AI for QA & Testing

> Your quick-start guide to using AI in your daily work.

---

## AI in Your Day

A typical QA engineer spends their day writing test cases, executing test plans, filing detailed bug reports, building automation scripts, analyzing test results, and triaging regression failures. AI does not replace the curiosity and instinct that make great testers — it handles the repetitive drafting so you can focus on the thinking. Here is where AI fits into your daily workflow:

| Daily Task | How AI Helps | Time Saved |
|------------|-------------|------------|
| Writing test cases from requirements | AI generates comprehensive test cases — positive, negative, boundary, and edge — from a user story or acceptance criteria in Jira | 1–2 hours per feature |
| Creating test plans for new features | AI drafts a structured test plan with scope, strategy, entry/exit criteria, and risk assessment from a feature description or epic | 2–3 hours per plan |
| Writing detailed bug reports | AI structures reproduction steps, expected vs. actual behavior, and environment details from a rough description of the issue | 10–15 min per bug |
| Creating automation scripts | AI generates test automation code (Selenium, Playwright, REST Assured) from manual test case descriptions, matched to your framework conventions | 30–60 min per script |
| Analyzing test failure patterns | AI reviews test run results and identifies flaky tests, environment-specific failures, and recurring regression areas | 30–45 min per analysis |
| Generating test data for edge cases | AI produces realistic synthetic data sets covering boundary values, special characters, locale-specific formats, and data relationship constraints | 20–30 min per data set |

---

## 30-Minute Quick Start

> Get productive with AI in 30 minutes. Follow these steps in order.

### Step 1: Generate test cases from a user story (10 min)

Open your current sprint board in Jira and pick a user story that has clear acceptance criteria. Copy the story title, description, and all acceptance criteria. Paste them into ChatGPT or Claude with this prompt:

```
You are a senior QA engineer. Generate comprehensive test cases for this user story.
Cover these categories: positive/happy path, negative, boundary, and edge cases.
For each test case, provide: Test ID, Category, Title, Preconditions, Steps, Expected Result.

USER STORY:
[paste here]

ACCEPTANCE CRITERIA:
[paste here]
```

Review the output. You will likely see 15-25 test cases. Check for anything the AI missed based on your knowledge of the system — integration points, performance under load, and scenarios that depend on state from other features. Add the refined test cases as sub-tasks in Jira or import them into Azure Test Plans.

### Step 2: Turn a rough bug into a proper report (10 min)

Think of a bug you recently found but have not documented yet, or one where the report could be stronger. Describe the issue informally to AI — what you did, what happened, what you expected. Use the Bug Report Enhancer template from the Prompt Templates section below. Paste the AI output into a new Jira bug ticket. Before submitting, verify the reproduction steps by walking through them yourself and attach any relevant screenshots or logs.

### Step 3: Generate a test automation script (10 min)

Pick a manual test case that your team runs repeatedly — a login flow, a form submission, or an API call. Copy the manual steps and paste them into AI along with your framework details (for example: "We use Playwright with TypeScript and the Page Object Model pattern"). Ask AI to generate the automation script. Review the output for correct selectors, proper waits, and alignment with your project's test structure. Drop the script into your test project and run it against your test environment.

---

## Concrete Examples

### Example 1: Generating comprehensive test cases from a user story

**Scenario.** Your sprint includes this Jira story: "As a customer, I want to reset my password via email so that I can regain access to my account." The acceptance criteria state: reset link sent to registered email, link expires after 24 hours, link is single-use, password complexity rules enforced on new password, and all active sessions terminated after reset.

**Prompt.**
```
You are a senior QA engineer with expertise in security testing. Generate
comprehensive test cases for the following user story.

USER STORY: As a customer, I want to reset my password via email so that I can
regain access to my account.

ACCEPTANCE CRITERIA:
- Registered email receives a reset link within 2 minutes
- Unregistered email shows generic message (no information leakage)
- Reset link expires after 24 hours
- Reset link is single-use
- New password must meet complexity rules (8+ chars, uppercase, lowercase, number, special char)
- Old password is invalidated after successful reset
- All active sessions are terminated after reset

APPLICATION CONTEXT:
- Web application with REST API backend
- Authentication via JWT tokens
- Rate limiting enabled on API endpoints

Generate test cases covering: functional, negative, boundary, security, and
accessibility. For each test case provide: TC-ID, Category, Priority (P1/P2/P3),
Title, Preconditions, Steps, Expected Result, and Test Data.
```

**What to expect.** AI typically generates 20-30 test cases. Functional tests cover each acceptance criterion. Negative tests include: empty email, malformed email, deactivated account. Boundary tests include: password at exactly 8 characters, reset link at exactly 24 hours. Security tests include: verifying cryptographically random tokens, HTTPS enforcement, rate limiting, and no information leakage about registered emails. Review the output and add tests specific to your system that AI could not know about — email provider integration, mail server downtime behavior, and SSO interaction.

### Example 2: Creating a test plan for a new feature

**Scenario.** Your team is building a new order export feature that lets users download order history as CSV or PDF, with date range filters and role-based access. The product owner has written an epic in Jira with five stories. You need a test plan before the team starts development.

**Prompt.**
```
You are a senior QA lead creating a test plan for a new feature. Draft a
complete test plan document with the following sections:

1. Overview and Objectives
2. Scope (in-scope and out-of-scope)
3. Test Strategy (test types to be used and why)
4. Test Environment Requirements
5. Entry and Exit Criteria
6. Test Cases Summary (categories and estimated count per category)
7. Risk Assessment (what could go wrong and how testing mitigates it)
8. Schedule and Resource Needs
9. Dependencies and Assumptions

FEATURE DESCRIPTION:
Order Export feature allowing users to download order history as CSV or PDF.
- Date range filter (last 7 days, 30 days, 90 days, custom range)
- Role-based access: Admins export all orders, Managers export their team's
  orders, Users export only their own orders
- Export includes: Order ID, date, customer name, items, total, status
- PDF includes company branding and formatting
- CSV is plain comma-separated with headers
- Maximum export size: 10,000 orders
- Background job for large exports with email notification when ready

TECH STACK: .NET 8 API, React frontend, Azure Blob Storage for generated files,
Azure Service Bus for background job queue.

TOOLS: Azure Test Plans for test management, Jira for tracking, Playwright for
UI automation, REST Assured for API testing.
```

**What to expect.** AI produces a structured test plan suitable for Confluence or Google Docs. The risk assessment is particularly useful — it flags risks like large export timeouts, PDF rendering inconsistencies, role-based access bypass via direct API calls, and Blob Storage token expiration. Adjust the schedule section to match your actual team capacity.

### Example 3: Writing automation scripts from manual test cases

**Scenario.** You have a set of manual test cases for a login flow that your team runs every regression cycle. You want to automate them using Playwright so they run in your Azure DevOps pipeline.

**Prompt.**
```
You are a senior QA automation engineer. Convert these manual test cases into
automated Playwright tests using TypeScript and the Page Object Model pattern.

MANUAL TEST CASES:
1. TC-001: Valid Login — Navigate to /login, enter valid username and password,
   click Sign In, verify redirect to /dashboard, verify username displayed in
   top-right header.
2. TC-002: Invalid Password — Navigate to /login, enter valid username with
   wrong password, click Sign In, verify error message "Invalid credentials"
   displayed, verify user stays on /login.
3. TC-003: Empty Fields — Navigate to /login, click Sign In without entering
   any credentials, verify validation messages on both fields.
4. TC-004: Account Lockout — Attempt login with wrong password 5 times in a
   row, verify account locked message on 6th attempt.

CONVENTIONS:
- Base URL is configured via environment variable BASE_URL
- Use data-testid attributes for selectors (e.g., data-testid="login-username")
- All tests must be independent and clean up after themselves
- Include retry logic for flaky network conditions
- Tests run in Azure DevOps Pipeline as a pipeline task

Generate:
1. A LoginPage page object class
2. A DashboardPage page object class (just the header verification)
3. The test file with all four test cases
4. A brief explanation of how to integrate into Azure DevOps Pipeline YAML
```

**What to expect.** AI generates three files: two page object classes and a test spec with four `test()` blocks following Arrange-Act-Assert. It includes a note on adding a Playwright task to your Azure DevOps pipeline YAML. Review selectors, verify waits match your app's behavior, and run locally before committing.

### Example 4: Analyzing bug patterns to predict risk areas

**Scenario.** You have had a rough few sprints with more bugs escaping to production than usual. You want to analyze the pattern and focus testing effort on the riskiest areas. You export your Jira bug data from the past three months.

**Prompt.**
```
You are a senior QA analyst specializing in defect analysis and test strategy.
Analyze the following bug data and identify patterns, risk areas, and
recommendations for targeted testing.

BUG DATA (exported from Jira):
[Paste a table or CSV with columns: Bug ID, Title, Component, Severity,
Sprint Found, Sprint Introduced, Root Cause Category, Time to Fix]

Provide:
1. Bug distribution by component — which components have the most defects?
2. Severity trend — are we finding more critical bugs over time?
3. Escape analysis — which bugs were found in production vs. in testing?
4. Root cause patterns — what categories of bugs recur most frequently?
5. Sprint-over-sprint trend — is quality improving or declining?
6. Risk heat map — rank components by risk (bug count x average severity)
7. Recommendations — where should we increase test coverage, add automation,
   or request code review focus?

Format the analysis with tables and clear headings suitable for a Confluence page
or a presentation to the development team.
```

**What to expect.** AI produces tables showing bug counts by component, severity trends, and a risk-ranked list. Recommendations are actionable — for example, "The Payment component has had 12 Critical/Major bugs in three months, 4 escaped to production. Recommend: add API contract tests, increase code review focus on payment PRs, and build a dedicated payment regression suite." Use this analysis in sprint planning to justify where to focus testing effort.

---

## Prompt Templates

### 1. Test Case Generator from User Story

```
You are a senior QA engineer. Generate comprehensive test cases for the
following user story.

USER STORY:
[PASTE THE USER STORY TITLE AND DESCRIPTION]

ACCEPTANCE CRITERIA:
[PASTE THE ACCEPTANCE CRITERIA]

CONTEXT:
- Application type: [web / mobile / API]
- Authentication method: [e.g., JWT, OAuth, session-based]
- Key integrations: [e.g., payment gateway, email service, third-party API]
- Known constraints: [e.g., rate limits, file size limits, browser support]

Generate test cases in these categories:
1. **Positive/Happy path** — Verify each acceptance criterion works as expected
2. **Negative** — Invalid inputs, unauthorized access, missing required fields
3. **Boundary** — Min/max values, character limits, empty and full states
4. **Edge cases** — Concurrent users, timeout scenarios, special characters,
   Unicode, locale-specific formats
5. **Accessibility** — Keyboard navigation, screen reader compatibility, color
   contrast, ARIA labels

For each test case, provide:
- Test ID (TC-001, TC-002, etc.)
- Category
- Priority (P1 = critical path / P2 = important / P3 = edge case)
- Title
- Preconditions
- Steps (numbered)
- Expected result
- Test data needed

Format as a table that can be pasted into Azure Test Plans or a spreadsheet.
```

### 2. Test Plan Creator

```
You are a senior QA lead. Create a test plan for the following feature or epic.

FEATURE/EPIC DESCRIPTION:
[PASTE THE FEATURE DESCRIPTION, USER STORIES, OR EPIC SUMMARY FROM JIRA]

TECH STACK: [e.g., .NET 8, React, Azure SQL, Redis]
TESTING TOOLS: [e.g., Azure Test Plans, Playwright, REST Assured, Jira]

Create a test plan with these sections:
1. Objective — what this plan validates and why
2. Scope — in scope and explicitly out of scope
3. Test Strategy — which test types apply and why
4. Test Environment — required environments, data, and configurations
5. Entry/Exit Criteria — what must be true before and after testing
6. Test Case Categories — summary of groups with estimated count
7. Risk Assessment — risks and how testing addresses each
8. Dependencies — external teams, services, or data needed
9. Schedule — estimated effort in tester-days by phase

Format for pasting into a Confluence page.
```

### 3. Bug Report Enhancer

```
Write a professional bug report based on my informal description.

WHAT HAPPENED:
[DESCRIBE THE BUG IN YOUR OWN WORDS — be as rough and informal as you like]

APPLICATION: [NAME AND VERSION]
ENVIRONMENT: [BROWSER, OS, DEVICE, OR API CLIENT]

Format as:
- **Title**: Clear summary (under 80 characters)
- **Severity**: Critical / Major / Minor / Cosmetic (with justification)
- **Priority**: P1 / P2 / P3 / P4
- **Component**: [suggest the likely module]
- **Preconditions**: What must be true before reproducing
- **Steps to Reproduce**: Numbered, specific steps
- **Expected Result**: What should happen
- **Actual Result**: What actually happens, including exact error messages
- **Frequency**: Always / Intermittent / Once
- **Workaround**: If any exists
- **Attachments Needed**: What screenshots or logs to attach
- **Related Areas to Test**: Other features that might share the same root cause

If my description is ambiguous, state your assumptions.
```

### 4. Regression Test Priority Analyzer

```
You are a senior QA strategist. Recommend a prioritized regression test suite.

RELEASE CHANGES:
[PASTE JIRA TICKETS, PR DESCRIPTIONS, OR CHANGELOG FOR THIS RELEASE]

COMPONENTS CHANGED: [LIST MODULES OR SERVICES MODIFIED]
HISTORICAL BUG DATA (optional): [RECENT BUGS BY COMPONENT OR KNOWN RISK AREAS]
AVAILABLE TEST TIME: [TESTER-HOURS OR PIPELINE MINUTES AVAILABLE]

Provide:
1. **P1 — Must Run**: Tests covering areas directly changed. Map each to
   the specific change that makes it critical.
2. **P2 — Should Run**: Tests for tightly coupled areas or those with
   historical regression bugs.
3. **P3 — Run If Time Permits**: Tests for stable, low-defect-rate areas.
4. **Recommended Cuts**: If time is limited, what can be safely deferred.
5. **Automation Candidates**: P1/P2 tests still manual that should be automated.

Format as a table: Priority, Test Area, Reason, Estimated Effort, Automation Status.
```

---

## AI-Powered Testing Tools

Beyond general-purpose AI assistants like ChatGPT and Claude, several purpose-built tools are transforming QA workflows. Here is a brief overview of the most relevant options for your stack:

| Tool | What It Does | How It Fits Your Stack |
|------|-------------|----------------------|
| **Azure Test Plans** | Test case management with AI-assisted features for organizing, executing, and tracking manual and automated tests | Native integration with Azure DevOps and your existing pipelines |
| **testRigor** | Write and maintain automated tests in plain English — no code, no selectors. AI handles element identification and self-healing | Integrates with Jira and CI/CD pipelines; good for teams scaling automation quickly |
| **Katalon** | Full-stack test automation platform (web, API, mobile, desktop) with built-in GenAI features for test generation and self-healing scripts | Integrates with Jira, Azure DevOps, and CI/CD; supports teams with mixed skill levels |
| **Applitools** | AI-powered visual regression testing that compares UI screenshots and intelligently identifies meaningful visual changes | Plugs into Playwright, Selenium, and Cypress; catches visual regressions that functional tests miss |
| **Mabl** | Intelligent test automation with auto-healing tests that adapt when the UI changes, plus built-in performance and accessibility checks | Integrates with Jira and CI/CD; reduces test maintenance burden |

**When to use what.** Use ChatGPT or Claude for generating test cases, test plans, and analyzing defect data. Use the specialized tools when you need persistent automation in your pipeline, self-healing tests, or visual regression coverage. The two categories complement each other.

---

## AI Evaluation Is Not The Same As Traditional Test Automation

As soon as your product includes prompts, retrieval, model routing, or agent behavior, QA work expands beyond standard UI and API testing.

You also need to ask:

- does the answer stay grounded in the right sources,
- does the prompt change improve one case while breaking another,
- does the system return the required format consistently,
- and does the AI behave safely under adversarial or ambiguous inputs.

This is why AI-enabled products benefit from dedicated evaluation sets, regression suites, and rubric-based review in addition to normal automated tests.

For a deeper treatment of this topic, continue to [AI Evaluation and Quality Engineering](../21-Appendix-AI-Evaluation-And-Quality/Appendix-AI-Evaluation-And-Quality.md).

---

## Pitfalls & Anti-Patterns

### 1. Trusting AI test cases as exhaustive

AI generates test cases based on the requirements you provide. If the requirements are incomplete, the test cases will have the same blind spots. AI is especially weak at identifying tests that require understanding the broader system context — how this feature interacts with other features, what data states exist in production, and what users actually do versus what the requirements say. Always apply your domain knowledge and exploratory testing instincts on top of AI-generated test suites. Treat AI output as a thorough first draft, not a finished test plan.

### 2. Generating automation scripts without providing your conventions

If AI generates a Playwright script but your team uses a custom base class, specific selector strategies, or particular patterns for waits and data setup, the generated code will not fit your project. You will spend more time refactoring than writing from scratch. Always provide AI with an example of an existing test from your project so it matches your conventions. Include your page object patterns, fixture setup, and naming standards in the prompt.

### 3. Over-relying on AI for exploratory testing

AI can suggest exploratory testing charters and risk-based testing focus areas, but real exploratory testing requires human curiosity, intuition, and the ability to notice when something "feels wrong." Use AI to generate a starting checklist or charter, then let your instincts guide you beyond it. The most valuable bugs are found when you go off-script, and AI cannot go off-script by definition.

### 4. Pasting production data or PII into AI prompts

When generating test data or analyzing bug reports, it is tempting to paste real production data into ChatGPT or Claude for context. Do not do this. Production data may contain customer PII, financial information, or other sensitive content that should never leave your controlled environments. Instead, describe the data structure and ask AI to generate synthetic equivalents. If you need to share real data patterns, anonymize them first. Use your organization's enterprise AI deployment (such as Azure OpenAI) for any work involving sensitive information.

### 5. Skipping review of AI-generated test logic

AI-generated test cases can contain logical errors — asserting the wrong expected result, missing a critical precondition, or testing a scenario that is actually impossible in your system. A test case that looks correct on the surface but validates the wrong behavior is worse than no test case at all, because it creates false confidence. Read every generated test case critically. Ask yourself: "Does this precondition actually make sense? Is this expected result correct for our system? Would this test actually catch the bug it claims to cover?"

---

*[Back to Curriculum Overview](00-Curriculum-Overview.md)*
