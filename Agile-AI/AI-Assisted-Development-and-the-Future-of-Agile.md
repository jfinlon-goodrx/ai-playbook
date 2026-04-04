# AI-Assisted Development and the Future of Agile

An analysis of how AI coding assistants fundamentally change the economics of software development, why traditional SCRUM ceremonies and story decomposition patterns create friction in this new paradigm, and practical approaches to preserving engineering discipline without sacrificing the 5-10x throughput gains that skilled prompt engineers can achieve.

Related hub: [[README]]

---

## 1. The Paradigm Shift

### What Changed

Traditional software development operates under a set of assumptions that SCRUM was designed to optimize for:

1. **Context switching is expensive.** A developer working on the API layer doesn't have the UI code loaded in their head. Handoffs between layers cost time.
2. **Integration is risky.** Changes in one layer can break another. Small PRs reduce blast radius.
3. **Individual velocity is bounded.** A senior developer can produce maybe 200-400 lines of meaningful, tested code per day.
4. **Review is the quality gate.** Code review catches bugs, enforces standards, and distributes knowledge.
5. **Estimation is hard.** Breaking stories small makes estimation more predictable.

AI-assisted development invalidates assumptions 1, 3, and partially 4:

| Assumption | Traditional | AI-Assisted |
|---|---|---|
| Context switching cost | High — developer must rebuild mental model | Near-zero — the LLM holds the full context of both repos simultaneously |
| Integration risk | High — layers developed in isolation | Low — developer iterates across all layers in real-time with live containers |
| Individual velocity | 200-400 LOC/day | 2,000-4,000+ LOC/day for a skilled prompt engineer with domain expertise |
| Review as quality gate | Essential — catches bugs humans miss | Still valuable, but the LLM already caught many classes of bugs during development |
| Estimation accuracy | Improves with smaller stories | Story size matters less when execution is 4-10x faster |

### The New Developer Profile

The force multiplier isn't the AI alone. It's the combination of:

- **Domain expertise** — understanding what to build and why
- **Engineering judgment** — knowing good architecture, patterns, and tradeoffs
- **Prompt engineering skill** — knowing how to direct the LLM effectively, when to intervene, when to let it run
- **System thinking** — seeing the full stack as one unit, not isolated layers

A developer with these four skills and a capable coding assistant (Cursor, Claude Code, Copilot Workspace) can do in an afternoon what a traditional team might spread across a sprint. Not because the AI writes more code — but because the human-AI pair eliminates the dead time between decisions.

---

## 2. Where SCRUM Creates Friction

### 2.1 Story Decomposition Overhead

Traditional SCRUM decomposes a feature like "Add user profile page" into:

```
Story 1: Create user_profiles database table (Data)
Story 2: Add GET /api/users/{id}/profile endpoint (API)
Story 3: Add PUT /api/users/{id}/profile endpoint (API)
Story 4: Create ProfilePage component (UI)
Story 5: Wire ProfilePage to API (UI)
Story 6: Add profile link to navigation (UI)
```

Each story goes through: grooming, estimation, assignment, development, PR, review, merge, QA, acceptance. That's 6 stories x ~8 process steps = 48 process events for what is, architecturally, one feature.

**With AI-assisted development**, a skilled developer can:
1. Spin up the full stack via `docker compose`
2. Tell the coding assistant: "Add a user profile feature — database migration, API endpoints, and React page"
3. Iterate on the output across all three layers simultaneously
4. Fix integration issues in real-time because the assistant sees the logs from all containers
5. Produce a working, tested feature in 2-4 hours

The feature is the same. The quality is the same (or better, because integration was tested continuously). But the process overhead is reduced from 48 events to: 1 story, 1-2 PRs, 1 review, 1 merge.

### 2.2 The PR Bottleneck

Small PRs are a best practice when:
- Reviewers have limited time and attention
- Changes are hard to understand in isolation
- Integration risk scales with PR size

AI-assisted development changes the calculus:
- The developer iterated across layers in real-time, so integration is already validated
- The coding assistant can generate a PR summary that explains every change and its rationale
- The diff is larger but more coherent — it represents a complete feature, not a fragment

**The bottleneck shifts from "writing code" to "reviewing and merging code."** If the review process can't keep up with development throughput, the gains evaporate. Developers sit idle waiting for reviews, or stack PRs that create merge conflicts.

### 2.3 Sprint Planning Mismatch

When a developer can complete in 2 days what was estimated as a 2-week effort, sprint planning becomes disconnected from reality:

- **Overestimation** — stories are sized for traditional development but completed in a fraction of the time
- **Underloaded sprints** — developers finish their committed work by Wednesday and pull from the backlog, but the backlog wasn't groomed for that velocity
- **Ceremony overhead** — daily standups, sprint reviews, and retrospectives consume a fixed time budget that becomes proportionally larger relative to the shrinking development time

### 2.4 The Knowledge Distribution Problem

One legitimate purpose of small stories and frequent PRs is **knowledge distribution**. When multiple developers work on small pieces, everyone understands the system. When one developer + AI builds an entire feature, knowledge concentrates.

This is a real risk. The AI doesn't attend standups. It doesn't write documentation unless asked. And the developer who built the feature at 10x speed may move on before anyone else understands the code.

---

## 3. What SCRUM Gets Right (Don't Throw Away)

Before proposing changes, it's worth identifying what SCRUM provides that AI-assisted development does NOT replace:

### 3.1 Alignment

Sprint planning, grooming, and reviews ensure the team is building the right thing. AI makes you faster at building — it doesn't tell you what to build. Product alignment ceremonies remain essential.

### 3.2 Quality Gates

Code review catches:
- Architectural violations the AI doesn't know about (team conventions, security policies)
- Business logic errors the AI misunderstood from the prompt
- Dependencies and side effects the developer didn't consider

Review remains valuable. The question is how to do it efficiently at higher throughput.

### 3.3 Visibility

Stakeholders need to see progress. Sprint reviews, burndown charts, and demos provide this. The cadence may change, but the need doesn't.

### 3.4 Retrospectives

Continuous improvement is even more important when the development process itself is evolving rapidly. How the team uses AI, what prompts work, what patterns fail — this is critical institutional knowledge.

---

## 4. Proposed: Adaptive Agile for AI-Assisted Teams

### 4.1 Feature-Sized Stories, Not Layer-Sized

**Change:** Stop decomposing features into layer-specific stories. A "user profile" feature is one story, regardless of how many layers it touches.

**Story definition:**
- **Vertical slice** — includes data, API, and UI changes needed for the feature
- **Acceptance criteria** — defined at the feature level, not the layer level
- **Size** — measured in complexity, not effort (since effort is now 4-10x lower)

**Estimation shift:**
- Replace story points with **complexity categories**: Trivial (< 1 hour), Small (half day), Medium (1-2 days), Large (3-5 days), Epic (needs decomposition)
- A "Large" story under AI-assisted development roughly equals what was a 2-week effort traditionally

### 4.2 Trunk-Based Development with Feature Flags

**Change:** Replace the branch-per-story, PR-per-layer model with trunk-based development.

**How it works:**
1. Developer works on a feature branch (one branch per feature, not per layer)
2. Commits frequently (the AI generates clean, atomic commits)
3. Opens **one PR per feature** that spans all affected layers
4. Merges to trunk behind a feature flag if the feature isn't ready for production
5. Feature flag is toggled on after QA/acceptance

**Benefits:**
- One review per feature instead of 3-6
- No merge conflicts from stacked layer PRs
- Feature is always integration-tested because it was developed as a unit
- Rollback is clean — toggle the flag off

### 4.3 Async Review with AI-Assisted Summaries

**Change:** Replace synchronous PR review bottlenecks with async, AI-augmented review.

**Process:**
1. Developer opens PR with an AI-generated summary (what changed, why, architectural decisions, test coverage)
2. The coding assistant runs a pre-review pass: linting, test execution, security scan, dependency audit
3. The PR is tagged with a complexity rating (auto-detected from diff size and blast radius)
4. **Trivial/Small PRs** — one reviewer, 24-hour SLA
5. **Medium/Large PRs** — two reviewers, one of whom is an architect/lead, 48-hour SLA
6. Reviewers focus on **architectural decisions, business logic, and security** — not formatting, naming, or obvious bugs (the AI already handled those)

### 4.4 Continuous Grooming, Not Sprint Grooming

**Change:** Replace the formal grooming ceremony with a continuously maintained, prioritized backlog.

**How it works:**
- Product owner maintains a ranked backlog at all times
- Stories are written as feature-level descriptions with clear acceptance criteria
- When a developer finishes a feature, they pull the next item from the top of the backlog
- No waiting for sprint boundaries to start new work

This is closer to **Kanban** than SCRUM for the development phase, while retaining SCRUM's planning and review ceremonies for alignment.

### 4.5 Shorter Cycles, Lighter Ceremonies

**Change:** Reduce sprint length and ceremony overhead proportionally to the increase in velocity.

| Ceremony | Traditional (2-week sprint) | Adapted |
|---|---|---|
| Sprint planning | 2-4 hours | 1 hour (features are already groomed) |
| Daily standup | 15 min/day | 15 min, 3x/week (Mon/Wed/Fri), async updates on other days |
| Sprint review/demo | 1-2 hours | 30 min demo at end of each week |
| Retrospective | 1 hour/sprint | 30 min every 2 weeks |
| Sprint length | 2 weeks | 1 week (or continuous flow with weekly checkpoints) |

**Key insight:** The ceremonies exist to create alignment and feedback loops. When development is 4-10x faster, the feedback loops need to be tighter, not looser. But each loop should be lighter.

### 4.6 Pair Review Sessions (Knowledge Distribution)

**Change:** Replace the knowledge distribution function of small PRs with intentional pair review sessions.

**Process:**
1. After completing a feature, the developer does a **30-minute walkthrough** with one or two teammates
2. The walkthrough covers: architecture decisions, key code paths, integration points, and "where to look if this breaks"
3. The session is recorded (screen + voice) and linked to the PR
4. This replaces the slow knowledge distribution that came from multiple developers touching multiple small stories

**Why this works better:** A 30-minute walkthrough transfers more knowledge than 6 small PR reviews because it includes the developer's intent and reasoning, not just the diff.

---

## 5. The Anti-Patterns to Watch For

### 5.1 "The AI Wrote It, Ship It"

AI-generated code is not automatically correct. It's fluent, compiles, and often passes tests — but it can:
- Introduce subtle security vulnerabilities
- Misunderstand business requirements
- Create technical debt through over-engineering or incorrect abstractions
- Hallucinate API signatures or library features

**Mitigation:** Review remains essential. The AI is a junior developer with perfect syntax and no judgment. The human provides the judgment.

### 5.2 Knowledge Silos

One developer + AI builds entire features. If that developer leaves, the knowledge leaves.

**Mitigation:** Pair review sessions, recorded walkthroughs, and rotating feature ownership. The AI can also generate documentation on demand — make it part of the definition of done.

### 5.3 Process Erosion

"We're so fast now, we don't need reviews/tests/documentation."

**Mitigation:** Automate the gates. CI/CD pipeline enforces: tests pass, linter clean, security scan green, minimum review count met. The human can skip ceremonies; the pipeline cannot be skipped.

### 5.4 Prompt Engineering as a Single Point of Failure

If only one person on the team knows how to effectively use the AI tools, velocity depends on that person.

**Mitigation:** Treat prompt engineering as a team skill. Share prompts, patterns, and `.cursorrules` files. Retrospectives should include "what prompts worked" and "what prompts failed."

### 5.5 Metric Gaming

If you measure velocity in story points and stories are now 4-10x faster, velocity numbers become meaningless (or misleading to stakeholders).

**Mitigation:** Measure outcomes, not output. Features shipped, bugs per feature, time-to-production, customer impact. Stop measuring story points entirely if the team is using AI-assisted development.

---

## 6. A Practical Transition Path

For a team currently running traditional SCRUM and adopting AI-assisted development:

### Phase 1: Keep SCRUM, Change Story Sizing (Weeks 1-4)

- Start writing vertical-slice stories instead of layer-specific stories
- Allow developers to work across layers in a single branch
- Keep all existing ceremonies and review processes
- Measure: how much faster are features completing?

### Phase 2: Streamline Reviews (Weeks 5-8)

- Require AI-generated PR summaries
- Implement tiered review SLAs based on PR complexity
- Introduce pair review walkthroughs for large features
- Reduce sprint length to 1 week
- Measure: is review latency the bottleneck?

### Phase 3: Move Toward Continuous Flow (Weeks 9-12)

- Replace sprint planning with continuous backlog pull
- Keep weekly demo/checkpoint meetings
- Implement feature flags for trunk-based development
- Reduce standups to 3x/week with async updates
- Measure: is the team shipping more features with fewer defects?

### Phase 4: Optimize (Ongoing)

- Retrospect on what ceremonies add value and which are overhead
- Share prompt engineering patterns across the team
- Build team-specific `.cursorrules` and coding assistant configurations
- Consider whether SCRUM is still the right framework, or if Kanban with weekly checkpoints is a better fit

---

## 7. The Bottom Line

SCRUM was designed for a world where:
- Writing code was the bottleneck
- Integration was risky and expensive
- Small batches reduced risk

AI-assisted development creates a world where:
- **Deciding what to build** is the bottleneck
- Integration is cheap because the AI works across layers simultaneously
- **Review and merge** are the new bottlenecks
- Small batches add process overhead with diminishing risk reduction

The answer is not to abandon Agile. It's to return to the original Agile Manifesto:

> **Individuals and interactions** over processes and tools
> **Working software** over comprehensive documentation
> **Customer collaboration** over contract negotiation
> **Responding to change** over following a plan

AI-assisted development is the most powerful argument for the left side of those statements since the manifesto was written. The teams that thrive will be the ones that use AI to amplify human judgment — and restructure their processes to keep up with the speed that combination produces.

The teams that fail will be the ones that layer AI-assisted development on top of heavyweight SCRUM without adapting — producing code at 10x speed while their process runs at 1x.

---

## Summary: What to Keep, What to Change, What to Drop

| Practice | Keep | Change | Drop |
|---|---|---|---|
| Product alignment (planning, grooming) | X | Lighter, continuous | |
| Vertical-slice stories | | X (replace layer stories) | |
| Story point estimation | | | X (use complexity categories) |
| Sprint boundaries | | X (1-week or continuous) | |
| Daily standups | | X (3x/week + async) | |
| Code review | X | AI-augmented, tiered SLAs | |
| Small PRs per layer | | | X (1 PR per feature) |
| Sprint review/demo | X | Weekly, 30 min | |
| Retrospective | X | Include prompt engineering | |
| CI/CD quality gates | X | Add AI pre-review | |
| Branch-per-story | | X (branch-per-feature) | |
| Knowledge distribution via small stories | | X (pair walkthroughs) | |
| Feature flags | | X (adopt) | |
