---
title: "Appendix: AI for Dev Managers & Tech Leads"
status: first-draft
include_in_compile: true
synopsis: "AI-assisted code review, architecture decisions, team productivity, and coaching a development team into better AI habits."
---

# AI for Dev Managers & Tech Leads

> Your quick-start guide to using AI in your daily work.

---

## AI in Your Day

A typical dev manager or tech lead splits their time between reviewing code, making architecture decisions, tracking team health, managing technical debt, writing documentation, and mentoring developers. AI does not replace any of these responsibilities, but it can handle the first-draft grunt work so you spend more time on judgment calls and less time on boilerplate.

| Daily Task | How AI Helps | Time Saved |
|------------|-------------|------------|
| **Code review** | AI pre-screens pull requests for common issues — naming, error handling, security anti-patterns — so your human review focuses on design, business logic, and maintainability | 20–30 min per review cycle |
| **Architecture decision records** | AI drafts structured ADRs from a verbal description of the problem, options considered, and chosen approach; you edit for accuracy and team context | 30–45 min per ADR |
| **Technical debt assessment** | AI analyzes code patterns, dependency age, and complexity metrics to produce a prioritized tech debt inventory with estimated remediation effort | 2–3 hours per assessment |
| **Sprint health and velocity analysis** | AI interprets Jira velocity data and sprint history to surface trends, carryover patterns, and bottlenecks you might miss scanning the board manually | 30–60 min per analysis |
| **Team capacity planning** | AI cross-references velocity history, planned leave, and backlog complexity to recommend realistic sprint commitments | 20–30 min per sprint |
| **Knowledge documentation** | AI generates onboarding guides, runbooks, and architecture docs from existing code and Confluence pages, giving you a first draft to refine | 1–2 hours per document |

---

## 30-Minute Quick Start

> Get productive with AI in 30 minutes. Follow these three steps in order.

### Step 1: Pre-screen a pull request with AI (10 min)

1. Open a recent pull request in Azure Repos or GitHub. Copy the full diff (or the most substantial file changes if the diff is very large).
2. Paste the diff into ChatGPT, Claude, or your preferred LLM. Use the **PR Pre-Review Checklist** prompt template below.
3. Read the AI's findings. Note which issues you agree with and which are false positives.
4. Use the AI output as a checklist when you do your human review. Over the next few PRs, you will calibrate how much you can rely on the AI's judgment for your codebase.

**What to expect:** The AI will typically catch 3–8 findings per PR: missing null checks, inconsistent naming, potential security gaps, and obvious performance issues. It will occasionally flag correct code as problematic (false positives), which is why human review remains essential.

### Step 2: Draft an architecture decision record (10 min)

1. Think of a recent technical decision your team made — choosing between message queues, adopting a new library, changing a deployment pattern.
2. Use the **ADR Generator** prompt template below. Fill in the problem, options, constraints, and chosen option.
3. Review the generated ADR for technical accuracy. AI often presents balanced pros and cons but may miss constraints specific to your environment.
4. Share the draft with your team for feedback, then publish to your Confluence architecture space.

**What to expect:** A well-structured ADR with clear context, a justified decision, and honest trade-off analysis. You will need to add team-specific details (internal system names, compliance requirements, budget numbers) that the AI could not know.

### Step 3: Analyze your team's velocity trend (10 min)

1. Export the last six sprints of velocity data from Jira — story points committed vs. completed per sprint, plus carryover counts.
2. Paste the data into AI and use the **Team Capacity Planning** prompt template below, adjusted to request a trend analysis.
3. Review the AI's assessment: Is velocity stable, improving, or declining? What patterns does it identify?
4. Use the insights in your next sprint planning session or 1:1 conversations — as a data point, not a verdict.

**What to expect:** The AI will identify trends humans often overlook: gradually increasing carryover, a correlation between sprint size and completion rate, or a dip in velocity that coincides with a team change. It will not know why the pattern exists — that is your job.

---

## AI Coaching and Team Enablement

For a dev manager or tech lead, the highest-leverage use of AI is not just personal productivity. It is raising the capability of the whole team.

Your role is to help the team move from:

- isolated prompting,
- inconsistent tool usage,
- unshared personal tricks,
- and anxiety about quality or job impact

to:

- shared workflows,
- explicit guardrails,
- reusable prompt and rule assets,
- faster review loops,
- and visible examples of safe, high-quality AI-assisted work.

### What An Effective AI Coach Does

- creates a sanctioned path for experimentation,
- makes good examples visible,
- treats prompt quality as a learnable engineering skill,
- pushes the team toward reusable assets instead of one-off heroics,
- protects quality by strengthening review, testing, and rollout controls,
- and avoids turning AI adoption into ideology or pressure theater.

### A Practical 90-Day Team Pattern

**Month 1: Reduce friction**

- ensure tool access exists,
- ensure project context files exist,
- identify one or two trusted workflows,
- run live demos using real work.

**Month 2: Make reuse normal**

- create a shared prompt or pattern library,
- define when to use AI and when not to,
- introduce AI-generated PR summaries or pre-review,
- collect examples of both wins and failure modes.

**Month 3: Shift the operating model**

- tighten review and testing around AI-assisted speed,
- change story shaping toward clearer intent and risk,
- measure cycle time and review latency,
- coach skeptics through practical side-by-side work rather than lectures.

### Metrics That Help Without Becoming Surveillance

Useful team-level signals include:

- cycle time,
- PR review turnaround,
- escaped defects,
- test coverage on changed code,
- number of reusable prompt/rule contributions,
- number of engineers using at least one known-good workflow.

Avoid using AI metrics as individual performance weapons. The goal is capability building and flow improvement, not new ways to micromanage.

---

## Concrete Examples

### Example 1: AI-Assisted Code Review (Pre-Review Before Human Review)

**The scenario.** You are a tech lead responsible for reviewing PRs across three squads. A developer submits a 400-line PR touching the authentication middleware and two API controllers. Rather than spending 45 minutes on a cold read, you run the diff through AI first.

**The workflow.**

1. Copy the diff from Azure Repos (or use a tool like CodeRabbit or Qodo Merge that integrates directly).
2. Paste the diff into your LLM with the PR Pre-Review Checklist prompt (see Prompt Templates below).
3. The AI returns 6 findings: a missing authorization check on one endpoint (Critical), two methods with swallowed exceptions (Warning), an N+1 query in a loop (Warning), and three naming inconsistencies (Suggestion).
4. You confirm the authorization gap and the N+1 query are real issues. One of the "swallowed exceptions" is actually intentional (a retry mechanism with its own logging) — you discard that finding.
5. You add comments on the PR for the confirmed issues and spend your remaining review time on the architectural fit and business logic correctness that the AI cannot evaluate.

**Why this works.** The AI catches the "mechanical" issues in 2 minutes instead of 15, freeing your review time for the questions only a human can answer: Does this change fit the system's direction? Does it handle our specific business rules correctly? Is the approach one the team can maintain?

**Tool options.** CodeRabbit and Qodo Merge automate this workflow by posting AI review comments directly on PRs in GitHub and Azure Repos. The Qodo 2025 AI Code Quality report found that AI code reviews led to quality improvements 81% of the time, and Atlassian's RovoDev study showed 38.7% of AI review comments resulted in additional code fixes.

### Example 2: Architecture Decision Record Generation

**The scenario.** Your team needs to choose between Azure Service Bus and Azure Event Grid for an event-driven notification feature. The decision was discussed in a 30-minute meeting, and now you need to document it formally.

**The workflow.**

1. Summarize the discussion: the use case (fan-out notifications to multiple downstream services), throughput requirements (approximately 500 events per second at peak), ordering requirements (per-entity ordering needed), team familiarity (team has experience with Service Bus, none with Event Grid), and the decision (Service Bus with topic/subscription model).
2. Paste this summary into your LLM using the ADR Generator prompt template.
3. The AI generates a complete ADR with three options compared (Service Bus, Event Grid, and a custom solution using Azure Storage Queues), pros and cons for each, and consequences of the chosen approach.
4. You edit the draft: correct the throughput comparison (the AI slightly overstated Event Grid's latency advantage), add your team's specific compliance requirement for message retention, and note that the decision should be revisited when the team evaluates Azure Event Grid v2.
5. Publish to Confluence under your team's Architecture Decisions space.

**Why this works.** Writing an ADR from scratch takes 45–60 minutes. Editing an AI draft takes 15–20 minutes. More importantly, the AI ensures you do not skip sections — it always includes consequences and alternatives, which are the sections most teams leave blank when writing ADRs manually.

### Example 3: Technical Debt Assessment and Prioritization

**The scenario.** Your VP of Engineering asks for a technical debt inventory ahead of a planning quarter. You need to assess three services and recommend where to invest remediation effort.

**The workflow.**

1. For each service, gather: a summary of the codebase (language, framework, age, size), known pain points from the team, dependency audit output (e.g., `npm audit` or `dotnet list package --outdated`), and SonarQube or similar code quality metrics.
2. Paste this information into your LLM using the Tech Debt Assessment prompt template.
3. The AI categorizes debt into buckets — dependency risk, code complexity hotspots, test coverage gaps, architectural drift, and documentation gaps — and assigns each item a priority based on risk and remediation effort.
4. You review the output with your team leads. They flag two items the AI missed (a hand-rolled caching layer that should be replaced with Redis, and a deprecated internal library dependency) and confirm the AI's top priorities are accurate.
5. Present the consolidated inventory to your VP with effort estimates and a recommended sequencing.

**Why this works.** The AI is good at systematic categorization and at applying standard prioritization frameworks (risk vs. effort matrices) consistently. It processes dependency lists and code metrics faster than a human can scan them. The team review step catches the context-dependent items that only people who work in the codebase every day would know.

### Example 4: Team Velocity Analysis and Sprint Health

**The scenario.** Your team's velocity has been declining for three sprints and you want to understand why before the next retrospective.

**The workflow.**

1. Export sprint data from Jira: story points committed vs. completed, carryover items, bug count per sprint, number of scope changes mid-sprint, and team member availability (if tracked).
2. Paste the data into your LLM and ask for a sprint health analysis.
3. The AI identifies three patterns: (a) committed points have increased each sprint while team size stayed flat, suggesting over-commitment; (b) carryover items are consistently the same type (integration stories involving the payment service); (c) mid-sprint scope additions have averaged 15% of committed points.
4. You bring these data points to the retrospective — not as accusations, but as patterns to discuss. The team confirms the payment service integration is blocked by an external team's availability, and that scope additions are coming from urgent production bugs in an aging module.
5. You use these insights to negotiate with the external team for dedicated integration time and to make a business case for remediating the aging module.

**Why this works.** AI turns raw sprint numbers into a narrative. It connects dots across sprints that are hard to see when you are reviewing one sprint at a time. The key is presenting the analysis as a conversation starter, never as a performance judgment.

---

## Prompt Templates

### 1. PR Pre-Review Checklist

```
You are a senior software engineer reviewing a pull request. Analyze the
following code diff for a [LANGUAGE/FRAMEWORK] application.

DIFF:
[PASTE THE CODE DIFF HERE]

PR DESCRIPTION:
[PASTE PR DESCRIPTION OR TICKET SUMMARY]

Review for:
1. **Bugs**: Logic errors, off-by-one errors, null reference risks, race conditions
2. **Security**: Injection vulnerabilities, hardcoded secrets, improper auth checks,
   sensitive data exposure
3. **Error handling**: Missing try/catch, swallowed exceptions, unclear error messages,
   missing logging on failure paths
4. **Design**: SOLID violations, unnecessary coupling, code duplication, mixing
   concerns across layers
5. **Naming**: Unclear variable/method names, inconsistent conventions
6. **Performance**: N+1 queries, unnecessary allocations, missing pagination,
   unindexed queries

For each finding:
- **Severity**: Critical (blocks merge) / Warning (should fix before merge) /
  Suggestion (improve when convenient)
- **Location**: File name and approximate location in the diff
- **Issue**: What is wrong and why it matters
- **Suggested fix**: Specific code change or approach (with code snippet if applicable)

End with an overall assessment: APPROVE / REQUEST CHANGES / NEEDS DISCUSSION.
If the code looks clean, say so — do not invent problems.
```

### 2. ADR Generator

```
Generate an Architecture Decision Record (ADR) for publishing to Confluence.

PROBLEM:
[DESCRIBE THE TECHNICAL PROBLEM OR DECISION TO BE MADE]

OPTIONS CONSIDERED:
[LIST THE OPTIONS — AIM FOR AT LEAST 3 — WITH BRIEF DESCRIPTIONS]

CONSTRAINTS:
- Team size and skills: [DESCRIBE]
- Timeline: [DEADLINE OR PHASE]
- Existing tech stack: [LIST KEY TECHNOLOGIES]
- Non-functional requirements: [PERFORMANCE, SCALE, COMPLIANCE NEEDS]
- Budget considerations: [ANY RELEVANT BUDGET INFO]

CHOSEN OPTION:
[WHICH OPTION WAS SELECTED AND THE PRIMARY REASONS]

Write the ADR in this format:

- **Title**: ADR-[NUMBER]: [DESCRIPTIVE TITLE]
- **Status**: [Proposed | Accepted | Deprecated | Superseded]
- **Date**: [DATE]
- **Context**: Why this decision is needed. Include the business driver, technical
  trigger, and any urgency factors. (2–3 paragraphs)
- **Decision Drivers**: Ranked list of the factors that most influenced the decision
  (e.g., team familiarity, operational cost, time to market, maintainability)
- **Options Considered**: For each option, provide a balanced summary with genuine
  pros and cons. Do not create straw-man alternatives.
- **Decision**: What we decided and why, explicitly linking back to the decision
  drivers. (2–3 paragraphs)
- **Consequences**:
  - Positive outcomes (bulleted list)
  - Negative outcomes and trade-offs (bulleted list)
  - Risks introduced by this decision
- **Compliance**: Note any security, privacy, or regulatory implications.
- **Review Date**: When this decision should be revisited.
```

### 3. Tech Debt Assessment

```
You are a senior engineering manager assessing technical debt for prioritization
and remediation planning.

Analyze the following information about a software service and produce a
structured tech debt inventory.

SERVICE OVERVIEW:
- Name: [SERVICE NAME]
- Language/Framework: [e.g., C# / .NET 8 / Azure App Service]
- Age: [HOW OLD IS THE CODEBASE]
- Team size: [NUMBER OF DEVELOPERS]
- Deployment frequency: [HOW OFTEN IT SHIPS]

CODE QUALITY METRICS:
[PASTE SONARQUBE METRICS, LINTING SUMMARY, OR SIMILAR]

DEPENDENCY AUDIT:
[PASTE OUTPUT OF DEPENDENCY CHECK — e.g., npm audit, dotnet list package --outdated]

KNOWN PAIN POINTS FROM THE TEAM:
[LIST ITEMS THE TEAM HAS FLAGGED]

Produce:
1. **Debt Inventory Table**:
   | ID | Category | Description | Risk (H/M/L) | Effort (H/M/L) | Priority |
   Categories: Dependency Risk, Code Complexity, Test Coverage, Architectural
   Drift, Documentation Gap, Security Exposure

2. **Top 5 Priority Items**: For each, explain why it is high priority, what
   happens if it is not addressed, and a recommended remediation approach with
   rough effort estimate (days or sprints).

3. **Quick Wins**: Items that are low effort but reduce meaningful risk — good
   candidates for embedding in feature sprints.

4. **Recommended Sequencing**: Suggest an order for tackling the top items,
   considering dependencies between debt items.

Be specific. Reference the actual metrics and dependency data provided.
Do not generate generic advice.
```

### 4. Team Capacity Planning

```
You are an experienced engineering manager analyzing sprint data to support
capacity planning and sprint health assessment.

SPRINT DATA (last 6 sprints):
[PASTE A TABLE WITH: Sprint name, Points committed, Points completed,
Carryover items, Bugs found in sprint, Scope changes mid-sprint,
Team members available]

UPCOMING SPRINT CONTEXT:
- Planned team availability: [WHO IS OUT, HOLIDAYS, ETC.]
- Key commitments or deadlines: [RELEASES, DEMOS, ETC.]
- Known risks: [DEPENDENCIES, BLOCKERS, ETC.]

Analyze the data and provide:

1. **Velocity Trend**: Is velocity stable, improving, or declining? Show the
   trend numerically (average, standard deviation, direction).

2. **Commitment Accuracy**: How well does the team estimate? What is the
   average ratio of completed to committed points?

3. **Carryover Analysis**: Are the same types of stories carrying over? What
   pattern explains the carryover?

4. **Sprint Health Indicators**: Flag any concerning patterns — increasing bug
   counts, rising scope changes, velocity variance.

5. **Capacity Recommendation**: Based on historical data and the upcoming sprint
   context, recommend a realistic commitment range (points) for the next sprint.
   Explain your reasoning.

6. **Discussion Points**: Suggest 2–3 questions the team should discuss in sprint
   planning based on the data.

Present findings as data points for team discussion, not performance judgments.
Velocity is a planning tool, not an evaluation metric.
```

---

## Pitfalls & Anti-Patterns

### 1. Treating AI code review as a replacement for human review

AI catches surface-level issues well — naming, null checks, common security anti-patterns — but it frequently misses context-dependent problems. It does not know your system's invariants, your team's conventions that are not in a linter, or the business rules behind a conditional. Use AI as a first pass that lets you focus your review time on higher-order concerns: architectural fit, business logic correctness, and long-term maintainability. The moment you skip human review because "the AI said it looked fine," you have a process gap.

### 2. Publishing AI-generated ADRs without team discussion

The value of an ADR is in the decision process, not just the document. If you feed a problem description into AI, publish the output to Confluence, and call it decided, you have skipped the most important part: the team's input. Engineers who were not consulted will not feel ownership of the decision, and the AI may have missed constraints that only someone working in the codebase would know. Draft with AI, decide with your team, then publish.

### 3. Using AI velocity analysis to evaluate individual performance

AI can identify that velocity dropped or that certain story types consistently carry over. It cannot tell you why, and it certainly should not be used to single out individuals. Team velocity is a planning tool for forecasting and capacity management. The moment you use AI-generated velocity reports in performance reviews, you incentivize point inflation and discourage engineers from taking on complex, risky work. Keep velocity analysis at the team level and use it for process improvement, not personnel decisions.

### 4. Accepting AI tech debt assessments without team validation

AI is good at spotting outdated dependencies and high-complexity code, but it lacks the context to know which debt actually hurts. It might flag a legacy module as high priority when, in reality, that module is stable, rarely changed, and scheduled for replacement next quarter. Always review AI debt assessments with the engineers who work in the code daily. They know which pain points slow down feature delivery and which are theoretical risks that rarely materialize.

### 5. Over-automating the human parts of leadership

AI can draft your sprint retrospective analysis, generate your 1:1 talking points, and summarize your team's PR activity. If you let it, it will turn your leadership role into a series of AI-generated reports. Resist this. The value you bring as a dev manager is in the conversations, not the documents. Use AI to prepare faster so you have more time for the human work — coaching, unblocking, building trust — not less.

### 6. Treating AI adoption as a mandate instead of a capability ramp

If you announce that “everyone should use AI now” without examples, shared assets, or support, people will either fake adoption or bounce off poor first experiences. A bad first week creates more resistance than no rollout at all.

**The fix:** Make the first workflows easy, concrete, and relevant to current work. Pair skeptics with practitioners. Celebrate real wins and openly discuss false positives, hallucinations, and mistakes.

### 7. Letting review stay slow while development gets faster

One of the first bottlenecks in AI-assisted teams is review latency. Developers start producing coherent features faster, but the review process still assumes older throughput. That mismatch creates hidden queues, frustration, and stacked risk.

**The fix:** Use AI to pre-screen PRs, require stronger summaries, and clarify what humans should focus on. Human review should move up the value chain, not disappear.

---

*[Back to Curriculum Overview](00-Curriculum-Overview.md)*
