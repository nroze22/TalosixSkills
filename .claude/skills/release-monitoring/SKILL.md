---
name: release-monitoring
description: "Post-release monitoring plan for Talosix EDC deployments. Covers key metrics, alert thresholds, smoke test execution, and escalation procedures."
allowed-tools: Read, Grep, Glob, Bash
---

# Release Monitoring Skill

Generate post-release monitoring plans and analyze monitoring data for Talosix EDC system releases. Clinical trial systems require heightened post-release observation to detect issues that could impact data integrity or patient safety.

## When to Use

- Immediately after a production deployment
- When defining monitoring criteria for an upcoming release
- When analyzing post-release metrics to assess deployment health
- When investigating anomalies detected after a release

## Monitoring Plan Structure

### 1. Monitoring Phases

**Phase 1: Immediate (0-2 hours post-deploy)**
- Active monitoring by deployment team
- Execute smoke tests
- Watch error rates, response times, system resources
- Go/no-go for rollback window closure

**Phase 2: Short-term (2-24 hours post-deploy)**
- Elevated monitoring with on-call awareness
- Monitor async processes (data exports, integrations, scheduled jobs)
- Watch for gradual degradation patterns

**Phase 3: Extended (1-7 days post-deploy)**
- Standard monitoring with release-specific alert rules
- Monitor user-reported issues for new patterns
- Watch clinical workflow completion rates

### 2. Key Metrics to Watch

**System Health Metrics:**
| Metric | Normal Range | Warning | Critical |
|--------|-------------|---------|----------|
| API response time (p50) | < 200ms | > 500ms | > 1000ms |
| API response time (p99) | < 1000ms | > 2000ms | > 5000ms |
| Error rate (5xx) | < 0.1% | > 0.5% | > 1% |
| Error rate (4xx) | < 2% | > 5% | > 10% |
| CPU utilization | < 60% | > 75% | > 90% |
| Memory utilization | < 70% | > 80% | > 95% |
| Database connection pool | < 70% | > 80% | > 95% |
| Disk I/O wait | < 10% | > 20% | > 40% |

**Clinical Workflow Metrics:**
| Metric | Normal Range | Warning | Critical |
|--------|-------------|---------|----------|
| eCRF form save success rate | > 99.5% | < 99% | < 98% |
| Electronic signature success rate | > 99.9% | < 99.5% | < 99% |
| Audit trail write success rate | 100% | < 100% | < 100% |
| Query workflow completion rate | > 99% | < 98% | < 95% |
| Data export success rate | > 99% | < 98% | < 95% |
| User login success rate | > 99% | < 98% | < 95% |

**Integration Metrics:**
| Metric | Normal Range | Warning | Critical |
|--------|-------------|---------|----------|
| External API call success rate | > 99% | < 98% | < 95% |
| Message queue depth | < 1000 | > 5000 | > 10000 |
| Integration sync latency | < 5 min | > 15 min | > 30 min |

### 3. Smoke Test Execution

Execute the following smoke tests immediately after deployment:

**Core Functionality:**
1. User authentication (login, logout, session management)
2. eCRF form rendering for each form type modified in release
3. Data entry and save on a test subject
4. Edit check / validation rule firing
5. Electronic signature application
6. Audit trail generation and viewing
7. Query open, respond, close workflow
8. Role-based access control verification

**Integration Points:**
1. Lab data import processing
2. Randomization system connectivity (RTSM/IRT)
3. Coding dictionaries (MedDRA, WHODrug) availability
4. Data export generation (SAS, CSV)
5. Email notification delivery

**Administrative Functions:**
1. Study configuration access
2. User management operations
3. Report generation
4. System administration pages

### 4. Escalation Procedures

**Severity Levels:**

| Level | Definition | Response Time | Escalation |
|-------|-----------|---------------|------------|
| SEV-1 | System down or data integrity issue | Immediate | VP Engineering + CTO + Regulatory |
| SEV-2 | Major feature broken, workaround unavailable | 15 minutes | Engineering Manager + Product |
| SEV-3 | Feature degraded, workaround available | 1 hour | Engineering Lead |
| SEV-4 | Minor issue, no workflow impact | Next business day | Sprint backlog |

**Escalation Path:**
1. On-call engineer detects or is alerted to issue
2. Assess severity using criteria above
3. For SEV-1/SEV-2: Initiate incident response (see incident-response skill)
4. For SEV-1: Evaluate rollback per rollback plan
5. Notify stakeholders per severity communication matrix
6. Document all actions and decisions in incident channel

**Clinical Impact Assessment:**
For any issue that may affect clinical data, additionally:
- Notify Clinical Operations lead immediately
- Assess whether affected studies need to pause data entry
- Determine if a field safety notice is required
- Evaluate 21 CFR Part 11 compliance impact

### 5. Monitoring Dashboard Checklist

Verify these dashboards are accessible and showing data:
- [ ] Application performance dashboard
- [ ] Error tracking dashboard (Sentry, Datadog, etc.)
- [ ] Database performance dashboard
- [ ] Infrastructure metrics dashboard
- [ ] Business metrics dashboard (form submissions, active users)
- [ ] Integration health dashboard
- [ ] Audit trail monitoring dashboard

### 6. Monitoring Completion Criteria

The release monitoring period ends when:
- [ ] All smoke tests passed
- [ ] No SEV-1 or SEV-2 issues in the monitoring period
- [ ] All metrics within normal ranges for 24+ hours
- [ ] No anomalous patterns in error logs
- [ ] Clinical workflow metrics stable
- [ ] Deployment team signs off on monitoring completion
- [ ] Monitoring completion documented in change control record

## Using Allowed Tools

When analyzing post-release data:
- Use **Read** to examine log files and configuration
- Use **Grep** to search for error patterns in logs
- Use **Glob** to find relevant log and config files
- Use **Bash** to run monitoring queries or check system status

## Output Format

Generate monitoring plans as structured markdown with clear metrics tables, runbook-style procedures, and checklists suitable for the on-call team.
