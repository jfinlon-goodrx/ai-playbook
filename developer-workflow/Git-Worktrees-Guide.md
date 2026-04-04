# Git Worktrees — A Practical Guide for .NET Full-Stack Developers

> Work on multiple branches simultaneously without stashing, switching, or losing build state. This guide is tailored for complex .NET projects with submodules, cross-platform development (Linux, macOS, Windows), and GitHub Pro workflows.

---

## Table of Contents

1. [What Are Worktrees and Why Use Them](#what-are-worktrees-and-why-use-them)
2. [Core Concepts](#core-concepts)
3. [Getting Started](#getting-started)
4. [Worktree Lifecycle](#worktree-lifecycle)
5. [Practical Workflows for .NET Projects](#practical-workflows-for-net-projects)
6. [Working with Submodules in Worktrees](#working-with-submodules-in-worktrees)
7. [Cross-Platform Considerations](#cross-platform-considerations)
8. [IDE and Editor Integration](#ide-and-editor-integration)
9. [GitHub Pro Workflows](#github-pro-workflows)
10. [Multi-Machine Strategies](#multi-machine-strategies)
11. [Performance and Housekeeping](#performance-and-housekeeping)
12. [Troubleshooting](#troubleshooting)
13. [Quick Reference](#quick-reference)

---

## What Are Worktrees and Why Use Them

A **git worktree** lets you check out multiple branches of the same repository into separate directories on disk — simultaneously. Each worktree has its own working directory and index, but they all share the same `.git` object database.

### The Problem Worktrees Solve

Without worktrees, switching branches in a .NET project means:

- Waiting for `dotnet restore` and `dotnet build` to re-run (bin/obj churn)
- Losing running Docker containers or debug sessions tied to the previous branch
- Stashing half-finished work and hoping you remember to pop it later
- Closing and reopening IDE solutions/workspaces

With worktrees, each branch lives in its own directory. You can:

- **Build and run two branches side-by-side** — compare behaviour, run integration tests across versions
- **Keep a hotfix branch always ready** while deep in feature work
- **Review a colleague's PR** without touching your working branch
- **Run long builds or tests** on one branch while coding on another
- **Keep `main` permanently checked out** as a reference copy

### When Worktrees Shine

| Scenario | Without Worktrees | With Worktrees |
|---|---|---|
| Urgent hotfix while mid-feature | Stash, switch, fix, switch back, pop | Open the hotfix worktree, fix, done |
| PR review | Stash or commit WIP, checkout PR branch | Open a review worktree, inspect freely |
| Comparing two branches | One at a time, or clone the repo again | Both open in separate terminals/IDEs |
| Long-running test suite | Blocks you from coding | Tests run in one worktree, code in another |
| .NET build cache per branch | Destroyed on every switch | Each worktree keeps its own `bin/` and `obj/` |

---

## Core Concepts

### Bare Clone vs Standard Clone

There are two ways to set up a worktree-centric workflow:

**Standard clone (add worktrees to an existing repo)**
```bash
# Your existing clone IS the main worktree
cd /mnt/workspace/repos/Projects/DNA
git worktree add ../DNA-hotfix feature/hotfix-auth
```

**Bare clone (worktree-first approach — recommended for new setups)**
```bash
# The bare repo has no working directory — all work happens in worktrees
git clone --bare git@github.com:jim-finlon/dna-workspace.git DNA.git
cd DNA.git
git worktree add ../DNA-main main
git worktree add ../DNA-feature-channels feature/phase4-channels
```

> **Recommendation:** For an established project like DNA that already has a standard clone, use the standard approach — add worktrees alongside your existing checkout. For new projects, consider starting with a bare clone.

### How Worktrees Share State

All worktrees linked to the same repository share:

- **Object database** — commits, blobs, trees (no duplication of git history)
- **Refs** — branches, tags, remote tracking refs
- **Config** — `.git/config` settings
- **Hooks** — `.git/hooks/` scripts

Each worktree has its own:

- **Working directory** — the actual files on disk
- **Index** (staging area)
- **HEAD** — which commit/branch is checked out
- **Build artifacts** — `bin/`, `obj/`, `node_modules/`, etc.

### The One-Branch Rule

A branch can only be checked out in **one worktree at a time**. If `main` is checked out in your primary worktree, you cannot check out `main` in another worktree. You can work around this with detached HEAD if needed.

---

## Getting Started

### Creating Your First Worktree

```bash
# From your existing DNA repo
cd /mnt/workspace/repos/Projects/DNA

# Create a worktree for a feature branch (existing branch)
git worktree add ../DNA-phase7-admin feature/phase7-admin-ui

# Create a worktree with a NEW branch
git worktree add -b feature/phase9-cleanup ../DNA-phase9 main

# Create a worktree in detached HEAD (for read-only inspection)
git worktree add --detach ../DNA-inspect v2.1.0
```

### Directory Layout Convention

Adopt a consistent naming convention so you always know what lives where. Recommended layout for your DNA project:

```
/mnt/workspace/repos/Projects/
├── DNA/                              # Primary worktree (main)
├── DNA-wt-phase7-admin/              # feature/phase7-admin-ui
├── DNA-wt-hotfix/                    # hotfix branch
├── DNA-wt-review/                    # Temporary — PR reviews
└── DNA-wt-r2-observability/          # feature/r2-observability
```

> **Naming tip:** Use a `wt-` prefix so worktree directories sort together and are visually distinct from other project folders.

### Listing and Inspecting Worktrees

```bash
# List all worktrees
git worktree list

# Verbose output with branch info
git worktree list --porcelain
```

Example output:
```
/mnt/workspace/repos/Projects/DNA                    abc1234 [main]
/mnt/workspace/repos/Projects/DNA-wt-phase7-admin    def5678 [feature/phase7-admin-ui]
/mnt/workspace/repos/Projects/DNA-wt-hotfix          (detached HEAD)
```

---

## Worktree Lifecycle

### Creating

```bash
# From existing branch
git worktree add <path> <branch>

# From new branch (based on a start point)
git worktree add -b <new-branch> <path> <start-point>

# Track a remote branch that doesn't exist locally yet
git fetch origin
git worktree add <path> -b feature/new-thing origin/feature/new-thing
```

### Working

Each worktree is a normal directory. Use it exactly like a regular checkout:

```bash
cd /mnt/workspace/repos/Projects/DNA-wt-phase7-admin
dotnet build
dotnet test
git add .
git commit -m "feat(admin): add dashboard widget"
git push origin feature/phase7-admin-ui
```

### Removing

```bash
# First, leave the worktree directory
cd /mnt/workspace/repos/Projects/DNA

# Remove the worktree (deletes the directory)
git worktree remove ../DNA-wt-phase7-admin

# If the worktree has uncommitted changes, force removal
git worktree remove --force ../DNA-wt-phase7-admin
```

### Pruning Stale Worktrees

If a worktree directory is manually deleted (e.g., `rm -rf`), git still tracks it. Clean up:

```bash
# Show what would be pruned
git worktree prune --dry-run

# Actually prune
git worktree prune
```

---

## Practical Workflows for .NET Projects

### Workflow 1: Permanent Main + Feature Worktrees

Keep `main` always available for reference, hotfixes, and as a build baseline:

```bash
cd /mnt/workspace/repos/Projects/DNA

# You're already on main in the primary worktree.
# Create feature worktrees as needed:
git worktree add ../DNA-wt-channels feature/phase4-channels
git worktree add ../DNA-wt-memory feature/r3-memory
```

When a feature is done and merged:

```bash
git worktree remove ../DNA-wt-channels
```

### Workflow 2: Hotfix While Deep in Feature Work

You're midway through a complex feature with a half-built UI, Docker containers running, and tests in progress. Production needs a fix.

```bash
# Don't touch your feature worktree. From ANYWHERE:
cd /mnt/workspace/repos/Projects/DNA
git fetch origin
git worktree add -b hotfix/auth-token-expiry ../DNA-wt-hotfix main

# Fix, build, test, commit, push — all in the hotfix worktree
cd ../DNA-wt-hotfix
# ... make fixes ...
dotnet build && dotnet test
git add -A && git commit -m "fix(auth): extend token expiry window"
git push origin hotfix/auth-token-expiry

# Create PR from the hotfix worktree
gh pr create --title "fix: auth token expiry" --base main

# Clean up after merge
cd ../DNA
git worktree remove ../DNA-wt-hotfix
git branch -d hotfix/auth-token-expiry
```

Your feature worktree was never touched. Your containers are still running. Your IDE is still open.

### Workflow 3: PR Review Worktree

```bash
# Fetch the PR branch
git fetch origin pull/42/head:pr-42

# Create a temporary review worktree
git worktree add ../DNA-wt-review pr-42

# Review, build, run tests
cd ../DNA-wt-review
dotnet build
dotnet test

# Clean up
cd ../DNA
git worktree remove ../DNA-wt-review
git branch -D pr-42
```

### Workflow 4: Side-by-Side Comparison

Run two versions of the same service to compare behaviour:

```bash
# Worktree 1: current main
cd /mnt/workspace/repos/Projects/DNA
dotnet run --project SelfHostedPlatform/  # runs on port 5000

# Worktree 2: feature branch (separate terminal)
cd /mnt/workspace/repos/Projects/DNA-wt-channels
dotnet run --project SelfHostedPlatform/ --urls "http://localhost:5001"
```

### Workflow 5: Build Caching Per Branch

One of the biggest wins for .NET developers. Each worktree maintains its own `bin/` and `obj/` directories, so:

- Switching between feature branches doesn't trigger full rebuilds
- Incremental builds remain fast within each worktree
- NuGet restore only runs once per worktree (unless dependencies change)

This alone can save significant time on large solutions like `DotNetAgents.All.sln`.

---

## Working with Submodules in Worktrees

Your DNA project uses submodules extensively (BusinessAgent, GatewayAgent, JARVIS, etc.). Worktrees and submodules require some extra care.

### Initializing Submodules in a New Worktree

When you create a worktree, submodules are **not** automatically initialized. You must do it manually:

```bash
git worktree add ../DNA-wt-feature feature/some-branch
cd ../DNA-wt-feature

# Initialize and update all submodules
git submodule update --init --recursive
```

> **Important:** Each worktree gets its own copy of submodule working directories. This means disk usage increases per worktree, but each worktree's submodules are independent.

### Submodule Branches in Worktrees

When working on a feature that spans the root repo and submodules (per your SUBMODULE_WORKFLOW.md):

```bash
cd /mnt/workspace/repos/Projects/DNA-wt-feature

# The submodule is checked out at the commit recorded in the parent
cd Agent-Projects/GatewayAgent

# Create or switch to the matching feature branch inside the submodule
git checkout -b feature/same-feature-name
# ... make changes, commit, push ...

# Back in the worktree root, record the updated submodule ref
cd ../..
git add Agent-Projects/GatewayAgent
git commit -m "chore: update GatewayAgent submodule ref"
```

### Selective Submodule Init

For large projects, you don't always need every submodule. Initialize only what you need:

```bash
cd /mnt/workspace/repos/Projects/DNA-wt-feature

# Only initialize the submodules you're working on
git submodule update --init Agent-Projects/GatewayAgent
git submodule update --init DotNetAgents
```

This keeps worktree creation fast and reduces disk usage.

### Submodule Gotcha: Shared `.git` Files

In a standard clone, submodules store their git data in the parent's `.git/modules/` directory. When you create a worktree, the submodule working directories are new, but they can reference the same module data. If you encounter issues:

```bash
# Re-initialize submodules cleanly in the worktree
git submodule deinit -f .
git submodule update --init --recursive
```

---

## Cross-Platform Considerations

### Line Endings

With Linux, macOS, and Windows in the mix, configure this globally on each machine:

```bash
# On Linux and macOS
git config --global core.autocrlf input

# On Windows
git config --global core.autocrlf true
```

Or better — enforce it in the repo with a `.gitattributes` file:

```gitattributes
# In your repo root
* text=auto
*.cs text eol=lf
*.csproj text eol=lf
*.sln text eol=lf
*.json text eol=lf
*.xml text eol=lf
*.yaml text eol=lf
*.yml text eol=lf
*.sh text eol=lf
*.ps1 text eol=crlf
*.bat text eol=crlf
*.cmd text eol=crlf
```

### Path Length Limits (Windows)

Windows has a 260-character path limit by default. Worktrees add directory depth. Enable long paths:

```powershell
# Run as Administrator in PowerShell
git config --global core.longpaths true

# Also enable in Windows itself (requires restart)
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
  -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

### Worktree Path Conventions Per OS

Keep worktree paths short and consistent across machines:

| OS | Primary Clone | Worktree Pattern |
|---|---|---|
| Linux | `/mnt/workspace/repos/Projects/DNA` | `/mnt/workspace/repos/Projects/DNA-wt-<name>` |
| macOS | `~/repos/Projects/DNA` | `~/repos/Projects/DNA-wt-<name>` |
| Windows | `C:\repos\Projects\DNA` | `C:\repos\Projects\DNA-wt-<name>` |

### Case Sensitivity

macOS (APFS default) and Windows (NTFS) are case-insensitive. Linux (ext4) is case-sensitive. If you have files that differ only by case, you'll hit problems on macOS/Windows. Avoid this in your codebase.

```bash
# Check your repo for case conflicts
git ls-files | sort -f | uniq -di
```

### Symlinks

If your project uses symlinks, be aware:

- **Linux:** Full support
- **macOS:** Full support
- **Windows:** Requires Developer Mode or elevated permissions

```bash
# On Windows, enable symlink support
git config --global core.symlinks true
```

---

## IDE and Editor Integration

### Visual Studio (Windows)

- Each worktree is a separate directory — open the `.sln` file from the worktree path
- VS maintains separate `bin/`, `obj/`, and `.vs/` per worktree automatically
- You can have multiple VS instances open, each pointing to a different worktree
- **Tip:** Use "Open Recent" to quickly switch between worktree solutions

### Visual Studio Code

- Open each worktree as a separate window: `code /path/to/worktree`
- VS Code workspace files (`.code-workspace`) are per-directory, so each worktree can have its own
- The Source Control panel shows the correct branch for each worktree window
- **Multi-root workspace:** You can add multiple worktrees to a single workspace for comparison

```bash
# Open two worktrees side by side
code /mnt/workspace/repos/Projects/DNA
code /mnt/workspace/repos/Projects/DNA-wt-feature
```

### JetBrains Rider

- Open each worktree as a separate project
- Rider's solution-wide analysis runs independently per worktree
- NuGet caches are shared system-wide, so restore is fast across worktrees

### Cursor / AI-Assisted Editors

- Each worktree gets its own `.cursor/` or equivalent config directory
- AI context is scoped to the worktree's files — no cross-contamination between branches
- `.cursorrules` at the worktree root apply correctly per worktree

---

## GitHub Pro Workflows

### Worktrees + GitHub CLI

The `gh` CLI works naturally within worktrees since each worktree knows its branch:

```bash
cd /mnt/workspace/repos/Projects/DNA-wt-feature

# Create a PR from the worktree's branch
gh pr create --title "feat: phase 7 admin UI" --base main

# Check CI status
gh pr checks

# View PR in browser
gh pr view --web
```

### Worktrees + GitHub Actions

If you use self-hosted runners, worktrees can speed up CI:

- Keep a persistent worktree per active PR branch on the runner
- Avoid full clones on every run — just fetch and reset

### Draft PR + Worktree Pattern

A productive pattern for complex features:

1. Create a worktree for the feature branch
2. Push the branch and open a **draft PR** immediately
3. Work in the worktree, pushing commits as you go
4. CI runs on every push — you get feedback without blocking `main`
5. When ready, mark the PR as "Ready for Review"
6. After merge, remove the worktree and delete the branch

```bash
# All in one flow
git worktree add -b feature/new-thing ../DNA-wt-new-thing main
cd ../DNA-wt-new-thing
git push -u origin feature/new-thing
gh pr create --draft --title "feat: new thing" --base main
```

### Protected Branches and Worktrees

With GitHub Pro, you likely have branch protection on `main`. Worktrees make this workflow natural:

- `main` worktree is read-only in practice (you never commit directly)
- All work happens in feature worktrees
- Merges happen through PRs on GitHub, not local `git merge`

---

## Multi-Machine Strategies

### Shared Worktree Layout

Use the same relative structure on every machine so muscle memory transfers:

```
<workspace-root>/
├── DNA/                    # Primary clone (main)
├── DNA-wt-<feature>/      # Feature worktrees
└── DNA-wt-review/         # Temporary review worktree
```

### Syncing Work Across Machines

Worktrees are local — they don't sync between machines. Your workflow:

1. **Push your branch** from the worktree on Machine A
2. On Machine B, **fetch and create a worktree** for the same branch:

```bash
git fetch origin
git worktree add ../DNA-wt-feature feature/same-branch
```

The branch and commits travel through the remote. The worktree is recreated locally.

### Machine-Specific Scripts

Create a helper script for consistent worktree creation across machines:

```bash
#!/usr/bin/env bash
# wt-create.sh — Create a DNA worktree with submodules
# Usage: ./wt-create.sh <branch-name> [start-point]

set -euo pipefail

BRANCH="${1:?Usage: wt-create.sh <branch-name> [start-point]}"
START="${2:-main}"
REPO_ROOT="$(git rev-parse --show-toplevel)"
PARENT_DIR="$(dirname "$REPO_ROOT")"
WT_DIR="${PARENT_DIR}/DNA-wt-${BRANCH##*/}"

if git show-ref --verify --quiet "refs/heads/$BRANCH"; then
    git worktree add "$WT_DIR" "$BRANCH"
else
    git worktree add -b "$BRANCH" "$WT_DIR" "$START"
fi

echo "Initializing submodules..."
cd "$WT_DIR"
git submodule update --init --recursive

echo ""
echo "Worktree ready at: $WT_DIR"
echo "Branch: $BRANCH"
```

```bash
# Make it executable and use it
chmod +x scripts/wt-create.sh
./scripts/wt-create.sh feature/phase9-cleanup
./scripts/wt-create.sh hotfix/urgent-fix main
```

### Worktree Teardown Script

```bash
#!/usr/bin/env bash
# wt-remove.sh — Remove a DNA worktree cleanly
# Usage: ./wt-remove.sh <worktree-name-or-path>

set -euo pipefail

TARGET="${1:?Usage: wt-remove.sh <worktree-name-or-path>}"
REPO_ROOT="$(git rev-parse --show-toplevel)"
PARENT_DIR="$(dirname "$REPO_ROOT")"

# If just a name was given, construct the path
if [[ "$TARGET" != /* ]]; then
    TARGET="${PARENT_DIR}/DNA-wt-${TARGET##*/}"
fi

if [ ! -d "$TARGET" ]; then
    echo "Worktree not found: $TARGET"
    exit 1
fi

# Show status before removing
echo "Worktree: $TARGET"
cd "$TARGET"
if [ -n "$(git status --porcelain)" ]; then
    echo "WARNING: Uncommitted changes detected:"
    git status --short
    read -p "Remove anyway? [y/N] " confirm
    [[ "$confirm" == [yY] ]] || exit 0
fi

cd "$REPO_ROOT"
git worktree remove "$TARGET" --force
echo "Removed: $TARGET"
```

---

## Performance and Housekeeping

### Disk Usage

Each worktree duplicates the **working directory** but shares the git object database. For a project like DNA:

- Git objects (shared): stored once
- Working files per worktree: full copy of checked-out files
- Build artifacts per worktree: `bin/`, `obj/`, `node_modules/` — these add up

**Mitigations:**

```bash
# Check worktree disk usage
du -sh /mnt/workspace/repos/Projects/DNA-wt-*

# Clean build artifacts in a worktree before removing
cd /path/to/worktree
dotnet clean
rm -rf **/bin **/obj

# Then remove the worktree
cd /mnt/workspace/repos/Projects/DNA
git worktree remove ../DNA-wt-old-feature
```

### Regular Maintenance

```bash
# Prune worktrees whose directories no longer exist
git worktree prune

# Run git maintenance (gc, packfiles, etc.)
git maintenance run

# List all worktrees to audit
git worktree list
```

### Don't Hoard Worktrees

A common mistake is creating worktrees and forgetting about them. Set a personal rule:

- **Active feature work:** 1-3 worktrees max
- **Review worktrees:** Create and destroy per-review
- **Hotfix worktrees:** Create when needed, destroy after merge

### NuGet Cache Sharing

NuGet packages are cached globally (`~/.nuget/packages`), so multiple worktrees don't re-download packages. Only `dotnet restore` runs per worktree to generate `project.assets.json`.

---

## Troubleshooting

### "fatal: '<branch>' is already checked out at '<path>'"

A branch can only be in one worktree. Solutions:

```bash
# Check which worktree has the branch
git worktree list

# Option 1: Remove the other worktree first
git worktree remove /path/to/other/worktree

# Option 2: Check out a different branch in the other worktree
cd /path/to/other/worktree
git checkout different-branch

# Option 3: Use detached HEAD if you just need to inspect
git worktree add --detach /path/to/new-worktree <commit-or-branch>
```

### Worktree Shows as "prunable"

This means the worktree directory was deleted without using `git worktree remove`:

```bash
git worktree prune
```

### Submodules Not Appearing in Worktree

Submodules are not auto-initialized in new worktrees:

```bash
cd /path/to/worktree
git submodule update --init --recursive
```

### IDE Shows Wrong Branch

Some IDEs cache git state. Solutions:

- Restart the IDE
- Ensure you opened the **worktree** directory, not the main repo directory
- Check that `.git` in the worktree root is a file (not a directory) pointing to the main repo — this is normal

### Lock Conflicts

If a worktree is on a network drive or shared filesystem and another process might interfere:

```bash
# Lock a worktree to prevent pruning
git worktree lock /path/to/worktree --reason "In use on remote machine"

# Unlock when done
git worktree unlock /path/to/worktree
```

### Build Errors After Creating Worktree

The worktree starts clean — no build artifacts. Always:

```bash
cd /path/to/new/worktree
dotnet restore
dotnet build
```

### Windows: "filename too long" Errors

```powershell
git config --global core.longpaths true
```

And ensure the Windows registry setting for long paths is enabled (see [Cross-Platform Considerations](#cross-platform-considerations)).

---

## Quick Reference

### Essential Commands

| Command | Description |
|---|---|
| `git worktree add <path> <branch>` | Create worktree from existing branch |
| `git worktree add -b <new> <path> <start>` | Create worktree with new branch |
| `git worktree add --detach <path> <ref>` | Create worktree in detached HEAD |
| `git worktree list` | List all worktrees |
| `git worktree remove <path>` | Remove a worktree |
| `git worktree remove --force <path>` | Force-remove (uncommitted changes) |
| `git worktree prune` | Clean up stale worktree references |
| `git worktree lock <path>` | Prevent worktree from being pruned |
| `git worktree unlock <path>` | Unlock a locked worktree |
| `git worktree move <path> <new-path>` | Move a worktree to a new location |

### Cheat Sheet for DNA Project

```bash
# Create a feature worktree with submodules
git worktree add ../DNA-wt-myfeature -b feature/myfeature main
cd ../DNA-wt-myfeature && git submodule update --init --recursive

# Quick PR review
git fetch origin pull/42/head:pr-42
git worktree add ../DNA-wt-review pr-42
cd ../DNA-wt-review && dotnet build && dotnet test

# Emergency hotfix
git worktree add ../DNA-wt-hotfix -b hotfix/critical main
cd ../DNA-wt-hotfix
# ... fix, commit, push ...
gh pr create --title "fix: critical issue" --base main

# Clean up
git worktree remove ../DNA-wt-review
git worktree remove ../DNA-wt-hotfix

# Audit and maintain
git worktree list
git worktree prune
```

### Shell Aliases

Add these to your shell profile (`~/.bashrc`, `~/.zshrc`, or PowerShell `$PROFILE`):

```bash
# Bash/Zsh aliases
alias gwl='git worktree list'
alias gwa='git worktree add'
alias gwr='git worktree remove'
alias gwp='git worktree prune'

# Quick worktree creation function
gwn() {
    local branch="$1"
    local start="${2:-main}"
    local name="${branch##*/}"
    local root="$(git rev-parse --show-toplevel)"
    local parent="$(dirname "$root")"
    local project="$(basename "$root")"
    git worktree add -b "$branch" "${parent}/${project}-wt-${name}" "$start"
    echo "Created: ${parent}/${project}-wt-${name}"
}
```

```powershell
# PowerShell aliases (add to $PROFILE)
function gwl { git worktree list }
function gwa { git worktree add @args }
function gwr { git worktree remove @args }
function gwp { git worktree prune }
```

---

## Summary

Git worktrees transform how you work on complex .NET projects:

- **No more context-switching tax** — each branch keeps its own build state
- **Parallel work is natural** — hotfix, review, and feature development coexist
- **Submodules work** — just remember to `git submodule update --init` in each worktree
- **Cross-platform friendly** — works identically on Linux, macOS, and Windows with the right config
- **Pairs well with GitHub Pro** — draft PRs, branch protection, and `gh` CLI integrate seamlessly

Start small: keep `main` in your primary clone and create one worktree for your current feature branch. Once that feels natural, expand to hotfix and review worktrees as needed.
