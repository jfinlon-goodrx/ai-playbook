---
title: "Appendix: AI Evaluation and Quality Engineering"
status: first-draft
include_in_compile: true
synopsis: "How to evaluate AI systems, prompts, RAG flows, and agent behavior with enough rigor to ship them safely."
---

# AI Evaluation and Quality Engineering

> A practical appendix for teams building or operating AI-enabled features, assistants, RAG systems, or agent workflows.

---

## Why This Appendix Matters

Traditional software teams already know how to test deterministic systems. You can write a unit test, assert exact output, and trust the result to remain stable as long as the code does not change.

AI systems are different.

- Outputs are often open-ended rather than exact.
- A response can be fluent but wrong.
- A prompt change can improve one scenario and quietly break another.
- A better model on a public benchmark can still be worse for your application.

That is why AI quality work needs its own discipline. The goal is not perfect certainty. The goal is reliable evidence that a change made the system better, or at least did not make it worse.

## What To Evaluate

When teams say “the AI seems better,” they are often blending several different questions:

- Is the answer correct?
- Is it grounded in the right source material?
- Did it follow the requested format?
- Is it safe and policy-compliant?
- Did the agent use tools correctly?
- Is it fast enough?
- Is it affordable enough?

You need to separate those questions because they often move independently.

### Common Evaluation Dimensions

| Dimension | What it asks |
|---|---|
| **Task success** | Did the system actually help the user accomplish the goal? |
| **Accuracy** | Was the answer factually or operationally correct? |
| **Faithfulness / groundedness** | Did the answer stay anchored to approved source material? |
| **Instruction following** | Did the model respect format, constraints, and scope? |
| **Safety** | Did it avoid unsafe or policy-violating output and actions? |
| **Tool behavior** | Did the agent choose the right tool with the right arguments? |
| **Latency** | Was the response fast enough for the workflow? |
| **Cost** | Did the request consume an acceptable amount of tokens, compute, or operations? |

## The Four Layers of AI Evaluation

Think of evaluation in four layers.

### 1. Benchmark Evaluation

This is where public benchmarks like HumanEval, SWE-bench, MMLU, or GPQA are useful. They tell you something about broad capability and help compare model families.

Use benchmark evaluation for:

- high-level model selection,
- tracking research progress,
- understanding rough strengths and weaknesses.

Do not use benchmark scores alone to decide production readiness. Your users do not interact with “average benchmark performance.” They interact with your prompt, your documents, your business rules, and your error handling.

### 2. Application Evaluation

This is the most important layer for real teams. Build a test set from your own workflows and failure modes.

Examples:

- support questions from your actual documentation,
- coding prompts based on your team’s repo conventions,
- bug triage tasks from real tickets,
- internal search and summarization tasks,
- workflow agents that must use tools in a bounded way.

### 3. Regression Evaluation

Every meaningful change should be evaluated against what came before:

- prompt updates,
- model swaps,
- retrieval changes,
- chunking strategy updates,
- safety filter updates,
- tool description changes,
- agent workflow changes.

Regression evaluation protects you from silent quality decay.

### 4. Production Evaluation

Offline tests are necessary but incomplete. Once a system is in production, you should measure:

- user satisfaction,
- retry behavior,
- abandonment,
- escalation rate,
- actual task completion,
- runtime failures,
- safety incidents.

Production evaluation is where you learn what users actually experience, not just what your test set predicted.

## Building A Useful Evaluation Set

The fastest practical way to start is with a small but representative set of examples.

### A Good Starter Set

Build 50 to 200 examples split roughly like this:

- **Common cases**: most of the set
- **Known edge cases**: scenarios that historically cause trouble
- **Adversarial or stress cases**: prompts or inputs designed to expose weaknesses

For each example, capture:

- the input,
- the expected output or rubric,
- the failure mode to watch for,
- the business importance,
- and any notes on what “good enough” means.

### Examples of Useful Test Cases

- “Summarize this incident postmortem into a three-bullet executive update.”
- “Generate a PR summary for this change without inventing nonexistent tests.”
- “Answer a policy question using only the provided Confluence content.”
- “Route this request to the right tool and do not write outside the approved directory.”
- “Return JSON matching this schema exactly.”

## Exact Match, Rubrics, and LLM-as-Judge

Different tasks need different evaluation strategies.

### Exact Match

Best for:

- classification,
- extraction,
- schema-constrained JSON,
- deterministic fields,
- tool-call arguments in strict formats.

This is the cleanest kind of evaluation, but it only covers a subset of AI work.

### Rubric-Based Evaluation

Best for:

- summaries,
- explanations,
- prioritization outputs,
- draft documents,
- open-ended recommendations.

A rubric should name the dimensions that matter, such as:

- factual accuracy,
- completeness,
- clarity,
- formatting,
- groundedness,
- tone appropriateness.

### LLM-as-Judge

A strong model can score another model’s output against a rubric. This is useful because it scales much better than manual review.

Use it when:

- you need to compare many outputs quickly,
- exact match is impossible,
- and you have already validated that the judge aligns reasonably well with human review.

Do not use it blindly. Judge models have biases:

- they may prefer verbose answers,
- they may over-reward “confident” style,
- they may share the same blind spots as the candidate model,
- and they can be inconsistent around borderline cases.

The safest pattern is:

1. Define a rubric.
2. Use human review on a sample.
3. Compare the judge model’s scoring against the human sample.
4. Only then scale the judge approach.

## Evaluating RAG Systems

RAG systems fail in specific ways that ordinary prompt evaluation misses.

### What Can Go Wrong

- the retriever pulls the wrong documents,
- the right document exists but is never retrieved,
- the answer includes facts not supported by retrieved context,
- the answer is technically correct but cites weak or stale material,
- access control allows the wrong document set,
- chunking destroys meaning or context.

### Questions To Ask

- Did retrieval return the right chunks?
- Did the answer use those chunks faithfully?
- Was the answer complete enough for the task?
- Were citations or references trustworthy?
- Did the system retrieve too much noise?

### Practical Metrics

- retrieval precision,
- retrieval recall,
- contextual relevance,
- faithfulness,
- answer relevance,
- citation correctness.

For many teams, the highest-value practice is not a perfect metric suite. It is keeping a curated set of “known hard questions” and checking whether the system retrieves and answers them better over time.

## Evaluating Agents

Agent evaluation should not stop at the final answer. You also need to evaluate the behavior that produced the answer.

### What To Measure

- task completion rate,
- number of tool calls,
- tool error rate,
- retries and loops,
- whether the right tool was chosen,
- whether the tool arguments were valid,
- whether the agent stopped when it should have,
- whether it escalated correctly when it lacked confidence.

### Why This Matters

Two agents can produce similar final answers while behaving very differently:

- one uses the right tool once and stops,
- the other loops through six tool calls, wastes cost, and only gets lucky.

From an engineering perspective, those are not equivalent systems.

## Regression Testing For AI Systems

Any time you change:

- a prompt,
- a model,
- a retrieval strategy,
- a tool description,
- a system prompt,
- or an agent workflow,

you should run a regression suite.

### A Good Regression Suite Should Include

- happy-path cases,
- known historical failures,
- strict formatting cases,
- safety-sensitive cases,
- tool-use cases,
- and high-value business scenarios.

Track not just pass/fail but directional movement:

- improved,
- unchanged,
- regressed,
- indeterminate.

This is especially helpful when a change improves one cluster of scenarios but hurts another.

## AI Quality In CI/CD

Treat AI quality checks as part of delivery, not as an afterthought.

### A Practical Pipeline

1. Run schema or exact-match checks for constrained outputs.
2. Run curated regression examples.
3. Run RAG and faithfulness checks if retrieval is involved.
4. Run safety and adversarial probes for sensitive systems.
5. Require human sign-off if the change affects high-risk behavior.

In a mature system, an AI-related pull request should be able to answer:

- What changed?
- What evaluation set was run?
- What improved?
- What regressed?
- What residual risk remains?

## Red Teaming and Adversarial Testing

Do not wait for public incidents to discover failure modes.

Include tests for:

- prompt injection,
- jailbreak attempts,
- misleading retrieved documents,
- malformed tool outputs,
- policy edge cases,
- ambiguous or contradictory user requests.

For high-risk applications, red teaming should be periodic, documented, and repeatable.

## Human Evaluation Still Matters

Human review is expensive, but it remains the gold standard for:

- nuanced correctness,
- usefulness,
- domain appropriateness,
- and trustworthiness.

Use humans especially for:

- validating a new rubric,
- calibrating LLM-as-judge,
- reviewing high-risk workflows,
- and deciding whether a system is good enough to expose to users.

## A Simple Starter Checklist

- [ ] Define the task clearly.
- [ ] Build a small representative test set.
- [ ] Separate exact-match cases from rubric-based cases.
- [ ] Add known failure cases, not just happy paths.
- [ ] Record a baseline before changing prompts or models.
- [ ] Run regression checks on every meaningful change.
- [ ] Measure cost and latency alongside quality.
- [ ] Add human review for important or ambiguous cases.

## Key Takeaways

- Public benchmarks are useful, but they are not production readiness.
- The most important evaluation set is the one built from your own workflows.
- Regression testing is mandatory once prompts, models, tools, or retrieval are changing regularly.
- RAG and agent systems need behavior-level evaluation, not just answer-level evaluation.
- LLM-as-judge is powerful, but only when calibrated against human judgment.
- AI quality work belongs in your delivery pipeline, not in a disconnected experiment notebook.

---

*[Back to Curriculum Overview](../00-Curriculum-Overview/00-Curriculum-Overview.md)*
