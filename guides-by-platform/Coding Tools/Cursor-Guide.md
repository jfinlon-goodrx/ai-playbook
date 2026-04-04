# Cursor — The Complete Guide

> **A comprehensive reference for getting the most out of Cursor, the AI-powered code editor.**
> Last updated: 2026-02-24

---

## Table of Contents

- [[#Quick Start]]
- [[#Cheat Sheet]]
- [[#Core AI Features]]
- [[#Tab Completion]]
- [[#Inline Edit (Cmd+K)]]
- [[#Sidebar Chat (Cmd+L)]]
- [[#Agent Mode (Composer)]]
- [[#Background Agent]]
- [[#@ Context Mentions]]
- [[#Rules & Configuration]]
- [[#Models]]
- [[#MCP Servers]]
- [[#Codebase Indexing]]
- [[#Docs Indexing (@Docs)]]
- [[#Terminal Integration]]
- [[#Multi-File Editing & Diffs]]
- [[#Bugbot (Code Review)]]
- [[#Privacy & Security]]
- [[#Settings Reference]]
- [[#The .cursor Directory]]
- [[#Keyboard Shortcuts]]
- [[#Pricing]]
- [[#Cursor vs GitHub Copilot]]
- [[#Best Practices & Tips]]

---

## Quick Start

1. Download from [cursor.com](https://cursor.com)
2. Import your VS Code settings (prompted on first launch)
3. Open a project folder
4. Start coding — Tab suggestions appear automatically
5. Press `Cmd+L` to chat, `Cmd+K` to edit inline, `Cmd+Shift+A` for Agent

---

## Cheat Sheet

> [!tip] Pin this section for daily reference.

### Core Shortcuts

| What you want | macOS | Windows/Linux |
|---|---|---|
| Open sidebar chat | `Cmd+L` | `Ctrl+L` |
| Chat with selection | `Cmd+Shift+L` | `Ctrl+Shift+L` |
| Inline edit | `Cmd+K` | `Ctrl+K` |
| Open Agent mode | `Cmd+Shift+A` | `Ctrl+Shift+A` |
| Quick Agent toggle | `Cmd+.` | `Ctrl+.` |
| Background Agent panel | `Cmd+B` | `Ctrl+Shift+B` |
| Accept all diffs | `Cmd+Enter` | `Ctrl+Enter` |
| Reject changes | `Esc` | `Esc` |
| Accept Tab suggestion | `Tab` | `Tab` |
| Accept word-by-word | `Ctrl+→` | `Ctrl+→` |
| Reject Tab suggestion | `Esc` | `Esc` |
| Terminal AI command | `Cmd+K` (in terminal) | `Ctrl+K` (in terminal) |
| Execute terminal cmd | `Cmd+Enter` | `Ctrl+Enter` |
| Open settings | `Cmd+Shift+J` | `Ctrl+Shift+J` |

### Everyday Workflows

| Task | How |
|---|---|
| Ask about code | Select code → `Cmd+L` |
| Quick edit | Select code → `Cmd+K` → describe change |
| Multi-file feature | `Cmd+Shift+A` → describe feature |
| Reference a file | Type `@filename` in chat |
| Reference docs | Type `@Docs` → select library |
| Generate terminal command | `Cmd+K` in terminal → describe what you need |
| Review a PR | Bugbot runs automatically (if enabled) |
| Run parallel tasks | Open Background Agent panel → spawn agents |

### Key @ Mentions

| Mention | Purpose |
|---|---|
| `@Files & Folders` | Attach specific files/folders as context |
| `@Code` | Reference specific symbols/snippets |
| `@Docs` | Reference indexed documentation |
| `@Past Chats` | Pull in previous conversation context |
| `@Recommended` | Auto-pull relevant context (Agent mode) |
| `@Cursor Rules` | Manually invoke specific rules |

---

## Core AI Features

Cursor has four main AI interaction modes, each suited to different tasks:

| Mode | Shortcut | Best For |
|---|---|---|
| **Tab Completion** | `Tab` | Line-by-line coding, auto-complete |
| **Inline Edit** | `Cmd+K` | Quick, targeted code changes |
| **Sidebar Chat** | `Cmd+L` | Questions, explanations, exploration |
| **Agent Mode** | `Cmd+Shift+A` | Multi-file features, autonomous coding |

```
               Autonomy Level
Tab ──── Cmd+K ──── Cmd+L ──── Agent
Low                                High
```

---

## Tab Completion

Cursor Tab goes beyond traditional autocomplete — it predicts multi-line completions based on your entire project context.

### How It Works

- Analyzes your current file, open tabs, project structure, and recent edits
- Predicts not just the next line, but entire functions, loops, and logic blocks
- Shows suggestions as ghost text (simple) or diff popups (complex)
- Predicts **where you'll edit next** and pre-fills suggestions

### Controls

| Action | Shortcut |
|---|---|
| Accept full suggestion | `Tab` |
| Accept word-by-word | `Ctrl+→` |
| Reject suggestion | `Esc` |

### Configuration

- **Toggle on/off**: Hover over "Cursor Tab" in the status bar (bottom-right)
- **Disable in comments**: Settings → Tab Completion → uncheck "Trigger in comments"
- **Remap keys**: Settings → Keybindings → search "Cursor Tab"

### Tips for Better Suggestions

- Write clear comments describing intent before the code
- Use explicit type annotations
- Keep related files open in tabs
- Maintain consistent code patterns
- Configure `.cursor/rules/` for project conventions

---

## Inline Edit (Cmd+K)

Direct code editing through a small prompt box in the editor.

### Workflow

1. **Select code** in the editor (or place cursor where you want to insert)
2. Press `Cmd+K` / `Ctrl+K`
3. **Describe** your edit in plain English
4. **Review** the inline diff (green = additions, red = deletions)
5. Press `Cmd+Enter` to accept or `Esc` to reject

### Best For

- Refactoring a specific function
- Adding error handling to selected code
- Converting between patterns (e.g., callback → async/await)
- Quick fixes and transformations

### Limitations

- Context window: ~10,000 tokens
- Does not support image pasting (use Chat instead)
- Single-file scope (use Agent for multi-file)

---

## Sidebar Chat (Cmd+L)

A conversational AI assistant in the sidebar for questions, explanations, and guided code suggestions.

### How to Use

1. Press `Cmd+L` / `Ctrl+L` to open
2. Type your question or instruction
3. Use `@` mentions to attach context
4. Review response — accept code suggestions or continue conversation

### Chat with Selection

1. Select code in the editor
2. Press `Cmd+Shift+L` / `Ctrl+Shift+L`
3. Code is automatically included as context
4. Ask your question about the selected code

### Best For

- Understanding unfamiliar code
- Exploring approaches before implementing
- Getting explanations of complex logic
- Asking architecture questions with file context

### Context Window

- ~20,000 tokens in standard mode
- Up to 200,000 tokens in normal mode
- Up to 1,000,000+ tokens in Max Mode (20% cost upcharge)

---

## Agent Mode (Composer)

The most powerful mode — an autonomous AI agent that can edit multiple files, run terminal commands, search your codebase, and iterate on solutions.

### Opening Agent Mode

| Method | Shortcut |
|---|---|
| Full Agent mode | `Cmd+Shift+A` / `Ctrl+Shift+A` |
| Quick toggle | `Cmd+.` / `Ctrl+.` |
| From settings | Toggle "Agent" switch in Composer window |

### Capabilities

- Create and edit files across your entire project
- Execute terminal commands (up to 25 tool calls per run)
- Search and understand your codebase semantically
- Install dependencies
- Run tests and iterate until passing
- Make autonomous decisions with reasoning
- Git operations

### YOLO Mode

> [!warning] Use with caution — Agent executes commands without asking.

Enable: **Settings → Features → Chat & Composer → YOLO Mode**

When enabled, Agent can:
- Execute terminal commands automatically
- Delete and create files without confirmation
- Install packages
- Run tests

### Checkpoint System

Every Agent iteration creates a checkpoint:
- View checkpoints in the Composer panel
- Revert to any previous checkpoint
- Each checkpoint is a complete project state
- Safe to experiment — you can always roll back

### Plans

Agent can generate and save plans:
1. Ask Agent to plan before implementing
2. Click **"Save to workspace"**
3. Plans stored in `.cursor/plans/`
4. Resume interrupted work later
5. Plans are version-controlled and shareable

### Best Workflow

```
1. Describe task clearly
2. Ask Agent to plan first
3. Review the plan
4. Let Agent implement
5. Review diffs per-file
6. Accept or revert via checkpoints
7. Run tests to verify
```

---

## Background Agent

Cloud-based autonomous agent that works separately from your local machine.

> [!info] Requires Cursor Pro plan ($20/month minimum).

### How It Works

1. Open panel: `Cmd+B` / `Ctrl+Shift+B`
2. Click **"New Agent"**
3. Describe the task
4. Select model and branch
5. Agent clones your repo in the cloud
6. Works autonomously — creates a PR when done
7. You review and merge

### Key Advantages

- **Parallel execution**: Run multiple agents on different tasks simultaneously
- **Non-blocking**: Continue working locally while agents run in the cloud
- **Isolated**: Each agent works on its own branch
- **PR-based**: All changes come as reviewable pull requests

### Monitoring

- View agent list and status in the Background Agent panel
- Inspect reasoning and to-do list
- Watch `workflow_state.md` for detailed progress

---

## @ Context Mentions

Control what context the AI sees by using `@` mentions in chat and Agent mode.

### Active Mentions (Cursor 2.0)

| Mention | What It Does |
|---|---|
| `@Files & Folders` | Attach specific files or entire folders |
| `@Code` | Reference specific code symbols/snippets |
| `@Docs` | Access indexed documentation |
| `@Past Chats` | Pull in previous conversations |
| `@Recommended` | Auto-pull relevant context (Agent mode) |
| `@Cursor Rules` | Invoke specific project rules |

### Usage Tips

- Navigate suggestions with arrow keys, press Enter to select
- For folders, press `/` to explore subdirectories
- Drag and drop files directly into chat
- Combine multiple `@` mentions for precise context

### Deprecated Mentions (Removed in 2.0)

These were removed because **Agent now self-gathers context automatically**:

| Removed | Replacement |
|---|---|
| `@Web` | Agent searches the web when needed |
| `@Linter Errors` | Agent detects linting issues automatically |
| `@Git` | Agent accesses git history automatically |
| `@Codebase` | Agent indexes and searches automatically |

---

## Rules & Configuration

Rules provide persistent instructions that guide AI behavior across all interactions.

### Rule Types

| Type | Location | Scope | Shared? |
|---|---|---|---|
| **Project Rules** | `.cursor/rules/*.md` | Per-project | Yes (via git) |
| **User Rules** | Cursor Settings → Rules | All projects | No |
| **Team Rules** | Cursor Dashboard (admin) | Organization | Yes |
| **AGENTS.md** | Project root | Per-project | Yes (via git) |

### Creating Project Rules

**Via UI:**
1. `Cmd+Shift+P` → "New Cursor Rule"
2. Select template or create blank

**Manually:**
```bash
mkdir -p .cursor/rules
```

### Rule Format (MDC — Markdown with Code)

```markdown
---
title: React Components
patterns:
  - src/components/**/*.tsx
apply: intelligently
---

# React Component Standards

1. Use functional components with hooks
2. Extract custom hooks to hooks/ directory
3. Props must be typed with interfaces, not type aliases

Example:
@component-template.tsx
```

### Application Types

| Type | Behavior | Use Case |
|---|---|---|
| `always` | Included in every session | Critical standards, security rules |
| `intelligently` | AI decides if relevant | General best practices |
| `file-pattern` | Matched via gitignore patterns | File-type-specific rules |
| `manual` | Only when invoked with `@` | On-demand reference |

### Best Practices for Rules

- Keep individual rules under 500 lines
- Split large rules into composable pieces
- Provide concrete code examples (use `@filename.tsx`)
- Be specific and actionable — avoid vague guidance
- Don't duplicate what linters already enforce
- Use path patterns for file-specific rules

### Legacy .cursorrules

> [!warning] `.cursorrules` in the project root is deprecated. Migrate to `.cursor/rules/` or `AGENTS.md`.

It still works for backward compatibility, but new projects should use the rules directory.

### Example Rules

**TypeScript Standards** (`.cursor/rules/typescript.md`):
```markdown
---
title: TypeScript Patterns
patterns:
  - src/**/*.ts
  - src/**/*.tsx
apply: always
---

# TypeScript Standards
- Use explicit return types on all exported functions
- Prefer interfaces over type aliases for object shapes
- Use strict mode
- Never use `any` — use `unknown` with type guards
```

**API Layer** (`.cursor/rules/api.md`):
```markdown
---
title: API Conventions
patterns:
  - src/api/**/*
apply: always
---

# API Conventions
- All endpoints return { data, error, meta } envelope
- Use zod for request validation
- Errors use RFC 7807 problem details format
- Always include request ID in responses
```

**Testing** (`.cursor/rules/testing.md`):
```markdown
---
title: Testing Rules
patterns:
  - **/*.test.ts
  - **/*.spec.ts
apply: always
---

# Testing Standards
- Use vitest, not jest
- Use describe/it pattern
- Mock external services, never databases
- Each test file mirrors source file structure
```

---

## Models

### Available Models

**Anthropic Claude:**
- Claude 4.6 Opus — complex reasoning, architecture
- Claude 4.6 Sonnet — balanced daily use
- Claude 4.5 Haiku — fast, simple tasks

**OpenAI:**
- GPT-5 — general purpose
- GPT-5 Fast — speed-optimized
- GPT-5 Mini — lightweight
- GPT-5.x Codex — code-specialized

**Google Gemini:**
- Gemini 3.1 Pro — large context (1M tokens)
- Gemini 3 Flash — fast and cheap

**Cursor Proprietary:**
- Composer 1 / 1.5 — optimized for multi-file IDE editing (~2x faster than Sonnet)

**xAI:**
- Grok Code

### Switching Models

- Click the **model selector dropdown** at the top of any chat/composer window
- Change model mid-conversation
- Unhide additional models: **Settings → Models** → check boxes

### Model Selection Guide

| Task | Recommended Model |
|---|---|
| Daily coding, features | Composer 1.5 or Claude Sonnet |
| Complex architecture | Claude Opus |
| Quick fixes, small edits | Haiku or GPT-5 Mini |
| Large codebase analysis | Gemini 3.1 Pro (1M context) |
| Multi-file refactoring | Composer 1.5 |
| Cost-sensitive work | Gemini Flash or Haiku |

### Max Mode

Extended context window (up to 1M+ tokens for some models).

- **Cost**: 20% upcharge on token prices
- **When to use**: Large codebase analysis, project-wide refactoring, broad architectural questions
- **Enable**: Toggle per-conversation in the chat window

---

## MCP Servers

Model Context Protocol (MCP) servers extend Cursor's Agent with external tools and data sources.

### Configuration Locations

| Scope | File |
|---|---|
| Global | `~/.cursor/mcp.json` |
| Project | `.cursor/mcp.json` |

### Configuration Format

```json
{
  "mcpServers": {
    "database-tools": {
      "command": "node",
      "args": ["./mcp-servers/database.js"],
      "env": {
        "DB_URL": "postgresql://localhost:5432/mydb",
        "API_KEY": "your-api-key"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_..."
      }
    }
  }
}
```

### How Agent Uses MCP Tools

1. Configure servers in `.cursor/mcp.json`
2. Check "Available Tools" in Settings → MCP
3. Agent automatically uses relevant tools when needed
4. No manual invocation required

### Limitations

- Maximum **40 tools** visible to Agent (only first 40 sent)
- **Stdio and network** connections only (no SSH)
- Tools only (Resources not yet supported)
- Works with local machine or network

---

## Codebase Indexing

Cursor automatically indexes your codebase for semantic search and context-aware suggestions.

### How It Works

1. **Local chunking** — files split into semantic chunks locally
2. **Merkle tree** — hash tree computed for change detection
3. **Embeddings** — chunks sent to Cursor servers for embedding
4. **Continuous sync** — checks every 5 minutes for changes

### Configuration

**Exclude files from indexing** — create `.cursorignore` (gitignore syntax):

```gitignore
node_modules/
.git/
build/
dist/
*.log
*.min.js
private/**
vendor/
```

Also respects `.gitignore` and `.git/info/exclude`.

**View indexed files**: Settings → Indexing & Docs → "View included files"

### Security

- Embeddings created **without filenames or source code**
- Filenames obfuscated in transit
- Code chunks encrypted with client-generated keys
- Keys exist on server only for request duration
- All cached content is temporary
- Never used for training

---

## Docs Indexing (@Docs)

Index external documentation so AI can reference it accurately.

### Adding Documentation

1. **Settings → Indexing & Docs**
2. Click **"Add Documentation"**
3. Enter URL or upload file
4. Name the documentation
5. Wait for the book icon to confirm indexing

### Using in Chat

```
@Docs Next.js — how do I set up middleware?
@Docs Stripe — create a subscription checkout flow
```

### Supported Formats

- Markdown (.md)
- HTML (from URLs)
- JSON
- Plain text

### Team Sharing

On the Teams plan, toggle **"Share with team"** in Settings → Indexing & Docs to make indexed docs available to all members.

### Tips

- Index all official libraries your project uses
- Index internal READMEs, wikis, and API docs
- Update indexed docs when library versions change
- Reduces hallucinations about API usage

---

## Terminal Integration

### Terminal Cmd+K

AI-powered command generation directly in the integrated terminal.

**Workflow:**
1. Click in the terminal
2. Press `Cmd+K` / `Ctrl+K`
3. Describe what you want in plain English
4. Review the generated command
5. Press `Cmd+Enter` / `Ctrl+Enter` to execute

**Context provided**: Recent terminal history, current directory, your description.

**Examples:**
```
"Find all TypeScript files modified today"
"Create a new migration for adding user roles"
"Generate a commit message for staged changes"
"Install React with TypeScript support"
```

### Agent Terminal Access

In Agent mode, the AI can:
- View recent terminal output
- Execute commands autonomously
- Chain commands with code changes
- Run tests and iterate

---

## Multi-File Editing & Diffs

### How Diffs Work

For each modified file, Cursor shows:
- **Green lines** — additions
- **Red lines** — deletions
- **Gray lines** — unchanged context

### Accept/Reject Workflow

| Action | How |
|---|---|
| Accept all changes | `Cmd+Enter` / `Ctrl+Enter` or green checkmark |
| Reject all changes | `Esc` or red X |
| Review per-file | Navigate files in Composer panel |
| Edit before accepting | Modify lines in diff view, then accept |
| Revert to checkpoint | Click checkpoint in Composer panel |

### Checkpoint System

- Every Agent iteration creates a checkpoint
- View all checkpoints in the Composer panel
- Revert to any previous state instantly
- Each checkpoint is a complete project snapshot
- Safe to experiment freely

### Multi-File Coordination

Agent ensures:
- Import statements stay in sync
- Type definitions match across files
- No circular dependencies introduced
- Consistent naming conventions
- Architecture patterns maintained

---

## Bugbot (Code Review)

AI-powered code review that finds real bugs in pull requests.

### What It Does

- Detects **logic bugs**, not just style issues
- Reviews the entire PR impact, not just changed lines
- Understands how changes interact with existing code
- Especially strong at reviewing AI-generated code
- Low false positive rate

### Performance

- Returns ~40% of time spent on code reviews
- 70%+ of flagged issues resolved before merge

### Setup

1. **Settings → Features → Code Review**
2. Enable Bugbot
3. Runs automatically on new PRs

### Autofix

Configure Bugbot to automatically resolve issues:
- Uses Background Agent to create follow-up fixes
- Reduces manual review overhead

---

## Privacy & Security

### Privacy Modes

| Mode | Data Retention | Best For |
|---|---|---|
| **Full Privacy Mode** | Zero (via Baseten/Together) | Sensitive/proprietary code |
| **Standard** | Temporary caching only | General use |

Enable: **Settings → Account → Privacy Mode**

### Data Handling

- **Zero data retention agreements** with OpenAI, Anthropic, Baseten, Together
- File contents **temporarily cached** (encrypted, ephemeral)
- Encryption keys exist only for request duration
- **Never used for training** in any mode
- **Never permanently stored**

### Certifications

- **SOC 2 Type II** certified
- Annual recertification
- Active bug bounty program
- Security incidents acknowledged within 5 business days

### Full Privacy Mode Trade-offs

When enabled:
- Background Agent disabled
- Some caching features disabled
- May have slightly higher latency

---

## Settings Reference

Open settings: `Cmd+Shift+J` / `Ctrl+Shift+J`

### Key Settings Areas

| Section | What It Controls |
|---|---|
| **General** | Theme, font, editor layout |
| **Models** | Show/hide models, set defaults |
| **Tab Completion** | Enable/disable, trigger in comments, keybindings |
| **Chat & Composer** | Agent mode, YOLO mode, chat behavior |
| **Indexing & Docs** | Codebase indexing, custom docs, file inclusion |
| **Rules** | User-level rules, enable/disable rules |
| **Privacy** | Full Privacy Mode, encryption |
| **Keybindings** | Customize all keyboard shortcuts |
| **MCP** | Configure MCP servers and tools |

### Settings Storage

Cursor stores settings in SQLite (not plain JSON like VS Code):

| Platform | Path |
|---|---|
| macOS | `~/Library/Application Support/Cursor/User/globalStorage/state.vscdb` |
| Windows | `%APPDATA%\Cursor\User\globalStorage\state.vscdb` |
| Linux | `~/.config/Cursor/User/globalStorage/state.vscdb` |

---

## The .cursor Directory

```
.cursor/
├── rules/                   # Project rules (composable, version-controlled)
│   ├── typescript.md
│   ├── react-components.md
│   ├── api-conventions.md
│   └── testing.md
├── mcp.json                 # Project-level MCP server configs
└── plans/                   # Saved Agent plans
    ├── feature-auth.md
    └── refactor-api.md
```

### Other Project-Root Files

| File | Purpose | Status |
|---|---|---|
| `.cursorignore` | Exclude files from indexing (gitignore syntax) | Active |
| `AGENTS.md` | Simple markdown rules (alternative to `.cursor/rules/`) | Active |
| `.cursorrules` | Legacy project rules | **Deprecated** |

---

## Keyboard Shortcuts

### AI Commands

| Action | macOS | Windows/Linux |
|---|---|---|
| Sidebar Chat | `Cmd+L` | `Ctrl+L` |
| Chat with selection | `Cmd+Shift+L` | `Ctrl+Shift+L` |
| Inline Edit | `Cmd+K` | `Ctrl+K` |
| Agent Mode | `Cmd+Shift+A` | `Ctrl+Shift+A` |
| Quick Agent toggle | `Cmd+.` | `Ctrl+.` |
| Background Agent panel | `Cmd+B` | `Ctrl+Shift+B` |
| Accept all diffs | `Cmd+Enter` | `Ctrl+Enter` |
| Settings | `Cmd+Shift+J` | `Ctrl+Shift+J` |

### Tab Completion

| Action | Shortcut |
|---|---|
| Accept suggestion | `Tab` |
| Accept word-by-word | `Ctrl+→` |
| Reject suggestion | `Esc` |

### Terminal

| Action | macOS | Windows/Linux |
|---|---|---|
| AI command generation | `Cmd+K` (in terminal) | `Ctrl+K` (in terminal) |
| Execute generated command | `Cmd+Enter` | `Ctrl+Enter` |

### Standard Editor (Inherited from VS Code)

| Action | macOS | Windows/Linux |
|---|---|---|
| Command Palette | `Cmd+Shift+P` | `Ctrl+Shift+P` |
| Quick Open file | `Cmd+P` | `Ctrl+P` |
| Find in file | `Cmd+F` | `Ctrl+F` |
| Find in project | `Cmd+Shift+F` | `Ctrl+Shift+F` |
| Toggle terminal | `` Ctrl+` `` | `` Ctrl+` `` |
| Split editor | `Cmd+\` | `Ctrl+\` |
| Close tab | `Cmd+W` | `Ctrl+W` |
| Go to definition | `F12` | `F12` |
| Peek definition | `Opt+F12` | `Alt+F12` |
| Rename symbol | `F2` | `F2` |
| Format document | `Cmd+Shift+F` | `Ctrl+Shift+F` |

### Customizing Shortcuts

**Settings → Keybindings** → search for the command you want to remap.

---

## Pricing

### Individual Plans

| Plan | Price | Requests/Month | Key Features |
|---|---|---|---|
| **Hobby** | Free | 50 premium + 500 free model | 1-week Pro trial, no credit card |
| **Pro** | $20/mo | 500 fast premium | All features, Background Agent |
| **Pro+** | $60/mo | 3× Pro usage | Higher limits |
| **Ultra** | $200/mo | 20× Pro usage | Maximum usage, priority access |

### Team Plans

| Plan | Price | Key Features |
|---|---|---|
| **Teams** | $40/user/mo | Admin controls, billing, analytics |
| **Enterprise** | Custom | SCIM 2.0, audit logs, pooled credits, API |

> [!tip] Save 20% with annual billing.

---

## Cursor vs GitHub Copilot

### Feature Comparison

| Feature | Cursor | GitHub Copilot |
|---|---|---|
| **Type** | Full IDE | VS Code extension |
| **Tab completion** | Multi-line, project-aware, predicts edit location | Single-line focused |
| **Inline edit** | `Cmd+K` with diff view | Limited |
| **Chat** | Full sidebar + Agent | Sidebar chat |
| **Multi-file editing** | Agent mode (autonomous) | Not available |
| **Background Agent** | Yes (cloud, parallel) | Not available |
| **Code review (Bugbot)** | Yes | Not available |
| **MCP support** | Yes | Limited |
| **Custom rules** | `.cursor/rules/` | Not available |
| **Model selection** | 15+ models, switch per-chat | GPT-4 family only |
| **Terminal AI** | `Cmd+K` in terminal | Not available |
| **Codebase indexing** | Semantic search | Limited |

### Pricing Comparison

| Tier | Cursor | Copilot |
|---|---|---|
| Free | 50 requests/mo | 12k completions/mo |
| Pro/Individual | $20/mo | $10/mo |
| Team/Business | $40/user/mo | $19/user/mo |

### Choose Cursor If You...

- Need multi-file autonomous editing (Agent mode)
- Want AI code review (Bugbot)
- Need parallel cloud agents (Background Agent)
- Want to choose between many AI models
- Need customizable project rules
- Want deep terminal integration

### Choose Copilot If You...

- Want a lightweight VS Code plugin
- Prefer lower cost
- Have a heavily customized VS Code setup you can't migrate
- Only need basic autocomplete

---

## Best Practices & Tips

### 1. Configure Rules First

> [!important] The single most impactful thing you can do is set up `.cursor/rules/` for your project.

Rules eliminate repetition — bake in "Use TypeScript", "Use vitest not jest", "Follow our API conventions" once, and every interaction respects them.

### 2. One Task Per Session

Context accumulates with each exchange. After 6–8 back-and-forths, conversation history consumes significant tokens.

- Start a **new chat** for each distinct task
- Don't bundle unrelated requests
- Use Plans (save to workspace) instead of long conversations

### 3. Write Tests First

The most effective Agent workflow:

```
"Write tests first for [feature], then implement the code,
then run the tests and iterate until they all pass."
```

This gives Agent a verification target and dramatically improves output quality.

### 4. Use Agent Mode for Real Work

Most developers only use Tab completion. The real power is in Agent mode:
- Multi-file changes with full context
- Terminal command execution
- Autonomous iteration
- Checkpoint safety net

### 5. Enable YOLO Mode (When Appropriate)

Settings → Features → Chat & Composer → YOLO Mode

Lets Agent execute commands and file operations without confirmation. Best for:
- Personal projects
- Sandboxed environments
- When you trust the task scope

### 6. Plan Before Implementing

Ask Agent to **plan first**:

```
"Plan how you would implement [feature]. Don't write code yet."
```

Review the plan, then let Agent implement. Save plans to workspace for documentation.

### 7. Leverage Background Agents

Run multiple agents in parallel:
- One agent refactors the API
- Another writes tests
- A third updates documentation
- Each creates its own PR

### 8. Index Your Documentation

Settings → Indexing & Docs → Add Documentation

Index every library and framework your project uses. This eliminates API hallucinations and ensures generated code follows official patterns.

### 9. Keep .cursorignore Updated

Exclude irrelevant files from indexing:

```gitignore
node_modules/
build/
dist/
.git/
*.log
*.min.js
vendor/
coverage/
```

Faster indexing = faster Agent searches = better suggestions.

### 10. Use @ Mentions Strategically

- `@file.ts` — when you know exactly what context is needed
- `@folder/` — for broader architectural context
- `@Docs` — for library-specific accuracy
- `@Recommended` — let Agent decide (in Agent mode)

### 11. Checkpoint Discipline

Before accepting Agent changes:
1. Review diffs per-file
2. Run tests locally
3. If something breaks, revert to checkpoint
4. Iterate with more specific instructions

### 12. Cost Management

- Use **Composer 1.5** or **Haiku** for routine work
- Reserve **Opus** for complex architecture decisions
- Avoid Max Mode unless you genuinely need 1M token context
- Start new sessions to reset context usage
- Keep rules concise — they load into every session

---

## Appendix: Project Setup Checklist

- [ ] Create `.cursor/rules/` directory
- [ ] Add language/framework rules
- [ ] Add testing rules
- [ ] Add API/architecture rules
- [ ] Create `.cursorignore`
- [ ] Configure MCP servers (`.cursor/mcp.json`) if needed
- [ ] Index external documentation (Settings → Indexing & Docs)
- [ ] Set up team rules (if on Teams plan)
- [ ] Enable Bugbot for PR reviews
- [ ] Configure YOLO mode preference
- [ ] Share `.cursor/` directory via git

---

> [!quote] **Remember**: Cursor is most effective when you configure rules, use Agent mode for real work, write tests for verification, and keep context focused with one task per session. Set up `.cursor/rules/` first — everything else builds on that foundation.
