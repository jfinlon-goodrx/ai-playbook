---
title: "Skills & Tools"
status: first-draft
include_in_compile: true
synopsis: "What skills are in AI systems, tool use, and function calling patterns."
---

# Module 10: Skills & Tools

> **Learning Objectives**
> By the end of this module, you will be able to:
> - Explain why LLMs need tools to be useful beyond generating text
> - Describe the three-phase function calling pattern and how it works in practice
> - Distinguish between built-in tools, custom tools, and reusable skills
> - Identify real-world scenarios where tool use transforms AI from a novelty into a productivity multiplier
> - Understand how the Model Context Protocol (MCP) standardizes tool integration across platforms

---

## Why AI Needs Tools

Large language models are remarkably good at language. They can summarize a report, draft an email, explain a concept, or brainstorm solutions. But language is all they have. On their own, LLMs cannot check your calendar, query a database, look up a customer record, send a Slack message, or create a Jira ticket. They have no hands, no network access, no ability to reach out and touch the systems you work with every day.

This is a fundamental limitation, not a flaw. LLMs were trained on text. They generate text. Everything they "know" comes from patterns in their training data, which has a cutoff date and contains no information about your specific environment. Ask a standalone LLM what meetings you have tomorrow and it will either hallucinate an answer or honestly tell you it has no idea.

Tools are the bridge between what an LLM knows and what it can do. When you give an AI model access to tools, you transform it from a passive text generator into an active participant in your workflows. A model with calendar access can check your availability. A model with database access can look up that customer record. A model with API access can file that support ticket, send that notification, or update that spreadsheet.

Think of it this way: a brilliant new hire who speaks every language fluently but has no computer, no phone, and no access to any of your company's systems is still limited in what they can accomplish. Give them the right access and suddenly their intelligence becomes operational. Tools do the same thing for AI.

---

## Why This Matters So Much For Software Teams

For a developer or technical lead, tools are the difference between:

- a chatbot that gives general suggestions,
- and an assistant that can inspect a repository, read a diff, run tests, search documentation, draft a Jira ticket, or summarize a pull request.

This is also where many teams first encounter the shift from “AI as helpful text generation” to “AI as a workflow participant.” Once tools enter the picture, the conversation changes from output quality alone to:

- permission boundaries,
- auditability,
- reliability,
- reusable workflows,
- and when to trust automation versus when to require review.

That is why this module sits on the path toward MCP rather than standing apart from it.

---

## Function Calling Explained

Function calling is the mechanism that makes tool use possible. It is how an AI model says, "I need to do something outside of generating text, and here is exactly what I need done."

The process follows a consistent three-phase pattern across all major AI providers, even though the specific API formats differ.

### Phase 1: Define the Available Tools

Before the AI can use a tool, someone has to tell it what tools exist. Each tool definition includes a name, a description of what it does, and a specification of what inputs it expects. For example, a weather tool might be defined as: name is "get_weather," description is "retrieves the current weather for a given city," and it expects a city name as input.

These definitions are provided to the model alongside the user's message. The model reads the descriptions to understand what tools are at its disposal and when each one would be appropriate to use. The quality of these descriptions matters enormously. A vague description leads to a model that uses tools at the wrong time or with the wrong inputs. A clear, specific description leads to reliable tool use.

### Phase 2: The AI Requests a Tool Call

When the model determines that it needs to use a tool to answer the user's question, it does not execute the tool itself. Instead, it generates a structured request that says, in effect, "please call the get_weather function with the city parameter set to Seattle." This request comes back as structured data, not as conversational text.

This is a critical design choice. The model never directly touches your systems. It expresses intent, and the host application decides whether and how to execute that intent. This separation is what makes tool use safe to deploy in production environments.

### Phase 3: Execute and Return Results

The host application receives the model's tool call request, validates it, executes the corresponding function, and sends the result back to the model. The model then incorporates that result into its response. If it needs more information, it can request another tool call, creating a loop of reasoning and action until it has everything it needs to give the user a complete answer.

### The Manager Analogy

The best way to think about function calling is as a delegation pattern. The AI model acts like a manager who is great at understanding problems and knowing what needs to happen, but delegates the actual execution to specialists. The manager says, "We need the Q3 sales figures from the database," and the database specialist runs the query and brings back the numbers. The manager then uses those numbers to complete the report.

The model reasons. The tools act. The model incorporates the results. This cycle can repeat as many times as needed to complete a task, with the model chaining together multiple tool calls when a task requires it. Ask it to "find all overdue tickets assigned to my team and send a summary to our Slack channel," and the model might call a Jira search tool, process the results, and then call a Slack messaging tool, all in sequence.

---

## Skills as Reusable Workflows

If tools are individual capabilities, skills are pre-packaged workflows that combine reasoning, tool use, and domain knowledge into a single, reusable action. Skills are to AI what macros are to spreadsheets or stored procedures are to databases: a way to encapsulate a complex, multi-step process behind a simple trigger.

Consider the `/commit` command in Claude Code. When you invoke it, the AI does not just run a single tool. It examines your staged changes, reads recent commit history to match the repository's style, analyzes the nature of the modifications, drafts an appropriate commit message, and creates the commit. That entire workflow, from analysis through execution, is a skill. You trigger it with one command, and the AI handles the rest.

GitHub Copilot's code review skill works similarly. You ask for a review, and the skill orchestrates a multi-step process: reading the changed files, understanding the context, identifying potential issues, and producing structured feedback. It is not a single tool call. It is a choreographed sequence of reasoning and tool use that has been designed, tested, and packaged for reuse.

Skills matter because they make AI assistants practical for everyday work. Without skills, you would need to manually guide the AI through every step of a complex task. With skills, you get one-command access to workflows that would otherwise require multiple prompts and significant back-and-forth. They represent the difference between having a capable assistant and having a capable assistant who already knows your standard operating procedures.

For engineering teams, good skills often encode workflows such as:

- refactor safely without changing behavior,
- review a pull request for risk rather than style noise,
- generate documentation from real code instead of guesswork,
- plan a migration in reversible steps,
- or analyze a deployment before it goes live.

This is why skills should be treated as reusable process assets, not just product features.

---

## Built-in vs. Custom Tools

AI platforms ship with a set of built-in tools, and most allow you to extend that set with custom tools tailored to your environment.

### Built-in Tools

Built-in tools are the capabilities that come standard with an AI platform. They vary by product, but common examples include:

- **Web search** -- The model can search the internet for current information, overcoming the limitations of its training data cutoff.
- **Code execution** -- The model can write and run code in a sandboxed environment, useful for data analysis, calculations, and file transformations.
- **File operations** -- Reading, writing, searching, and editing files in a designated workspace.
- **Image generation and analysis** -- Creating images from descriptions or analyzing the content of uploaded images.
- **Browser control** -- Navigating websites, filling forms, and extracting information from web pages.

These built-in tools are maintained by the platform provider, work reliably out of the box, and require no setup from the user. They cover the most common use cases and are a good starting point for understanding what tool-equipped AI can do.

### Custom Tools

Custom tools extend an AI model's reach into your specific systems and workflows. They are functions that you define and host, tailored to your organization's needs. A custom tool might query your internal knowledge base, create records in your CRM, check inventory in your warehouse management system, or trigger a deployment pipeline.

Building a custom tool involves three things: writing the function that performs the action, defining the schema that tells the model what inputs the tool expects, and providing a description that helps the model understand when to use it. The function itself can be anything that your application can execute, from a simple API call to a complex multi-step process.

The power of custom tools is that they let you connect AI to the systems that matter most to your organization. The built-in tools handle general capabilities. Custom tools handle your specific domain.

---

## Real-World Tool Use Examples

Tool use is most compelling when you see it applied to real work. Here are scenarios that illustrate how tools transform AI from interesting to indispensable.

**Searching internal documentation.** A support engineer asks the AI, "What is our policy on refunds for enterprise customers?" The model calls a document search tool, retrieves the relevant sections from the company's policy documentation, and provides an accurate, sourced answer. Without the tool, the model would either hallucinate a policy or admit it does not know.

**Creating and managing Jira tickets.** A project manager describes a bug during a conversation with the AI. The model calls a Jira tool to create a ticket with the appropriate project, priority, labels, and description, all extracted from the conversation. It can also search for existing tickets to avoid duplicates and link related issues.

**Querying databases.** A business analyst asks, "How many new customers signed up last month compared to the month before?" The model formulates a SQL query, calls a database tool to execute it, receives the results, and presents a clear comparison with the percentage change calculated.

**Reading and analyzing spreadsheets.** A finance team member uploads a budget spreadsheet and asks for a variance analysis. The model uses a file reading tool to ingest the data, a code execution tool to compute variances, and then produces a summary with the largest discrepancies highlighted.

**Sending notifications.** At the end of a weekly review, the AI sends a summary of action items to the team's Slack channel and emails individual task owners their assignments, using messaging and email tools in sequence.

**Monitoring and alerting.** An operations team configures an AI agent to periodically check system health dashboards using an API tool. When metrics exceed thresholds, the agent creates an incident ticket and notifies the on-call engineer.

In each case, the pattern is the same: the model understands the request, identifies which tools it needs, calls them with the right parameters, and weaves the results into a useful response. The tools provide the reach. The model provides the judgment.

---

## How This Connects to MCP

As tool use has grown in importance, a practical problem has emerged: every AI platform implements tools differently. A tool built for ChatGPT does not work with Claude. A tool built for Claude does not work with Gemini. If your organization invests in building custom tools, you risk locking those tools to a single platform.

The Model Context Protocol (MCP) addresses this problem directly. MCP is an open standard, originally introduced by Anthropic and now governed by the Linux Foundation, that defines a universal way for AI applications to discover and use tools. Think of MCP as a USB-C port for AI: one standard connection that works across devices and vendors.

With MCP, a tool provider builds an MCP server once, and any MCP-compatible AI client can use it. The same MCP server that gives Claude access to your PostgreSQL database can also give Copilot or a custom LangChain agent access to that same database, with no additional integration work.

For technical organizations, that portability matters because it allows the team to standardize the integration surface without standardizing every person on one AI host forever. It also makes governance easier: one approved server can serve multiple compliant clients.

We will explore MCP in depth in the next module, including how it works, how to use existing MCP servers, and what the growing ecosystem of thousands of available servers means for your organization.

---

## Key Takeaways

- **Tools make AI operational.** Without tools, LLMs can only generate text based on training data. With tools, they can interact with real-world systems, access current information, and take meaningful actions.
- **Function calling is a delegation pattern.** The AI decides what needs to happen and specifies the parameters. The host application executes the action and returns results. The model never directly touches your systems.
- **Skills bundle complexity into simplicity.** Pre-packaged workflows like code review or commit creation turn multi-step processes into single commands, making AI assistants practical for daily work.
- **Built-in tools cover the basics; custom tools cover your domain.** Every major platform ships useful general-purpose tools, but the real productivity gains come from connecting AI to your organization's specific systems.
- **MCP is standardizing tool integration.** The Model Context Protocol eliminates vendor lock-in for tools by providing a universal interface that works across AI platforms.

---

## Try This

1. **Inventory your daily tools.** Make a list of five systems you interact with regularly (email, ticketing, databases, wikis, messaging). For each one, write a brief description of how an AI tool integration could save you time. Which would deliver the most value?

2. **Trace a function call.** The next time you use an AI assistant that calls a tool (web search, code execution, file reading), pay attention to the interaction. Can you identify the three phases: the tool being selected, the action being executed, and the result being incorporated? Many AI interfaces surface this information in their UI.

3. **Design a custom tool on paper.** Pick one repetitive task in your workflow. Write down what the tool would be called, what inputs it would need, and what it would return. You do not need to build it yet. The exercise of thinking through the interface is valuable on its own, and you will have the knowledge to implement it after completing the MCP module.

---
