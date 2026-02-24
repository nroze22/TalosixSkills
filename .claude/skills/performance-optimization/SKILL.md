---
name: performance-optimization
description: Analyze and optimize application performance for Talosix EDC systems. Covers database query optimization, caching strategies, frontend and backend profiling, and load testing recommendations.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Performance Optimization for Clinical Trial EDC Systems

You are a performance engineer for Talosix. Performance in a clinical trial EDC system directly impacts user experience during patient visits, data entry timelines, and ultimately trial quality. Slow systems lead to incomplete data capture, missed queries, and frustrated clinical site staff. Performance must be optimized without compromising data integrity, audit trail completeness, or regulatory compliance.

## Performance Targets

| Operation | Target (p95) | Critical Threshold |
|-----------|-------------|-------------------|
| Page load (initial) | < 2 seconds | 5 seconds |
| Page navigation | < 500ms | 2 seconds |
| CRF form save | < 1 second | 3 seconds |
| Edit check evaluation | < 200ms | 1 second |
| Data query list load | < 1 second | 3 seconds |
| Report generation (small) | < 5 seconds | 15 seconds |
| Report generation (large) | < 30 seconds | 60 seconds |
| Data export (CSV/SAS) | < 60 seconds for 100K records | 5 minutes |
| Audit trail query | < 2 seconds | 5 seconds |
| Login/authentication | < 1 second | 3 seconds |

## Analysis Process

### Step 1: Identify the Bottleneck

Before optimizing, measure. Determine where time is being spent.

**Backend Profiling**

Use Bash to examine application logs and metrics:

```bash
# Check for slow database queries (PostgreSQL)
psql -c "SELECT query, calls, mean_exec_time, total_exec_time
FROM pg_stat_statements
ORDER BY mean_exec_time DESC
LIMIT 20;"

# Check for slow API endpoints from access logs
awk '{print $7, $NF}' access.log | sort -k2 -rn | head -20

# Check application-level profiling (Python example)
python -m cProfile -s cumulative app.py

# Check Node.js profiling
node --prof app.js
node --prof-process isolate-*.log > profile.txt
```

**Database Analysis**

```bash
# Check table sizes and bloat
psql -c "SELECT schemaname, tablename,
  pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS total_size,
  pg_size_pretty(pg_relation_size(schemaname||'.'||tablename)) AS table_size,
  pg_size_pretty(pg_indexes_size(schemaname||'.'||tablename)) AS index_size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC
LIMIT 20;"

# Check index usage
psql -c "SELECT schemaname, tablename, indexname, idx_scan, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
ORDER BY idx_scan ASC
LIMIT 20;"

# Check for sequential scans on large tables
psql -c "SELECT schemaname, relname, seq_scan, seq_tup_read,
  idx_scan, idx_tup_fetch
FROM pg_stat_user_tables
WHERE seq_scan > 0
ORDER BY seq_tup_read DESC
LIMIT 20;"
```

**Frontend Analysis**

- Examine bundle size and code splitting.
- Check for unnecessary re-renders in React components.
- Analyze network waterfall for sequential API calls that could be parallelized.
- Check for large payloads being transferred.

### Step 2: Database Query Optimization

#### Common Issues in EDC Systems

**N+1 Query Problem**

Frequently occurs when loading CRF forms with multiple fields, or subject lists with associated visit data.

```sql
-- BAD: N+1 (one query per subject for visits)
SELECT * FROM subjects WHERE study_id = $1;
-- Then for each subject:
SELECT * FROM visits WHERE subject_id = $subjectId;

-- GOOD: Single query with JOIN or subquery
SELECT s.*, v.visit_name, v.visit_date
FROM subjects s
LEFT JOIN visits v ON v.subject_id = s.id
WHERE s.study_id = $1
ORDER BY s.subject_number, v.visit_order;

-- GOOD: Batch loading
SELECT * FROM visits WHERE subject_id = ANY($1::uuid[]);
```

**Missing Indexes**

Use EXPLAIN ANALYZE to identify missing indexes:

```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT * FROM audit_trail
WHERE table_name = 'subjects'
AND record_id = 'uuid-here'
ORDER BY created_at DESC;

-- If you see "Seq Scan" on a large table, add an index:
CREATE INDEX CONCURRENTLY idx_audit_trail_table_record
ON audit_trail (table_name, record_id, created_at DESC);
```

**Inefficient Aggregate Queries**

Common for dashboards and reports:

```sql
-- BAD: Counting all rows for pagination
SELECT COUNT(*) FROM subjects WHERE study_id = $1;
SELECT * FROM subjects WHERE study_id = $1 LIMIT 25 OFFSET 0;

-- BETTER: Use estimated count for large tables
SELECT reltuples::bigint AS estimate
FROM pg_class WHERE relname = 'subjects';

-- BEST: Use cursor-based pagination (no COUNT needed)
SELECT * FROM subjects
WHERE study_id = $1 AND id > $lastSeenId
ORDER BY id
LIMIT 26; -- Fetch one extra to determine has_more
```

**Unoptimized JSON Queries**

EDC systems often store flexible form data in JSONB columns:

```sql
-- BAD: Full JSONB scan
SELECT * FROM form_data WHERE data->>'field_123' = 'abnormal';

-- GOOD: GIN index on JSONB
CREATE INDEX CONCURRENTLY idx_form_data_gin ON form_data USING GIN (data);

-- GOOD: Expression index for frequently queried fields
CREATE INDEX CONCURRENTLY idx_form_data_field123
ON form_data ((data->>'field_123'));
```

#### PostgreSQL-Specific Optimizations

**Connection Pooling**

Use PgBouncer or built-in connection pooling:

```
# pgbouncer.ini settings for EDC workload
pool_mode = transaction
max_client_conn = 200
default_pool_size = 25
reserve_pool_size = 5
reserve_pool_timeout = 3
```

**Table Partitioning for Large Tables**

Partition audit trail and form data tables:

```sql
-- Partition by study for form_data (queries are always study-scoped)
CREATE TABLE form_data (
    id UUID,
    study_id UUID,
    subject_id UUID,
    form_definition_id UUID,
    data JSONB,
    created_at TIMESTAMPTZ
) PARTITION BY HASH (study_id);

-- Create partitions
CREATE TABLE form_data_p0 PARTITION OF form_data FOR VALUES WITH (MODULUS 8, REMAINDER 0);
CREATE TABLE form_data_p1 PARTITION OF form_data FOR VALUES WITH (MODULUS 8, REMAINDER 1);
-- ... etc.
```

**Vacuum and Analyze**

Ensure autovacuum is properly configured for high-write tables:

```sql
-- Check autovacuum stats
SELECT schemaname, relname, last_vacuum, last_autovacuum,
  last_analyze, last_autoanalyze, n_dead_tup
FROM pg_stat_user_tables
ORDER BY n_dead_tup DESC
LIMIT 10;

-- Tune autovacuum for high-write tables
ALTER TABLE audit_trail SET (
  autovacuum_vacuum_scale_factor = 0.01,
  autovacuum_analyze_scale_factor = 0.005
);
```

### Step 3: Caching Strategies

#### What to Cache in an EDC System

| Data Type | Cache Strategy | TTL | Invalidation |
|-----------|---------------|-----|-------------|
| Study configuration (form definitions, edit checks) | Application cache (Redis) | 1 hour | On publish/update |
| User permissions and roles | Application cache (Redis) | 5 minutes | On role change |
| Reference data (countries, lab units, MedDRA terms) | Application cache (Redis) | 24 hours | On reference data update |
| Static assets (JS, CSS, images) | CDN + browser cache | 1 year (with hash-based cache busting) | On deploy |
| API responses for read-heavy endpoints | HTTP cache headers (ETag) | Varies | On data change |

#### What NOT to Cache

- **Clinical data (CRF form data)**: Must always reflect the latest state. Stale clinical data could lead to incorrect medical decisions.
- **Audit trail queries**: Must always be real-time and complete.
- **User authentication tokens**: Must be validated on every request for security.
- **Randomization results**: Must always be retrieved from the authoritative source.

#### Cache Implementation Patterns

**Read-Through Cache**

```
Request -> Check Cache -> [HIT] -> Return cached data
                       -> [MISS] -> Query database -> Store in cache -> Return data
```

**Cache-Aside with Invalidation**

```
Write operation -> Update database -> Invalidate cache key
Read operation -> Check cache -> [MISS] -> Query database -> Store in cache
```

**Important**: Always invalidate cache on write, never update cache on write (to avoid race conditions with concurrent requests).

### Step 4: Backend Optimization

#### API Response Optimization

- **Field Selection**: Allow clients to request only needed fields. Reduces serialization time and payload size.
- **Compression**: Enable gzip/brotli compression for API responses.
- **Pagination**: Enforce pagination on all list endpoints. Never return unbounded result sets.
- **Async Processing**: Move long-running operations (report generation, data export, bulk operations) to background jobs. Return a job ID and let the client poll for completion.

#### Batch Operations

```
# BAD: 100 individual API calls to save 100 form fields
POST /api/v1/form-data/field-1
POST /api/v1/form-data/field-2
...

# GOOD: Single batch API call
POST /api/v1/form-data/batch
{
  "fields": [
    { "fieldId": "field-1", "value": "..." },
    { "fieldId": "field-2", "value": "..." }
  ]
}
```

#### Edit Check Evaluation Optimization

Edit checks are clinical validation rules that run on form save. They can be computationally expensive for complex protocols:

- **Pre-compile edit check rules**: Parse and compile rule expressions once, cache the compiled form.
- **Evaluate only affected checks**: On field change, only evaluate checks that reference the changed field (dependency graph).
- **Parallelize independent checks**: Checks that do not depend on each other can be evaluated concurrently.
- **Short-circuit evaluation**: If a check fails early, skip remaining conditions in the same check.

### Step 5: Frontend Optimization

#### Bundle Optimization

```bash
# Analyze bundle size
npx webpack-bundle-analyzer stats.json

# Check for duplicate dependencies
npx depcheck

# Identify large dependencies
npx bundlephobia <package-name>
```

- **Code Splitting**: Split by route. Clinical users typically work in one area at a time (data entry, query management, reporting).
- **Lazy Loading**: Load CRF form components on demand. Each form definition can be a separate chunk.
- **Tree Shaking**: Ensure unused code is eliminated. Verify with bundle analyzer.

#### Rendering Optimization

- **Virtualized Lists**: For long subject lists, query lists, or audit trail views, use virtual scrolling (e.g., react-window, react-virtualized).
- **Memoization**: Memoize expensive computations (edit check results, derived values) with useMemo/React.memo.
- **Debounce Input**: Debounce form field changes to avoid triggering edit checks on every keystroke.
- **Optimistic Updates**: Show the user the expected result immediately, then confirm with the server. Roll back if the server rejects.

### Step 6: Load Testing

#### Test Scenarios for EDC Systems

| Scenario | Description | Expected Load |
|----------|-------------|---------------|
| Concurrent Data Entry | Multiple data entry users saving CRF forms simultaneously | 50-200 concurrent users per study |
| Bulk Query Resolution | Monitors resolving large batches of data queries | 20 concurrent users, 100+ query actions per session |
| Report Generation | Multiple users generating study reports simultaneously | 5-10 concurrent report requests |
| Data Export | Large data extracts for statistical analysis | 1-3 concurrent exports of 100K+ records |
| Peak Enrollment | High enrollment period with many subject creations and randomizations | 2x normal load |
| Audit Trail Query | Compliance team querying audit trail for specific subjects | 5-10 concurrent users, large result sets |

#### Load Testing Tools

```bash
# k6 load test example
k6 run --vus 50 --duration 10m load-test.js

# Artillery
artillery run load-test.yml

# Apache Benchmark (quick test)
ab -n 1000 -c 50 -H "Authorization: Bearer $TOKEN" https://api.talosix.com/v1/subjects
```

#### Load Test Script Template (k6)

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '2m', target: 50 },   // Ramp up
    { duration: '5m', target: 50 },   // Sustain
    { duration: '2m', target: 100 },  // Peak
    { duration: '5m', target: 100 },  // Sustain peak
    { duration: '2m', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<2000'],  // 95% of requests under 2s
    http_req_failed: ['rate<0.01'],     // Less than 1% failure rate
  },
};

export default function () {
  const res = http.get('https://api.talosix.com/v1/subjects', {
    headers: { Authorization: `Bearer ${__ENV.TOKEN}` },
  });
  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time < 2000ms': (r) => r.timings.duration < 2000,
  });
  sleep(1);
}
```

## Optimization Checklist

### Database

- [ ] No sequential scans on tables with > 10K rows
- [ ] All foreign keys are indexed
- [ ] Composite indexes match common query patterns
- [ ] EXPLAIN ANALYZE shows index usage for critical queries
- [ ] Connection pooling configured and tuned
- [ ] Autovacuum configured for high-write tables
- [ ] Large tables are partitioned (audit trail, form data)
- [ ] No N+1 query patterns in data access layer

### API

- [ ] All list endpoints are paginated
- [ ] Response compression enabled
- [ ] Long-running operations use async/background processing
- [ ] Batch endpoints available for bulk operations
- [ ] Field selection supported to minimize payload size
- [ ] ETag/conditional requests for cacheable resources

### Caching

- [ ] Study configuration cached with proper invalidation
- [ ] Reference data cached (countries, coding dictionaries)
- [ ] Static assets served via CDN with cache headers
- [ ] No caching of clinical data or audit trails

### Frontend

- [ ] Bundle size analyzed and optimized
- [ ] Code splitting by route
- [ ] Virtual scrolling for long lists
- [ ] Form input debounced
- [ ] No unnecessary re-renders on data entry forms

### Infrastructure

- [ ] Database read replicas for read-heavy operations (reports, exports)
- [ ] Auto-scaling configured for peak load periods
- [ ] Monitoring and alerting for p95 latency thresholds
- [ ] Load testing performed with realistic clinical workflows

## Important Constraints

- **Never sacrifice data integrity for performance**: Every clinical data write must still go through validation, audit trail generation, and proper transaction management.
- **Never skip audit trail entries**: Even if audit trail writes are a bottleneck, they cannot be deferred or batched in a way that risks losing entries. Async audit trail writes are acceptable only with guaranteed delivery (message queue with persistence).
- **Never cache clinical data**: Clinical users must always see the latest data. Stale clinical data is a patient safety risk.
- **Always measure before and after**: Document performance metrics before optimization and after. Include the methodology and test conditions.
