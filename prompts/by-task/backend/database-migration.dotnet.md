# Task: Database Migration for .NET

## Best Fit

Use this for EF Core or other .NET data access stacks where entity, DbContext, migration, and query changes must stay aligned.

## Prompt

```text
@Data/[DbContextFile].cs
@Models/[RelevantEntity].cs
@Migrations/[LatestMigration].cs

Create a database migration for the following schema change:

Change description: [what is changing]
Tables affected: [table names]
New columns:
- [ColumnName] [Type] [Nullable?] [Default?] [Constraints]

Relationships:
- [describe foreign keys and navigation properties]

Indexes:
- [query patterns and indexes needed]

Requirements:
1. Update the entity class or configuration
2. Update the DbContext and Fluent API configuration
3. Generate a descriptive migration name
4. Include a Down() method that cleanly reverses the change
5. Update any query, repository, or handler code affected by the schema
6. Add comments for any PHI or sensitive columns that require encryption or special handling

Do not run the migration. Generate the files only.
```

## Notes

- Prefer Fluent API over scattered entity annotations when the repo already uses it.
- Review generated SQL carefully for destructive operations.
- Call out staged rollout needs if old and new code must coexist during deployment.
