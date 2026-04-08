# Task: Database Migration for Python

## Best Fit

Use this for Alembic, Django migrations, or another Python migration framework.

## Prompt

```text
@[model file]
@[migration file or migrations directory]
@[repository, ORM, or query file]
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
1. Update the model definitions
2. Generate the migration using the repo's framework pattern
3. Include the rollback path
4. Update ORM queries, repositories, or serializers affected by the change
5. Call out any backfill, dual-read, or deployment sequencing issues
6. Mark sensitive columns that need encryption or restricted logging

Do not apply the migration. Generate reviewable files only.
```

## Notes

- Alembic projects should usually keep schema changes explicit and readable in the revision file.
- Django projects should keep model and migration intent easy to review.
- If the change requires data migration, ask for a two-phase plan when risk is high.
