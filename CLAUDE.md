# CLAUDE.md

## Repository Mission

This repository is a practical playbook for AI-assisted software delivery.

It is intended to help an Agile SDLC team get productive with AI across:

- implementation
- testing
- review
- CI/CD
- incident response
- team operating model change

The repo began with a strong .NET/Azure focus, but it now aims to support mixed-language teams. Treat .NET, Python, and Go as first-class implementation targets when creating or revising example-driven content.

## Primary Audiences

- developers
- QA and test automation engineers
- DevOps and platform engineers
- tech leads and architects
- engineering managers
- product and delivery partners

## How To Extend This Repo

When adding or revising content:

1. Prefer one canonical workflow page for shared guidance.
2. Add language-specific companion files only where implementation details differ.
3. Make the filename explicit when a file is stack-specific.
4. Cross-link rather than duplicating the same guidance in multiple places.
5. Keep the repo practical, not theoretical.

## Language Variant Rule

When a workflow needs examples for multiple implementation stacks, use this naming pattern:

- `topic.md`
- `topic.dotnet.md`
- `topic.python.md`
- `topic.go.md`

Examples:

- `new-api-endpoint.md`
- `new-api-endpoint.dotnet.md`
- `new-api-endpoint.python.md`
- `new-api-endpoint.go.md`

Do not mechanically translate C# examples into Python or Go. Adapt the workflow to idiomatic tooling and conventions for that ecosystem.

## Content Priorities

Favor:

- real task workflows
- prompts that generate reviewable artifacts
- skills that encode repeatable team procedures
- onboarding paths by role
- examples that improve delivery quality and speed together

Avoid:

- generic AI hype
- duplicate pages with minor wording changes
- stack-specific content framed as if it applies to everyone
- large rewrites that break navigation without adding redirects or links

## Current Structure

Use these sections intentionally:

- `handbook/` for canonical concepts, maps, and operating model guidance
- `Agile-AI/` for AI-native delivery model and team transformation content
- `0-to-60-ai/` for practical learning paths and role-based education
- `quick-starts/` for fast entry points by role or need
- `prompts/` for reusable prompt assets and patterns
- `skills/` for repeatable workflow definitions
- `guides-by-platform/` for editor/tool guidance
- `cursorrules/` for reusable project rule examples

## Foundation Context Files

Agents should use these files first when working in this repo:

- `AGENTS.md`
- `CLAUDE.md`
- `repo-context.yaml`
- `.cursor/rules/*.mdc`
- `REPO-ENHANCEMENT-PLAN.md`

## Safe Editing Guidance

- preserve useful existing content unless a clearer canonical replacement exists
- keep diffs reviewable
- prefer improving navigation over moving everything at once
- add "See also" or legacy-path notes when normalizing older docs
- keep guardrails, testing, and verification close to the workflow being documented

## Best Immediate Moves For Agents

If asked to improve the repo further, do the following in order:

1. improve top-level navigation
2. normalize the highest-value workflow docs into shared plus language-specific variants
3. strengthen role-based quick starts
4. add cross-links and reduce duplication
5. add real case studies and worked examples
