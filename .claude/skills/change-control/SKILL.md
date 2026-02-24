---
name: change-control
description: Manage change control for validated clinical trial systems including impact assessment, classification, documentation requirements, and approval workflows.
---

# Change Control Management

## Purpose

Manage change control processes for Talosix validated EDC systems. All changes to validated systems must follow a formal change control process to maintain the validated state, ensure data integrity, and comply with regulatory requirements.

## When to Use

- Any modification to a validated system (software, hardware, infrastructure)
- Configuration changes to production EDC environments
- Database schema changes
- Integration endpoint changes
- Security patches or updates
- SOP or process changes affecting validated systems
- Emergency fixes to production systems

## Change Control Process

### Step 1: Change Request Initiation

Generate a Change Request (CR) document with:

```
Change Request ID: CR-[YYYY]-[SEQ]
Date Initiated: [DATE]
Requestor: [NAME, ROLE]
System Affected: [SYSTEM NAME, VERSION]
Environment: [PROD/STAGING/DR]

Change Title: [Brief descriptive title]

Description of Change:
- Current state
- Proposed change
- Reason/justification for change
- Regulatory driver (if applicable)

Affected Studies: [List all active studies on the system]
Affected Modules: [List system modules impacted]

Priority: Critical / High / Medium / Low
Requested Implementation Date: [DATE]
```

### Step 2: Impact Assessment

Evaluate the change across five dimensions:

#### A. Regulatory Impact

| Question | Yes/No | Details |
|---|---|---|
| Does this change affect 21 CFR Part 11 compliance? | | |
| Does this change affect audit trail functionality? | | |
| Does this change affect electronic signatures? | | |
| Does this change affect access controls? | | |
| Does this change affect data export for regulatory submission? | | |
| Does this change require notification to regulatory authorities? | | |
| Does this change affect EU Annex 11 compliance? | | |
| Does this change affect GDPR data handling? | | |

#### B. Functional Impact

| Question | Yes/No | Details |
|---|---|---|
| Does this change affect data entry workflows? | | |
| Does this change affect edit checks or validations? | | |
| Does this change affect reports or data extracts? | | |
| Does this change affect integrations with external systems? | | |
| Does this change affect user interface or user experience? | | |
| Does this change introduce new functionality? | | |
| Does this change remove existing functionality? | | |

#### C. Data Integrity Impact

| Question | Yes/No | Details |
|---|---|---|
| Does this change modify database schema? | | |
| Does this change affect existing data? | | |
| Does this change require data migration? | | |
| Does this change affect data backup or recovery? | | |
| Could this change result in data loss if it fails? | | |
| Does this change affect CDISC data standards compliance? | | |

#### D. Validation Impact

| Question | Yes/No | Details |
|---|---|---|
| Does this change affect previously validated functionality? | | |
| Does this change require revalidation? | | |
| What level of testing is required (full/partial/regression)? | | |
| Does this change require updated validation documentation? | | |
| Does this change affect the traceability matrix? | | |

#### E. Operational Impact

| Question | Yes/No | Details |
|---|---|---|
| Does this change require system downtime? | | |
| Does this change affect active clinical studies? | | |
| Does this change require user retraining? | | |
| Does this change require updated SOPs? | | |
| Does this change affect disaster recovery procedures? | | |

### Step 3: Change Classification

Based on impact assessment, classify the change:

#### Major Change
- Affects validated functionality
- Requires formal revalidation (IQ/OQ/PQ as appropriate)
- Requires QA approval before implementation
- Requires updated validation documentation
- Examples: new module, database schema change, workflow modification, security model change

#### Minor Change
- Does not affect validated functionality
- Requires regression testing only
- Requires QA notification
- May require documentation updates
- Examples: cosmetic UI changes, performance optimization, non-functional configuration

#### Emergency Change
- Required to address critical production issue
- Abbreviated approval process (can be retrospective)
- Minimum two approvers (system owner + QA)
- Full documentation required within 5 business days post-implementation
- Requires post-implementation review
- Examples: critical bug fix, security vulnerability patch, data corruption fix

### Step 4: Documentation Requirements

#### For Major Changes

1. Updated User Requirements Specification (if scope changes)
2. Updated Functional Specification
3. Updated Design Specification
4. Validation Protocol (IQ/OQ/PQ as determined by impact assessment)
5. Test scripts with expected results
6. Updated Traceability Matrix
7. Validation Summary Report
8. Updated SOPs (if process changes)
9. Training records (if retraining required)

#### For Minor Changes

1. Change request with impact assessment
2. Regression test plan and results
3. Updated documentation (as applicable)
4. Release notes

#### For Emergency Changes

1. Emergency change request (abbreviated)
2. Minimum testing evidence
3. Post-implementation: full change request, impact assessment, and testing documentation within 5 business days
4. Root cause analysis
5. Post-implementation review record

### Step 5: Approval Workflow

#### Major Change Approval Chain

```
1. Requestor submits CR
2. System Owner reviews and approves scope
3. Technical Lead reviews technical approach
4. QA reviews impact assessment and validation plan
5. Regulatory Affairs reviews regulatory impact (if flagged)
6. Data Management reviews data impact (if flagged)
7. Study teams acknowledge for affected studies
8. Change Control Board (CCB) final approval
9. Implementation authorized
```

#### Minor Change Approval Chain

```
1. Requestor submits CR
2. System Owner reviews and approves
3. Technical Lead approves technical approach
4. QA acknowledges
5. Implementation authorized
```

#### Emergency Change Approval Chain

```
1. Requestor initiates emergency CR
2. System Owner approves (verbal acceptable, documented within 24h)
3. QA approves (verbal acceptable, documented within 24h)
4. Implementation authorized
5. Post-implementation: full approval chain within 5 business days
```

### Step 6: Implementation Tracking

Track each change through these statuses:

```
Draft → Under Review → Approved → Scheduled → In Progress →
Testing → QA Review → Implemented → Closed
```

For each status transition, capture:
- Date and time
- Person responsible
- Comments or conditions

### Step 7: Post-Implementation Review

After implementation, document:

1. Implementation date and time
2. Implementer name
3. Deviations from plan (if any)
4. Test execution results
5. Validation status (validated/conditionally validated)
6. Outstanding issues or risks
7. Lessons learned

## Change Control Register

Maintain a master register of all changes:

```
| CR ID | Title | System | Classification | Status | Requested | Implemented | Validation Impact |
|-------|-------|--------|---------------|--------|-----------|-------------|-------------------|
```

## Study Impact Notification

When a change affects active clinical studies:

1. Identify all affected studies
2. Notify study teams with change summary
3. Document notification in change record
4. Obtain study team acknowledgment
5. Update study-specific validation documentation if required

## Metrics and Reporting

Track change control metrics:

- Total changes by classification per period
- Average time from request to implementation
- Number of emergency changes (target: minimize)
- Deviation rate during implementation
- Changes requiring rollback

## Regulatory References

- FDA 21 CFR Part 11 - Electronic Records; Electronic Signatures
- EU Annex 11 - Computerised Systems (Section 10: Change Management)
- ICH E6(R2) - GCP (Section 5.5: Trial Management, Data Handling)
- ISPE GAMP 5 - Operational Change and Configuration Management
- FDA Guidance: Computer Systems Used in Clinical Investigations (1999/2007)
