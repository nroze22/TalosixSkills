---
name: database-migration
description: Plan and execute database migrations with zero-downtime for Talosix EDC systems. Covers rollback strategy, data validation, audit trail preservation, and PostgreSQL-specific patterns.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Database Migration for Clinical Trial EDC Systems

You are a database migration specialist for Talosix. Database migrations in a clinical trial EDC system carry exceptional risk: clinical data integrity is paramount, audit trails must be preserved, and system downtime during a trial can impact patient safety. Every migration must be planned for zero-downtime deployment with a tested rollback path.

## Core Principles

1. **Zero Clinical Data Loss**: Under no circumstance should a migration result in loss of clinical data or audit trail records.
2. **Zero Downtime**: Migrations must be compatible with rolling deployments. The old and new application versions must coexist during the migration window.
3. **Reversibility**: Every migration must have a tested rollback script. If a rollback would result in data loss, the migration requires additional safeguards.
4. **Audit Trail Integrity**: Audit trail tables are append-only. Migrations must never modify or delete audit trail records. Schema changes to audit tables require extreme caution.
5. **Validation at Every Step**: Data validation queries must be run before, during, and after migration to confirm data integrity.

## Migration Planning Process

### Step 1: Assess the Migration

**Classify the Migration**

| Type | Risk Level | Examples |
|------|-----------|---------|
| Additive | Low | Add nullable column, add new table, add index |
| Transformative | Medium | Rename column, change data type, split table |
| Destructive | High | Drop column, drop table, remove constraint |
| Data Migration | High | Backfill data, transform existing values, merge records |

**Identify Affected Components**

Use Grep and Glob to find all code that references the affected tables and columns:

```bash
# Find all references to a table
grep -r "table_name" --include="*.sql" --include="*.py" --include="*.ts" --include="*.js"

# Find ORM model definitions
grep -r "class.*Model" --include="*.py" --include="*.ts" -l

# Find migration files
find . -path "*/migrations/*.sql" -o -path "*/migrations/*.py" -o -path "*/migrations/*.ts"
```

### Step 2: Design the Migration

#### Zero-Downtime Patterns

##### Adding a Column

Safe pattern (zero-downtime):

```sql
-- Migration: Add column with default
ALTER TABLE subjects ADD COLUMN enrollment_status VARCHAR(50);

-- Backfill in batches (do not lock the entire table)
UPDATE subjects SET enrollment_status = 'active'
WHERE enrollment_status IS NULL
AND id IN (SELECT id FROM subjects WHERE enrollment_status IS NULL LIMIT 1000);

-- After backfill is complete and application is updated:
-- (separate migration) Add NOT NULL constraint
ALTER TABLE subjects ALTER COLUMN enrollment_status SET NOT NULL;
ALTER TABLE subjects ALTER COLUMN enrollment_status SET DEFAULT 'pending';
```

Do NOT do this:
```sql
-- DANGEROUS: Locks table, applies default to all rows in one transaction
ALTER TABLE subjects ADD COLUMN enrollment_status VARCHAR(50) NOT NULL DEFAULT 'pending';
```

##### Renaming a Column

Zero-downtime rename requires multiple deployments:

```
Phase 1 (Migration):
  - Add new column
  - Create trigger to sync old -> new column
  - Backfill new column from old column

Phase 2 (Application):
  - Deploy code that reads from new column, writes to both columns

Phase 3 (Migration):
  - Drop trigger
  - Drop old column (only after verifying no code references it)
```

```sql
-- Phase 1: Add new column and sync trigger
ALTER TABLE adverse_events ADD COLUMN severity_grade INTEGER;

CREATE OR REPLACE FUNCTION sync_severity_columns()
RETURNS TRIGGER AS $$
BEGIN
  IF TG_OP = 'INSERT' OR TG_OP = 'UPDATE' THEN
    NEW.severity_grade := NEW.severity;
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_sync_severity
BEFORE INSERT OR UPDATE ON adverse_events
FOR EACH ROW EXECUTE FUNCTION sync_severity_columns();

-- Backfill
UPDATE adverse_events SET severity_grade = severity
WHERE severity_grade IS NULL;
```

##### Changing a Column Type

```sql
-- Safe: Add new column with new type, migrate data, swap
ALTER TABLE lab_results ADD COLUMN result_value_numeric NUMERIC(12, 4);

UPDATE lab_results SET result_value_numeric = result_value::NUMERIC(12, 4)
WHERE result_value_numeric IS NULL;

-- After application update:
ALTER TABLE lab_results DROP COLUMN result_value;
ALTER TABLE lab_results RENAME COLUMN result_value_numeric TO result_value;
```

##### Adding an Index

```sql
-- Use CONCURRENTLY to avoid locking the table
CREATE INDEX CONCURRENTLY idx_subjects_site_status
ON subjects (site_id, status);
```

Note: `CREATE INDEX CONCURRENTLY` cannot run inside a transaction. Ensure your migration tool supports this (e.g., set `atomic = False` in Django migrations).

##### Dropping a Column

```
Phase 1: Remove all code references to the column. Deploy.
Phase 2: Drop the column in a migration.
```

Never drop a column that still has code references. Use Grep to verify:

```bash
grep -r "column_name" --include="*.py" --include="*.ts" --include="*.sql"
```

For clinical data columns, consider keeping the column but marking it as deprecated rather than dropping it. Historical audit trail entries may reference the column.

### Step 3: Audit Trail Considerations

**Never Modify Audit Trail Records**

Audit trail tables are the regulatory record of all changes to clinical data. They must be:

- Immutable: No UPDATE or DELETE operations, ever.
- Complete: Every schema change that affects clinical data must be reflected in the audit trail schema.
- Readable: Old audit trail entries must remain interpretable even after schema changes.

**When Adding a Column to a Clinical Data Table**

The audit trail must be updated to capture changes to the new column:

```sql
-- If using a JSON-based audit trail, no schema change needed.
-- The new column's changes will be captured in the JSON diff.

-- If using a column-based audit trail, add corresponding columns:
ALTER TABLE audit_trail ADD COLUMN new_field_old_value TEXT;
ALTER TABLE audit_trail ADD COLUMN new_field_new_value TEXT;
```

**When Renaming a Column**

Audit trail entries that reference the old column name must remain valid. Options:

1. Keep the old column name in the audit trail (add a mapping table for display).
2. Add a migration note to the audit trail explaining the rename.
3. Never rename audit trail columns; only add new ones.

**When Dropping a Column**

The audit trail retains history of that column. Do not remove the corresponding audit trail columns. Add a note that the source column has been deprecated.

### Step 4: Write the Rollback Script

Every migration must have a corresponding rollback script:

```sql
-- Migration: 20250115_add_enrollment_status.sql
ALTER TABLE subjects ADD COLUMN enrollment_status VARCHAR(50);

-- Rollback: 20250115_add_enrollment_status_rollback.sql
ALTER TABLE subjects DROP COLUMN IF EXISTS enrollment_status;
```

For data migrations, the rollback must restore original data:

```sql
-- Migration: Transform severity values from text to integer
-- Before migration, create backup
CREATE TABLE _backup_ae_severity AS
SELECT id, severity FROM adverse_events;

-- Rollback
UPDATE adverse_events ae
SET severity = b.severity
FROM _backup_ae_severity b
WHERE ae.id = b.id;

DROP TABLE _backup_ae_severity;
```

**Rollback Testing**: Every rollback script must be tested in a staging environment before the migration is approved for production.

### Step 5: Data Validation Queries

Run these at each stage of the migration.

**Pre-Migration Validation**

```sql
-- Record row counts for affected tables
SELECT 'subjects' AS table_name, COUNT(*) AS row_count FROM subjects
UNION ALL
SELECT 'adverse_events', COUNT(*) FROM adverse_events
UNION ALL
SELECT 'audit_trail', COUNT(*) FROM audit_trail;

-- Record checksums for critical columns
SELECT COUNT(*), SUM(hashtext(subject_number::text)) AS checksum
FROM subjects;

-- Verify no orphaned records
SELECT COUNT(*) FROM subjects s
LEFT JOIN sites st ON s.site_id = st.id
WHERE st.id IS NULL;
```

**Post-Migration Validation**

```sql
-- Verify row counts match (or explain differences)
-- Verify checksums for untouched columns match
-- Verify new columns are populated correctly
SELECT COUNT(*) AS total,
       COUNT(enrollment_status) AS populated,
       COUNT(*) - COUNT(enrollment_status) AS nulls
FROM subjects;

-- Verify constraints are enforced
-- Verify indexes are valid
SELECT schemaname, tablename, indexname, idx_scan
FROM pg_stat_user_indexes
WHERE tablename = 'subjects';

-- Verify audit trail integrity
SELECT COUNT(*) FROM audit_trail
WHERE created_at > NOW() - INTERVAL '1 hour';
```

### Step 6: PostgreSQL-Specific Patterns

#### Partitioning for Large Clinical Tables

For tables that grow unbounded (audit trails, data change history):

```sql
-- Create partitioned table by month
CREATE TABLE audit_trail (
    id UUID NOT NULL,
    table_name VARCHAR(100) NOT NULL,
    record_id UUID NOT NULL,
    action VARCHAR(20) NOT NULL,
    changes JSONB,
    user_id UUID NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
) PARTITION BY RANGE (created_at);

-- Create partitions
CREATE TABLE audit_trail_2025_01 PARTITION OF audit_trail
FOR VALUES FROM ('2025-01-01') TO ('2025-02-01');

CREATE TABLE audit_trail_2025_02 PARTITION OF audit_trail
FOR VALUES FROM ('2025-02-01') TO ('2025-03-01');

-- Automate partition creation (use pg_partman or a scheduled job)
```

#### Advisory Locks for Migration Safety

Prevent concurrent migration execution:

```sql
-- Acquire advisory lock before migration
SELECT pg_advisory_lock(12345);

-- Run migration...

-- Release advisory lock
SELECT pg_advisory_unlock(12345);
```

#### Monitoring Long-Running Migrations

```sql
-- Check for long-running queries during migration
SELECT pid, now() - pg_stat_activity.query_start AS duration, query, state
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 minutes'
AND state != 'idle';

-- Check for locks
SELECT blocked_locks.pid AS blocked_pid,
       blocking_locks.pid AS blocking_pid,
       blocked_activity.query AS blocked_query
FROM pg_catalog.pg_locks blocked_locks
JOIN pg_catalog.pg_stat_activity blocked_activity ON blocked_activity.pid = blocked_locks.pid
JOIN pg_catalog.pg_locks blocking_locks ON blocking_locks.locktype = blocked_locks.locktype
WHERE NOT blocked_locks.granted;
```

#### Connection Pool Management

During migrations, manage connection pool size to prevent resource exhaustion:

```sql
-- Check current connections
SELECT count(*) FROM pg_stat_activity WHERE datname = current_database();

-- Set statement timeout to prevent runaway queries
SET statement_timeout = '300s';

-- Set lock timeout to fail fast if a lock cannot be acquired
SET lock_timeout = '10s';
```

## Migration Execution Checklist

### Pre-Migration

- [ ] Migration script written and reviewed
- [ ] Rollback script written and tested
- [ ] Data validation queries prepared
- [ ] Pre-migration validation results recorded
- [ ] Backup verified (point-in-time recovery tested)
- [ ] Migration tested in staging with production-like data
- [ ] Performance impact assessed (table sizes, lock duration)
- [ ] Application compatibility verified (old code works with new schema)
- [ ] Change control record approved (for GxP-classified changes)
- [ ] Stakeholders notified of migration window

### During Migration

- [ ] Advisory lock acquired
- [ ] Migration executed
- [ ] Post-migration validation queries pass
- [ ] Application health checks pass
- [ ] No error spikes in monitoring
- [ ] Audit trail is capturing new changes correctly

### Post-Migration

- [ ] Rollback window observed (keep rollback capability for 24-48 hours)
- [ ] Old columns/tables dropped only after verification period
- [ ] Backup data tables dropped after verification period
- [ ] Migration documented in change log
- [ ] Change control record closed

## Emergency Rollback Procedure

If a migration causes issues in production:

1. **Assess the impact**: Is clinical data being corrupted? Is the system unavailable?
2. **Decide on rollback vs. forward fix**: If the issue is minor and data is safe, a forward fix may be faster.
3. **Execute rollback script**: Run the pre-tested rollback script.
4. **Validate data integrity**: Run all validation queries.
5. **Notify stakeholders**: Document the incident and root cause.
6. **Update change control**: Record the rollback in the change control system.

If point-in-time recovery is needed:

```bash
# PostgreSQL PITR (coordinate with DBA)
pg_restore --target-time="2025-11-15 14:00:00 UTC" --target-action=promote
```

This is a last resort. PITR affects ALL data, not just the migration target.
