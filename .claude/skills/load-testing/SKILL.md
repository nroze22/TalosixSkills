---
name: load-testing
description: "Design and execute load testing plans for Talosix EDC systems with performance benchmarks, tool setup (k6, Artillery), result analysis, and concurrent clinical site user simulation."
allowed-tools: Read, Grep, Glob, Bash
---

# Load Testing for Talosix EDC

## Domain Context

Talosix EDC systems must handle concurrent access from hundreds of clinical sites across global time zones during active clinical trials. A trial with 500 sites and 10 users per site means 5,000 potential concurrent users, with peak activity during site business hours. Performance degradation during data entry can lead to site frustration, data entry errors, and ultimately delays in bringing treatments to patients. Regulatory submissions have strict timelines, and a system that cannot generate exports under load puts those timelines at risk.

## Performance Benchmarks for EDC Systems

### Response Time Targets

| Operation | Target (p50) | Acceptable (p95) | Unacceptable (p99) |
|-----------|-------------|-------------------|---------------------|
| Page load / form render | < 1s | < 2s | > 3s |
| Form data save | < 500ms | < 1.5s | > 3s |
| Edit check execution | < 200ms | < 500ms | > 1s |
| Query listing (paginated) | < 500ms | < 1.5s | > 3s |
| Subject listing (paginated) | < 500ms | < 1.5s | > 3s |
| Audit trail retrieval | < 1s | < 3s | > 5s |
| Data export (per 1000 records) | < 10s | < 30s | > 60s |
| Report generation | < 5s | < 15s | > 30s |
| User login / authentication | < 1s | < 2s | > 3s |

### Throughput Targets

| Metric | Target |
|--------|--------|
| Concurrent authenticated users | 2,000+ |
| Form saves per minute | 500+ |
| API requests per second | 1,000+ |
| Concurrent data exports | 20+ |

### Resource Utilization Limits

| Resource | Warning Threshold | Critical Threshold |
|----------|------------------|-------------------|
| CPU utilization | 70% | 85% |
| Memory utilization | 75% | 90% |
| Database connections | 70% of pool | 85% of pool |
| Disk I/O wait | 10% | 20% |
| Network bandwidth | 60% of capacity | 80% of capacity |

## Load Testing Plan Design

### Step 1: Define Test Scenarios

#### Core EDC Workflows to Test

**Scenario 1: Data Entry Session**
```
1. User logs in (authentication)
2. Navigate to study dashboard (API calls for study metadata)
3. Select a subject (subject listing query)
4. Open a visit form (form definition + existing data retrieval)
5. Enter data in 10-15 fields
6. Save form (data write + edit check execution + audit trail write)
7. Navigate to next form
8. Repeat steps 4-7 for 3-5 forms
9. Log out
```

**Scenario 2: Query Management**
```
1. User logs in as data manager
2. View open queries dashboard (aggregation query)
3. Filter queries by site and status
4. Open a query thread
5. Add a response to the query
6. Close the query
7. Repeat for 10-20 queries
```

**Scenario 3: Data Export**
```
1. User logs in as data manager
2. Navigate to export page
3. Configure export parameters (study, sites, date range, format)
4. Trigger export generation
5. Poll for export completion
6. Download export file
```

**Scenario 4: Monitoring Dashboard**
```
1. User logs in as sponsor monitor
2. Load study overview dashboard (aggregation queries)
3. Drill into site-level metrics
4. View subject enrollment timeline
5. Review data completion rates
6. Refresh dashboard periodically
```

**Scenario 5: Concurrent Site Activity**
```
Simulate realistic multi-site activity:
- 60% of virtual users performing data entry (Scenario 1)
- 15% performing query management (Scenario 2)
- 10% viewing dashboards (Scenario 4)
- 10% performing administrative tasks (user management, study config)
- 5% running exports or reports (Scenario 3)
```

### Step 2: Define Load Profiles

#### Ramp-Up Profile
```
Phase 1 (0-5 min):   Ramp from 0 to 100 users
Phase 2 (5-15 min):  Ramp from 100 to 500 users
Phase 3 (15-25 min): Ramp from 500 to 1000 users
Phase 4 (25-35 min): Hold at 1000 users (steady state)
Phase 5 (35-45 min): Ramp to 2000 users (peak load)
Phase 6 (45-55 min): Hold at 2000 users (sustained peak)
Phase 7 (55-60 min): Ramp down to 0
```

#### Stress Test Profile
```
Ramp continuously from 0 to 5000 users over 30 minutes.
Identify the breaking point where response times exceed acceptable thresholds.
Continue beyond breaking point to verify graceful degradation (no data loss, no crashes).
```

#### Soak Test Profile
```
Maintain 1000 concurrent users for 8-24 hours.
Monitor for memory leaks, connection pool exhaustion, disk space growth.
Verify performance does not degrade over time.
```

#### Spike Test Profile
```
Maintain 500 users steady state.
At 15 min: Spike to 3000 users within 30 seconds (simulating start of business day across time zones).
Hold spike for 5 minutes.
Return to 500 users.
Repeat spike at 30 min.
```

### Step 3: Prepare Test Data

#### Data Requirements
- Pre-populate the test database with realistic data volumes:
  - 50+ studies in various states (setup, enrolling, locked, archived).
  - 10,000+ subjects across studies.
  - 500,000+ form data records with audit trail entries.
  - 50,000+ queries in various states.
- Use anonymized or synthetic data that matches production data distributions.
- Ensure test users have appropriate role assignments across studies and sites.

#### Test User Pool
- Create a pool of test users with varied roles:
  - 500+ site users (data entry, investigator).
  - 100+ data managers.
  - 50+ monitors.
  - 20+ administrators.
- Each virtual user should use a unique test account to avoid session conflicts.

## Tool Setup

### k6

#### Installation
```bash
# macOS
brew install k6

# Docker
docker pull grafana/k6
```

#### Example Test Script
```javascript
import http from 'k6/http';
import { check, sleep, group } from 'k6';
import { Rate, Trend } from 'k6/metrics';

// Custom metrics for EDC operations
const formSaveDuration = new Trend('form_save_duration');
const formSaveErrors = new Rate('form_save_errors');
const editCheckDuration = new Trend('edit_check_duration');

export const options = {
  stages: [
    { duration: '5m', target: 100 },
    { duration: '10m', target: 500 },
    { duration: '10m', target: 1000 },
    { duration: '10m', target: 1000 },  // steady state
    { duration: '5m', target: 0 },
  ],
  thresholds: {
    http_req_duration: ['p(95)<1500'],
    form_save_duration: ['p(95)<1500'],
    form_save_errors: ['rate<0.01'],      // <1% error rate
    edit_check_duration: ['p(95)<500'],
  },
};

const BASE_URL = __ENV.BASE_URL || 'https://staging.talosix.com';

export function setup() {
  // Authenticate and get tokens for test users
  // Return shared test data
}

export default function (data) {
  group('Data Entry Session', function () {
    // Login
    const loginRes = http.post(`${BASE_URL}/api/auth/login`, JSON.stringify({
      username: `testuser_${__VU}`,
      password: __ENV.TEST_PASSWORD,
    }), { headers: { 'Content-Type': 'application/json' } });

    check(loginRes, { 'login successful': (r) => r.status === 200 });
    const token = loginRes.json('accessToken');
    const authHeaders = {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`,
    };

    // Load form
    const formRes = http.get(
      `${BASE_URL}/api/studies/STUDY-001/subjects/SUB-${__VU}/forms/FORM-001`,
      { headers: authHeaders }
    );
    check(formRes, { 'form loaded': (r) => r.status === 200 });

    sleep(Math.random() * 3 + 2); // Simulate data entry think time (2-5 seconds)

    // Save form data
    const saveStart = Date.now();
    const saveRes = http.post(
      `${BASE_URL}/api/studies/STUDY-001/subjects/SUB-${__VU}/forms/FORM-001/data`,
      JSON.stringify({
        fields: {
          WEIGHT: Math.floor(Math.random() * 80) + 50,
          HEIGHT: Math.floor(Math.random() * 50) + 150,
          VISIT_DATE: '2025-01-15',
        }
      }),
      { headers: authHeaders }
    );
    formSaveDuration.add(Date.now() - saveStart);
    formSaveErrors.add(saveRes.status !== 200);

    check(saveRes, {
      'form saved': (r) => r.status === 200,
      'audit trail created': (r) => r.json('auditId') !== undefined,
    });

    sleep(1);
  });
}
```

#### Running k6
```bash
# Local execution
k6 run --env BASE_URL=https://staging.talosix.com --env TEST_PASSWORD=secret load-test.js

# With output to InfluxDB for Grafana dashboards
k6 run --out influxdb=http://localhost:8086/k6 load-test.js

# Distributed execution with k6 Cloud
k6 cloud load-test.js
```

### Artillery

#### Installation
```bash
npm install -g artillery
```

#### Example Configuration
```yaml
config:
  target: "https://staging.talosix.com"
  phases:
    - duration: 300
      arrivalRate: 10
      name: "Warm up"
    - duration: 600
      arrivalRate: 50
      name: "Sustained load"
    - duration: 300
      arrivalRate: 100
      name: "Peak load"
  defaults:
    headers:
      Content-Type: "application/json"
  plugins:
    expect: {}
  ensure:
    thresholds:
      - http.response_time.p95: 1500
      - http.request_rate: 100

scenarios:
  - name: "Data Entry"
    weight: 60
    flow:
      - post:
          url: "/api/auth/login"
          json:
            username: "{{ $randomString() }}"
            password: "{{ $processEnvironment.TEST_PASSWORD }}"
          capture:
            - json: "$.accessToken"
              as: "token"
      - get:
          url: "/api/studies/STUDY-001/subjects/SUB-001/forms/FORM-001"
          headers:
            Authorization: "Bearer {{ token }}"
      - think: 3
      - post:
          url: "/api/studies/STUDY-001/subjects/SUB-001/forms/FORM-001/data"
          headers:
            Authorization: "Bearer {{ token }}"
          json:
            fields: { WEIGHT: 75, HEIGHT: 170 }
  - name: "Dashboard View"
    weight: 20
    flow:
      - post:
          url: "/api/auth/login"
          json: { username: "monitor_user", password: "{{ $processEnvironment.TEST_PASSWORD }}" }
          capture:
            - json: "$.accessToken"
              as: "token"
      - get:
          url: "/api/studies/STUDY-001/dashboard"
          headers:
            Authorization: "Bearer {{ token }}"
```

#### Running Artillery
```bash
# Local execution
artillery run load-test.yml

# With detailed report
artillery run --output report.json load-test.yml
artillery report report.json
```

## Result Analysis

### Key Metrics to Evaluate

#### Response Time Analysis
- **p50 (median)**: Typical user experience. Should meet target benchmarks.
- **p95**: Experience for the majority. Must stay within acceptable limits.
- **p99**: Worst-case for almost all users. Should not exceed unacceptable thresholds.
- **Max**: Identify outliers. Investigate any response over 10 seconds.

#### Error Analysis
- Overall error rate (target: <0.1% for data operations).
- Error breakdown by type (timeout, 5xx, 4xx, connection refused).
- Error rate correlation with load level (at what point do errors start appearing?).
- Specific attention to form save errors -- any data loss is unacceptable.

#### Throughput Analysis
- Requests per second at each load level.
- Identify the saturation point where throughput plateaus.
- Compare actual throughput against target benchmarks.

#### Resource Utilization Analysis
- CPU, memory, disk I/O for application servers.
- CPU, memory, connections, disk I/O for database servers.
- Connection pool utilization (application to database).
- Queue depths for async processing.
- Network I/O and latency between components.

### Identifying Bottlenecks

#### Database Bottlenecks
- **Symptom**: Response times increase linearly with load; database CPU high.
- **Investigation**: Check slow query logs, examine query plans, look for missing indexes.
- **Common EDC causes**: Unindexed queries on audit_trail table (grows very large), full table scans on form_data during export.

#### Application Server Bottlenecks
- **Symptom**: CPU saturated on app servers; adding more database capacity does not help.
- **Investigation**: Profile application code, check for CPU-intensive operations.
- **Common EDC causes**: Edit check engine evaluating complex rules, PDF generation, data transformation for exports.

#### Connection Pool Exhaustion
- **Symptom**: Sudden spike in response times or errors; connection pool metrics at maximum.
- **Investigation**: Check for long-running transactions holding connections, connection leaks.
- **Common EDC causes**: Export queries holding connections for extended periods, uncommitted transactions in data entry flows.

#### Memory Pressure
- **Symptom**: Gradual performance degradation, eventual OOM errors.
- **Investigation**: Heap analysis, check for large in-memory collections.
- **Common EDC causes**: Loading entire datasets into memory for export, caching too much study metadata.

### Reporting Results

#### Load Test Report Template
```markdown
## Load Test Report

**Date**: [date]
**Environment**: [staging/performance]
**Test Duration**: [duration]
**Peak Concurrent Users**: [number]

### Summary
[Pass/Fail against benchmarks. Key findings.]

### Response Time Results

| Operation | p50 | p95 | p99 | Target p95 | Status |
|-----------|-----|-----|-----|------------|--------|
| Form Load | Xms | Xms | Xms | 2000ms | Pass/Fail |
| Form Save | Xms | Xms | Xms | 1500ms | Pass/Fail |
| ... | ... | ... | ... | ... | ... |

### Error Rate
- Overall: X%
- Data operations: X%
- Data integrity errors: [must be 0]

### Resource Utilization at Peak
| Resource | Peak Value | Threshold | Status |
|----------|-----------|-----------|--------|
| App CPU | X% | 85% | OK/Warning/Critical |
| DB CPU | X% | 85% | OK/Warning/Critical |
| ... | ... | ... | ... |

### Bottlenecks Identified
1. [Description, evidence, recommended fix]
2. [Description, evidence, recommended fix]

### Recommendations
1. [Prioritized optimization recommendations]
2. [Capacity planning recommendations]
```

## Optimization Recommendations

### Common Optimizations for EDC Systems

1. **Database indexing**: Ensure indexes on audit_trail(record_id, table_name), form_data(subject_id, form_id), queries(study_id, status).
2. **Connection pooling**: Use PgBouncer or similar for database connection pooling.
3. **Read replicas**: Route read-heavy operations (dashboards, listings, exports) to read replicas.
4. **Caching**: Cache study metadata, form definitions, edit check configurations (invalidate on change).
5. **Async processing**: Move exports, reports, and notifications to background job queues.
6. **Pagination**: Enforce pagination on all list endpoints; never return unbounded result sets.
7. **Query optimization**: Use EXPLAIN ANALYZE on slow queries; avoid N+1 patterns in ORMs.
8. **CDN**: Serve static assets (form definitions, UI bundles) from a CDN.
9. **Compression**: Enable gzip/brotli for API responses.
10. **Batch operations**: Batch audit trail writes where safe (within the same transaction).

## When Applying This Skill

1. Review existing load test scripts and results if available.
2. Identify the critical EDC workflows to test based on the application architecture.
3. Design test scenarios that reflect realistic clinical trial usage patterns.
4. Set up the appropriate tool (k6 or Artillery) with scripts tailored to the API.
5. Run tests against a staging or performance environment (never production without explicit approval).
6. Analyze results against the benchmarks defined above.
7. Provide specific, actionable optimization recommendations.
8. Document results for validation records if the system is in a validated state.
