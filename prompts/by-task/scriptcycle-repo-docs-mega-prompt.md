ScriptCycle Repository Documentation Prompt (Mega Prompt)

You are a Principal Engineer generating production-ready documentation for this repository.
Accuracy is mandatory. Do NOT guess or fabricate. Only state what is proven by repository evidence.
Follow GoodRx/ScriptCycle conventions: HIPAA awareness, no secrets, align with .cursor/rules
and ai_readiness practices if present.
Use Mermaid for diagrams whenever appropriate; avoid ASCII/text diagrams.
Architecture diagrams must follow the C4 model and be rendered in Mermaid.
All Markdown output MUST be UTF-8 encoded with LF line endings (GitHub preference).

SCOPE AND OUTPUT GOALS
- Produce a README that explains what the project is and how to get started quickly.
- Produce a docs runbook with the full, detailed setup/run procedures.
- Preserve good existing documentation. Do not discard content without reason.
- If existing documentation is already in a sensible location, leave it there.
- If it would be more sensible alongside the new docs set (docs/Docs/Documentation),
  move it there and update links accordingly.
- Do not move content to a less convenient location.
- Preserve Contact/Support info in the root README when present (Slack, email, team).

DOCS LOCATION AND CASING RULES
- Use the repo’s existing docs root casing (docs/ or Docs/). Do NOT create both.
- If Documentation/ is already the convention, keep using it consistently.
- For scriptcycle-tx and scriptcycle-switch-ui, keep Docs/ (capital D).
- If you must move legacy docs, move to the chosen docs root (docs/ or Docs/).

ALLOWED FILE CHANGES ONLY (DOCS-ONLY WORK)
- Root README.md
- docs/ or Docs/ or Documentation/ (as appropriate)
- .cursorignore (only if needed to avoid excluding docs)
- .cursor/rules/*
- Legacy doc files that are moved into the docs root
If any non-doc files change, revert them immediately and note it.

PHASE 1 - REPOSITORY INVENTORY (REQUIRED)
1) Identify repo type: Application / Library / Monorepo / Infrastructure-only.
2) Enumerate evidence sources:
   - Project files (e.g., package.json, pyproject.toml, go.mod, *.sln/*.csproj)
   - Build tooling (Makefile, justfile, scripts/)
   - CI/CD workflows
   - Docker files / compose
   - Config files (appsettings*, launchSettings*, .env.example, etc.)
   - .cursor/rules and docs/ai_readiness.md (if present)
   - Existing docs (README, docs/Docs/Documentation, component READMEs)
3) Create an EVIDENCE LOG listing:
   - File path
   - What it proves (language, framework, commands, runtime, entrypoint, etc.)
If evidence is insufficient for any later section, use:
TBD - See <specific file path>
Do not proceed until inventory is complete.

PHASE 2 - SAFETY & VALIDATION RULES
- Do NOT invent commands.
- Only include commands that exist in scripts/targets, workflows, Dockerfiles, or docs.
- If multiple project files exist, map each command to its project path.
- If monorepo, separate instructions per service/project.
- Env vars: NAMES ONLY, and only those found in code/config/workflows.
- External services: names only.
- Never include secrets, tokens, or PHI.
- Configuration keys (appsettings/config files): describe purpose only if documented in
  code/comments/docs; otherwise state "Purpose not documented" with a file pointer.
- If migrations exist, document how they are executed and where files live.
- If CI/CD exists, summarize what workflows actually do, based on YAML.
- Use UTC unless evidence indicates another timezone.
- Ensure Markdown outputs are UTF-8 (rewrite if existing files are not UTF-8).
- Ensure docs markdown is not excluded by .cursorignore; add exceptions if necessary.

PHASE 3 - SCRIPTCYCLE DRIFT & RISK CHECKS
Explicitly check and document when present:
- Multiple startup projects
- CI test execution
- Docker overrides local run behavior
- EF Core migrations
- Background services / hosted workers
- External dependencies (Redis/SQL/queues/etc.)
- AI readiness / PHI handling (docs/ai_readiness.md, HIPAA notes)
- Health checks, monitoring/observability links, and incident-response entrypoints

PHASE 4 - CONTENT PLACEMENT RULES
- Keep README in repo root; all other docs should be in the chosen docs root
  unless they are clearly scoped to a specific subcomponent and are more useful
  near that component (e.g., component-level README).
- If a doc in root (e.g., ARCHITECTURE.md, CONTRIBUTING.md, Deployment/README.md,
  EventFeeders/*.md) would be clearer in the docs root, move it and update links.
- If a doc already lives in a location that is clear and convenient, keep it there
  and link to it from docs/README.md.
- Do not lose content when moving; preserve and update links.

PHASE 5 - OUTPUT FORMAT
Output clean, commit-ready Markdown.
Use Mermaid for diagrams (flowcharts, sequences, and C4).
Use Markdown links (not just inline code) for doc navigation in README/docs index.

README.md sections (omit if no evidence or if the section would be empty/TBD):
# <Repository Name>
## Overview
## Architecture
## Key Directories
## Tech Stack
## Prerequisites
## Configuration
## Setup (short quickstart only)
## Run Locally (short quickstart only)
## Testing
## Database / Migrations
## CI/CD
## Observability (or Monitoring, if that is the repo convention)
## Common Developer Tasks
## Troubleshooting
## Contributing
## Contact / Support (required if present in original README)

docs/ outputs (if docs root exists or should be created):
- docs/README.md (or Docs/README.md) index with links
- docs/ai_readiness.md (required for PHI repos; otherwise include if present)
- docs/Architecture/C4-System-Architecture.md (C4 model using Mermaid)
- docs/local-environment-setup.md (detailed setup)
- docs/runbook.md (full setup/run/runbook detail)
- docs/troubleshooting.md
If Tiltfile exists, include Tilt instructions in docs/runbook.md or a docs/tilt.md file.
If entrypoints exist (boot, HTTP/gRPC/TCP, queues, cron), include Mermaid flowcharts
for them in architecture docs.
If a file or section lacks evidence, write:
TBD - Insufficient repository evidence. See <specific file path>

PHASE 6 - FINAL QUALITY GATE
Verify:
- Every command exists in evidence
- Every project path exists
- Every env var name appears in code/config
- No speculation or secrets
- Output is clean and commit-ready
- Markdown files are UTF-8 with LF
Remove the Evidence Log from final output unless explicitly requested.
