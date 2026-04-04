---
name: ci-ephemeral-environments
description: Designs CI/CD workflows and ephemeral environment strategies for safer delivery. Use when creating Azure Pipelines or GitHub Actions, reviewing deployment workflows, or planning preview and short-lived environments.
---
# CI and Ephemeral Environments

## Quick Start

When this skill applies:

1. Read the current pipeline or deployment flow.
2. Identify the actual delivery pain: slow feedback, risky deploys, shared environment collisions, or weak rollback.
3. Recommend the smallest workflow change that improves safety and speed.
4. Include lifecycle and cleanup rules for any ephemeral environment proposal.

## Workflow

### 1. Understand the delivery model

Map:
- build steps,
- test gates,
- artifact flow,
- deployment targets,
- approval points,
- environment ownership.

### 2. Design the workflow

For CI/CD changes:
- separate validation from deployment,
- use placeholders for environment-specific secrets,
- keep outputs readable and reviewable.

For ephemeral environments:
- define create, seed, validate, review, and teardown steps,
- include TTL and cleanup ownership,
- keep cost and environment sprawl visible.

### 3. Review for operational risk

Check for:
- secret leakage,
- weak approval boundaries,
- unsafe defaults,
- missing rollback paths,
- and fragile environment assumptions.

### 4. Produce an adoption path

Give:
- first step,
- dependencies,
- risks,
- and what should remain manual at first.

## Good Output

Good output includes:
- workflow YAML or lifecycle guidance,
- placeholders called out clearly,
- operational risks,
- and explicit next steps for the team.

## Related Prompts

- `../../prompts/by-task/ci-cd/azure-pipelines-dotnet-service.md`
- `../../prompts/by-task/ci-cd/github-actions-pr-automation.md`
- `../../prompts/by-task/environments/ephemeral-environment-plan.md`
