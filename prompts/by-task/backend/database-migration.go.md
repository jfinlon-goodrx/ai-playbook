# Task: Database Migration for Go

## Best Fit

Use this for SQL-first Go services using migration tools such as Goose, Atlas, Flyway, or custom SQL migrations.

## Prompt

```text
@[schema or SQL migration file]
@[repository or query file]
@[struct or model file]
@[project context or rules file]

Create a database migration for the following schema change:

Change description: [what is changing]
Tables affected: [table names]
New columns:
- [ColumnName] [Type] [Nullable?] [Default?] [Constraints]

Relationships:
- [foreign keys or logical references]

Indexes:
- [query patterns and indexes needed]

Requirements:
1. Generate the forward migration
2. Generate the rollback migration
3. Update any structs, queries, repository code, or scan logic affected by the schema change
4. Call out backfill, rollout ordering, and compatibility concerns
5. Flag sensitive columns that need encryption or restricted logging

Do not run the migration. Generate reviewable SQL and code changes only.
```

## Notes

- Prefer explicit SQL that is easy to audit.
- If the repo uses generated query layers, update the inputs those tools depend on.
- Call out zero-downtime constraints for services with live traffic.
