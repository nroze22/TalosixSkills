---
name: debugging-guide
description: Systematic debugging methodology for Talosix EDC systems including log analysis, stack trace interpretation, database debugging, API debugging, and production issue investigation.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Debugging Guide for Talosix EDC

## Domain Context

Debugging in a clinical trial EDC system carries elevated stakes. Production bugs can affect data integrity for active clinical trials, block site users from entering patient data, or compromise audit trail completeness. Speed matters, but correctness matters more. Every debugging action in production must be traceable, and fixes must go through change control before deployment to validated environments.

## Systematic Debugging Methodology

### Phase 1: Triage and Characterize

Before diving into code, establish the facts.

#### Questions to Answer
1. **What is the symptom?** Exact error message, unexpected behavior, or data discrepancy.
2. **Who is affected?** Single user, single site, all users, specific role?
3. **When did it start?** Correlate with recent deployments, configuration changes, or data events.
4. **Is it reproducible?** Always, intermittent, or one-time occurrence?
5. **What is the impact?** Data integrity risk, workflow blocked, cosmetic issue?
6. **What changed?** Recent deployments, dependency updates, infrastructure changes, data migrations.

#### EDC-Specific Triage
- **Is clinical data at risk?** If yes, escalate immediately. Consider read-only mode for affected studies.
- **Is the audit trail intact?** Verify audit records exist for recent data changes.
- **Are electronic signatures affected?** If e-signature validation is broken, halt signing workflows.
- **How many studies/sites are impacted?** Determines severity and communication requirements.

#### Priority Matrix

| Data Integrity Risk | Users Blocked | Priority |
|---------------------|--------------|----------|
| Yes | Yes | P0 -- Immediate response, all hands |
| Yes | No | P1 -- Urgent, fix within hours |
| No | Yes | P1 -- Urgent, workaround if possible |
| No | No | P2/P3 -- Standard bug fix process |

### Phase 2: Gather Evidence

#### Log Analysis

##### Finding Relevant Logs
```bash
# Search application logs by trace ID
grep "traceId.*abc-123-def" /var/log/app/*.log

# Search by user who reported the issue
grep "userId.*user-42" /var/log/app/*.log | grep "ERROR\|WARN"

# Search by study and time window
grep "studyId.*STUDY-001" /var/log/app/*.log | awk '$1 >= "2025-01-15T14:00" && $1 <= "2025-01-15T15:00"'

# Search across services in a microservices setup (using centralized logging)
# Kibana/Grafana Loki query: traceId:"abc-123-def" AND level:ERROR
```

##### Log Analysis Checklist
- Identify the first error in the chain (root cause, not cascading failures).
- Check timestamps for ordering across services.
- Look for warnings that preceded the error.
- Compare log patterns between the failing request and a successful one.
- Check for resource exhaustion signals: connection pool, memory, disk.
- Look for slow query warnings or timeout messages.

##### EDC Log Patterns to Watch For
- `AUDIT_WRITE_FAILURE` -- Critical: audit trail integrity compromised.
- `TRANSACTION_ROLLBACK` -- Data save failed; check if the user was notified.
- `CONCURRENT_MODIFICATION` -- Optimistic lock conflict; check if conflict resolution worked.
- `EDIT_CHECK_ENGINE_ERROR` -- Validation rules failed to execute; data may have been saved without validation.
- `ESIG_VERIFICATION_FAILED` -- Electronic signature could not be verified.

#### Stack Trace Interpretation

##### Reading Stack Traces
1. Start from the **bottom** (or innermost cause) -- this is the root cause.
2. Identify the exception type: Is it a known/expected error or an unexpected failure?
3. Find the first frame that belongs to **your code** (not library/framework code).
4. Note the file, line number, and method name.
5. Look for "Caused by" chains -- the last "Caused by" is usually the root cause.

##### Common Stack Trace Patterns

**NullPointerException / TypeError: Cannot read property of undefined**
- A value was expected but not present.
- Check: Was a database query expected to return a result but returned null?
- Check: Was an optional configuration value assumed to be present?
- Check: Was a related record deleted or not yet created?

**ConnectionTimeout / ETIMEDOUT**
- A network call exceeded its timeout.
- Check: Is the target service healthy? Check its logs and metrics.
- Check: Is the connection pool exhausted? Look for pool size and active connections.
- Check: Is there a network partition or DNS resolution failure?

**DeadlockDetected / Lock wait timeout**
- Two or more transactions are waiting on each other.
- Check: What queries were involved? Examine the deadlock log.
- Check: Are transactions being held open too long?
- Check: Is there a missing index causing full table locks?

**OutOfMemoryError / JavaScript heap out of memory**
- The process exceeded its memory allocation.
- Check: Is there a memory leak? Look for unbounded caches, event listener accumulation, or large result sets loaded into memory.
- Check: Was a large data export or report attempted?

### Phase 3: Database Debugging

#### Query Analysis
```sql
-- Find slow queries (PostgreSQL)
SELECT pid, now() - pg_stat_activity.query_start AS duration, query, state
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 seconds'
ORDER BY duration DESC;

-- Check for locks (PostgreSQL)
SELECT blocked_locks.pid AS blocked_pid,
       blocked_activity.usename AS blocked_user,
       blocking_locks.pid AS blocking_pid,
       blocking_activity.usename AS blocking_user,
       blocked_activity.query AS blocked_statement
FROM pg_catalog.pg_locks blocked_locks
JOIN pg_catalog.pg_stat_activity blocked_activity ON blocked_activity.pid = blocked_locks.pid
JOIN pg_catalog.pg_locks blocking_locks ON blocking_locks.locktype = blocked_locks.locktype
JOIN pg_catalog.pg_stat_activity blocking_activity ON blocking_activity.pid = blocking_locks.pid
WHERE NOT blocked_locks.granted;
```

#### EDC Data Debugging
```sql
-- Verify audit trail completeness for a subject
SELECT a.* FROM audit_trail a
WHERE a.subject_id = 'SUB-1042'
  AND a.study_id = 'STUDY-001'
ORDER BY a.audit_timestamp DESC;

-- Check for data without audit trail entries (data integrity issue)
SELECT d.id, d.form_id, d.subject_id, d.modified_at
FROM form_data d
LEFT JOIN audit_trail a ON a.record_id = d.id AND a.table_name = 'form_data'
WHERE a.id IS NULL
  AND d.modified_at > '2025-01-01';

-- Check for optimistic locking conflicts
SELECT * FROM form_data
WHERE subject_id = 'SUB-1042'
  AND form_id = 'FORM-AE-001'
ORDER BY version DESC;
```

#### Database Health Checks
- Connection pool utilization: Are connections being returned properly?
- Table bloat: Are large tables vacuumed regularly?
- Index health: Are indexes being used? Check for sequential scans on large tables.
- Replication lag: Is the read replica behind the primary?
- Disk space: Is the database volume approaching capacity?

### Phase 4: API Debugging

#### Request/Response Analysis
- Capture the exact request: method, URL, headers, body.
- Compare with API documentation -- is the request well-formed?
- Check authentication: Is the token valid and not expired?
- Check authorization: Does the user have the required role for this study/site?
- Examine the response: status code, headers (especially cache headers), body.

#### Common API Issues

**401 Unauthorized**
- Token expired or malformed.
- Check token expiration time.
- Check if the identity provider is reachable.
- Verify token signing key has not rotated.

**403 Forbidden**
- User authenticated but lacks permission.
- Check role assignments for the specific study and site.
- Check if the study or site is in a locked/frozen state.
- Verify permission matrix configuration.

**409 Conflict**
- Optimistic locking conflict on data modification.
- Another user or process modified the same record.
- Check audit trail for recent changes to the same record.

**422 Unprocessable Entity**
- Validation failed on the request data.
- Check edit check configuration for the form.
- Verify that the validation rules match the expected data format.

**500 Internal Server Error**
- Unhandled exception in the server.
- Check application logs for the stack trace using the request's trace ID.
- This should never happen for expected error conditions -- it indicates a missing error handler.

#### API Performance Debugging
- Check response times against SLAs (typically <500ms for form saves, <2s for reports).
- Look for N+1 query patterns in the ORM.
- Check if caching is working (cache hit rate, stale cache issues).
- Verify that pagination is used for list endpoints.

### Phase 5: Production Issue Investigation

#### Safe Investigation Practices
- **Read-only**: Use read-only database replicas for queries. Never modify production data directly.
- **Minimal footprint**: Avoid running resource-intensive queries during peak hours.
- **Auditable**: Log your investigation actions. Note what you queried and when.
- **Communicate**: Keep stakeholders informed of investigation progress.
- **Time-box**: Set a time limit for investigation before escalating.

#### Investigation Workflow
1. Reproduce the issue in a lower environment if possible.
2. If not reproducible, add targeted debug logging (temporary, with change control).
3. Correlate the issue timeline with deployment and change logs.
4. Check monitoring dashboards for anomalies at the time of the issue.
5. Review recent changes to configuration, feature flags, or study setup.
6. If data integrity is affected, quantify the scope: how many records, which studies, which sites.

#### Post-Incident Documentation
```markdown
## Incident Report

**Incident ID**: INC-[YYYY]-[NNN]
**Severity**: P0/P1/P2
**Duration**: [start time] to [resolution time]
**Impact**: [studies, sites, users affected]

### Timeline
- [Time]: Issue first reported by [who]
- [Time]: Investigation started
- [Time]: Root cause identified
- [Time]: Fix deployed
- [Time]: Issue confirmed resolved

### Root Cause
[Detailed technical explanation]

### Data Impact Assessment
- Were any clinical data records affected? [Yes/No]
- Was the audit trail complete and accurate? [Yes/No]
- Were any electronic signatures invalidated? [Yes/No]
- Corrective data actions taken: [description]

### Corrective Actions
- Immediate fix: [description]
- Preventive measures: [description, with ticket references]

### Lessons Learned
[What can prevent similar incidents]
```

## Debugging Tools and Techniques

### Log Aggregation Queries
- Search by trace ID across all services.
- Filter by time window, service, log level, study ID, user ID.
- Aggregate error counts by type and time bucket to find patterns.

### Database Tools
- `EXPLAIN ANALYZE` for query performance investigation.
- `pg_stat_statements` for identifying frequently slow queries.
- Connection pool monitoring (PgBouncer stats, HikariCP metrics).

### Network Debugging
- Check DNS resolution: `nslookup`, `dig`.
- Check connectivity: `curl -v` to target services.
- Check SSL certificate validity and chain.
- Review load balancer access logs for request routing.

### Memory and Performance
- Heap dumps for memory leak investigation (generate during off-peak).
- CPU profiling to identify hot paths.
- Garbage collection logs for GC pressure analysis.

## When Applying This Skill

1. Follow the phased approach: triage, gather evidence, analyze, fix.
2. Always check for data integrity impact first in an EDC context.
3. Use Grep to search logs and code for error patterns.
4. Use Read to examine specific files, configurations, and stack traces.
5. Use Bash for running diagnostic commands (log queries, database checks, network tests).
6. Document findings as you go -- production debugging in a validated system must be traceable.
7. Recommend fixes that address the root cause, not just the symptom.
