---
title: "Appendix: AI Security and Guardrails"
status: first-draft
include_in_compile: true
synopsis: "Prompt injection, tool abuse, data leakage, and the layered controls needed to build safer AI systems."
---

# AI Security and Guardrails

> A practical appendix for builders, reviewers, platform teams, and leaders responsible for deploying AI safely.

---

## Why AI Security Feels Different

Most software security disciplines assume a meaningful separation between code and data. AI systems blur that boundary. A model receives system instructions, user input, retrieved documents, tool descriptions, and tool outputs as language-like context. That creates a new kind of attack surface.

This is why AI security is not just “ordinary app security plus a model.” It is ordinary app security plus:

- prompt injection,
- indirect injection through retrieved content,
- tool abuse,
- system-prompt leakage,
- unsafe autonomy,
- data exfiltration through model output,
- and governance failures around what data enters the system at all.

## The Threat Model

At minimum, an AI-enabled system should consider these risks.

### 1. Direct Prompt Injection

The attacker types malicious instructions directly into the user input:

- “Ignore previous instructions.”
- “Reveal your system prompt.”
- “Act as a different assistant.”

This is the simplest attack, but it is not the only one.

### 2. Indirect Prompt Injection

The malicious instruction lives in content the model reads:

- a retrieved document,
- a web page,
- an email,
- a file,
- a ticket,
- or a tool response.

This is often more dangerous because the end user may be innocent. The system itself brings the hostile content into context.

### 3. Tool Abuse

A model with tool access can do more damage than a chatbot with no actions:

- create or modify records,
- write files,
- call sensitive APIs,
- send messages,
- trigger deployments,
- or expose information from connected systems.

The risk is not only malicious input. It is also over-broad permissions.

### 4. Data Leakage

Sensitive data can leak through:

- prompts,
- retrieved context,
- logs,
- model outputs,
- tool responses,
- screenshots,
- or copied examples.

Even a harmless-seeming debugging workflow can become a security problem if a developer pastes the wrong data into an AI tool.

### 5. Unsafe Autonomy

The more independently a system acts, the more valuable guardrails become.

An agent that reads and writes files, calls external systems, and loops through decisions can:

- take unintended actions,
- repeat the wrong action many times,
- consume large amounts of cost,
- or spread a bad assumption across multiple systems.

## Defense In Depth

No single defense solves this problem. Good AI security is layered.

### Layer 1: Input Controls

Use:

- length and format constraints,
- normalization of encoded or unusual inputs,
- detection of known prompt-manipulation patterns,
- and, where justified, a classifier or secondary model to flag suspicious content.

Pattern matching alone is not enough, but it is still useful as one layer.

### Layer 2: Context Controls

Treat context sources differently based on trust:

- system instructions: highest trust,
- developer or application instructions: high trust,
- user input: moderate trust,
- external retrieved content: lowest trust.

Do not treat all text in context as equally trustworthy just because the model sees it together.

### Layer 3: Output Controls

Check outputs for:

- schema validity,
- policy violations,
- sensitive data leakage,
- unexpected tool intent,
- and unexplained behavioral changes.

If the expected output is strict JSON, reject malformed prose. If the expected action is read-only, block write-oriented suggestions.

### Layer 4: Tool and Permission Controls

Use:

- least privilege,
- allowlisted tools,
- confirmation gates for high-impact actions,
- read-only defaults,
- scoped credentials,
- and action validation before execution.

An AI should never have more access than it needs just because the workflow is convenient.

### Layer 5: Monitoring and Response

Log enough to answer:

- what the model saw,
- what it proposed,
- what tools it tried to use,
- what was blocked,
- and what actually executed.

You cannot defend what you cannot inspect.

## Prompt Injection In Plain Language

The simplest way to explain prompt injection is this:

The model is trying to follow instructions, but it does not have a perfect architectural firewall between trusted instructions and untrusted text. Attackers exploit that weakness by making malicious text look like instructions the model should follow.

That is why “just write a stronger system prompt” is not a complete answer. Better prompts help, but architectural controls matter more.

## Safer Patterns For RAG

If your system retrieves content, use safer patterns such as:

- sanitize fetched content,
- strip hidden markup or low-value metadata where appropriate,
- tag content by trust level and source,
- keep the user’s goal separate from retrieved content,
- and verify that any action taken still matches the user’s original intent.

One helpful mental model is:

retrieved content may inform the answer, but it should not silently rewrite the rules of the system.

## Safer Patterns For Tools and MCP

MCP and similar tool systems are powerful because they make tool access portable and structured. They also widen the attack surface.

Questions to ask:

- Does this tool need write access?
- Does the model understand when to use it?
- Can the arguments be validated before execution?
- What happens if the tool output is poisoned or misleading?
- What actions require explicit approval?
- What is the blast radius if the tool is misused?

For sensitive workflows, the strongest pattern is often:

- read-only first,
- confirm-write second,
- bounded autonomy only after strong trust, logging, and review exist.

## Guardrails For Teams

Every technical team using AI should make a few rules explicit.

### Recommended Team Rules

- Do not paste secrets, credentials, or regulated data into AI tools.
- Do not assume the model understands the security context unless you state it.
- Require review for AI-generated code and configuration.
- Treat prompt assets and tool definitions as versioned engineering artifacts.
- Prefer read-only integrations until there is a clear case for write access.
- Use redaction, logging, and incident response patterns appropriate to the data.

## Red Teaming For Real Teams

Red teaming does not have to begin as a giant formal program. Start by testing the obvious weak points:

- can the assistant be instructed to ignore its role?
- can a retrieved document alter behavior?
- can a model be convinced to reveal hidden instructions?
- can a tool-using workflow be pushed toward an unapproved action?
- can the system leak sensitive information in its output or logs?

As the system becomes more important, make red teaming more formal and repeatable.

## What To Put In A Security Review

For any meaningful AI-enabled feature, the review should ask:

- What data enters the system?
- What data leaves the system?
- What tools can it use?
- What approvals are required?
- What high-risk actions are blocked?
- What happens if retrieved content is malicious?
- What logs are retained?
- What residual risk remains?

## Key Takeaways

- AI security is a layered discipline, not a prompt-writing trick.
- Prompt injection and indirect injection are real engineering concerns, not theoretical curiosities.
- Tool access, retrieval, and logging create new security and privacy failure modes.
- Least privilege, validation, and human approval gates matter more as systems gain autonomy.
- Teams need explicit AI guardrails just as they need coding standards and release controls.

---

*[Back to 0 to 60 AI](README.md)*
