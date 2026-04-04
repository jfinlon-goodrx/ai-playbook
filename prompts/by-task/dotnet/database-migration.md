# Task: Database Migration

## When to Use
You need to add or modify database tables, columns, indexes, or relationships. Covers EF Core migrations, raw SQL migrations, and data seeding.

## Prerequisites
- [ ] Database context file identified
- [ ] Existing migration files available for pattern reference
- [ ] Understanding of whether the project uses EF Core Code-First or Database-First

## The Prompt (EF Core Code-First)

```
@Data/ApplicationDbContext.cs
@Models/[RelevantEntity].cs
@Migrations/[LatestMigration].cs

Create a database migration for the following schema change:

**Change description:** [What you're adding/modifying/removing]
**Table(s) affected:** [Table names]
**New columns:**
- [ColumnName] [Type] [Nullable?] [Default?] [Constraints]

**Relationships:**
- [Describe any foreign keys or navigation properties]

**Indexes:**
- [Any indexes needed for query performance]

Requirements:
1. Add or modify the entity class(es) in the Models folder
2. Update the DbContext with any new DbSet<T> or configuration
3. Use Fluent API configuration in OnModelCreating (not data annotations) for:
   - Column types and lengths
   - Required/optional
   - Indexes
   - Relationships
4. Generate the migration file name: [YYYYMMDD]_[DescriptiveName]
5. Include a Down() method that cleanly reverses the migration
6. If any columns contain PHI, add a comment noting encryption requirements

Do NOT run the migration — just create the files. I'll review and run it manually.
```

## Example: Adding Refill Tracking

```
@Data/PharmacyDbContext.cs
@Models/Prescription.cs
@Migrations/

Create a database migration to add refill tracking:

**New table: RefillRequests**
- Id (Guid, PK, default newsequentialid())
- PrescriptionId (Guid, FK to Prescriptions, required)
- PharmacyId (Guid, FK to Pharmacies, required)
- RequestedQuantity (int, required)
- Status (string, max 50, required, default "Pending") — values: Pending, Approved, Denied, Filled
- RequestedAt (DateTimeOffset, required, default UTC now)
- ProcessedAt (DateTimeOffset, nullable)
- ProcessedBy (Guid, nullable, FK to Users)
- Notes (string, max 500, nullable) — PHI: may contain clinical notes, encrypt at rest
- DenialReason (string, max 500, nullable)

**Modify existing table: Prescriptions**
- Add column: RemainingRefills (int, required, default 0)
- Add column: LastRefillDate (DateTimeOffset, nullable)

**Indexes:**
- IX_RefillRequests_PrescriptionId (for lookup by prescription)
- IX_RefillRequests_PharmacyId_Status (for pharmacy dashboard queries)
- IX_RefillRequests_RequestedAt (for date-range queries)

**Relationships:**
- RefillRequest.Prescription → Prescription (many-to-one)
- RefillRequest.Pharmacy → Pharmacy (many-to-one)
- RefillRequest.ProcessedByUser → User (many-to-one, optional)
- Prescription.RefillRequests → collection navigation property

Use Fluent API for all configuration. The Notes column contains PHI — add a comment
in the entity and migration noting it requires encryption at rest per HIPAA policy.
```

## Prompt: Raw SQL Migration (for Dapper / non-EF projects)

```
Write a SQL migration script for MSSQL that:

**Forward migration (UP):**
[Describe the schema changes]

**Rollback migration (DOWN):**
[The exact reverse of the forward migration]

Requirements:
- Use IF NOT EXISTS checks for idempotency
- Wrap in a transaction
- Include PRINT statements for progress
- Use proper MSSQL data types (nvarchar not varchar for Unicode, datetimeoffset not datetime)
- Add comments for any PHI columns noting encryption requirements

Format:
-- Migration: [Name]
-- Date: [Today's date]
-- Description: [What this does]

BEGIN TRANSACTION
[statements]
COMMIT
```

## Prompt: Data Seeding

```
@Data/ApplicationDbContext.cs
@Models/[Entity].cs

Create a data seed for the [Entity] table with the following reference data:

[List the seed data]

Requirements:
- Use HasData() in the Fluent API configuration
- Use deterministic GUIDs (create them from a namespace + name hash) so seeds are idempotent
- Do NOT include any real patient data or PHI — use clearly fake data
- Mark test data with obvious names (e.g., "Test Pharmacy #1", not "CVS")
```

## Tips
- Always ask the AI to generate the Down() method — it often forgets
- Review the generated SQL in the migration carefully, especially for data loss operations (dropping columns)
- For PHI columns, manually verify that encryption-at-rest is configured at the database level — the migration creates the column but the DBA configures TDE/column-level encryption
- If you're adding indexes, include the expected query patterns in the prompt so the AI can optimize the index design
- Always test the migration on a dev database before applying to staging/production
