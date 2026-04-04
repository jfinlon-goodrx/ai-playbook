# AI-Native Delivery Principles

This document condenses the strongest ideas from the Agile essays and operating-model notes into a single reference.

## The Shift

Traditional Agile assumed that implementation time was expensive, cross-layer work was risky, and smaller batches were the safest way to ship. AI changes that equation for teams that know how to use it well.

What gets cheaper:

- first-draft implementation,
- navigating across layers,
- scaffolding tests and docs,
- summarizing changes and producing review artifacts.

What becomes more important:

- problem clarity,
- architecture and guardrails,
- review quality,
- rollout safety,
- observability and feedback.

## The Four-Layer Operating Model

### 1. Intent Layer

Define:

- user problem,
- business outcome,
- acceptance criteria,
- scope boundaries,
- risk level,
- rollout expectations,
- success measures.

This is where product, engineering, design, and QA align on the change before generation begins.

### 2. Change Generation Layer

Generate:

- implementation candidates,
- tests,
- documentation,
- release notes,
- migration and rollout artifacts.

This is where AI gives the biggest leverage, especially when the team builds vertical slices instead of layer-isolated fragments.

### 3. Guardrail Layer

Validate:

- correctness,
- security,
- design and architecture fit,
- performance,
- accessibility,
- maintainability,
- policy compliance.

The faster the generation loop becomes, the more important this layer becomes.

### 4. Feedback Layer

Learn from:

- telemetry,
- support signals,
- defect escape,
- deployment health,
- user behavior,
- retrospectives,
- prompt and workflow quality.

## What Good Story Shaping Looks Like Now

A modern story should describe intent and constraints, not micro-manage implementation steps.

Include:

- the problem being solved,
- the user or business value,
- the acceptance criteria,
- the explicit non-functional constraints,
- the risk classification,
- the rollout plan,
- the observability or analytics expectation,
- the out-of-scope boundaries.

Avoid:

- decomposing one feature into many ceremony-heavy layer tickets too early,
- vague requests like “make this better,”
- stories with no security or rollout context for sensitive changes.

## The New Default: Vertical Slice Delivery

For bounded work, the best default is one feature, one branch, one coherent PR, one review path.

Use the vertical-slice pattern when:

- the work spans API, service, data, and UI,
- the feature can be validated as one whole unit,
- the team has enough context and guardrails to review the integrated change safely.

Support it with:

- project rules or context files,
- feature flags,
- strong CI,
- AI-generated summaries,
- review checklists by risk tier.

## Review Has To Change

Human review should increasingly focus on:

- architectural fit,
- domain correctness,
- security-sensitive decisions,
- edge cases and failure modes,
- rollout and operational risk.

Automation and AI review should increasingly handle:

- linting and formatting,
- static quality checks,
- common bug patterns,
- security pattern scanning,
- test coverage gaps,
- documentation drift,
- diff summaries.

## Risk Tiers Keep Speed Safe

### Low Risk

Examples:

- copy changes,
- low-risk UI tweaks inside approved components,
- bounded config changes.

Controls:

- automation,
- preview evidence,
- lightweight review.

### Medium Risk

Examples:

- normal feature work,
- typical API and business logic changes,
- user-facing behavior changes with bounded blast radius.

Controls:

- full CI,
- engineering review,
- product or design verification where needed,
- targeted QA.

### High Risk

Examples:

- auth and permissions,
- financial calculations,
- regulated data handling,
- data migrations,
- infrastructure changes,
- performance-critical logic.

Controls:

- stronger engineering review,
- domain-owner review,
- explicit test evidence,
- stronger rollout and rollback planning,
- security or compliance review.

## Metrics Worth Tracking

Track the system, not just the coding speed:

- cycle time from story creation to production,
- review turnaround time,
- escaped defects,
- rollback rate,
- release frequency,
- adoption of shared prompt/rule assets,
- contribution to reusable patterns,
- time spent in manual QA versus automated confidence building.

## What To Stop Optimizing For

- story point theater,
- layer-by-layer ticket decomposition as a default,
- manual review of mechanical issues,
- using velocity as a proxy for individual worth,
- treating QA as the final manual bottleneck instead of a design and risk function.

## Bottom Line

AI-native delivery is not “move faster and hope.” It is:

- clearer intent,
- faster generation,
- stronger guardrails,
- tighter feedback,
- and more human attention spent where judgment matters most.
