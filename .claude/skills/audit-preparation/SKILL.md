---
name: audit-preparation
description: Prepare for regulatory audits and inspections from FDA, EMA, and sponsors including checklist generation, document review, gap analysis, CAPA preparation, and common findings remediation.
---

# Audit Preparation

## Purpose

Prepare Talosix for regulatory audits and inspections of EDC systems used in clinical trials. Covers FDA inspections, EMA inspections, sponsor audits, and internal quality audits with systematic readiness assessment.

## When to Use

- Notification of upcoming regulatory inspection
- Scheduled sponsor audit
- Internal audit program execution
- Post-audit CAPA development and tracking
- Ongoing inspection readiness maintenance
- New client due diligence audit

## Audit Types

### FDA Inspection

**Triggers**: Pre-Approval Inspection (PAI), routine surveillance, for-cause investigation, NDA/BLA review.

**Scope typically covers**:
- 21 CFR Part 11 compliance
- System validation documentation
- Data integrity controls
- Electronic signature implementation
- Audit trail review
- Change control records
- SOPs and training records
- CAPA history

**Duration**: Typically 3-5 days on-site (or remote).

### EMA/National Competent Authority Inspection

**Triggers**: GCP inspection program, marketing authorization application, for-cause.

**Scope typically covers**:
- EU Annex 11 compliance
- Computerised system validation
- Data integrity per ALCOA+ principles
- GDPR compliance for clinical data
- Vendor qualification
- Business continuity

### Sponsor Audit

**Triggers**: Vendor qualification, periodic oversight, for-cause.

**Scope varies by sponsor but typically includes**:
- System validation summary
- SOPs and compliance documentation
- Security and access controls
- Data handling procedures
- Change control and release management
- Incident and CAPA history
- Business continuity and disaster recovery
- Subcontractor management

## Pre-Audit Preparation

### Phase 1: Audit Notification Response (Days 1-3)

1. **Acknowledge receipt** of audit notification
2. **Identify audit scope** from notification letter
3. **Assign audit coordinator** (typically QA lead)
4. **Form preparation team** with subject matter experts
5. **Establish preparation timeline** working backward from audit date
6. **Communicate audit details** to all affected departments

### Phase 2: Document Readiness Review (Days 3-14)

#### Core Document Inventory

Verify availability and currency of:

```
Validation Documentation:
[ ] Validation Master Plan (current version)
[ ] System validation protocols (IQ, OQ, PQ)
[ ] Validation execution records
[ ] Validation summary reports
[ ] Requirements traceability matrix
[ ] Risk assessments

SOPs:
[ ] All GxP SOPs current (not past review date)
[ ] SOP index/master list
[ ] Superseded SOP archive accessible

Training Records:
[ ] Training matrix for all system users
[ ] Training records for current SOPs
[ ] Role-specific qualification records
[ ] CV/resume file for key personnel

Change Control:
[ ] Change control register (complete)
[ ] Recent change requests (last 12-24 months)
[ ] Emergency change records
[ ] Change implementation evidence

Quality Records:
[ ] CAPA log (open and closed)
[ ] Deviation log
[ ] Incident reports
[ ] Internal audit reports
[ ] Management review minutes

System Records:
[ ] System configuration documentation
[ ] Access control matrix (current)
[ ] User account list with roles
[ ] Periodic access review records
[ ] Audit trail samples
[ ] Backup and recovery test records
[ ] Disaster recovery test records
[ ] Security assessment reports
```

#### Document Gap Analysis

For each document category:

```
| Document | Required | Available | Current | Complete | Gap | Remediation | Owner | Due Date |
|----------|----------|-----------|---------|----------|-----|-------------|-------|----------|
```

### Phase 3: System Readiness Verification (Days 7-21)

#### Technical Checklist

```
Audit Trail:
[ ] Audit trail active on all GxP tables
[ ] Audit trail entries include who, what, when, old value, new value
[ ] Reason for change captured where required
[ ] Audit trail cannot be disabled by users
[ ] Audit trail readable and exportable
[ ] Sample audit trail report generated and reviewed

Access Controls:
[ ] All accounts are individual (no shared/generic accounts)
[ ] Dormant accounts disabled (per policy, typically 90 days)
[ ] Role assignments appropriate (no excessive privileges)
[ ] Password policy enforced (complexity, expiration, history)
[ ] Session timeout configured and working
[ ] Account lockout functioning

Electronic Signatures:
[ ] Signatures require unique ID + password
[ ] Signature meaning displayed and recorded
[ ] Signed records show signer name, date/time, meaning
[ ] Consecutive signatures require full re-authentication
[ ] Signature binding to record is intact

System Administration:
[ ] Time synchronization verified (NTP)
[ ] SSL/TLS certificates valid and not expiring soon
[ ] Backup schedule current and verified
[ ] Most recent backup restoration test documented
[ ] System monitoring active
[ ] Capacity adequate
```

### Phase 4: Personnel Preparation (Days 14-28)

#### Staff Briefing Checklist

```
[ ] All potential interviewees identified
[ ] Audit conduct expectations communicated
[ ] Subject matter experts assigned to audit topics
[ ] Back room team assembled (document retrieval, real-time support)
[ ] Roles defined: audit host, scribe, runners, SMEs
[ ] Practice sessions conducted for key topics

Interview Preparation Topics:
[ ] System description and intended use
[ ] Validation approach and current validated state
[ ] Change control process (walk through recent example)
[ ] Incident management (walk through recent example)
[ ] Data integrity controls
[ ] User access management
[ ] Audit trail functionality
[ ] Electronic signature process
[ ] Backup and disaster recovery
[ ] Vendor management
[ ] Training process
```

#### Interview Guidelines

- Answer only what is asked; do not volunteer extra information
- If unsure, say "I will verify and provide that information"
- Never guess or speculate
- Refer to documented procedures
- Be prepared to demonstrate system functionality live
- Have printed copies of key documents available as backup

## During the Audit

### Daily Audit Management

```
Morning:
[ ] Review agenda for the day
[ ] Confirm SME availability
[ ] Prepare requested documents

During sessions:
[ ] Scribe documents all questions and responses
[ ] Track all document requests
[ ] Note potential observations immediately
[ ] Escalate concerns to audit coordinator

Evening:
[ ] Debrief with preparation team
[ ] Review day's observations and commitments
[ ] Prepare documents requested for next day
[ ] Update management on audit progress
[ ] Plan remediation for any identified gaps
```

### Observation Response

When an auditor raises an observation:

1. Listen fully; do not interrupt
2. Confirm understanding by restating
3. If additional context would help, provide it factually
4. If evidence exists that addresses the concern, offer to provide it
5. Document the observation exactly as stated
6. Do not argue or become defensive

## Post-Audit Activities

### Audit Report Response

#### Timeline

| Activity | FDA | EMA | Sponsor |
|----------|-----|-----|---------|
| Response due | 15 business days (483) | Per inspection report | Per audit report |
| CAPA plan due | With response | Typically 30 days | Typically 30 days |
| CAPA completion | As committed | As committed | As committed |

### CAPA Development

For each finding/observation:

```
CAPA Record:

CAPA ID: CAPA-[YYYY]-[SEQ]
Finding Reference: [Audit report item number]
Finding Category: Critical / Major / Minor / Observation

Finding Description:
[Exact text from audit report]

Root Cause Analysis:
- Method used: [5-Why / Fishbone / Fault Tree]
- Root cause identified: [Description]
- Contributing factors: [List]

Corrective Action (fix the specific instance):
- Action: [Description]
- Owner: [Name]
- Due date: [Date]
- Evidence of completion: [What will demonstrate completion]

Preventive Action (prevent recurrence):
- Action: [Description]
- Owner: [Name]
- Due date: [Date]
- Evidence of completion: [What will demonstrate completion]

Effectiveness Check:
- Method: [How effectiveness will be verified]
- Date: [When effectiveness will be checked]
- Owner: [Name]
```

### Common Audit Findings for EDC Systems

#### 21 CFR Part 11 Findings

| Finding | Root Cause | Remediation |
|---------|-----------|-------------|
| Shared user accounts | Operational convenience | Enforce individual accounts; update SOP |
| Incomplete audit trail | Configuration gap | Enable audit trail on all GxP fields; verify |
| Missing reason for change | System configuration | Configure mandatory reason for change fields |
| Inadequate password controls | Policy not enforced technically | Implement technical password policy controls |
| No periodic access review | Process gap | Implement quarterly access review procedure |
| Audit trail modifiable | System design flaw | Implement immutable audit trail; revalidate |
| E-signature not linked to record | Implementation gap | Fix signature binding; revalidate |

#### Validation Findings

| Finding | Root Cause | Remediation |
|---------|-----------|-------------|
| Incomplete validation | Scope gaps | Gap analysis and supplemental validation |
| No traceability matrix | Documentation gap | Create RTM; verify coverage |
| Test scripts lack detail | Inadequate review | Revise test scripts; re-execute as needed |
| Validation not maintained | Change control gap | Assess cumulative changes; revalidate |
| No periodic review | Process gap | Implement periodic review SOP; execute |

#### Data Integrity Findings

| Finding | Root Cause | Remediation |
|---------|-----------|-------------|
| Data deleted without audit trail | System gap | Implement soft delete with audit trail |
| Backdated entries possible | No system control | Implement system-generated timestamps |
| No backup verification | Process gap | Implement and document backup restore tests |
| Inadequate data review | Process gap | Implement documented data review procedures |

## Ongoing Inspection Readiness

### Monthly Activities
- Review and resolve open CAPAs
- Verify training records current
- Check SOP review dates

### Quarterly Activities
- User access review
- Audit trail sample review
- Backup restoration test
- CAPA effectiveness checks
- Update document inventory

### Annual Activities
- Internal audit (full scope)
- Risk assessment review
- Validation periodic review
- Disaster recovery test
- Management review of quality metrics
- Mock inspection (recommended)

## Output Format

When preparing for an audit, generate:

1. Tailored preparation checklist based on audit type and scope
2. Document inventory with gap analysis
3. Technical system readiness checklist
4. Personnel preparation guide with interview topics
5. CAPA templates for identified gaps
6. Daily audit management checklist

## Regulatory References

- FDA Compliance Program Guidance Manual 7348.811 (Bioresearch Monitoring - Sponsors, CROs, and Monitors)
- FDA Regulatory Procedures Manual Chapter 5 (Establishment Inspections)
- EMA Procedure for GCP Inspections
- ICH E6(R2) - GCP (Section 5.5.3)
- 21 CFR Part 11
- EU Annex 11
- FDA Form 483 response guidance
- FDA Warning Letter response guidance
