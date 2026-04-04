# Production AI Quality and Operations

This page is a practical bridge between experimentation and production.

Teams often learn prompting and tooling first, then discover the harder question later:

How do we know the system is actually good, safe, and stable enough to trust?

## The Three Missing Disciplines

Once an AI workflow matters in production, three disciplines become mandatory.

### 1. Evaluation

You need evidence that:

- a prompt change improved quality,
- a model swap did not break critical scenarios,
- a RAG update actually retrieves better context,
- and an agent workflow completes tasks reliably.

Good teams build:

- curated test sets,
- regression suites,
- rubric-based review,
- exact-match tests where possible,
- and human calibration for high-risk outputs.

### 2. Observability

You need traces that show:

- model and prompt version,
- token and cost usage,
- retrieval behavior,
- tool-call sequences,
- runtime failures,
- and user feedback signals.

Without this, debugging AI systems becomes storytelling instead of engineering.

### 3. Security and Guardrails

You need layered controls for:

- prompt injection,
- indirect injection from retrieved content,
- unsafe tool use,
- sensitive-data leakage,
- over-broad permissions,
- and unsafe autonomy.

Prompt wording helps, but architecture and permissions matter more.

## A Practical Production Standard

Before calling an AI workflow “production ready,” be able to answer:

- What exact task is it meant to perform?
- How do we evaluate success?
- What known failure cases are in the regression suite?
- What do we log?
- What data do we deliberately avoid logging?
- What actions can it take automatically?
- What actions require approval?
- What happens when it fails, loops, or sees hostile input?

## Where This Material Lives In The Curriculum

For the full standalone treatment, use:

- `../0-to-60-ai/21-AI-Evaluation-And-Quality.md`
- `../0-to-60-ai/22-AI-Observability-And-Operations.md`
- `../0-to-60-ai/23-AI-Security-And-Guardrails.md`

## Rule Of Thumb

If a system is important enough to automate, it is important enough to evaluate, observe, and secure like any other production system.
