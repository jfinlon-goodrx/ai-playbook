# AI Adoption Playbook: From Kanban to AI-Driven Development

A phased approach to transforming a mixed-velocity team from traditional Kanban into an AI-augmented development organization. Addresses the reality that adoption is uneven, resistance is real, and the process must accommodate both AI-accelerated developers and those still working at traditional speed — while systematically closing that gap.

Companion to: [[AI-Assisted Development and the Future of Agile]]

Related hub: [[README]]

---

## 1. Honest Assessment: Where We Are

### The Two-Speed Problem

The team currently operates at two velocities:

| Profile | Characteristics | Estimated % of Team |
|---|---|---|
| **AI-Accelerated** | Uses Cursor/Claude Code/Copilot daily. Writes prompts as naturally as code. Delivers features across layers in hours. Thinks in systems, not stories. | ~15-20% |
| **AI-Curious** | Has tried Copilot or ChatGPT. Uses it for isolated tasks (regex, boilerplate). Hasn't integrated it into their core workflow. Open to learning. | ~40-50% |
| **Traditional** | Writes code manually. Views AI tools as unreliable or threatening. May have tried and been burned by hallucinations. Skeptical. | ~30-40% |

This isn't a criticism of the traditional developers. Many are excellent engineers. The gap is in **tooling adoption**, not in talent. But the gap is real, and it's growing — every month the AI-accelerated developers pull further ahead in throughput while the traditional developers feel increasing pressure without understanding how to close the distance.

### The Real Bottleneck

The bottleneck is not the Kanban board, the sprint cadence, or the story size. The bottleneck is **uneven adoption creating a bifurcated team** where:

- Fast developers are starved for reviewed/merged work because reviewers can't keep up
- Slow developers feel outpaced and disengaged
- Process is designed for the median velocity, which satisfies neither group
- Knowledge of AI-effective workflows is concentrated in a few people
- There's no structured path from "AI-curious" to "AI-accelerated"

### What Resistance Actually Looks Like

Resistance to AI adoption rarely presents as "I refuse to use AI." It presents as:

- **"I tried it and the code was wrong."** — The developer prompted poorly, got hallucinated output, and concluded the tool is unreliable. They need to see effective prompting, not be told to "just use it."
- **"It doesn't work for our codebase."** — The developer tried a generic prompt without project context (no `.cursorrules`, no architecture docs). They need to see project-specific prompting.
- **"I'm faster without it."** — For small, well-understood tasks, this is often true. The developer hasn't experienced the force multiplication on complex, cross-cutting work.
- **"It's going to replace us."** — Fear. The only cure is demonstrating that AI makes skilled developers more valuable, not less.
- **"I don't have time to learn a new tool."** — The highest-leverage objection. The answer is to make learning happen *inside* the work, not as a separate activity.

---

## 2. Foundation: The Shared Prompt Repository

### Why This Is the Single Highest-Impact First Step

A shared prompt repository does three things simultaneously:

1. **Demonstrates** — skeptical developers can see real prompts that produce real results in their own codebase
2. **Lowers the barrier** — developers don't have to invent prompts from scratch; they copy, modify, and learn
3. **Creates a feedback loop** — prompts improve over time as more people use and refine them

### Repository Structure

```
ai-playbook/
├── README.md                           # What this is, how to use it, how to contribute
├── getting-started/
│   ├── 01-installing-cursor.md         # Step-by-step setup for each tool
│   ├── 02-installing-claude-code.md
│   ├── 03-your-first-prompt.md         # "Hello world" of AI-assisted coding
│   ├── 04-project-context.md           # .cursorrules, CLAUDE.md, architecture docs
│   └── 05-when-to-use-ai.md           # Decision framework: when AI helps vs. hurts
│
├── prompts/
│   ├── by-task/
│   │   ├── new-api-endpoint.md         # Prompt template + example output
│   │   ├── new-react-component.md
│   │   ├── database-migration.md
│   │   ├── bug-investigation.md
│   │   ├── code-review-assist.md
│   │   ├── test-generation.md
│   │   ├── refactoring.md
│   │   └── pr-description.md
│   │
│   ├── by-project/
│   │   ├── dotnetagents/               # Project-specific prompts
│   │   ├── education-agent/
│   │   ├── jarvis/
│   │   └── sdlc-agent/
│   │
│   └── patterns/
│       ├── vertical-slice.md           # "Build the full feature across layers"
│       ├── debug-with-logs.md          # "Here are the container logs, find the bug"
│       ├── handoff-prompt.md           # How to hand context to another session
│       └── iterative-refinement.md     # "This is close but fix X, Y, Z"
│
├── cursorrules/
│   ├── dotnet-api.cursorrules          # Shared .cursorrules files
│   ├── react-frontend.cursorrules
│   ├── python-ml.cursorrules
│   └── full-stack.cursorrules
│
├── case-studies/
│   ├── 2026-03-user-profile-feature.md # "I built this feature in 3 hours. Here's how."
│   ├── 2026-03-auth-migration.md
│   └── template.md                     # Template for writing case studies
│
└── anti-patterns/
    ├── common-mistakes.md              # What goes wrong and how to fix it
    ├── when-not-to-use-ai.md           # Security-sensitive code, novel algorithms
    └── hallucination-recovery.md       # How to detect and recover from bad output
```

### Prompt Template Format

Each prompt file follows a standard format so anyone can pick it up:

```markdown
# Task: [Name]

## When to Use
[One-sentence description of the scenario]

## Prerequisites
- [ ] What context the AI needs (files, architecture docs, running services)

## The Prompt
```
[The actual prompt, ready to copy-paste]
```

## Example Output
[What the AI produced when this prompt was used on our codebase]

## Variations
- [Variation for different frameworks/projects]

## Tips
- [What to watch for, common failure modes]

## Contributed By
[Name, date, which project this was used on]
```

### Contribution Model

- **Anyone can add a prompt** via PR — low barrier
- **Weekly "prompt of the week"** highlighted in team chat — creates visibility
- **Case studies are the gold standard** — "I used this prompt to build X in Y hours" is the most persuasive artifact for skeptics

---

## 3. The AI Learning Path

### Why Classroom Training Doesn't Work

Sending developers to a 2-hour "intro to AI coding" workshop produces the same result as sending someone to a 2-hour "intro to vim" workshop: they'll forget everything by the time they sit down at their desk.

AI coding proficiency is a **practiced skill**, not a learned fact. It has to be built through repeated application on real work.

### The Apprenticeship Model

Instead of classroom training, pair AI-accelerated developers with AI-curious developers on real stories:

| Week | Activity | Goal |
|---|---|---|
| 1 | **Shadow session** — AI-curious developer watches AI-accelerated developer work on a real feature for 2 hours. No hands on keyboard. Just observe. | See the workflow in action. Understand the rhythm of prompt → output → evaluate → refine. |
| 2 | **Guided session** — AI-curious developer drives, AI-accelerated developer coaches. Work on a real story from the backlog. | Experience the workflow with a safety net. |
| 3 | **Solo with review** — AI-curious developer works alone on a story using AI tools. AI-accelerated developer reviews the prompts (not just the code) afterward. | Build independence. Get feedback on prompting, not just output. |
| 4+ | **Independent** — Developer is on their own but can ask questions in a shared channel. | Full adoption. Contribute prompts back to the shared repo. |

**Time cost:** ~6 hours of pairing per developer over 3 weeks. That's a trivial investment for a permanent 3-5x velocity increase.

### The "Lunch and Learn" Rotation

Every two weeks, one developer does a 30-minute demo of a feature they built with AI assistance:

- Screen recording of the actual session (edited for length)
- Show the prompts, the mistakes, the iterations
- Emphasize the failures as much as the successes — "here's where the AI hallucinated and here's how I caught it"
- Q&A

**Why this works for skeptics:** They're not being told AI is great. They're watching a peer — someone who works on the same codebase, with the same constraints — build something faster. The proof is in the demo.

---

## 4. Process Modifications: Kanban + AI Urgency

### 4.1 Dual-Track WIP Limits

Current Kanban likely has WIP limits per column (e.g., max 3 items in "In Progress"). Modify to account for velocity differences:

**Traditional model:**
```
Backlog → In Progress (WIP: 3) → Review (WIP: 4) → Done
```

**AI-adapted model:**
```
Backlog → In Progress (WIP: 5*) → Review (WIP: 6*) → Done
                                     ↑
                              * Higher WIP because AI-accelerated
                                devs cycle through faster
```

But the real change is: **measure cycle time, not WIP count.** An AI-accelerated developer with 3 items in progress that each take 4 hours is moving faster than a traditional developer with 1 item that takes 3 days. WIP limits should be per-developer and velocity-aware.

### 4.2 Expedite Lane for AI-Accelerated Work

Add an **expedite lane** to the Kanban board for stories that an AI-accelerated developer can complete in under a day:

```
Backlog → [Expedite Lane] → Review (Priority) → Done
       → [Standard Lane]  → Review (Standard) → Done
```

Rules:
- Stories in the expedite lane get **same-day review** commitment from the team
- Only stories that are vertical slices (complete features) qualify
- The developer must include an AI-generated PR summary and passing tests

**Why this matters:** The biggest demotivator for AI-accelerated developers is finishing work fast and then waiting 2-3 days for review. The expedite lane creates a social contract: if you deliver faster, the team reviews faster.

### 4.3 "Ready to Build" Definition

Add a new column or tag: **"AI-Ready"** — stories that are fully specified, have clear acceptance criteria, and include enough context for an AI-assisted developer to pick up with minimal grooming.

An AI-Ready story includes:
- Clear acceptance criteria (not "make the profile page better")
- Referenced architecture decisions or constraints
- Links to relevant existing code
- Explicit mention of affected layers (data/API/UI)
- Any security, compliance, or performance requirements

**Why:** AI-assisted development is only fast when the *what* is clear. Ambiguous stories are just as slow with AI because the developer spends time figuring out what to build instead of building it.

### 4.4 Review Cadence: The 4-Hour Rule

Commit to a team norm: **no PR sits unreviewed for more than 4 business hours.**

Implementation:
- Rotate a daily "review champion" who is responsible for triaging incoming PRs
- Review champion doesn't have to review everything — they assign reviewers and ensure SLAs
- AI-generated PR summaries reduce review time (the reviewer reads the summary first, then focuses on architectural and business logic decisions)
- Small PRs (< 200 lines changed): 1 reviewer
- Large PRs (> 200 lines): 2 reviewers, one senior

### 4.5 Weekly Velocity Showcase (Replacing Standup Overhead)

Replace 2-3 standups per week with a **weekly velocity showcase**:

**Monday (30 min):** What's in progress, what's blocked, what's being pulled from the backlog. This is the planning/alignment meeting.

**Wednesday (async):** Post updates in a dedicated channel. Include: what you shipped, what you're working on, blockers. No meeting.

**Friday (30 min):** Velocity showcase. Each developer shows what they shipped that week. 2-3 minutes per person. This is the demo/review meeting.

**Why this works:**
- Monday creates urgency and alignment for the week
- Wednesday avoids a meeting that could be a Slack post
- Friday creates social proof — developers see what others accomplished, including those using AI tools

The Friday showcase is where culture shifts happen. When a developer using AI shows they shipped 3 features while others shipped 1, curiosity replaces skepticism.

### 4.6 Monthly AI Metrics (Not Vanity Metrics)

Track and share monthly:

| Metric | What It Shows |
|---|---|
| **Cycle time per story** (AI-assisted vs. traditional) | The velocity gap, transparently |
| **Review turnaround time** | Whether reviews are the bottleneck |
| **Features shipped per developer** | Outcome, not output |
| **Defect rate** (AI-assisted vs. traditional) | Quality comparison — addresses "AI code is buggy" narrative |
| **AI tool adoption %** | How many developers are using AI tools daily |
| **Prompt repo contributions** | Is the team sharing knowledge |

**Don't use these to shame anyone.** Present them as team metrics, not individual scorecards. The goal is to show the trend and motivate the AI-curious group.

---

## 5. Overcoming Resistance: Specific Tactics

### 5.1 The "Side-by-Side Challenge"

Pick a well-defined story from the backlog. Have two developers implement it:
- One using traditional methods
- One using AI-assisted development

**Rules:**
- Same story, same acceptance criteria, same definition of done
- Both developers have comparable seniority and domain knowledge
- Time themselves honestly
- Both present their implementation at the Friday showcase

This isn't about proving AI is better. It's about making the gap visible and concrete. When the team sees 2 hours vs. 2 days for the same feature, the conversation shifts from "should we adopt AI" to "how do I get started."

### 5.2 The "AI Buddy" System

Every developer who hasn't adopted AI tools gets paired with someone who has. Not for a formal training session — just as a standing offer:

- "When you hit a task that feels tedious, ping me and I'll show you how I'd approach it with AI"
- The buddy responds within an hour and does a 15-minute screen share
- The goal is to create **aha moments** on real work, not theoretical demos

### 5.3 Remove Friction from Tool Access

If adoption is low, check whether the tools are actually accessible:

- Is Cursor/Claude Code licensed for everyone? (If not, that's your first problem)
- Are `.cursorrules` files in every repo? (If not, the first experience with AI on the codebase will be bad)
- Are there proxy/VPN/firewall issues blocking API access?
- Is the AI tool integrated into the IDE everyone already uses?

**One hour of infrastructure setup can unblock months of adoption.**

### 5.4 Make AI Proficiency Part of Career Growth

Add AI-assisted development skills to the engineering competency matrix:

| Level | Expectation |
|---|---|
| Junior | Uses AI for code generation and learning. Can follow prompt templates. |
| Mid | Writes effective prompts for complex tasks. Contributes to prompt repo. |
| Senior | Designs prompt strategies for entire features. Mentors others in AI workflows. |
| Staff/Lead | Defines AI-augmented development standards. Designs team processes around AI capabilities. |

**This sends a clear signal:** AI proficiency isn't optional or experimental. It's a core engineering skill, like testing or code review.

### 5.5 Celebrate Early Adopters from the Reluctant Group

When a previously skeptical developer has a success with AI tools, amplify it. Publicly. In team chat, in the Friday showcase, in the retrospective.

"Sarah was skeptical about Cursor, but she used it to refactor the payment module this week and cut 3 days of work down to 4 hours."

This is more persuasive than any presentation from an AI enthusiast because it comes from someone the skeptics identify with.

---

## 6. The Technical Guides Repository

### Structure

The shared prompt repo (Section 2) focuses on prompts. This repo focuses on **how things work** — educational material that helps developers understand AI tools, the infrastructure, and the workflow:

```
ai-engineering-guides/
├── README.md
│
├── foundations/
│   ├── how-llms-work.md                # Non-technical explanation of tokens, context, temperature
│   ├── prompt-engineering-101.md        # Core principles: specificity, context, iteration
│   ├── prompt-engineering-201.md        # Advanced: chain-of-thought, few-shot, system prompts
│   ├── understanding-hallucinations.md  # Why they happen, how to detect, how to prevent
│   └── choosing-the-right-model.md      # When to use local vs. cloud, small vs. large
│
├── tools/
│   ├── cursor-deep-dive.md             # Features, shortcuts, .cursorrules authoring
│   ├── claude-code-guide.md            # CLI usage, hooks, MCP integration
│   ├── copilot-guide.md                # GitHub Copilot setup and effective use
│   ├── continue-extension.md           # Continue for VSCode with local models
│   └── aider-guide.md                  # Terminal-based AI pair programming
│
├── workflows/
│   ├── full-stack-feature.md           # End-to-end: prompt → implement → test → PR
│   ├── bug-investigation.md            # Using AI to trace bugs across layers
│   ├── legacy-code-refactoring.md      # Safely refactoring code you don't fully understand
│   ├── test-first-with-ai.md           # Writing tests before implementation with AI
│   └── code-review-with-ai.md          # Using AI to review others' PRs
│
├── infrastructure/
│   ├── local-llm-setup.md              # Running models locally (Seshat, Ptah, Ollama)
│   ├── seshat-gateway-usage.md         # How to use the model gateway from your code
│   ├── docker-compose-ai-workflow.md   # Spin up full stack, iterate with AI
│   └── mcp-for-developers.md           # How MCP connects AI tools to our services
│
└── team/
    ├── ai-adoption-faq.md              # Answers to common objections and concerns
    ├── our-ai-standards.md             # Team agreements on AI use (review, attribution, security)
    └── contributing.md                 # How to add guides and prompts
```

### Key Content Principle

Every guide should include:
1. **What** — what the tool/technique is
2. **Why** — why a developer should care (what problem it solves)
3. **How** — step-by-step instructions on their actual codebase
4. **Gotchas** — where it fails and what to do about it

Avoid generic tutorials. Everything should reference the team's repos, conventions, and infrastructure.

---

## 7. Phased Rollout Timeline

### Phase 1: Foundation (Weeks 1-4)

**Goal:** Make AI tools accessible and demonstrate value.

| Action | Owner | Deliverable |
|---|---|---|
| Ensure every developer has Cursor/Claude Code licensed | Engineering Manager | Tool access confirmed for all |
| Create `.cursorrules` for every active repo | AI-accelerated developers | PR per repo |
| Initialize the shared prompt repo with 10-15 high-value prompts | AI-accelerated developers | `ai-playbook/` repo live |
| First "Lunch and Learn" demo (30 min) | Volunteer from AI-accelerated group | Recording shared in team channel |
| Add "AI-Ready" tag to Kanban board | Scrum Master / PM | Board updated |
| Announce the AI Buddy system | Engineering Manager | Pairings posted |

**Process changes:** None yet. Keep existing Kanban as-is. The goal is to build the foundation without disrupting current work.

### Phase 2: Adoption (Weeks 5-10)

**Goal:** Get the AI-curious group actively using AI tools on real work.

| Action | Owner | Deliverable |
|---|---|---|
| Begin shadow/guided sessions (apprenticeship model) | AI-accelerated developers | 2-3 sessions per AI-curious developer |
| Run the first "Side-by-Side Challenge" | Engineering Manager | Results presented at Friday showcase |
| Start weekly Friday velocity showcases | Team lead | Recurring meeting |
| Reduce standups to Mon/Wed/Fri (Wed async) | Scrum Master | Calendar updated |
| Implement the 4-hour review SLA | Team agreement | Review champion rotation |
| Add expedite lane to Kanban board | Scrum Master | Board updated |
| Begin tracking AI adoption metrics monthly | Engineering Manager | Dashboard or monthly post |
| Second and third "Lunch and Learn" demos | Rotating presenters | Recordings archived |

**Process changes:** Standup reduction, expedite lane, review SLA. These are low-risk changes that benefit everyone.

### Phase 3: Acceleration (Weeks 11-18)

**Goal:** AI adoption becomes the norm, not the exception. Traditional-only developers are the minority.

| Action | Owner | Deliverable |
|---|---|---|
| Initialize the technical guides repo | AI-accelerated developers + willing contributors | `ai-engineering-guides/` repo live |
| Shift stories to vertical-slice format | Product Owner + Scrum Master | Story template updated |
| Move to 1-week Kanban cadence (weekly planning + showcase) | Team | Calendar updated |
| Add AI proficiency to engineering competency matrix | Engineering Manager + HR | Competency doc updated |
| Run second "Side-by-Side Challenge" with wider participation | Team | Results shared |
| Begin requiring AI-generated PR summaries | Team agreement | PR template updated |
| Implement trunk-based development with feature flags for 1-2 projects | Tech lead | Branching strategy doc updated |
| Seshat/Ptah gateway live — developers can use local models | Infrastructure | Gateway running, guide published |

**Process changes:** Vertical-slice stories, 1-week cadence, AI-generated PR summaries, trunk-based development pilot.

### Phase 4: Optimization (Weeks 19+)

**Goal:** Continuous improvement. The team operates as an AI-augmented unit.

| Action | Owner | Deliverable |
|---|---|---|
| Retrospective on process changes — what worked, what didn't | Team | Action items |
| Expand trunk-based development to all projects | Tech lead | All projects migrated |
| Consider full continuous flow (drop sprint boundaries) | Team decision | Process doc updated |
| Publish team AI standards document | Senior developers | Living doc in guides repo |
| Share playbook externally (blog post, conference talk) | Volunteers | Content published |
| Evaluate whether WIP limits need restructuring | Scrum Master | Board updated |
| Begin cross-team knowledge sharing (if multi-team org) | Engineering Manager | Cross-team sessions scheduled |

---

## 8. How to Handle the Holdouts

After 18 weeks, there may still be developers who haven't adopted AI tools. Here's a decision framework:

### Distinguish "Won't" from "Can't"

| Situation | Response |
|---|---|
| **Tried and struggling** — genuinely having trouble with prompt engineering | More pairing time. Assign an AI buddy for a full sprint. Provide specific prompts for their current work. |
| **Hasn't tried** — too busy, hasn't prioritized | Allocate explicit time: "spend 4 hours this week on one story using AI tools." Remove the "no time" excuse. |
| **Tried and rejected** — philosophical objection | Have a direct conversation. Understand the objection. If it's about quality/reliability, address with data (defect rates, cycle times). If it's about job security, address honestly. |
| **Actively resistant** — refuses despite support | This is a performance conversation, not an AI conversation. AI proficiency is a professional skill. Refusing to develop it is the same as refusing to write tests or do code review. |

### The Uncomfortable Truth

Some developers will not adopt AI tools. This is the same pattern that played out with:
- IDEs replacing text editors (1990s)
- Version control replacing FTP deployments (2000s)
- CI/CD replacing manual deploys (2010s)
- Cloud replacing on-prem (2010s)

In each case, the developers who adapted thrived. Those who didn't eventually found themselves working on legacy systems or leaving the industry. AI-assisted development is the same inflection point. The compassionate response is to provide every opportunity to adapt — not to pretend adaptation is optional.

---

## 9. Measuring Success

### Leading Indicators (early signals it's working)

- Number of developers using AI tools daily (self-reported or tool telemetry)
- Prompt repo contribution rate (new prompts per week)
- Shadow/guided session completion rate
- Attendance at Lunch and Learn demos

### Lagging Indicators (outcomes)

- Average cycle time per story (trending down)
- Features shipped per developer per month (trending up)
- Review turnaround time (at or below 4-hour SLA)
- Defect rate (stable or improving — proves AI doesn't introduce more bugs)
- Developer satisfaction (survey — are people feeling more or less productive?)

### The North Star Metric

**Time from story creation to production deployment.** This single metric captures everything: development speed, review speed, process efficiency, and deployment automation. If this number is decreasing quarter over quarter, the transformation is working.

---

## 10. Summary: The Principles

1. **Show, don't tell.** Demos and case studies persuade. Mandates create resentment.
2. **Learn inside the work.** Pair on real stories, not training exercises.
3. **Remove friction first.** Tool access, `.cursorrules`, shared prompts — before process changes.
4. **Change process gradually.** Foundation → Adoption → Acceleration → Optimization.
5. **Measure outcomes, not adoption.** If cycle time drops and quality holds, the *how* doesn't matter.
6. **Create urgency through visibility.** The Friday showcase is the culture-change engine.
7. **Be compassionate but clear.** Provide every opportunity to adapt. Don't pretend adaptation is optional.
8. **Protect quality.** AI makes you faster, not infallible. Reviews, tests, and gates remain.
