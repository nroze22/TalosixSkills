---
name: risk-assessment
description: Conduct risk assessments for clinical trial systems using FMEA methodology with risk scoring, mitigation strategies, and acceptance criteria covering patient safety and data integrity.
---

# Risk Assessment for Clinical Trial Systems

## Purpose

Conduct formal risk assessments for Talosix EDC systems and related clinical trial infrastructure. Assessments use Failure Mode and Effects Analysis (FMEA) methodology to identify, score, and mitigate risks to patient safety, data integrity, and regulatory compliance.

## When to Use

- New system or module development
- Major system changes (as input to change control)
- Periodic risk reassessment of validated systems
- Incident investigation and CAPA development
- Regulatory audit preparation
- Supplier qualification assessment
- Infrastructure changes affecting GxP systems

## Risk Assessment Framework

### Step 1: Define Scope and Context

Document the following before beginning assessment:

```
Risk Assessment ID: RA-[SYSTEM]-[YYYY]-[SEQ]
System/Process: [Name and version]
Scope: [What is being assessed]
Assessment Team: [Names, roles, qualifications]
Date: [Assessment date]
Review Period: [Next scheduled review]

Applicable Regulations:
- 21 CFR Part 11
- EU Annex 11
- ICH E6(R2) GCP
- GDPR (if EU data involved)
- [Other applicable standards]

Risk Categories:
- Patient Safety (PS)
- Data Integrity (DI)
- Regulatory Compliance (RC)
- Business Operations (BO)
- Data Privacy (DP)
```

### Step 2: Identify Failure Modes

Systematically identify potential failure modes across system components:

#### EDC System Failure Mode Categories

**A. Data Entry and Capture**
- Incorrect data entered without detection
- Data loss during entry (network failure, timeout)
- Edit checks failing to fire
- Duplicate records created
- Data type mismatches accepted

**B. Access Control and Security**
- Unauthorized access to patient data
- Privilege escalation
- Shared user accounts
- Session hijacking
- Inadequate password controls

**C. Audit Trail**
- Audit trail gaps or missing entries
- Audit trail data corruption
- Inability to reconstruct data history
- Audit trail disabled or bypassed
- Incorrect timestamp recording

**D. Electronic Signatures**
- Signature applied without proper authentication
- Signature meaning not displayed
- Signed record modified without re-signing
- Non-repudiation failure

**E. Data Export and Reporting**
- Incorrect data in exports
- Missing data in regulatory submissions
- CDISC format non-compliance
- Report calculation errors

**F. System Availability**
- System downtime during critical data entry
- Backup failure
- Disaster recovery failure
- Performance degradation under load

**G. Integration**
- Data corruption during transfer between systems
- Integration endpoint failure
- Data mapping errors
- Timing/synchronization issues

**H. Data Migration**
- Data loss during migration
- Data corruption during transformation
- Incomplete migration
- Mapping errors between source and target

### Step 3: FMEA Risk Scoring

#### Severity (S) - Impact if failure occurs

| Score | Level | Patient Safety | Data Integrity | Regulatory |
|-------|-------|---------------|----------------|------------|
| 1 | Negligible | No impact | Cosmetic issue only | No impact |
| 2 | Minor | No direct impact | Minor data discrepancy, easily corrected | Minor finding |
| 3 | Moderate | Potential indirect impact | Data error requiring investigation | Major finding |
| 4 | Major | Potential direct impact on safety decision | Significant data loss or corruption | Critical finding, warning letter |
| 5 | Critical | Direct patient harm possible | Unrecoverable data loss, submission integrity compromised | Clinical hold, consent decree |

#### Probability (P) - Likelihood of occurrence

| Score | Level | Frequency |
|-------|-------|-----------|
| 1 | Rare | Less than once per year |
| 2 | Unlikely | Once per year |
| 3 | Possible | Once per quarter |
| 4 | Likely | Once per month |
| 5 | Almost Certain | Once per week or more |

#### Detectability (D) - Ability to detect before impact

| Score | Level | Detection Capability |
|-------|-------|---------------------|
| 1 | Certain | Automated detection and prevention |
| 2 | High | Automated detection with alert |
| 3 | Moderate | Manual review process likely to detect |
| 4 | Low | Manual review may miss |
| 5 | None | No detection mechanism exists |

#### Risk Priority Number (RPN)

```
RPN = Severity x Probability x Detectability
Range: 1 to 125
```

### Step 4: Risk Classification

| RPN Range | Risk Level | Required Action |
|-----------|-----------|-----------------|
| 1-15 | Low | Accept. Document rationale. Monitor. |
| 16-35 | Medium | Mitigate where practical. Document rationale if accepted. |
| 36-64 | High | Mitigation required before production use. |
| 65-125 | Critical | Immediate mitigation required. Escalate to management. |

**Special rule**: Any item with Severity = 5 (patient safety or unrecoverable data loss) is automatically classified as High regardless of RPN, and requires documented mitigation.

### Step 5: FMEA Worksheet

```
| ID | Component | Failure Mode | Effect | Risk Cat | S | P | D | RPN | Level | Mitigation | Residual S | Residual P | Residual D | Residual RPN | Owner | Status |
```

### Step 6: Mitigation Strategies

For each risk requiring mitigation, define controls from these categories:

#### Preventive Controls
- Input validation and edit checks
- Role-based access control
- Automated workflow enforcement
- Configuration controls
- Training requirements

#### Detective Controls
- Audit trail review
- Data reconciliation
- Automated monitoring and alerting
- Periodic access reviews
- Medical review of data

#### Corrective Controls
- Backup and restore procedures
- Rollback procedures
- Incident response procedures
- CAPA process

### Step 7: Risk Acceptance

Document risk acceptance decisions:

```
Risk Acceptance Record:

Risk ID: [From FMEA worksheet]
Residual Risk Level: [After mitigation]
Accepted By: [Name, Role]
Date: [Date]
Rationale: [Why residual risk is acceptable]
Conditions: [Any conditions on acceptance]
Review Date: [When to reassess]
```

#### Risk Acceptance Authority

| Residual Risk Level | Approval Authority |
|---------------------|-------------------|
| Low | System Owner |
| Medium | System Owner + QA Manager |
| High | Head of Quality + Management |
| Critical | Cannot be accepted; must be mitigated further |

### Step 8: Patient Safety Risk Focus

Clinical trial EDC systems have specific patient safety risks requiring explicit assessment:

1. **Incorrect dosing information displayed** - Could a system error cause wrong dose to be administered?
2. **Adverse event reporting delay** - Could a system failure delay SAE reporting?
3. **Incorrect eligibility determination** - Could system errors enroll ineligible subjects?
4. **Unblinding** - Could the system inadvertently reveal treatment assignment?
5. **Missing safety data** - Could data loss hide safety signals?
6. **Incorrect lab reference ranges** - Could system display wrong normal ranges?

Each must be explicitly addressed in the risk assessment with documented controls.

### Step 9: Data Integrity Risk Focus (ALCOA+)

Verify risks to each ALCOA+ attribute:

| Attribute | Risk Question |
|-----------|--------------|
| **A**ttributable | Can we always identify who entered/modified data? |
| **L**egible | Is data always readable and permanent? |
| **C**ontemporaneous | Is data recorded at time of activity? |
| **O**riginal | Is original data preserved? |
| **A**ccurate | Does data correctly reflect what was observed? |
| **C**omplete | Is all data captured without gaps? |
| **C**onsistent | Is data chronologically ordered and consistent? |
| **E**nduring | Is data maintained for required retention period? |
| **A**vailable | Is data accessible when needed? |

### Step 10: Risk Assessment Report

Generate a report containing:

1. Executive summary with overall risk profile
2. Scope and methodology
3. Complete FMEA worksheet
4. Risk heat map (Severity vs Probability matrix)
5. Mitigation plan with owners and timelines
6. Risk acceptance records
7. Residual risk summary
8. Recommendations
9. Next review date
10. Approval signatures

## Ongoing Risk Management

- Review risk assessments annually or after significant changes
- Update after incidents or near-misses
- Track mitigation implementation status
- Report risk metrics to management quarterly
- Incorporate lessons learned from audits and inspections

## Regulatory References

- ICH Q9 - Quality Risk Management
- ISPE GAMP 5 - Risk-Based Approach
- FDA 21 CFR Part 11
- EU Annex 11 (Section 1: Risk Management)
- ICH E6(R2) - GCP
- ISO 14971 (risk management principles, adapted for software)
- FDA Guidance: Data Integrity and Compliance With Drug CGMP (2018)
