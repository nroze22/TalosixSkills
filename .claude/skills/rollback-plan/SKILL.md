---
name: rollback-plan
description: Create rollback plans for Talosix EDC deployments covering database rollback, code rollback, configuration rollback, data integrity verification, and communication plans.
---

# Rollback Plan Skill

Generate detailed rollback plans for Talosix EDC system deployments. In clinical trial systems, rollback must preserve data integrity, maintain audit trails, and comply with regulatory requirements.

## When to Use

- As part of every deployment preparation
- When updating the rollback strategy for a specific release
- When a deployment has failed and rollback execution guidance is needed

## Rollback Plan Structure

When asked to create a rollback plan, generate a document covering all sections below. Adapt to the specific deployment scope.

### 1. Rollback Decision Criteria

Define the conditions that trigger a rollback:

- **Immediate rollback triggers** (no discussion needed):
  - Data corruption detected in clinical trial data
  - Audit trail integrity failure
  - Electronic signature functionality broken
  - System unavailable for more than the defined RTO
  - Patient safety-impacting defect identified

- **Assessed rollback triggers** (team decision required):
  - Performance degradation beyond acceptable thresholds
  - Non-critical functionality regression
  - Integration failures with external systems
  - Validation test failures in production verification

- **Rollback window**: Define the maximum time after deployment during which a clean rollback is feasible. After this window, a forward-fix approach is required.

### 2. Database Rollback

```
Pre-deployment:
- Capture full database backup with timestamp
- Record current schema version
- Document all migration scripts in execution order

Rollback procedure:
1. Stop application services to prevent new writes
2. Verify no in-flight transactions
3. Execute database rollback scripts in reverse order
4. OR restore from pre-deployment backup if scripts unavailable
5. Verify schema version matches pre-deployment state
6. Verify row counts on critical tables (subjects, forms, audit_log)
7. Run data integrity checks (referential integrity, constraint validation)
8. Verify audit trail continuity (no gaps in sequence)
```

**Critical considerations for clinical data:**
- Any clinical data entered between deployment and rollback must be preserved or documented
- Audit trail entries created during the failed deployment period must be retained
- If data was collected on new form versions, a data migration strategy is required
- Coordinate with Data Management for any data reconciliation needs

### 3. Application Code Rollback

```
Rollback procedure:
1. Identify the previous known-good release tag/version
2. Deploy previous version artifacts (container images, packages)
3. Verify application starts successfully
4. Confirm version endpoint reports correct previous version
5. Execute smoke tests against rolled-back application
6. Verify all service health checks pass
```

**Deployment artifact management:**
- Previous release artifacts must be retained and accessible
- Container image tags must be immutable
- Never overwrite previous release artifacts

### 4. Configuration Rollback

```
Rollback procedure:
1. Restore environment variables to pre-deployment values
2. Revert feature flag changes
3. Restore API gateway / load balancer configuration
4. Revert DNS changes (if applicable, account for TTL)
5. Restore integration endpoint configurations
6. Verify configuration matches pre-deployment baseline
```

**Configuration tracking:**
- Maintain a configuration baseline document for each environment
- Track all configuration changes as part of the change record
- Store configuration snapshots alongside database backups

### 5. Data Integrity Verification After Rollback

After any rollback, execute these verification steps:

1. **Audit trail integrity**: Verify audit log sequence continuity and completeness
2. **Subject data**: Confirm all subject records are intact and accessible
3. **Form data**: Verify eCRF data matches pre-deployment state (or account for new entries)
4. **Electronic signatures**: Confirm all e-signatures remain valid and verifiable
5. **User access**: Verify role-based access control is functioning correctly
6. **Queries and discrepancies**: Confirm open query status is preserved
7. **Study configuration**: Verify study definitions, visit schedules, form definitions intact
8. **Integration data**: Check EDC-to-external system data consistency (CTMS, RTSM, labs)

### 6. Communication Plan

**During rollback execution:**

| Audience | Channel | Timing | Message |
|----------|---------|--------|---------|
| Engineering team | Slack war room | Immediate | Rollback initiated, reason, ETA |
| QA team | Slack + email | Immediate | Rollback in progress, standby for verification |
| Product management | Email + Slack | Within 15 min | Rollback summary, impact assessment |
| Customer Success | Email | Within 30 min | Customer-facing impact, talking points |
| Affected sponsors/sites | Email via CS | Within 1 hour | Service status, data impact (if any) |
| Regulatory/QA leadership | Email | Within 2 hours | Compliance impact assessment |

**Post-rollback communication:**
- Incident report within 24 hours
- Root cause analysis within 5 business days
- CAPA if regulatory impact identified

### 7. Post-Rollback Actions

1. Document the rollback execution with timestamps
2. Preserve all logs from the failed deployment
3. Update change control record with rollback details
4. Initiate root cause analysis
5. Assess whether a CAPA is required
6. Schedule retrospective
7. Update deployment procedures based on lessons learned
8. Re-plan the deployment with corrective measures

## Output Format

Generate the rollback plan as a structured markdown document with:
- Release identifier and version
- Target environment
- Rollback owner and team members
- Each section with numbered steps
- Estimated time for each rollback phase
- Sign-off section for plan approval

## Regulatory Notes

- Rollback of validated systems must be documented as part of the change control record
- Any period of system unavailability during rollback must be logged
- Data collected during a failed deployment period requires reconciliation documentation
- The rollback itself may require abbreviated validation (IQ at minimum)
