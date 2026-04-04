---
title: "MCP & Integrations"
status: first-draft
include_in_compile: true
synopsis: "Model Context Protocol, file system access, API access, and security considerations."
---

# Module 11: MCP & Integrations

> **Learning Objectives**
> By the end of this module, you will be able to:
> - Explain how AI systems access external tools, files, and services through defined protocols and permissions
> - Describe what the Model Context Protocol (MCP) is and why it matters for standardizing AI integrations
> - Identify how AI connects to file systems, applications, and APIs on your behalf
> - Evaluate the security controls that govern AI integrations, including consent, sandboxing, audit trails, and least privilege
> - Recognize enterprise integration patterns for safely adopting AI-powered tool access

---

## How AI Accesses Your World

When you ask an AI assistant to "check the latest Jira tickets" or "summarize that Confluence page," something concrete happens behind the scenes. The AI is not reaching out into the internet with some mysterious capability. It is calling a defined tool, through a defined protocol, with permissions you or your organization have granted.

This is a critical point for IT professionals: AI integration is not magic. It is software architecture. Every time an AI reads a file, queries a database, or posts a message to Slack, it does so through an explicit mechanism that can be inspected, controlled, and audited.

At the most basic level, AI systems access external resources through **tools** -- structured functions that the AI can invoke when it determines that an action is needed. The AI model itself cannot open a network socket or read a disk. Instead, the host application (the software you are running, such as an IDE, a chat interface, or a custom agent) provides the AI with a catalog of available tools. When the model decides it needs to use one, it generates a structured request describing which tool to call and with what parameters. The host application then executes that call on the model's behalf and returns the result.

This design means the AI is always operating within boundaries set by the host application. The model proposes actions; the host decides whether to execute them.

For software teams, this distinction is critical. The same AI assistant that can help you read code or summarize documentation can become dangerous if it is allowed to mutate infrastructure, open pull requests, or access production systems without clear controls. Integration design is therefore not an optional implementation detail. It is part of the operating model.

---

## A Practical Adoption Order

Teams usually get the best results from this sequence:

1. **Read-only integrations first** -- documentation search, repository browsing, ticket lookup.
2. **Confirm-write workflows second** -- create tickets, draft docs, update low-risk records with human approval.
3. **Autonomous bounded workflows later** -- CI review agents, release summaries, scripted operational helpers.

This order matters because it lets the organization learn where the real value is before exposing higher-risk actions.

---

## The Model Context Protocol (MCP)

### The Problem Before MCP

Before MCP, connecting an AI model to an external system required writing custom integration code for every pairing of model and tool. If you wanted Claude to access Jira, you wrote a Jira integration for Claude. If you wanted ChatGPT to access Jira, you wrote a separate Jira integration for ChatGPT. If you wanted either of them to also access Confluence, that was two more custom integrations. This is the classic N-times-M problem: N AI clients times M external tools, each requiring bespoke glue code.

### The USB-C for AI

The **Model Context Protocol (MCP)** is an open standard that solves this problem. Think of it as the USB-C for AI: a single, universal connector that lets any compliant AI application talk to any compliant tool or data source.

Anthropic introduced MCP in November 2024, and it quickly gained traction across the industry. OpenAI adopted it in March 2025. By the end of 2025, Anthropic donated governance of MCP to the Agentic AI Foundation under the Linux Foundation, making it a vendor-neutral, community-governed standard. As of early 2026, the ecosystem includes over 5,800 registered MCP servers, more than 300 MCP clients, and 97 million monthly SDK downloads.

### How It Works

MCP defines three roles in its architecture:

- **Host**: The AI application you interact with (Claude Desktop, Cursor, VS Code with Copilot, or a custom agent). The host manages your session, presents results, and enforces security policies.
- **Client**: A protocol component inside the host that maintains a connection to a single MCP server. A host can run multiple clients simultaneously, each connected to a different server.
- **Server**: A lightweight process that wraps an external system (a file system, a database, a SaaS application) and exposes its capabilities in a standardized way.

When a host starts up with MCP configured, each client performs a handshake with its server. During this handshake, both sides declare what they support. The server advertises the tools it offers, the data resources it can provide, and any prompt templates it includes. The client declares its own capabilities, such as whether it can provide filesystem boundaries or handle requests for additional user input.

From that point on, the AI model can discover what tools are available, understand what each tool does through structured descriptions, and invoke them by generating properly formatted requests. The host intercepts these requests and can require your approval before anything executes.

The key insight is that an MCP server written once works with every MCP-compliant host. A Jira MCP server works with Claude Desktop, Cursor, a LangChain agent, and any future MCP client -- without modification.

---

## File System Access

One of the most common AI integrations is file system access. When an AI coding assistant reads your project files or writes a new configuration, it does so through explicit filesystem tools.

**How it works in practice**: The host application (or an MCP filesystem server) provides tools such as `read_file`, `write_file`, `list_directory`, and `search_files`. When the AI needs to understand your codebase, it calls `read_file` with a specific path. The host validates the path, reads the file, and returns the content to the model.

**Permission boundaries matter**. File system access is governed by several controls:

- **Root restrictions**: The host declares which directories the AI can access. An MCP filesystem server configured with root `/home/user/project` will refuse any request that resolves outside that directory, including attempts to traverse upward with `../` paths.
- **Read-only vs. read-write**: Many deployments grant broad read access but require explicit confirmation for writes. This "read-browse, confirm-write" pattern is standard in tools like Claude Code and most AI-powered IDEs.
- **Managed workspaces**: In automated pipelines, the AI may have full read-write access within a designated workspace directory and no access outside it.
- **Version control as a safety net**: When the AI writes to a git-tracked directory, every change is visible in `git diff` and can be reviewed before committing. This provides a built-in audit trail and easy rollback.

---

## Application Integration

AI systems connect to the applications your teams use every day. Here is how those connections work across the most common enterprise tools.

**Project management (Jira, Azure DevOps, Linear)**: MCP servers wrap these platforms' APIs to expose tools for creating issues, searching backlogs, updating statuses, and analyzing sprint data. The AI can create a well-structured Jira ticket from a natural language description, automatically populating fields like priority, labels, and components.

**Documentation (Confluence, Notion, Google Docs)**: AI integrations enable searching documentation, generating summaries of long pages, and creating or updating content. A common pattern is the AI reading a code change from GitHub and automatically updating the relevant Confluence documentation.

**Communication (Slack, Microsoft Teams)**: MCP servers for messaging platforms expose tools for posting messages, reading conversation history, and searching threads. Authentication uses the platform's OAuth with configurable scopes, so the AI only gets access to the channels and operations you authorize.

**Cloud infrastructure (Azure, AWS, GCP)**: The official Microsoft Azure MCP server supports over 40 Azure services. Amazon integrates MCP through Bedrock AgentCore. These integrations let AI assist with resource management, deployment monitoring, and infrastructure queries.

**Multi-tool orchestration** is where MCP's architecture truly shines. Because a single host can maintain simultaneous connections to multiple MCP servers, an AI can read a code diff from GitHub, create a Jira issue for the change, post a summary to Slack, and update a Confluence page -- all within a single conversation. Each connection is independent, each has its own permissions, and each action can be individually audited.

---

## API Access

When an AI calls an external API on your behalf, it follows one of several patterns:

**Direct API integration**: The host application calls an external REST API directly, using credentials stored in its environment (such as API tokens or OAuth secrets). The AI generates the call parameters; the host executes the actual HTTP request. This offers maximum control but requires custom code for each API.

**MCP server as API wrapper**: An MCP server wraps an external API and exposes it as a set of standardized tools. This is the most common pattern in the MCP ecosystem. The server handles authentication, rate limiting, and error handling internally, presenting a clean interface to the AI.

**Browser automation (last resort)**: For applications that lack APIs entirely, the AI can drive a web browser to interact with the UI. This approach is slow, fragile, and difficult to audit. Use it only when no API or MCP server exists for the target system.

In all cases, the AI model never holds API credentials directly. Credentials are managed by the host application or the MCP server, and the AI interacts only through the structured tool interface.

---

## Security and Permissions

This is the section that matters most for your organization. AI integrations introduce new access paths into your systems. Understanding the security model is essential.

### The Layered Permission Model

The permissions available to an AI system are determined by the **intersection** of multiple boundaries -- not the union. All of the following must permit an action for it to proceed:

1. **Protocol-level capabilities**: What the MCP server exposes and what the client negotiates during the handshake.
2. **Authentication scope**: What the OAuth token or API key authorizes.
3. **Host-level policies**: What the host application allows (such as requiring confirmation before writes).
4. **User consent**: What you explicitly approve at runtime.
5. **Enterprise policies**: Organizational rules enforced through gateways, DLP, and access controls.

An MCP server might expose a `delete_database` tool. But if the OAuth token lacks delete permissions, if the host requires your confirmation, and if the enterprise gateway blocks destructive operations entirely -- nothing gets deleted. Every layer is a checkpoint.

### Consent Models

Different use cases call for different consent approaches:

- **Per-action approval**: You confirm each tool invocation individually. Most secure, best for high-risk operations.
- **Session-level consent**: You approve a category of operations at session start ("allow all file reads in this project"). Common in IDE integrations.
- **Policy-based consent**: An administrator defines rules applied consistently without per-action prompting.
- **Autonomous mode**: The AI operates within predefined boundaries without per-action approval. Used in automated pipelines where oversight occurs at the pipeline level.

### Sandboxing

MCP supports isolation at multiple levels. Local servers run as child processes with the permissions of the launching application -- hosts can further restrict these through containers or filesystem namespaces. Remote servers are isolated by network boundaries, with access controlled through OAuth scopes. Cloud-based code execution environments use container isolation or WebAssembly sandboxing to ensure AI-generated operations cannot escape their designated boundaries.

### Audit Trails

Every AI tool invocation in a properly configured environment produces an audit record containing: the timestamp, user identity, session ID, model identity, server identity, tool name, arguments, result status, and latency. MCP gateways centralize this logging across all connected servers, providing a single audit stream that integrates with your existing SIEM infrastructure.

### Least Privilege in Practice

Applying least privilege to AI integrations means:

- **Start read-only**. Add write permissions only when justified by specific use cases.
- **Use short-lived tokens**. OAuth access tokens should expire in minutes, not hours. Refresh tokens should be rotated on use.
- **Scope narrowly**. A token for GitHub should access only the specific repositories needed, not your entire organization.
- **Revoke promptly**. Session-based permissions should expire when the conversation ends.
- **Monitor actively**. An AI suddenly querying tables it has never accessed before warrants investigation.

---

## Enterprise Integration Patterns

Organizations adopting AI integrations at scale typically follow a phased approach.

### The Gateway Pattern

Enterprise deployments route all MCP traffic through a central **MCP gateway**. This gateway acts as the chokepoint where authentication, authorization, DLP scanning, rate limiting, and audit logging are enforced uniformly -- regardless of which client or server is involved. This is architecturally similar to an API gateway for microservices, and for good reason: the same principles apply.

### Network Architecture

Where AI services sit in your network depends on data sensitivity. The most common pattern uses cloud-hosted AI models with controlled egress: MCP servers run inside the enterprise network, connecting to internal systems (databases, Jira, file shares), while a gateway and firewall control what data leaves the network to reach the cloud AI provider. For highly regulated environments, the model runs on-premises with no external data flow. Many organizations adopt a hybrid approach, routing non-sensitive operations to cloud AI for maximum capability while processing restricted data exclusively on local models.

### Phased Adoption

A practical enterprise rollout looks like this:

1. **Discovery** (weeks 1-4): Inventory existing AI usage, including shadow AI. Map data flows. Classify data sensitivity. Identify low-risk, high-value quick wins.
2. **Foundation** (weeks 5-12): Deploy an MCP gateway with authentication tied to your identity provider. Define role-based access policies. Configure DLP rules and audit logging.
3. **Controlled expansion** (weeks 13-24): Deploy approved MCP servers, starting with read-only access to low-sensitivity systems. Enable team-specific integrations through a formal approval workflow. Build custom MCP servers for internal tools.
4. **Maturation** (ongoing): Automate governance with policy-based authorization. Expand to multi-agent architectures. Allow teams to publish their own MCP servers to an internal registry with automated security scanning.

### Managing Shadow AI

One of the biggest governance challenges is employees using unauthorized AI tools and sending corporate data to uncontrolled services. The best defense is providing a well-governed, sanctioned AI platform that meets employee needs -- combined with network-level controls, DLP scanning of outbound traffic, clear acceptable-use policies, and education about the risks.

---

## Key Takeaways

- AI accesses external systems through **defined tools and protocols**, not through any mysterious or unbounded capability. Every action can be inspected, controlled, and audited.
- The **Model Context Protocol (MCP)** is the emerging universal standard for connecting AI to tools and data. It eliminates the need for custom integrations between every AI client and every external system.
- File system access, application integration, and API calls all follow the same pattern: the AI proposes an action, and the host application decides whether to execute it based on permissions and policies.
- Security is enforced through **layered controls**: protocol capabilities, OAuth scopes, host policies, user consent, and enterprise governance. The effective permission is the intersection of all layers.
- **Least privilege, short-lived tokens, audit trails, and DLP scanning** are not optional extras -- they are foundational requirements for enterprise AI integration.
- Organizations should adopt AI integrations through a **phased approach**: start with discovery and governance foundations before expanding access.

If the system moves beyond simple read-only assistance, pair this module with the appendices on [AI Observability and Operations](22-AI-Observability-And-Operations.md) and [AI Security and Guardrails](23-AI-Security-And-Guardrails.md). Tool access becomes much safer when teams can trace behavior and reason about prompt injection, tool abuse, and action gating explicitly.

---

## Try This

> **Exercise 1 -- Explore the MCP Ecosystem**: Visit the MCP server registry and browse the available servers for tools your team uses (Jira, Confluence, Slack, Azure DevOps). Note what tools each server exposes and what permissions it requires. This gives you a concrete sense of what AI integration looks like in practice.
>
> **Exercise 2 -- Map Your Integration Surface**: Pick three applications your team uses daily. For each, document: (a) Does an MCP server exist for it? (b) What data would the AI need access to? (c) What is the sensitivity classification of that data? (d) What permission model would you recommend (read-only, confirm-write, policy-based)? This exercise builds the foundation for an integration strategy.
>
> **Exercise 3 -- Design an Audit Policy**: Draft a one-page audit logging policy for AI tool invocations in your organization. Define what fields should be logged, how long logs should be retained, who should have access, and what events should trigger alerts. Compare your policy against the audit requirements in your existing compliance framework (SOC 2, ISO 27001, HIPAA, or whichever applies).

---
