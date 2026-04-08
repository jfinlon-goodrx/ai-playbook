# Task: Database Migration

## When to Use

You need to add or modify tables, columns, indexes, relationships, or seed data and want the assistant to plan the schema change, rollback path, and verification work safely.

## Use With

- `.dotnet`: `database-migration.dotnet.md`
- `.python`: `database-migration.python.md`
- `.go`: `database-migration.go.md`

## Prompt: Plan the Migration

```text
Read the referenced data-layer files first, then plan a database migration.

Repo context:
@[data model or schema file]
@[latest migration]
@[repository or query code]
@[project context or rules file]

Schema change:
- Change description: [what is changing]
- Tables affected: [list]
- New columns: [type, nullability, default, constraints]
- Relationships: [foreign keys or logical references]
- Indexes: [query patterns to support]
- Data backfill needs: [yes/no and what]
- Sensitive fields: [PII, PHI, financial, secrets]

Produce:
1. The schema change plan
2. The migration or SQL files to create or edit
3. The application code that must change with the schema
4. The rollback approach
5. Verification steps before and after deployment
6. Risks and operational notes
```

## Prompt: Generate the Migration

```text
Using the approved plan, generate the migration files and any required model or query updates.

Requirements:
- include an explicit rollback path
- avoid unsafe destructive changes without calling them out
- preserve compatibility if a staged rollout is needed
- note any backfill or operational sequencing
- call out encryption, retention, or audit requirements for sensitive fields

Do not execute the migration. Generate reviewable artifacts only.
```

## Review Checklist

- Can the change be rolled back cleanly?
- Does the migration match real query patterns and indexes?
- Is sensitive data handled explicitly?
- Is there a staged rollout or dual-write concern?
- Is application compatibility preserved during deployment?
