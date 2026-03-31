# Installing and Configuring Cursor

Cursor is a fork of VS Code with AI built into every interaction. If you know VS Code, you know Cursor — all your extensions, keybindings, and settings carry over.

## Install

1. Download from [cursor.com](https://cursor.com)
2. Install and launch
3. When prompted, import your VS Code settings (extensions, keybindings, themes)
4. Sign in with your team license

## Key Features You'll Use Daily

| Feature | Shortcut | What It Does |
|---|---|---|
| **Composer** | `Ctrl+Shift+I` | Multi-file AI editing. This is your primary tool. Give it a task, it edits multiple files at once. |
| **Inline Edit** | `Ctrl+K` | Edit a specific selection. Highlight code, press Ctrl+K, describe the change. |
| **Chat** | `Ctrl+L` | Ask questions about your codebase. "How does authentication work in this project?" |
| **Tab Autocomplete** | `Tab` | AI-powered autocomplete that understands your project context. |

## First-Time Configuration

### 1. Set Your Model

**Settings > Models:** Select `claude-sonnet-4-20250514` as your default. It's the best balance of speed and quality for daily coding.

For complex architecture work, switch to `claude-opus-4-0520` in Composer — it's slower but significantly better at multi-file reasoning.

### 2. Enable Codebase Indexing

**Settings > Features > Codebase Indexing:** Turn this ON. Cursor will index your entire project so the AI can reference any file, not just open ones. This is critical for .NET projects where types are spread across many files.

### 3. Set Context Length

**Settings > Features > Large Context:** Enable this. .NET solutions have deep dependency trees — the AI needs to see more context to make good decisions.

### 4. Add Your .cursorrules File

This is the single most impactful thing you can do. See `cursorrules/` in this playbook for ready-made files. Copy the appropriate one to your project root as `.cursorrules`.

Without this file, the AI doesn't know your team conventions, architecture patterns, or tech stack preferences. With it, output quality improves dramatically.

## The Mental Model

Think of Cursor as a pair programmer who:
- Has read every file in your project
- Knows every .NET API and NuGet package
- Writes code at 10x speed
- Has no judgment about architecture or business logic
- Will confidently produce wrong code if your instructions are ambiguous

**Your job** is to provide clear direction, review the output, and course-correct. The AI handles the typing. You handle the thinking.

## Workflow Summary

```
1. Open Composer (Ctrl+Shift+I)
2. Describe what you want to build (be specific)
3. Review the proposed changes (Cursor shows diffs)
4. Accept, reject, or refine
5. Test the result
6. Iterate if needed
```

That's it. The rest of this playbook teaches you how to write better prompts for step 2.
