---
title: "Prompt Engineering"
status: first-draft
include_in_compile: true
synopsis: "Techniques, planning documents, system prompts, and avoiding common pitfalls."
---

# Module 8: Prompt Engineering

> **Learning Objectives**
> By the end of this module, you will be able to:
> - Explain why the quality of AI output depends directly on the quality of your input
> - Structure prompts using context, task, format, constraints, and examples
> - Apply key techniques — zero-shot, few-shot, chain-of-thought, and role-based prompting
> - Understand what system prompts are and why they matter for production AI
> - Build a prompt plan document using the PLAN framework for complex tasks

---

## Why Prompts Matter

Every interaction you have with a large language model begins with a prompt — the text you type or send through an API. That prompt is the single biggest factor determining whether you get a useful, accurate result or a vague, off-target one. The relationship is straightforward: **the quality of your output is directly proportional to the quality of your input.**

This may seem obvious, but it runs counter to how most people first approach AI tools. We are used to search engines, where a few keywords are enough. We type "quarterly report template" into Google and get useful links. But an LLM is not a search engine. It is a reasoning and generation system that takes your instructions literally and fills in everything you leave unspecified with its best guess. When you give it a vague prompt, you are handing it a blank canvas and saying "paint something." You might love the result. More likely, you will not.

Think of prompting like giving directions to a talented but literal-minded colleague who just joined your team. They have broad skills, but they know nothing about your project, your preferences, your audience, or your constraints. The more context and specificity you provide, the closer their first attempt will be to what you actually need.

The good news is that prompting well is a learnable skill, not an innate talent. The techniques in this module will give you a reliable framework for getting consistently better results from any LLM.

---

## Prompting for Software Delivery

In technical teams, prompting is not just about getting a polished paragraph or a clever explanation. It is a delivery skill. A prompt can shape:

- how an AI assistant reads a codebase,
- how it proposes a feature implementation,
- how it generates tests,
- how it reviews a pull request,
- how it summarizes a change for human reviewers,
- and how safely it interacts with real tools and systems.

This means the quality bar is higher than it is for casual chatbot usage. In software delivery, a prompt should usually answer five practical questions:

1. **What should the model read first?**
2. **What exact outcome do we want?**
3. **What constraints must it not violate?**
4. **What existing patterns should it copy?**
5. **How will we verify the result?**

If those questions are missing, the AI tends to fill the gaps with guesses. In code, those guesses become bad abstractions, missing security checks, weak tests, or output that does not match the repository's conventions.

---

## The One-Shot Pitfall

The most common mistake in working with AI is also the most understandable one: you type a quick question, scan the response, feel disappointed, and conclude that the tool is not very good.

Here is what that looks like in practice. You need help drafting a project status update, so you type:

> Write a status update.

The model produces something generic — maybe a template with placeholder text, maybe a vaguely corporate paragraph about "making great progress." It is technically a status update, but it is not *your* status update. It does not know your project, your audience, your format, or what happened this week.

This is the one-shot pitfall: treating AI like a vending machine where you insert a sentence and expect a finished product. It is the equivalent of walking up to that new colleague and saying "write something" without any further context.

The pitfall is reinforced by the fact that LLMs are designed to always produce *something*. They will never stop and say "I do not have enough information to help you." They will fill in the gaps with assumptions — and those assumptions are often wrong.

The fix is not complicated. It requires slowing down for thirty seconds to think about what you actually want before you start typing. That small investment in prompt quality pays for itself many times over in reduced back-and-forth and better first-draft results.

---

## Anatomy of a Great Prompt

A well-constructed prompt has up to five components. Not every prompt needs all five, but knowing them gives you a checklist to work from.

**Context** — Background information the model needs. Who are you? What is the situation? What does the model need to know to do its job?

**Task** — The specific action you want performed. Be precise. "Summarize" is different from "analyze," which is different from "critique."

**Format** — How you want the output structured. Bullet points? Numbered list? A table? Specific headings? A particular length?

**Constraints** — Boundaries and limitations. What to include, what to exclude, what tone to use, how long it should be, what audience it is written for.

**Examples** — Samples of what good output looks like. Even one example dramatically improves consistency.

### Before and After

Let us see these components in action with a real task: writing a status update for a sprint review.

**Before (vague prompt):**

> Write a status update for my team.

This produces generic filler. The model has no idea what your team does, what happened this sprint, who will read it, or what format your organization expects.

**After (structured prompt):**

> I am a tech lead on a platform engineering team. We just finished a two-week sprint focused on migrating our authentication service from a monolith to a microservice. Write a sprint status update for our bi-weekly review meeting with engineering leadership.
>
> Include these sections: Summary (2-3 sentences), Completed Work (bullet points), Blockers (bullet points, if any), Next Sprint Goals (bullet points).
>
> Tone should be professional but direct — no filler language. Keep it under 300 words.
>
> Completed items: extracted auth module into standalone service, set up CI/CD pipeline for new service, wrote integration tests covering OAuth and SAML flows, migrated 40% of internal consumers to new endpoint.
>
> Blockers: waiting on security team review of token rotation implementation.
>
> Next sprint: complete consumer migration, load testing, runbook documentation.

The second prompt gives the model everything it needs: context (who you are, what the project is), task (write a status update), format (specific sections), constraints (tone, length), and the raw information to work with. The result will be something you can use with minimal editing.

---

## Key Prompting Techniques

### Zero-Shot Prompting

Zero-shot means giving the model an instruction with no examples. You rely entirely on its training to understand what you want. This works well for straightforward, well-defined tasks where the expected output is obvious.

**Example:**

> Classify the following support ticket into one of these categories: Billing, Technical, Account, or Shipping.
>
> Ticket: "I was charged twice for my subscription this month and need a refund."
>
> Category:

Zero-shot is fast and requires no preparation. It is a good starting point, but it breaks down when the task is ambiguous, the expected format is unusual, or the domain is specialized.

### Few-Shot Prompting

Few-shot prompting provides several examples of input-output pairs before presenting the actual task. The model identifies the pattern across your examples and applies it. Research consistently shows that providing examples is the single most impactful thing you can do to improve prompt reliability.

**Example:**

> Analyze the sentiment of each product review. Respond with exactly one of: POSITIVE, NEGATIVE, or MIXED.
>
> Review: "Absolutely love this keyboard. The switches are smooth and the build quality is excellent."
> Sentiment: POSITIVE
>
> Review: "Terrible customer service. The product itself is fine but I will never buy from them again."
> Sentiment: MIXED
>
> Review: "Complete waste of money. Broke after two days."
> Sentiment: NEGATIVE
>
> Review: "The battery life is incredible and the camera quality exceeded my expectations."
> Sentiment:

Three to five examples is the sweet spot for most tasks. Choose examples that are diverse (covering different categories or edge cases), consistent in format, and representative of real inputs. If all your examples happen to be short and simple, the model will struggle when it encounters a long, nuanced input.

### Chain-of-Thought Prompting

Chain-of-thought (CoT) asks the model to show its reasoning step by step before arriving at a final answer. This dramatically improves performance on math, logic, multi-step analysis, and any task where a human would need to "think it through."

The simplest version is surprisingly effective — just add "Let's work through this step by step" to the end of your prompt.

**Example:**

> A store offers a 20% discount on all items, and members get an additional 10% off the discounted price. If an item originally costs $150, how much does a member pay?
>
> Let's work through this step by step.

Without chain-of-thought, models sometimes jump to an incorrect answer (like applying 30% off directly). With it, the model walks through each discount calculation sequentially and arrives at the correct result.

Use chain-of-thought for reasoning tasks and multi-step problems. Skip it for simple factual questions — asking the model to "think step by step" about the capital of France just wastes time and tokens.

### Role-Based Prompting

Role-based prompting assigns the model a specific persona or expertise area. This works because LLMs have internalized patterns of how different experts communicate, reason, and prioritize information. Telling the model *who it is* activates those patterns.

**Example:**

> You are a senior database administrator with 15 years of experience optimizing PostgreSQL for high-traffic web applications. You prioritize practical, battle-tested solutions over theoretical best practices.
>
> Our main query for the dashboard page is taking 4 seconds. The table has 12 million rows and we are joining across three tables with multiple WHERE clauses. What should we investigate first?

The key to effective role assignment is specificity. "You are a helpful assistant" adds nothing — that is already the model's default behavior. Instead, specify the role, the expertise level, the domain, and the communication style you want. A "senior database administrator" gives you different advice than a "junior developer learning SQL" would, because the model adjusts its vocabulary, assumptions about your knowledge level, and the sophistication of its recommendations.

---

## System Prompts

When you use an AI tool through a chat interface, you only see the user prompt — the text you type. But behind the scenes, most AI applications also include a **system prompt**: a set of instructions that runs before your message, shaping how the model behaves throughout the conversation.

System prompts are where application developers define the model's personality, expertise, boundaries, and default behaviors. When you use ChatGPT, Claude, or a custom AI tool at work, a system prompt is already in place telling the model things like what role to play, what topics to engage with, what format to default to, and what it should refuse to do.

If you are building AI-powered tools or workflows (even simple ones through an API), the system prompt is your most powerful lever for controlling behavior. It has elevated priority — models are trained to follow system prompt instructions more reliably than equivalent instructions buried in user messages.

A well-structured system prompt typically covers:

- **Role and identity.** Who the model is and what expertise it brings.
- **Communication style.** Tone, verbosity, technical level.
- **Task scope.** What the model should and should not do.
- **Output defaults.** Preferred format, length, structure.
- **Guardrails.** Topics to avoid, information to never disclose, safety constraints.

For example, a system prompt for an internal IT support bot might read:

> You are an IT support specialist for Contoso Corp. You help employees troubleshoot hardware, software, and network issues. You are friendly, patient, and concise. Always ask clarifying questions before suggesting solutions. Never provide instructions that require admin privileges — instead, direct the user to submit a ticket. Only discuss Contoso-supported software and hardware.

Two important principles for system prompts: keep them focused (a 500-word prompt that nails the core behavior outperforms a 3,000-word prompt that tries to cover every edge case), and treat them like code (version them, test them, review changes before deploying).

---

## Prompt Assets for Teams

Mature teams do not rely on every person reinventing prompts from scratch. They build reusable prompt assets:

- task prompts for repeatable work,
- pattern prompts for multi-step workflows,
- project-context files such as `CLAUDE.md` or `.cursorrules`,
- review checklists,
- and saved examples of prompts that produced high-quality results.

This is how prompting moves from individual talent to organizational capability.

Examples of reusable prompt assets include:

- a vertical-slice prompt for full-stack feature delivery,
- a bug-investigation prompt that expects logs, stack traces, and environment data,
- a test-generation prompt that insists on behavior-focused assertions,
- a PR-summary prompt that explains the intent and risk of the change,
- and a security-review prompt for sensitive code paths.

Prompt libraries are valuable for three reasons:

1. They shorten the path from curiosity to competence.
2. They reduce repeated prompting mistakes.
3. They make good workflows visible and shareable across the team.

---

## Prompt Hygiene and Security

As soon as prompts touch real code, tickets, logs, data, or internal systems, prompt quality becomes inseparable from prompt safety.

Good prompt hygiene means:

- never pasting secrets, API keys, or production credentials,
- redacting personal or regulated data before sharing logs or examples,
- explicitly stating sensitive constraints such as auth, compliance, or PHI handling,
- telling the model when not to make assumptions,
- and verifying output against source material before acting on it.

For engineering work, “secure prompting” is often just disciplined context-setting. If a change touches authentication, permissions, financial rules, privacy, or regulated data, those constraints should be named directly in the prompt instead of assumed.

That said, prompt hygiene is only the beginning. Once a system reads external content, retrieves documents, or uses tools, the security problem becomes larger than prompt wording alone. Prompt injection, indirect injection, and tool abuse require layered controls at the system level. See the appendix on [AI Security and Guardrails](../23-Appendix-AI-Security-And-Guardrails/Appendix-AI-Security-And-Guardrails.md).

---

## Building a Prompt Plan Document

For complex or high-stakes tasks, it pays to plan before you prompt. Many prompt failures happen before a single word is typed — they happen because the person did not think clearly about what they wanted. The result is vague instructions, missing constraints, and repeated re-prompting that wastes time.

The **PLAN framework** gives you a quick structure for thinking through complex prompting tasks before you start.

**P -- Purpose.** What specific outcome do you need? What will you do with the output? Who is the audience? A status update for your team lead requires a different prompt than a status update for a VP.

**L -- Limits.** What constraints exist? Length, format, tone, topics to include or exclude, required accuracy level. Constraints are the guardrails that keep the output on track.

**A -- Approach.** Which prompting technique will you use? Zero-shot for a simple task? Few-shot with examples for classification? Chain-of-thought for analysis? Will you break the task into a chain of smaller prompts? Choosing your approach before you write prevents mid-prompt confusion.

**N -- Norms.** What does "good" look like? How will you evaluate the output? If you cannot describe your acceptance criteria, you are not ready to prompt yet. "I will know it when I see it" is a recipe for frustration.

Here is a brief example of a PLAN document:

> **Purpose:** Generate a summary of our Q4 security audit findings for the engineering all-hands meeting. The audience is the full engineering department (100+ people, mixed seniority). The summary will be presented as slides.
>
> **Limits:** 400-600 words. No specific vulnerability details (these are confidential until remediated). Emphasize trends and improvements, not blame. Professional but accessible tone — avoid jargon that non-security engineers would not know.
>
> **Approach:** Provide the raw audit data as input. Use a single prompt with explicit format instructions (summary paragraph, three key findings as bullet points, one chart-ready data table, recommended next steps).
>
> **Norms:** Success means the security lead can use the output with under 10 minutes of editing. All statistics must match the source data. No audit findings should be fabricated.

This five-minute planning exercise prevents hours of unsatisfying back-and-forth. It also creates a reusable artifact — next quarter, you can update the data and reuse the same plan.

---

## Common Mistakes

Even with good intentions, certain patterns consistently lead to poor results. Here are the most frequent ones and how to avoid them.

**Being too vague.** "Make this better" or "help me with my project" gives the model almost nothing to work with. Always specify the task, the subject matter, and the desired outcome. Instead of "Make this better," try "Rewrite this paragraph to be more concise, targeting a non-technical audience, in under 75 words."

**Not providing context.** The model does not know your role, your company, your project, or your audience unless you tell it. Spending two sentences on context ("I am a QA engineer writing test cases for a payment processing API that handles credit card transactions") dramatically changes the relevance of the response.

**Not specifying format.** If you need bullet points but get paragraphs, or need a table but get prose, the fix is simple: state the format you want. Models follow formatting instructions reliably when you provide them, but they will guess (often wrong) when you do not.

**Not iterating.** No prompt is perfect on the first try. Treat prompting as a conversation. If the first result is close but not right, tell the model what to change rather than starting from scratch. "Good structure, but make the tone less formal and cut the introduction down to one sentence" is a productive follow-up.

**Trying to do too much in one prompt.** A single prompt that asks the model to research, analyze, compare, summarize, and format a recommendation is asking for trouble. Break complex tasks into steps. Each step becomes simpler, more reliable, and easier to debug when something goes wrong.

**Ignoring your examples.** When you provide examples that are inconsistent with each other — different formats, different levels of detail, contradictory approaches — the model gets confused. If you use examples, make sure they are consistent and intentional.

---

## Key Takeaways

1. **Quality in, quality out.** The single biggest factor in AI output quality is the prompt you provide. A few extra seconds of thought before typing saves minutes of revision afterward.

2. **Use the five components.** Context, task, format, constraints, and examples form the backbone of every effective prompt. Not every prompt needs all five, but knowing them prevents common omissions.

3. **Match the technique to the task.** Zero-shot for simple tasks, few-shot for consistency, chain-of-thought for reasoning, role-based for specialized expertise. Using the right technique for the job makes a measurable difference.

4. **System prompts are the control plane.** When building AI-powered tools, the system prompt is your primary mechanism for shaping behavior. Treat it with the same care you would give to any production configuration.

5. **Plan before you prompt.** For complex tasks, the PLAN framework (Purpose, Limits, Approach, Norms) turns vague intentions into specific, actionable prompts.

6. **Iterate, do not restart.** Prompting is a conversation. Build on what works, refine what does not, and save your best prompts for reuse.

---

## Try This

> **Exercise 1: Before and After**
> Pick a task you have recently used AI for (or wanted to). Write the quick, off-the-cuff prompt you would naturally type. Then rewrite it using the five components: context, task, format, constraints, and at least one example of desired output. Run both prompts and compare the results.

> **Exercise 2: Technique Matching**
> Try the same task with three different techniques. For example, take a classification task (categorizing support tickets, triaging bugs, labeling emails) and attempt it with zero-shot, few-shot (3 examples), and role-based prompting. Note which technique gives you the most consistent results.

> **Exercise 3: Build a PLAN**
> Choose a complex work task — drafting a proposal, summarizing a long document, creating test cases for a feature. Before prompting, write a PLAN document covering Purpose, Limits, Approach, and Norms. Then write your prompt based on the plan. Compare the experience and output quality to how you would normally approach the task.

> **Exercise 4: Prompt Iteration**
> Start with a deliberately minimal prompt for a task you care about. When the model responds, do not start over. Instead, send follow-up messages refining the output: adjust the tone, change the format, add missing sections, remove unnecessary content. Practice the skill of iterative refinement — it is faster than trying to write the perfect prompt on the first attempt.

---

**For role-specific prompt templates and concrete examples** (e.g. status updates, user stories, test cases, runbooks), see your role's appendix in the [Curriculum Overview](00-Curriculum-Overview.md#role-specific-appendices).
