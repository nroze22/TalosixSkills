---
name: hotfix-procedure
description: "Emergency hotfix process for Talosix EDC production issues. Covers expedited change control, abbreviated but compliant validation, and communication/notification procedures."
allowed-tools: Read, Grep, Glob, Bash
---

# Hotfix Procedure Skill

Guide the creation and execution of emergency hotfix procedures for Talosix EDC production systems. Hotfixes bypass the standard release cycle but must still maintain regulatory compliance for validated clinical trial systems.

## When to Use

- Production defect impacting clinical data collection or integrity
- Security vulnerability requiring immediate remediation
- System availability issue not resolvable by infrastructure changes alone
- Regulatory compliance gap discovered in production

## Hotfix Classification

### When a Hotfix Is Warranted

A hotfix (emergency change) is justified when:
- Patient safety is potentially impacted
- Clinical data integrity is at risk
- System is unavailable and no infrastructure workaround exists
- A security breach or critical vulnerability is actively exploitable
- Regulatory non-compliance is discovered that cannot wait for next release

### When a Hotfix Is NOT Warranted

Route through standard release process instead:
- Cosmetic or UI issues with no data impact
- Feature requests or enhancements
- Performance issues with acceptable workarounds
- Non-critical bugs with workarounds documented

## Hotfix Procedure

### Phase 1: Authorization (Target: < 30 minutes)

1. **Report and classify** the production issue
   - Document the issue: symptoms, impact, affected studies/sites
   - Classify severity (SEV-1 or SEV-2 required for hotfix)
   - Identify clinical impact: data integrity, patient safety, regulatory

2. **Obtain emergency change authorization**
   - Contact: Engineering Manager + QA Lead + Product Owner
   - For patient safety impact: additionally notify Clinical Operations Director
   - Document verbal approval (written follow-up within 24 hours)
   - Record: approver names, timestamp, justification for emergency path

3. **Assign hotfix team**
   - Developer (code fix)
   - Reviewer (code review)
   - QA Engineer (abbreviated validation)
   - Deployment lead (production deployment)

### Phase 2: Development (Target: scope-dependent)

1. **Create hotfix branch**
   ```
   Branch from: production tag (not develop)
   Naming: hotfix/<ticket-id>-<short-description>
   ```

2. **Implement the fix**
   - Minimal change scope: fix only the identified issue
   - No refactoring, no feature additions, no "while we're here" changes
   - Include unit tests covering the fix
   - Update any affected validation test scripts

3. **Code review**
   - Required even for hotfixes: minimum one reviewer
   - Focus review on: correctness, scope limitation, regression risk
   - Document review in pull request

4. **Merge strategy**
   - Merge hotfix branch to production branch/tag
   - Immediately also merge to develop/main to prevent regression

### Phase 3: Abbreviated Validation (Target: < 2 hours)

Hotfixes require abbreviated but compliant validation:

1. **Risk-based test scope**
   - Test the specific fix (direct verification)
   - Test immediately adjacent functionality (regression)
   - Test core critical paths: form save, audit trail, e-signatures
   - Document rationale for test scope limitation

2. **Required validation artifacts (abbreviated set)**
   - [ ] Hotfix change request document (can be post-hoc within 24 hours)
   - [ ] Risk assessment for the fix (brief, targeted)
   - [ ] Test protocol covering fix verification and critical regression
   - [ ] Test execution evidence (screenshots, logs)
   - [ ] Traceability: defect -> fix -> test -> result

3. **Validation execution**
   - Execute in pre-production environment first
   - All targeted tests must pass (no waivers on abbreviated set)
   - Document any observations or deviations
   - QA sign-off on test results

### Phase 4: Deployment

1. **Pre-deployment**
   - Database backup (mandatory, no exceptions)
   - Confirm rollback procedure for this specific hotfix
   - Notify stakeholders: deployment starting

2. **Deploy to production**
   - Follow standard deployment procedure
   - Execute with deployment checklist (abbreviated for hotfix scope)

3. **Post-deployment verification**
   - Execute smoke tests on the fixed functionality
   - Verify fix resolves the reported issue
   - Monitor for 1 hour minimum (2 hours for data integrity issues)
   - Confirm no new errors introduced

### Phase 5: Post-Hotfix Documentation (Within 24-48 hours)

1. **Complete change control documentation**
   - Formalize the emergency change request
   - Obtain written approvals (retroactive)
   - Document the full timeline of events
   - Attach all validation artifacts

2. **Update validation records**
   - Update traceability matrix
   - File test evidence in validation repository
   - Update system validation summary if needed

3. **Root cause analysis**
   - Why did the defect escape to production?
   - What process improvement prevents recurrence?
   - Document findings

4. **CAPA assessment**
   - Determine if a Corrective and Preventive Action is required
   - If yes, initiate CAPA process with QA

## Communication Templates

### Stakeholder Notification: Hotfix Initiated
```
Subject: [HOTFIX] <System> - <Brief Issue Description>

Severity: SEV-<1|2>
Issue: <Description of production issue>
Impact: <Affected studies, sites, functionality>
Clinical Impact: <Data integrity / patient safety assessment>
Hotfix ETA: <Estimated completion time>
Authorized by: <Name(s)>

Updates will be provided every <30|60> minutes.
```

### Stakeholder Notification: Hotfix Complete
```
Subject: [RESOLVED] <System> - <Brief Issue Description>

Resolution: <What was fixed>
Deployed: <Timestamp>
Verified: <Confirmation of fix>
Data Impact: <Any data reconciliation required>
Action Required: <Any follow-up needed from recipients>

Full incident report to follow within 24 hours.
```

## Using Allowed Tools

- Use **Read** to examine source code and configuration related to the issue
- Use **Grep** to search for error patterns, related code paths, and impact scope
- Use **Glob** to find affected files and test files
- Use **Bash** to run tests, check build status, and verify deployments

## Regulatory Compliance Notes

Even under emergency procedures:
- Audit trail must never be bypassed or disabled
- Electronic signature controls must remain intact
- All changes must be traceable and documented (post-hoc is acceptable)
- Abbreviated validation is permitted but must be risk-justified
- QA must be involved in validation, even if abbreviated
- The hotfix process itself should be defined in the company's SOP for change control
