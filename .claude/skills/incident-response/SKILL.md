---
name: incident-response
description: Production incident response framework for Talosix EDC systems. Covers severity classification with patient safety considerations, investigation steps, root cause analysis, communication templates, and CAPA follow-up.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Incident Response Skill

Guide production incident response for Talosix EDC clinical trial systems. Clinical trial systems have unique incident response requirements because system failures can impact patient safety, data integrity, and regulatory compliance.

## When to Use

- A production incident has been reported or detected
- Drafting an incident response plan for a new system
- Conducting post-incident review and root cause analysis
- Preparing CAPA documentation after an incident

## Severity Classification

### Clinical Trial System Severity Matrix

| Severity | System Impact | Clinical Impact | Examples |
|----------|--------------|-----------------|----------|
| **SEV-1: Critical** | System unavailable or data integrity compromised | Patient safety risk or regulatory non-compliance | Data corruption in eCRFs; audit trail failure; system-wide outage during active data collection; e-signature system failure |
| **SEV-2: High** | Major feature unavailable, no workaround | Clinical workflow blocked for multiple sites | Form submission failures; randomization integration down; data export broken; query workflow non-functional |
| **SEV-3: Medium** | Feature degraded, workaround available | Clinical workflow impacted but can continue | Slow performance; intermittent errors with retry success; single-site access issues; reporting delays |
| **SEV-4: Low** | Minor issue, minimal user impact | No clinical workflow impact | UI cosmetic issues; non-critical notification delays; minor display errors |

### Patient Safety Impact Assessment

For every SEV-1 and SEV-2 incident, immediately assess:
- Can sites still record adverse events and SAEs?
- Is subject randomization affected?
- Can protocol deviations be documented?
- Is unblinding functionality available (for emergencies)?
- Are safety reports (CIOMS, MedWatch) generation affected?

If any answer is "No," escalate to Clinical Operations and Regulatory immediately.

## Incident Response Procedure

### Phase 1: Detection and Triage (Target: < 15 minutes)

1. **Acknowledge the incident**
   - Confirm the report (automated alert or user report)
   - Assign an Incident Commander (IC)
   - Create an incident channel (e.g., #inc-YYYY-MM-DD-description)

2. **Initial triage**
   - Classify severity using the matrix above
   - Identify affected components and studies
   - Determine blast radius: how many users, sites, studies impacted
   - Assess clinical impact using the patient safety checklist

3. **Assemble response team**
   - SEV-1: IC + Backend + Frontend + DBA + QA + Clinical Ops + CTO
   - SEV-2: IC + Backend + relevant domain engineer + QA
   - SEV-3: IC + assigned engineer
   - SEV-4: Assigned to sprint backlog

### Phase 2: Investigation (Target: varies by severity)

1. **Gather evidence**
   - Application logs (error logs, access logs)
   - Database logs and slow query logs
   - Infrastructure metrics (CPU, memory, disk, network)
   - Recent deployments or configuration changes
   - User-reported symptoms and reproduction steps

2. **Establish timeline**
   - When did the issue start?
   - What changed around that time? (deploys, config, traffic patterns)
   - Is the issue ongoing, intermittent, or resolved?

3. **Identify root cause or contributing factors**
   - Code defect
   - Infrastructure failure
   - Configuration error
   - Data issue
   - External dependency failure
   - Capacity/scaling issue

4. **Determine resolution path**
   - Can it be resolved with a configuration change?
   - Does it require a hotfix? (see hotfix-procedure skill)
   - Does it require a rollback? (see rollback-plan skill)
   - Can it be mitigated while a permanent fix is developed?

### Phase 3: Resolution

1. **Implement the fix or mitigation**
   - Follow hotfix procedure if code change required
   - Document all actions taken with timestamps
   - Have a second person verify the fix

2. **Verify resolution**
   - Confirm the reported issue is resolved
   - Run targeted smoke tests
   - Monitor for recurrence (minimum 1 hour for SEV-1/SEV-2)
   - Verify no collateral damage

3. **Stand down**
   - IC declares incident resolved
   - Update status page / communication channels
   - Set monitoring watch period

### Phase 4: Post-Incident

1. **Incident report** (within 24 hours)
   - Timeline of events
   - Root cause analysis
   - Impact assessment (users, studies, data)
   - Resolution details
   - Action items for prevention

2. **Post-incident review** (within 5 business days)
   - Blameless review with all involved parties
   - Focus on systemic improvements, not individual fault
   - Document what went well and what needs improvement
   - Generate action items with owners and due dates

3. **CAPA assessment** (see below)

## Root Cause Analysis Framework

Use the "5 Whys" method or Fishbone (Ishikawa) diagram:

```
Problem: [Describe the incident]
  Why 1: [First-level cause]
    Why 2: [Deeper cause]
      Why 3: [Deeper cause]
        Why 4: [Deeper cause]
          Why 5: [Root cause]

Root Cause: [Summary]
Contributing Factors: [List]
```

Categories for Fishbone analysis:
- **Code**: Bugs, logic errors, edge cases
- **Infrastructure**: Hardware, network, cloud services
- **Process**: Deployment, testing, review gaps
- **People**: Training, communication, handoff issues
- **Data**: Data quality, migration, integration
- **External**: Third-party services, dependencies

## CAPA Follow-Up

A Corrective and Preventive Action is REQUIRED when:
- Patient safety was potentially impacted
- Clinical data integrity was compromised
- Regulatory non-compliance occurred
- The same root cause has caused previous incidents

CAPA documentation must include:
1. **Description of the nonconformity** (what happened)
2. **Impact assessment** (who/what was affected)
3. **Root cause** (from RCA above)
4. **Corrective action** (fix the immediate problem)
5. **Preventive action** (prevent recurrence)
6. **Effectiveness check** (how to verify the CAPA worked)
7. **Timeline and ownership** (who does what by when)

## Communication Templates

### Internal: Incident Declared
```
@channel INCIDENT DECLARED - SEV-<level>

Issue: <Brief description>
Impact: <What is affected>
Clinical Impact: <Assessment>
Affected Studies: <List or "Assessing">
IC: <Name>
War Room: <Channel/link>

Status: Investigating
Next update: <Time>
```

### Customer-Facing: Issue Acknowledged
```
Subject: Talosix System Alert - <Brief Description>

We have identified an issue affecting <description of impact>.

Impact: <What users may experience>
Workaround: <If available>
Status: Our team is actively investigating and working on resolution.

We will provide updates every <frequency>.

If you have questions, contact support@talosix.com.
```

### Regulatory Notification (if required)
```
Subject: System Incident Notification - <System Name>

Incident Date/Time: <Timestamp>
System: <Affected system>
Description: <What occurred>
Clinical Impact Assessment: <Detailed assessment>
Data Integrity Impact: <Assessment>
Studies Affected: <List>
Corrective Actions Taken: <Summary>
CAPA Status: <Initiated / Under Assessment>

This notification is provided in accordance with [applicable regulation/SOP].
```

## Using Allowed Tools

During incident investigation:
- Use **Read** to examine log files, configuration, and source code
- Use **Grep** to search for error patterns across logs and codebase
- Use **Glob** to locate relevant files (logs, configs, source)
- Use **Bash** to run diagnostic commands, check service status, query databases
