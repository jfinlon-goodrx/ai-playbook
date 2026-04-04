# Claude Code — The Complete Guide

> **A comprehensive reference for getting the most out of Claude Code, Anthropic's AI-powered CLI for software engineering.**
> Last updated: 2026-02-23

---

## Table of Contents

- [[#Quick Start]]
- [[#Cheat Sheet]]
- [[#Slash Commands]]
- [[#Keyboard Shortcuts]]
- [[#Vim Mode]]
- [[#CLI Flags & Options]]
- [[#Permission System]]
- [[#Configuration Files]]
- [[#CLAUDE.md & Memory System]]
- [[#MCP Servers (Model Context Protocol)]]
- [[#Hooks]]
- [[#Skills]]
- [[#Subagents]]
- [[#Models & Effort Levels]]
- [[#Context Management]]
- [[#Git Integration]]
- [[#VS Code Integration]]
- [[#Environment Variables]]
- [[#Debugging & Troubleshooting]]
- [[#Best Practices & Tips]]
- [[#Agent SDK Overview]]

---

## Quick Start

```bash
# Install
npm install -g @anthropic-ai/claude-code

# Launch interactive session
claude

# One-shot (non-interactive)
claude -p "explain this function"

# Resume last session
claude --continue

# Initialize project instructions
/init
```

---

## Cheat Sheet

> [!tip] Pin this section — it covers the most common operations at a glance.

### Everyday Commands

| What you want | How to do it |
|---|---|
| Start a session | `claude` |
| Ask a one-shot question | `claude -p "your question"` |
| Resume last conversation | `claude -c` or `claude --continue` |
| Pick a session to resume | `claude --resume` |
| Resume by name | `claude --resume my-session` |
| Clear context | `/clear` |
| Compact context | `/compact` or `/compact focus on auth logic` |
| Check context usage | `/context` |
| Check token costs | `/cost` |
| Switch model | `/model sonnet` or `Option+P` / `Alt+P` |
| Toggle extended thinking | `Option+T` / `Alt+T` |
| Run a bash command inline | `! git status` |
| Reference a file | `@filename.ts` |
| Create project instructions | `/init` |
| Edit memory | `/memory` |
| Open settings | `/config` |
| Manage permissions | `/permissions` |
| Manage MCP servers | `/mcp` |
| Create/manage hooks | `/hooks` |
| Create/manage subagents | `/agents` |
| Browse plugins | `/plugin` |
| Enter plan mode | `/plan` |
| Rename session | `/rename auth-refactor` |
| Copy last response | `/copy` |
| Rewind conversation | `Esc Esc` or `/rewind` |
| Check installation health | `/doctor` |

### Key Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+C` | Cancel current generation |
| `Ctrl+D` | Exit session |
| `Ctrl+L` | Clear terminal screen |
| `Ctrl+O` | Toggle verbose output |
| `Ctrl+R` | Search command history |
| `Ctrl+G` | Open prompt in external editor |
| `Shift+Tab` | Cycle permission modes |
| `Esc Esc` | Rewind / restore |
| `\ + Enter` | Newline in prompt |

### Common Permission Allowlist Entries

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git log *)",
      "Bash(git diff *)",
      "Bash(git status *)",
      "Bash(npx prettier *)",
      "Bash(npx eslint *)",
      "Read",
      "Glob",
      "Grep"
    ]
  }
}
```

---

## Slash Commands

All commands start with `/` at the prompt. Type `/` to see the full autocomplete list.

### Session & Navigation

| Command | Description |
|---|---|
| `/clear` | Wipe conversation history; start fresh |
| `/compact [instructions]` | Summarize conversation to free context. Optional focus: `/compact focus on the API changes` |
| `/context` | Visualize context usage as a colored grid |
| `/cost` | Show token usage and dollar costs for session |
| `/exit` | Exit the REPL |
| `/export [filename]` | Export conversation to file or clipboard |
| `/rename <name>` | Give current session a memorable name |
| `/resume [session]` | Resume a previous session by ID, name, or open picker |
| `/rewind` | Rewind conversation/code to a previous point |
| `/copy` | Copy last assistant response to clipboard |
| `/tasks` | List and manage background tasks |
| `/todos` | List current TODO items |

### Configuration & Setup

| Command | Description |
|---|---|
| `/config` | Open settings interface |
| `/init` | Generate a `CLAUDE.md` by analyzing your codebase |
| `/memory` | Edit CLAUDE.md memory files directly |
| `/model [name]` | Switch model (use arrow keys for effort level) |
| `/permissions` | View/edit permission rules |
| `/mcp` | Manage MCP server connections |
| `/hooks` | Interactive hook creation and management |
| `/agents` | Interactive subagent creation and management |
| `/plugin` / `/plugins` | Browse and manage plugins |
| `/statusline` | Configure status line display |
| `/theme` | Change color theme |
| `/vim` | Enable vim-style editing mode |
| `/add-dir` | Add additional working directory |

### Modes & Diagnostics

| Command | Description |
|---|---|
| `/plan` | Enter plan mode (analyze without modifying) |
| `/fast` | Toggle fast mode (same Opus model, faster output) |
| `/debug [description]` | Read session debug log |
| `/doctor` | Check installation health |
| `/help` | Show help |
| `/stats` | Daily usage stats, streaks, model preferences |
| `/status` | Version, model, account, connectivity |
| `/usage` | Plan usage limits and rate limit status |

### Advanced

| Command | Description |
|---|---|
| `/teleport` | Resume a web session (claude.ai) in local terminal |
| `/desktop` | Hand off CLI session to Claude Code Desktop (macOS/Windows) |
| `/mcp__<server>__<prompt>` | Invoke an MCP server's exposed prompt |

---

## Keyboard Shortcuts

### General Controls

| Shortcut | Action |
|---|---|
| `Ctrl+C` | Cancel current input or generation |
| `Ctrl+D` | Exit Claude Code |
| `Ctrl+F` | Kill all background agents (press twice to confirm) |
| `Ctrl+G` | Open prompt in external text editor |
| `Ctrl+L` | Clear terminal screen (keeps history) |
| `Ctrl+O` | Toggle verbose output (detailed tool usage) |
| `Ctrl+R` | Reverse search command history |
| `Ctrl+B` | Background running tasks (tmux users: press twice) |
| `Ctrl+T` | Toggle task list visibility |
| `Ctrl+V` / `Cmd+V` (iTerm2) / `Alt+V` (Windows) | Paste image from clipboard |
| `Esc + Esc` | Rewind/restore to previous point |
| `Shift+Tab` or `Alt+M` | Toggle permission modes |
| `Up` / `Down` | Navigate command history |
| `Left` / `Right` | Cycle through dialog tabs |
| `Option+P` (Mac) / `Alt+P` (Linux/Win) | Switch model |
| `Option+T` (Mac) / `Alt+T` (Linux/Win) | Toggle extended thinking |

### Text Editing

| Shortcut | Action |
|---|---|
| `Ctrl+K` | Delete to end of line |
| `Ctrl+U` | Delete entire line |
| `Ctrl+Y` | Paste deleted text |
| `Alt+Y` (after `Ctrl+Y`) | Cycle paste history |
| `Alt+B` | Move back one word |
| `Alt+F` | Move forward one word |

### Multiline Input

| Method | Shortcut |
|---|---|
| Quick escape | `\` + `Enter` |
| macOS default | `Option+Enter` |
| iTerm2, WezTerm, Ghostty, Kitty | `Shift+Enter` |
| Control sequence | `Ctrl+J` |
| Paste mode | Paste directly |

### Inline Shortcuts

| Prefix | Function |
|---|---|
| `!` at start of input | Run bash command directly |
| `@` | File path mention (triggers autocomplete) |

### Customizing Keybindings

Edit `~/.claude/keybindings.json`:

```json
[
  {
    "key": "ctrl+s",
    "command": "submit",
    "when": "inputFocused"
  },
  {
    "key": "ctrl+k ctrl+c",
    "command": "copy",
    "when": "inputFocused"
  }
]
```

Use `/keybindings-help` for interactive setup.

---

## Vim Mode

Enable with `/vim`. Provides modal editing for the input prompt.

### Mode Switching

| Key | Action | From |
|---|---|---|
| `Esc` | Enter NORMAL mode | INSERT |
| `i` / `I` | Insert before cursor / at line start | NORMAL |
| `a` / `A` | Insert after cursor / at line end | NORMAL |
| `o` / `O` | Open line below / above | NORMAL |

### Navigation (NORMAL mode)

| Key | Action |
|---|---|
| `h` `j` `k` `l` | Left, down, up, right |
| `w` / `e` / `b` | Next word / end of word / previous word |
| `0` / `$` / `^` | Line start / line end / first non-blank |
| `gg` / `G` | Start / end of input |
| `f{char}` / `F{char}` | Jump to / back to character |
| `t{char}` / `T{char}` | Jump before / after character |
| `;` / `,` | Repeat / reverse last f/F/t/T |

### Editing (NORMAL mode)

| Key | Action |
|---|---|
| `x` | Delete character |
| `dd` / `D` | Delete line / delete to end |
| `dw` / `de` / `db` | Delete word variants |
| `cc` / `C` | Change line / change to end |
| `cw` / `ce` / `cb` | Change word variants |
| `yy` / `Y` | Yank line |
| `yw` / `ye` / `yb` | Yank word variants |
| `p` / `P` | Paste after / before |
| `>>` / `<<` | Indent / dedent |
| `J` | Join lines |
| `.` | Repeat last change |

### Text Objects

Use with `d`, `c`, `y`, `v` — e.g., `diw` (delete inner word), `ca"` (change around quotes).

| Object | Inner (`i`) | Around (`a`) |
|---|---|---|
| `w` / `W` | Word / WORD | Word + spaces |
| `"` / `'` | Inside quotes | Including quotes |
| `(` / `)` | Inside parens | Including parens |
| `[` / `]` | Inside brackets | Including brackets |
| `{` / `}` | Inside braces | Including braces |

---

## CLI Flags & Options

Launch with `claude [command] [flags]`.

### Essential Flags

| Flag | Description |
|---|---|
| `-p` / `--print` | Non-interactive mode (prints response and exits) |
| `-c` / `--continue` | Resume most recent conversation |
| `-r` / `--resume [id]` | Resume specific session or open picker |
| `--model <name>` | Set model (`opus`, `sonnet`, `haiku`, or full ID) |
| `-w` / `--worktree [name]` | Start in isolated git worktree |
| `--add-dir <path>` | Add additional working directories |
| `-v` / `--version` | Print version |
| `--verbose` | Enable verbose logging |

### Prompt & System Prompt

| Flag | Description |
|---|---|
| `--system-prompt <text>` | **Replace** entire system prompt |
| `--system-prompt-file <path>` | **Replace** from file (print mode only) |
| `--append-system-prompt <text>` | **Append** to default system prompt |
| `--append-system-prompt-file <path>` | **Append** from file (print mode only) |

> [!warning] Use **replace** OR **append**, not both. They are mutually exclusive.

### Permissions & Tools

| Flag | Description |
|---|---|
| `--permission-mode <mode>` | Start in a specific mode (`default`, `plan`, `acceptEdits`, `dontAsk`, `bypassPermissions`) |
| `--allowedTools <tools...>` | Tools that execute without prompting |
| `--disallowedTools <tools...>` | Tools completely removed from Claude's context |
| `--tools <tools...>` | Restrict to only these tools |
| `--dangerously-skip-permissions` | Skip all permission prompts |

### Output & Format (Print Mode)

| Flag | Description |
|---|---|
| `--output-format <fmt>` | `text`, `json`, or `stream-json` |
| `--json-schema <schema>` | Get validated JSON output matching schema |
| `--include-partial-messages` | Include streaming partial events (requires `stream-json`) |
| `--no-session-persistence` | Don't save session |

### Session Management

| Flag | Description |
|---|---|
| `--session-id <uuid>` | Use specific session ID |
| `--fork-session` | Create new session when resuming |
| `--from-pr <number>` | Resume sessions linked to a GitHub PR |

### MCP & Agents

| Flag | Description |
|---|---|
| `--mcp-config <path>` | Load MCP servers from JSON file |
| `--strict-mcp-config` | Only use MCP servers from `--mcp-config` |
| `--agent <name>` | Specify agent for session |
| `--agents <json>` | Define custom subagents inline |

### Budget & Limits

| Flag | Description |
|---|---|
| `--max-turns <n>` | Limit agentic turns (print mode) |
| `--max-budget-usd <n>` | Max dollar spend before stopping (print mode) |
| `--fallback-model <name>` | Model to use when primary is overloaded (print mode) |

### Other Flags

| Flag | Description |
|---|---|
| `--chrome` / `--no-chrome` | Enable/disable Chrome browser integration |
| `--debug [categories]` | Enable debug mode (`"api,hooks"`, `"!statsig"` to exclude) |
| `--disable-slash-commands` | Disable all skills and slash commands |
| `--ide` | Auto-connect to IDE on startup |
| `--init` | Run init hooks, then start interactive mode |
| `--init-only` | Run init hooks and exit |
| `--maintenance` | Run maintenance hooks and exit |
| `--remote` | Create web session on claude.ai |
| `--teleport` | Resume web session in terminal |
| `--teammate-mode <mode>` | Team display: `auto`, `in-process`, `tmux` |
| `--settings <path>` | Path to settings JSON |
| `--setting-sources <list>` | Comma-separated: `user`, `project`, `local` |
| `--plugin-dir <path>` | Load plugins from directory (repeatable) |
| `--betas <headers>` | Beta API headers |
| `--input-format <fmt>` | Input format: `text`, `stream-json` |

---

## Permission System

### Permission Modes

| Mode | Behavior |
|---|---|
| `default` | Prompts on first use of each tool |
| `acceptEdits` | Auto-accept file edits; still prompts for bash |
| `plan` | Read-only — analyze but don't modify |
| `dontAsk` | Auto-deny everything not pre-approved |
| `bypassPermissions` | Skip all prompts (use only in sandboxes) |

Toggle with `Shift+Tab` or set in settings:

```json
{ "permissions": { "defaultMode": "default" } }
```

### Rule Syntax

Rules use the format `Tool` or `Tool(specifier)`.

**Bash rules (wildcards):**

```
Bash(npm run *)        → matches "npm run test", "npm run build"
Bash(git *)            → matches all git commands
Bash(* --help)         → matches any command ending in --help
```

**Read/Edit rules (gitignore-style globs):**

```
Read(src/**)           → read anything in src/ recursively
Edit(/docs/**)         → edit in project docs/ (relative to settings file)
Read(./.env)           → specific file
Edit(//etc/hosts)      → absolute path from filesystem root
Read(~/notes/**)       → home directory paths
```

**WebFetch rules:**

```
WebFetch(domain:github.com)
WebFetch(domain:*.example.com)
```

**MCP tool rules:**

```
mcp__puppeteer                     → all tools from puppeteer server
mcp__puppeteer__puppeteer_navigate → specific tool
```

**Task/subagent rules:**

```
Task(Explore)
Task(my-custom-agent)
```

### Precedence

Rules evaluate in order: **deny → ask → allow**. First match wins, deny always takes precedence.

### Example Config

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git log *)",
      "Bash(git diff *)",
      "Bash(git status)",
      "Bash(npx prettier *)",
      "Read",
      "Glob",
      "Grep"
    ],
    "deny": [
      "Bash(curl *)",
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Read(./.env)",
      "Read(./secrets/**)"
    ]
  }
}
```

---

## Configuration Files

### File Locations & Scopes

| File | Location | Scope | Shared via Git? |
|---|---|---|---|
| `settings.json` | `~/.claude/settings.json` | User (all projects) | No |
| `settings.json` | `.claude/settings.json` | Project | Yes |
| `settings.local.json` | `.claude/settings.local.json` | Local (personal) | No |
| `managed-settings.json` | System path | Organization (IT) | N/A |
| `CLAUDE.md` | `~/.claude/CLAUDE.md` | User memory | No |
| `CLAUDE.md` | `./CLAUDE.md` or `.claude/CLAUDE.md` | Project | Yes |
| `CLAUDE.local.md` | `./CLAUDE.local.md` | Local | No |
| `.claude/rules/*.md` | `.claude/rules/` | Project rules | Yes |
| `~/.claude/rules/*.md` | `~/.claude/rules/` | User rules | No |
| `.mcp.json` | `./.mcp.json` | Project MCP | Yes |
| `~/.claude.json` | `~/.claude.json` | User prefs & MCP | No |
| `keybindings.json` | `~/.claude/keybindings.json` | User keybindings | No |
| `agents/` | `.claude/agents/` | Project agents | Yes |
| `agents/` | `~/.claude/agents/` | User agents | No |
| `skills/` | `.claude/skills/` | Project skills | Yes |
| `skills/` | `~/.claude/skills/` | User skills | No |

### Managed Settings Paths (System Admin)

- **macOS**: `/Library/Application Support/ClaudeCode/managed-settings.json`
- **Linux/WSL**: `/etc/claude-code/managed-settings.json`
- **Windows**: `C:\Program Files\ClaudeCode\managed-settings.json`

### Configuration Precedence (Highest → Lowest)

1. Managed settings (system-level)
2. Command-line arguments
3. Local project settings (`.claude/settings.local.json`)
4. Shared project settings (`.claude/settings.json`)
5. User settings (`~/.claude/settings.json`)

### Full settings.json Example

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git log *)",
      "Bash(git diff *)",
      "Read"
    ],
    "deny": [
      "Bash(curl *)",
      "Read(./.env)"
    ],
    "defaultMode": "default"
  },
  "env": {
    "NODE_ENV": "development"
  },
  "model": "sonnet",
  "effortLevel": "medium",
  "alwaysThinkingEnabled": true,
  "additionalDirectories": ["/path/to/shared/lib"],
  "hooks": {},
  "plugins": {
    "enabledPlugins": []
  },
  "attribution": {
    "commit": "Generated with Claude Code",
    "pr": "Generated with Claude Code"
  }
}
```

---

## CLAUDE.md & Memory System

### How Memory Works

Claude loads context from multiple memory layers at session start:

```
Priority (highest first):
1. Managed policy CLAUDE.md (organization)
2. ./CLAUDE.local.md (personal, local)
3. ./CLAUDE.md or ./.claude/CLAUDE.md (project, shared)
4. ~/.claude/CLAUDE.md (user, global)
5. Auto memory: ~/.claude/projects/<project>/memory/MEMORY.md (first 200 lines)
```

Child `CLAUDE.md` files in subdirectories load on demand when editing in those dirs.

### Writing Effective CLAUDE.md

> [!tip] Keep under ~500 lines. Focus on what Claude can't infer from code.

**Include:**

- Build, test, and lint commands
- Code style rules that differ from language defaults
- Testing conventions and preferred runners
- Repo etiquette (branch naming, PR format, commit style)
- Key architectural decisions
- Required environment variables
- Common gotchas and non-obvious behaviors

**Exclude:**

- Standard language conventions (Claude already knows)
- Detailed API docs (link to them instead)
- Info that changes frequently
- Anything obvious from reading the code

### Example CLAUDE.md

```markdown
# Project: MyApp

## Build & Test
- `npm run dev` — start dev server
- `npm test` — run all tests (vitest)
- `npm run test:unit -- path/to/test` — run a single test
- `npm run lint` — eslint + prettier check

## Code Style
- Use named exports, not default exports
- Prefer `const` arrow functions for components
- Error messages: lowercase, no trailing period
- Tests: use `describe/it` pattern, not `test()`

## Architecture
- `src/api/` — Express route handlers
- `src/services/` — Business logic (no HTTP concerns)
- `src/models/` — Prisma models & DB queries
- All API responses use `{ data, error, meta }` envelope

## Git Conventions
- Branch: `feat/`, `fix/`, `chore/` prefixes
- Commits: conventional commits format
- Always run `npm test` before committing

## Gotchas
- The `auth` middleware reads from `req.headers['x-api-key']`, not Bearer token
- Prisma migrations require `DATABASE_URL` in `.env.local`
```

### @-Imports in CLAUDE.md

```markdown
See @README.md for project overview.
See @docs/architecture.md for system design.
See @~/.claude/personal-rules.md for my preferences.
```

Max 5 hops of recursive imports.

### Auto Memory

Claude automatically saves learnings to `~/.claude/projects/<project>/memory/`.

Structure:
```
memory/
├── MEMORY.md          ← index file, first 200 lines loaded
├── debugging.md       ← topic files for detailed notes
├── patterns.md
└── api-conventions.md
```

Disable with `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`.

### Path-Specific Rules

Create `.claude/rules/*.md` files with frontmatter globs:

```markdown
---
paths:
  - "src/**/*.ts"
  - "lib/**/*.ts"
---

# TypeScript Rules
- Use strict mode
- Prefer interfaces over type aliases for object shapes
- Always type function parameters
```

---

## MCP Servers (Model Context Protocol)

MCP servers extend Claude Code with external tools and data sources.

### Adding Servers

**HTTP (recommended for remote):**

```bash
claude mcp add --transport http github https://api.githubcopilot.com/mcp/
claude mcp add --transport http notion https://mcp.notion.com/mcp
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp
```

**Stdio (local processes):**

```bash
claude mcp add --transport stdio my-server -- npx -y my-server-package
claude mcp add --transport stdio my-server --env API_KEY=xyz -- python server.py
```

> [!warning] Options (`--transport`, `--env`) must come **before** the server name. Use `--` to separate from the command.

### Management Commands

```bash
claude mcp list                    # List all servers
claude mcp get github              # Server details
claude mcp remove github           # Remove server
claude mcp reset-project-choices   # Reset approval choices
```

### Scopes

| Scope | Storage | Shared? |
|---|---|---|
| `local` (default) | `~/.claude.json` | No — you only, current project |
| `project` | `.mcp.json` | Yes — via git |
| `user` | `~/.claude.json` | No — you only, all projects |

### Project MCP Config (`.mcp.json`)

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    },
    "database": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "db-mcp-server"],
      "env": {
        "DB_URL": "${DATABASE_URL}",
        "PORT": "${DB_PORT:-5432}"
      }
    }
  }
}
```

Environment variable expansion: `${VAR}` or `${VAR:-default}`.

### OAuth Authentication

```bash
claude mcp add --transport http \
  --client-id your-client-id \
  --client-secret \
  --callback-port 8080 \
  my-server https://mcp.example.com/mcp
```

Then authenticate in session: `/mcp`

### Using MCP Prompts

Connected servers can expose prompts as commands:

```
/mcp__github__list_prs
/mcp__jira__create_issue "Bug in login" high
```

### Tuning

| Variable | Default | Description |
|---|---|---|
| `MAX_MCP_OUTPUT_TOKENS` | `25000` | Max tokens per MCP output |
| `MCP_TIMEOUT` | `10000` | Server startup timeout (ms) |
| `ENABLE_TOOL_SEARCH` | `auto` | Tool search: `auto`, `auto:5`, `true`, `false` |

---

## Hooks

Hooks run shell commands at specific lifecycle events.

### Available Events

| Event | Fires When | Matcher Filters By |
|---|---|---|
| `SessionStart` | Session begins | Source: `startup`, `resume`, `clear`, `compact` |
| `UserPromptSubmit` | User sends a message | — |
| `PreToolUse` | Before a tool runs | Tool name: `Bash`, `Edit\|Write`, `mcp__*` |
| `PostToolUse` | After a tool completes | Tool name |
| `PostToolUseFailure` | After a tool fails | Tool name |
| `PermissionRequest` | Permission prompt shown | Tool name |
| `Notification` | Notification fires | Type: `permission_prompt`, `idle_prompt` |
| `SubagentStart` / `SubagentStop` | Subagent lifecycle | Agent type |
| `Stop` | Claude finishes responding | — |
| `PreCompact` | Before compaction | Trigger: `manual`, `auto` |
| `SessionEnd` | Session ends | Reason: `clear`, `logout`, `other` |

### Hook Configuration

Add to any `settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/validate-command.sh"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "notify-send 'Claude Code' 'Needs your attention'"
          }
        ]
      }
    ]
  }
}
```

### Exit Codes

| Exit Code | Behavior |
|---|---|
| **0** | Action proceeds. `stdout` fed to Claude as context |
| **2** | Action **blocked**. `stderr` shown as feedback to Claude |
| **Other** | Action proceeds. `stderr` logged but not shown |

### Hook JSON Output (Advanced)

Hooks can output JSON to control behavior:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "Use rg instead of grep"
  }
}
```

### Practical Hook Examples

**Auto-format on save:**
```json
{
  "PostToolUse": [{
    "matcher": "Edit|Write",
    "hooks": [{ "type": "command", "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write" }]
  }]
}
```

**Desktop notification when idle:**
```json
{
  "Notification": [{
    "matcher": "permission_prompt",
    "hooks": [{ "type": "command", "command": "notify-send 'Claude Code' 'Waiting for approval'" }]
  }]
}
```

**Block edits to protected files:**
```bash
#!/bin/bash
# ~/.claude/hooks/protect-files.sh
FILE=$(jq -r '.tool_input.file_path' < /dev/stdin)
if [[ "$FILE" == *".env"* || "$FILE" == *"secrets"* ]]; then
  echo "Blocked: cannot edit protected file $FILE" >&2
  exit 2
fi
```

---

## Skills

Skills package reusable instructions and workflows.

### Creating a Skill

```bash
mkdir -p .claude/skills/my-skill  # project scope
# or
mkdir -p ~/.claude/skills/my-skill  # user scope
```

Create `SKILL.md` inside the directory:

```markdown
---
name: my-skill
description: Describes when Claude should use this skill
user-invocable: true
allowed-tools: Read, Grep, Bash
model: inherit
argument-hint: [filename]
---

Instructions for what the skill does.

## Steps
1. First, read the target file
2. Analyze for issues
3. Suggest improvements

See [reference.md](reference.md) for additional context.
```

### Frontmatter Fields

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Lowercase with hyphens, becomes `/command` |
| `description` | Recommended | When Claude should auto-invoke |
| `user-invocable` | No | `true` to show in `/` menu (default: true) |
| `disable-model-invocation` | No | `true` to prevent automatic use |
| `allowed-tools` | No | Tools to auto-allow |
| `model` | No | `sonnet`, `opus`, `haiku`, `inherit` |
| `context` | No | `fork` to run as subagent |
| `agent` | No | Agent type for `context: fork` |
| `argument-hint` | No | Shown in autocomplete |
| `hooks` | No | Hooks scoped to skill lifecycle |

### Invoking Skills

```
/my-skill argument1 argument2
/explain-code src/auth.ts
/review-pr 456
```

### String Substitutions

- `$ARGUMENTS` — all passed arguments
- `$ARGUMENTS[0]` or `$0` — first argument
- `${CLAUDE_SESSION_ID}` — current session ID

### Supporting Files

```
my-skill/
├── SKILL.md          ← required
├── reference.md      ← referenced via [reference.md](reference.md)
├── examples.md
└── scripts/
    └── validate.sh
```

---

## Subagents

Specialized AI assistants that run in isolated contexts.

### Built-in Subagents

| Agent | Model | Access | Best For |
|---|---|---|---|
| `Explore` | Haiku | Read-only | Fast codebase search |
| `Plan` | Inherits | Read-only | Research for planning |
| `general-purpose` | Inherits | All tools | Complex multi-step tasks |

### Creating Custom Subagents

Create `.claude/agents/my-agent.md` (project) or `~/.claude/agents/my-agent.md` (user):

```markdown
---
name: code-reviewer
description: Reviews code for quality. Use proactively after code changes.
tools: Read, Grep, Glob, Bash
model: sonnet
permissionMode: default
maxTurns: 20
skills:
  - code-style
mcpServers:
  - github
background: false
isolation: worktree
---

You are a senior code reviewer focused on quality and best practices.

When reviewing:
1. Read the modified files
2. Check for common issues (error handling, edge cases, security)
3. Provide specific, actionable feedback
4. Suggest improvements with code examples
```

### Subagent Frontmatter

| Field | Description |
|---|---|
| `name` | Unique ID (lowercase, hyphens) |
| `description` | When Claude should delegate here |
| `tools` | Available tools (inherits all if omitted) |
| `disallowedTools` | Tools to deny |
| `model` | `sonnet`, `opus`, `haiku`, `inherit` |
| `permissionMode` | `default`, `acceptEdits`, `dontAsk`, `bypassPermissions`, `plan` |
| `maxTurns` | Max agentic turns |
| `skills` | Skills to preload |
| `mcpServers` | Available MCP servers |
| `memory` | `user`, `project`, `local` |
| `background` | `true` to always run in background |
| `isolation` | `worktree` for isolated git worktree |
| `hooks` | Hooks scoped to this agent |

### Inline Subagents (CLI)

```bash
claude --agents '{
  "reviewer": {
    "description": "Code reviewer",
    "prompt": "You are a senior code reviewer...",
    "tools": ["Read", "Grep"],
    "model": "sonnet",
    "maxTurns": 10
  }
}'
```

---

## Models & Effort Levels

### Available Models

| Alias | Model | Best For |
|---|---|---|
| `opus` | Claude Opus 4.6 | Complex reasoning, architecture |
| `sonnet` | Claude Sonnet 4.6 | Daily coding (fast + capable) |
| `haiku` | Claude Haiku 4.5 | Quick, simple tasks |
| `default` | Recommended for your tier | Auto-selected |
| `sonnet[1m]` | Sonnet with 1M context | Very long sessions |
| `opusplan` | Opus for planning, Sonnet for execution | Hybrid |

### Switching Models

```bash
# At launch
claude --model opus

# During session
/model sonnet

# Keyboard shortcut
Option+P (Mac) / Alt+P (Linux/Win)

# Environment variable
export ANTHROPIC_MODEL=haiku
```

### Effort Levels (Opus 4.6)

Controls how much reasoning budget Claude uses:

| Level | Behavior |
|---|---|
| `low` | Faster, cheaper — straightforward tasks |
| `medium` | Balanced (default) |
| `high` | Deeper reasoning — complex problems |

Set via:
```bash
/model                              # use arrow keys
export CLAUDE_CODE_EFFORT_LEVEL=high
# or in settings.json:
{ "effortLevel": "medium" }
```

### Extended Thinking

Toggle with `Option+T` / `Alt+T` or:
```json
{ "alwaysThinkingEnabled": true }
```

---

## Context Management

> [!tip] Aggressively managing context is one of the highest-leverage habits for effective Claude Code use.

### Key Commands

| Command | When to Use |
|---|---|
| `/context` | See what's consuming your context window |
| `/compact` | Summarize conversation to free space |
| `/compact focus on auth logic` | Summarize with a focus directive |
| `/clear` | Nuclear option — fresh start |

### Strategies

1. **`/clear` between unrelated tasks** — don't let context from task A pollute task B
2. **Use `/compact` with focus instructions** — tells Claude what to preserve during summarization
3. **Delegate verbose research to subagents** — their context is isolated from yours
4. **Avoid pasting large logs** — use grep/filters to extract relevant lines first
5. **Keep CLAUDE.md focused** — every line loads into context every session
6. **Use `@file` references** — Claude reads only what's needed instead of you pasting
7. **Use plan mode for exploration** — explore in plan mode, then switch to implement

### Automatic Compaction

Claude auto-compacts when approaching context limits. The `PreCompact` hook fires before this happens, allowing you to inject instructions about what to preserve.

---

## Git Integration

Claude Code has deep git awareness and can perform most git operations.

### Common Git Workflows

**Commit changes:**
```
commit these changes with a descriptive message
```
Claude will: `git status` → `git diff` → `git log` (for style) → stage files → commit.

**Create a PR:**
```
create a PR for this branch
```
Claude will: analyze all commits → write title + description → `gh pr create`.

**Review a PR:**
```
review PR #123
```

**Work in isolation:**
```bash
claude -w feature-name     # starts in a git worktree
```

### Git Safety

Claude Code follows these safety rules by default:
- Never force-pushes
- Never runs `reset --hard`, `checkout .`, `clean -f`
- Never amends commits without explicit request
- Prefers staging specific files over `git add -A`
- Won't commit unless explicitly asked
- Won't push unless explicitly asked

---

## VS Code Integration

### Opening Claude Code

| Method | How |
|---|---|
| Editor toolbar | Click Spark icon (top-right) |
| Status bar | Click "✱ Claude Code" (bottom-right) |
| Command palette | `Cmd+Shift+P` → "Claude Code: Open in New Tab" |
| Keyboard | `Cmd+Shift+Esc` / `Ctrl+Shift+Esc` |

### VS Code Keybindings

| Shortcut | Action |
|---|---|
| `Cmd+Esc` / `Ctrl+Esc` | Focus Claude input |
| `Cmd+Shift+Esc` / `Ctrl+Shift+Esc` | Open in new tab |
| `Cmd+N` / `Ctrl+N` | New conversation |
| `Option+K` / `Alt+K` | Insert @-mention |

### Extension Settings

| Setting | Default | Description |
|---|---|---|
| `selectedModel` | `default` | Model for new conversations |
| `useTerminal` | `false` | Launch in terminal mode |
| `initialPermissionMode` | `default` | Starting permission mode |
| `preferredLocation` | `panel` | `sidebar` or `panel` |
| `autosave` | `true` | Auto-save before read/write |
| `useCtrlEnterToSend` | `false` | Ctrl/Cmd+Enter to send |
| `respectGitIgnore` | `true` | Exclude .gitignore patterns |

---

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `ANTHROPIC_API_KEY` | API key | — |
| `ANTHROPIC_MODEL` | Default model | — |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | Model for `opus` alias | `claude-opus-4-6` |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | Model for `sonnet` alias | `claude-sonnet-4-6` |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | Model for `haiku` alias | `claude-haiku-4-5` |
| `CLAUDE_CODE_EFFORT_LEVEL` | Effort level | `medium` |
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY` | Disable auto memory | `0` |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | Enable prompt suggestions | `true` |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | Disable background tasks | — |
| `ENABLE_TOOL_SEARCH` | Tool search behavior | `auto` |
| `MAX_MCP_OUTPUT_TOKENS` | Max MCP output tokens | `25000` |
| `MCP_TIMEOUT` | MCP startup timeout (ms) | `10000` |
| `DISABLE_PROMPT_CACHING` | Disable prompt caching | — |
| `MAX_THINKING_TOKENS` | Thinking token budget | `31999` |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD` | Load CLAUDE.md from extra dirs | — |

---

## Debugging & Troubleshooting

### Diagnostic Commands

```bash
/doctor              # Check installation health
/debug               # Read session debug log
/debug "description" # Include issue description
/status              # Version, model, account info

# Launch with debug categories
claude --debug "api,hooks"
claude --debug "!statsig,!file"  # exclude categories
```

### Common Issues

| Problem | Solution |
|---|---|
| No response | Check internet; start new session |
| Extension won't install | Requires VS Code 1.98+; restart VS Code |
| MCP server not connecting | `/mcp` to check status; verify credentials |
| Too many permission prompts | Add to `permissions.allow` in settings |
| High memory/CPU | `/clear` old sessions; check `/tasks`; reduce MCP servers |
| Context too full | `/compact` with focus or `/clear` |
| Hook not firing | Check matcher regex; verify exit codes; use `--debug "hooks"` |

### Report Issues

https://github.com/anthropics/claude-code/issues

---

## Best Practices & Tips

### The #1 Tip: Give Claude Something to Verify

> [!important] Include tests, screenshots, or expected outputs so Claude can check its own work. This is the single highest-leverage thing you can do.

### Workflow: Explore → Plan → Implement

1. **Explore** (plan mode): Read files, understand code, ask questions
2. **Plan** (plan mode): Create detailed implementation plan
3. **Implement** (normal mode): Write code, run tests, verify
4. **Commit**: Create PR with description

### Prompting Tips

- **Be specific**: "Fix the null pointer in `src/auth.ts:45` when `user.email` is undefined" > "fix the bug"
- **Reference files**: Use `@filename.ts` to give Claude direct context
- **Paste screenshots/images**: Claude can read them (`Ctrl+V`)
- **Describe the outcome**: "The API should return 404 instead of 500 when the user doesn't exist"
- **Point to examples**: "Follow the pattern in `src/api/users.ts` for the new endpoint"

### Speed & Efficiency

- **`/clear` between tasks** — fresh context = faster, more accurate responses
- **Use Sonnet for most work** — Opus for complex architecture decisions
- **Allowlist common commands** — eliminates repetitive permission prompts
- **Use subagents** — isolate research-heavy work from your main context
- **`! command`** — run bash inline without leaving the conversation
- **`Esc` early** — if Claude is going in the wrong direction, stop and redirect
- **`/rewind`** — go back instead of trying to fix a bad path forward

### Cost Control

- Clear context between unrelated tasks
- Use `/compact` with focus instructions
- Prefer Sonnet over Opus for routine work
- Avoid reading massive files — grep first
- Delegate verbose operations to subagents
- Set `--max-budget-usd` for print-mode automations

### Project Setup Checklist

- [ ] Run `/init` to create `CLAUDE.md`
- [ ] Configure permissions in `.claude/settings.json`
- [ ] Add common commands to allowlist
- [ ] Set up MCP servers for your tools (GitHub, Sentry, etc.)
- [ ] Create hooks for auto-formatting
- [ ] Add path-specific rules in `.claude/rules/`
- [ ] Create skills for recurring workflows
- [ ] Share `.claude/` directory with team via git

---

## Agent SDK Overview

The Claude Agent SDK enables building custom AI agents programmatically. Available for **Python** and **TypeScript/Node.js**.

### Key Capabilities

- Create agentic workflows with tool use
- Define custom tools and data sources
- Manage sessions and state
- Stream responses in real-time
- Track costs and token usage
- Connect MCP servers
- Get structured JSON outputs

### Quick Example (TypeScript)

```typescript
import { Agent } from "@anthropic-ai/agent-sdk";

const agent = new Agent({
  model: "claude-sonnet-4-6",
  tools: [myCustomTool],
  systemPrompt: "You are a helpful code reviewer.",
});

const result = await agent.run("Review the latest PR changes");
console.log(result.output);
```

### Documentation

- Agent SDK: https://platform.claude.com/docs/en/agent-sdk/overview
- Claude API: https://platform.claude.com/docs

---

## Appendix: Useful Config Templates

### Minimal Personal Setup (`~/.claude/settings.json`)

```json
{
  "permissions": {
    "allow": [
      "Bash(npm *)",
      "Bash(git log *)",
      "Bash(git diff *)",
      "Bash(git status)",
      "Read",
      "Glob",
      "Grep"
    ],
    "defaultMode": "default"
  },
  "model": "sonnet",
  "alwaysThinkingEnabled": true
}
```

### Team Project Setup (`.claude/settings.json`)

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(npx prettier *)",
      "Bash(npx eslint *)",
      "Bash(git log *)",
      "Bash(git diff *)",
      "Bash(git status)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Read(./.env*)",
      "Read(./secrets/**)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

### Headless/CI Pipeline Usage

```bash
# Analyze code and output JSON
claude -p "List all TODO comments in the codebase" \
  --output-format json \
  --max-turns 5 \
  --max-budget-usd 1.00 \
  --dangerously-skip-permissions

# Generate structured output
claude -p "Analyze this PR for security issues" \
  --json-schema '{"type":"object","properties":{"issues":{"type":"array"},"severity":{"type":"string"}}}'

# Pipe input
git diff HEAD~1 | claude -p "Review this diff for bugs"
```

---

> [!quote] **Remember**: Claude Code is most effective when you give it clear context, something to verify against, and manage your context window aggressively. Start with `/init`, configure your permissions, and use `/clear` between tasks.
