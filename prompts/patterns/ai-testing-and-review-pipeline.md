# Pattern: AI-Powered Testing and Review Pipeline

## The Problem

AI-assisted development produces code 5-10x faster. But every line still needs:
- Testing (unit, integration, E2E, security, performance)
- Code review (correctness, security, architecture, compliance)
- QA validation (acceptance criteria, regression, edge cases)

If any of these stay at human speed, they become the bottleneck. The developer finishes in 3 hours and waits 2 days for review. The review passes and waits a week for QA. The speed gains evaporate.

**The answer is not "skip testing and review."** The answer is to apply the same AI force multiplication to the testing and review pipeline that we applied to development.

---

## 1. The AI Review Pipeline (Multi-Agent Code Review)

### The Concept

Instead of one human reviewer doing everything, deploy **multiple specialized AI review agents** that each focus on a different concern. They run automatically on every PR, and by the time a human reviewer opens the PR, the mechanical checks are already done.

### The Agents

| Agent | Focus | Runs On | Blocks Merge? |
|---|---|---|---|
| **Security Reviewer** | OWASP Top 10, HIPAA/PHI leaks, injection, auth gaps | Every PR | Yes (Critical/High findings) |
| **Architecture Reviewer** | Layer violations, dependency direction, pattern compliance | Every PR | No (advisory) |
| **Test Coverage Analyzer** | Missing tests, weak assertions, untested code paths | Every PR | Yes (if coverage drops below threshold) |
| **PHI Auditor** | PHI in logs, error messages, URLs, API responses | Every PR touching data/service layers | Yes (any finding) |
| **Performance Reviewer** | N+1 queries, missing async, large allocations, missing indexes | PRs touching data access | No (advisory) |
| **Convention Enforcer** | Naming, patterns, .cursorrules compliance | Every PR | No (advisory, auto-fixable) |
| **Documentation Checker** | Missing XML docs on public APIs, outdated README | Every PR | No (advisory) |

### Implementation: GitHub Actions + AI

Each agent runs as a GitHub Action step that calls an LLM API with the PR diff and a specialized system prompt.

```yaml
# .github/workflows/ai-review.yml
name: AI Review Pipeline

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  security-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Get PR diff
        run: git diff origin/${{ github.base_ref }}...HEAD > /tmp/pr-diff.txt

      - name: AI Security Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          python scripts/ai-review.py \
            --diff /tmp/pr-diff.txt \
            --agent security \
            --output /tmp/security-review.json

      - name: Post Review Comments
        uses: actions/github-script@v7
        with:
          script: |
            const review = require('/tmp/security-review.json');
            // Post findings as PR review comments
            // Block merge if Critical or High findings exist

  phi-audit:
    runs-on: ubuntu-latest
    steps:
      # Similar structure with PHI-specific system prompt

  architecture-review:
    runs-on: ubuntu-latest
    steps:
      # Similar structure with architecture-specific system prompt

  test-coverage:
    runs-on: ubuntu-latest
    steps:
      # Run tests with coverage, then AI analyzes gaps
```

### The Review Script

Each agent is the same script with a different system prompt:

```python
# scripts/ai-review.py
import anthropic
import json
import sys
import argparse

AGENTS = {
    "security": {
        "system": """You are a senior security engineer reviewing a pull request
for a healthcare application that processes prescription data.

Review the diff for OWASP Top 10 vulnerabilities and HIPAA compliance:
- SQL injection (string concatenation in queries)
- Missing authorization checks
- PHI in log statements, error messages, or API responses
- Missing input validation
- Hardcoded secrets or credentials
- Broken access control (users accessing others' data)
- Missing audit logging on data mutations

For each finding, respond with JSON:
{
  "findings": [
    {
      "severity": "critical|high|medium|low",
      "category": "owasp_category or hipaa",
      "file": "path/to/file",
      "line": 42,
      "description": "what's wrong",
      "recommendation": "how to fix it",
      "blocks_merge": true/false
    }
  ],
  "summary": "one paragraph overall assessment"
}

If no issues found, return empty findings array with a positive summary.
Be precise. Don't flag issues that aren't actually present in the diff.""",
    },

    "phi_audit": {
        "system": """You are a HIPAA compliance auditor reviewing code changes
in a prescription processing system.

PHI (Protected Health Information) includes: patient names, DOBs, SSNs,
addresses, phone numbers, medical record numbers, prescription data
(drug names, NDC codes, dosages), insurance information.

Scan the diff for:
1. PHI passed to any logging call (_logger.Log*, Console.Write*, etc.)
2. PHI included in exception messages or error responses
3. PHI in URL paths or query parameters
4. PHI returned in API responses beyond what's necessary
5. Missing audit logging on create/update/delete of PHI records
6. SELECT * on tables containing PHI

Every finding blocks merge. PHI leaks are not warnings — they are violations.

Respond with JSON in the same format as the security reviewer.""",
    },

    "architecture": {
        "system": """You are a software architect reviewing code changes in a
.NET Clean Architecture project.

Check for:
1. Layer violations (API directly accessing DbContext, Domain depending on Infrastructure)
2. Incorrect dependency direction
3. Business logic in controllers (should be in services/handlers)
4. Missing interface abstractions (concrete dependencies instead of interfaces)
5. God classes or methods (too many responsibilities)
6. Inconsistent patterns (doing the same thing differently in different places)

Be practical. Small violations in a focused change are advisory, not blocking.
Flag structural issues that will cause problems as the codebase grows.

Respond with JSON findings.""",
    },

    "test_coverage": {
        "system": """You are a test quality analyst reviewing code changes.

For each new or modified public method in the diff:
1. Is there a corresponding test?
2. Does the test verify behavior (not just that a method was called)?
3. Are error/edge cases tested?
4. For data mutations: is there a test verifying the database state after the operation?
5. For endpoints: is there a test for auth (401/403)?

Flag:
- New public methods with no tests → High
- Methods with tests that only verify mock interactions → Medium
- Missing edge case tests → Low

Respond with JSON findings.""",
    },

    "performance": {
        "system": """You are a performance engineer reviewing code changes in
a .NET application backed by MSSQL.

Check for:
1. N+1 query patterns (loading related data in loops)
2. Missing .AsNoTracking() on read-only queries
3. SELECT * or loading entire entities when only a few fields are needed
4. Synchronous database calls (missing async/await)
5. Missing pagination on collection endpoints
6. Large object allocations in hot paths
7. Missing caching on frequently-read, rarely-changed data
8. Missing database indexes for new query patterns

Respond with JSON findings.""",
    },
}

def review(diff_path, agent_name):
    client = anthropic.Anthropic()
    agent = AGENTS[agent_name]

    with open(diff_path) as f:
        diff = f.read()

    response = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=4096,
        system=agent["system"],
        messages=[{"role": "user", "content": f"Review this PR diff:\n\n```diff\n{diff}\n```"}]
    )

    return json.loads(response.content[0].text)

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--diff", required=True)
    parser.add_argument("--agent", required=True, choices=AGENTS.keys())
    parser.add_argument("--output", required=True)
    args = parser.parse_args()

    result = review(args.diff, args.agent)
    with open(args.output, "w") as f:
        json.dump(result, f, indent=2)
```

### What the Human Reviewer Sees

By the time a human opens the PR, they see:

```
✅ Security Review — No critical or high findings
⚠️ Architecture Review — 1 advisory: "Service method GetAll() should accept pagination parameters"
❌ PHI Audit — 1 blocking finding: "_logger.LogInformation includes patient name on line 47"
✅ Test Coverage — All new methods have tests, no coverage regression
⚠️ Performance — 1 advisory: "N+1 pattern detected in GetPrescriptionsWithRefills()"
```

The human reviewer now focuses on:
- **Business logic correctness** (does the code do the right thing?)
- **Architectural decisions** (is this the right approach?)
- **Resolving the PHI finding** (agree it's real or dismiss as false positive)
- **The performance advisory** (worth fixing now or later?)

They skip: security scanning, convention checking, test coverage analysis, PHI scanning. The AI did all of that.

**Result: Human review time drops from 1-2 hours to 15-30 minutes.**

---

## 2. The Automated Testing Pyramid

### The New Testing Pyramid for AI-Accelerated Teams

```
                    /\
                   /  \
                  / E2E \          ← AI generates from acceptance criteria
                 /--------\
                / Contract  \      ← AI generates from OpenAPI specs
               /-------------\
              / Integration    \   ← AI generates from service interfaces
             /------------------\
            /    Unit Tests       \ ← AI generates during development
           /------------------------\
          /   Static Analysis (AI)    \ ← Runs on every commit
         /------------------------------\
```

### Layer 1: Static Analysis (Every Commit)

Runs in < 60 seconds. Catches issues before tests even run.

```yaml
# Pre-commit or CI
- dotnet build --warnaserrors        # Compiler warnings as errors
- dotnet format --verify-no-changes  # Code style enforcement
- AI security scan (fast mode)       # Quick OWASP check on changed files only
- AI PHI scan (fast mode)            # Quick log/error message check on changed files
```

### Layer 2: Unit Tests (Every PR)

Generated by AI during development. The prompt from `prompts/by-task/test-generation.md` already covers this. The key change is making test generation **mandatory, not optional**:

**Definition of Done includes:**
- [ ] Every new public method has unit tests
- [ ] Tests cover happy path + at least 2 error cases
- [ ] Tests verify behavior, not implementation
- [ ] AI test coverage analyzer confirms no gaps

### Layer 3: Integration Tests (Every PR)

Test the full HTTP pipeline: request → controller → service → database → response.

**AI-Generated Integration Test Prompt:**

```
Read the new/modified controller endpoints in this PR.

For each endpoint, generate an integration test using WebApplicationFactory:

1. Seed the database with required test data
2. Send an HTTP request with valid auth
3. Assert the response status code, body shape, and key field values
4. Assert the database state changed correctly (for mutations)
5. Test without auth → 401
6. Test with wrong role → 403
7. Test with invalid input → 400 with ProblemDetails
8. Test with non-existent resource → 404

Use in-memory MSSQL (or the existing test database pattern in this project).
Test data must use obviously fake data — no real PHI.
```

### Layer 4: Contract Tests (Weekly or on API Changes)

Verify that API contracts haven't broken. AI generates these from your OpenAPI spec:

```
Read the OpenAPI/Swagger spec at /swagger/v1/swagger.json.

For each endpoint, generate a contract test that verifies:
1. The endpoint exists and is reachable
2. The response schema matches the spec (correct fields, types, nullability)
3. Required fields are always present
4. Enum values are within the documented set

Flag any endpoint in the code that isn't in the spec (undocumented endpoint)
and any endpoint in the spec that doesn't exist in the code (dead documentation).
```

### Layer 5: E2E / Acceptance Tests (On Merge to Main)

These test complete user workflows. AI generates them from Jira acceptance criteria:

```
Read Jira ticket [PROJ-1234] acceptance criteria via MCP.

Generate end-to-end tests that verify each acceptance criterion:

For each criterion:
1. Set up the precondition (seed data, user state)
2. Execute the user workflow (API calls in sequence)
3. Assert the final state matches the acceptance criterion
4. Clean up test data

These tests should run against a deployed test environment,
not in-memory. Use real HTTP calls. Use a test user account.
```

---

## 3. AI Test Generation in CI (Continuous Test Improvement)

### The Concept: Tests That Write Themselves

Don't just run tests in CI — have AI **generate new tests** in CI when it detects gaps:

```yaml
# .github/workflows/test-improvement.yml
name: AI Test Improvement

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  analyze-and-generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run tests with coverage
        run: |
          dotnet test --collect:"XPlat Code Coverage" \
            --results-directory ./coverage

      - name: AI Coverage Analysis
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          python scripts/ai-test-gap-analyzer.py \
            --coverage ./coverage \
            --diff <(git diff origin/main...HEAD) \
            --output ./suggested-tests.md

      - name: Post Suggested Tests
        if: steps.analyze.outputs.has_gaps == 'true'
        uses: actions/github-script@v7
        with:
          script: |
            // Post suggested test code as a PR comment
            // Developer can copy-paste to add the tests
```

The AI reads the coverage report, identifies uncovered code paths in the PR, and **writes the actual test code** as a PR comment. The developer reviews and commits the suggested tests — or asks the AI to refine them.

### Mutation Testing with AI

Traditional mutation testing is slow (modifies code, reruns all tests). AI can do it faster by analyzing tests conceptually:

```
@Services/PrescriptionRefillService.cs
@Tests/PrescriptionRefillServiceTests.cs

Perform a conceptual mutation analysis:

For each conditional branch in the service:
1. What would happen if this condition were inverted?
2. Would any existing test catch the mutation?
3. If no test catches it, write one.

For each return statement:
1. What would happen if a different value were returned?
2. Would any existing test catch it?

Report: which mutations would survive (no test catches them)?
Generate tests that kill those mutants.
```

---

## 4. Continuous Security Testing

### SAST (Static Application Security Testing) with AI

Traditional SAST tools (SonarQube, Snyk) find pattern-based vulnerabilities. AI finds **contextual** vulnerabilities — things that are only dangerous in your specific business context.

Run on every PR:

```
You are a SAST scanner specialized in healthcare applications.

Beyond standard OWASP patterns, check for healthcare-specific issues:
1. PHI being passed to external services without a BAA
2. Prescription data validation gaps (invalid NDC codes, impossible dosages)
3. Audit trail gaps (mutations without corresponding audit log entries)
4. Role-based access issues specific to pharmacy workflows
   (pharmacist vs. technician vs. patient permissions)
5. Data retention violations (PHI kept beyond the required retention period)
6. De-identification failures (data that should be de-identified but isn't)

These are domain-specific security issues that generic SAST tools miss.
```

### DAST (Dynamic Application Security Testing) with AI

Run against a deployed staging environment:

```
You have access to the API at https://staging.example.com/api.
Here is the OpenAPI spec: @swagger.json

Perform a security test against each endpoint:
1. Try common injection payloads on all string parameters
2. Test IDOR (Insecure Direct Object Reference): access patient A's data with patient B's token
3. Test privilege escalation: call admin endpoints with a regular user token
4. Test rate limiting: send 100 rapid requests to auth endpoints
5. Check response headers: HSTS, X-Content-Type-Options, X-Frame-Options
6. Check for information leakage in error responses

Report all findings with reproduction steps.
```

---

## 5. The Full Pipeline: From Commit to Production

```
Developer commits code
    │
    ├── Pre-commit hooks (< 10 seconds)
    │   ├── dotnet format
    │   ├── AI quick PHI scan (changed files only)
    │   └── dotnet build
    │
    ├── PR opened → AI Review Pipeline (< 5 minutes, parallel)
    │   ├── Security Reviewer agent
    │   ├── PHI Auditor agent
    │   ├── Architecture Reviewer agent
    │   ├── Test Coverage Analyzer agent
    │   ├── Performance Reviewer agent
    │   └── Convention Enforcer agent
    │
    ├── CI Test Suite (< 10 minutes)
    │   ├── Unit tests
    │   ├── Integration tests
    │   ├── AI test gap analysis → suggested tests posted as PR comment
    │   └── Code coverage report
    │
    ├── Human Review (15-30 minutes, focused on business logic)
    │   ├── AI findings already resolved or acknowledged
    │   ├── Reviewer focuses on: correctness, architecture, business rules
    │   └── Approve → merge
    │
    ├── Merge to main → Extended test suite (< 30 minutes)
    │   ├── Contract tests
    │   ├── E2E acceptance tests
    │   ├── AI security regression scan (full codebase, not just diff)
    │   └── Performance regression tests
    │
    └── Deploy to staging → DAST (< 1 hour)
        ├── AI dynamic security testing
        ├── Smoke tests
        └── Manual QA for UX-critical flows only
```

**Total time from commit to staging: ~2 hours** (mostly automated, human time is 15-30 minutes for review)

**Compare to traditional: 3-5 days** (2 days waiting for review, 1 day of manual testing, 1 day for QA)

---

## 6. Scaling with Local LLM Infrastructure

### Why This Matters

Running AI review agents against a cloud API (Anthropic, OpenAI) works but:
- Costs money per PR (could be $0.50-$2.00 per review run across all agents)
- Sends your code to a third-party API
- Rate limits can slow CI during high-commit periods
- HIPAA concerns about sending healthcare code to cloud APIs

### Using Seshat/Ptah for CI Review

Your local inference infrastructure (Seshat + Ptah) can run the review agents:

```yaml
# In CI, point the review script at your local infrastructure
env:
  LLM_ENDPOINT: http://192.168.5.35:8080/v1  # Ptah running Qwen 72B
  LLM_MODEL: Qwen/Qwen2.5-Coder-72B-Instruct
```

This keeps code on-premises, costs nothing per run, and has no rate limits.

**Model selection for CI agents:**
- Security + PHI review: Use the best available (72B on Ptah) — accuracy matters most
- Architecture + convention: 32B is sufficient — these are pattern-matching tasks
- Test generation: 32B coding model (Qwen Coder) — optimized for code output
- Performance analysis: 32B is sufficient

### Dedicated CI Slots

Reserve one slot on Ptah for CI tasks. Configure the gateway:

```
POST http://192.168.5.35:9090/models/load
{
  "model_id": "qwen-coder-72b-fp8",
  "slot": "slot3",
  "lease": {
    "holder": "ci-pipeline",
    "duration_minutes": 60,
    "preemptible": true
  }
}
```

The CI slot is preemptible — JARVIS can evict it for critical interactive work and CI will retry.

---

## 7. Practical First Steps

### Week 1: Add AI Security Review to CI

Start with the highest-value, lowest-risk agent. Security review on every PR:

1. Create `scripts/ai-review.py` with the security agent
2. Add the GitHub Action workflow
3. Configure it to post findings as PR comments (not block merges yet)
4. Run for 2 weeks. Calibrate: are the findings accurate? Too many false positives?
5. Once calibrated, enable merge blocking on Critical/High findings

### Week 2: Add PHI Auditor

This is non-negotiable for healthcare. PHI leaks are compliance violations.

1. Add the PHI audit agent to the same workflow
2. **Block merges immediately** — any PHI finding must be resolved
3. This alone will prevent more HIPAA incidents than a year of manual code review

### Week 3: Add Test Coverage Analyzer

1. Add coverage collection to the test step
2. Add the AI coverage analyzer
3. Post suggested tests as PR comments (advisory, not blocking)
4. Track: are developers using the suggested tests?

### Week 4: Add Architecture + Performance Reviewers

1. Add as advisory (non-blocking) agents
2. These improve code quality over time without slowing the pipeline

### Month 2: Add AI Test Generation

1. Implement the test gap analyzer
2. Generate integration tests from controller endpoints
3. Start generating E2E tests from Jira acceptance criteria

### Month 3: Local LLM for CI

1. Configure Ptah with a dedicated CI slot
2. Migrate CI agents from cloud API to local inference
3. Run DAST against staging with AI

---

## 8. Measuring Impact

| Metric | Before | Target |
|---|---|---|
| Human review time per PR | 1-2 hours | 15-30 minutes |
| Time from PR open to merge | 2-3 days | Same day (< 8 hours) |
| Security issues found in production | X per quarter | Near zero |
| PHI leaks caught before merge | Unknown | 100% |
| Test coverage on new code | Variable | > 80% |
| Time from merge to staging | 1 day | 2 hours |
| Manual QA effort per feature | 2-4 hours | 30 minutes (UX-critical flows only) |

The goal is not zero human involvement — it's **human attention focused on things only humans can evaluate** (business logic, architectural judgment, user experience) while AI handles the mechanical checks at machine speed.
