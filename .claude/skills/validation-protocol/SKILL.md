---
name: validation-protocol
description: Draft validation protocols for clinical trial software including IQ/OQ/PQ, 21 CFR Part 11 compliance, and GAMP 5 risk-based approach with complete test scripts and traceability.
---

# Validation Protocol Drafting

## Purpose

Generate validation protocols for Talosix clinical trial EDC software systems. Protocols must satisfy FDA 21 CFR Part 11, EU Annex 11, and follow ISPE GAMP 5 risk-based validation methodology.

## When to Use

- New system or module requires validation before production use
- Existing validated system undergoes a major change requiring revalidation
- Periodic revalidation is due
- Regulatory submission requires validation documentation

## Validation Lifecycle Phases

### 1. Validation Master Plan (VMP)

Every protocol suite begins with a VMP section that defines:

- System description and intended use in clinical trial context
- Regulatory requirements applicable (21 CFR Part 11, EU Annex 11, ICH E6(R2))
- GAMP 5 software category classification (typically Category 4 or 5 for EDC)
- Validation strategy and scope
- Roles and responsibilities (QA, IT, system owner, process owner)
- Acceptance criteria summary
- Deliverables list

### 2. Installation Qualification (IQ)

Verify the system is installed correctly per specifications.

**IQ Protocol Structure:**

```
1. Protocol Header
   - Protocol ID: IQ-[SYSTEM]-[VERSION]-[SEQ]
   - System name, version, environment
   - Author, reviewer, approver with dates

2. Objective
   - Confirm installation matches approved design specifications

3. Scope
   - Hardware, software, infrastructure components
   - Environment (production, staging, DR)

4. Prerequisites
   - Approved design specifications
   - Approved installation procedures
   - Environment readiness confirmation

5. Test Scripts (IQ)
   - IQ-001: Verify server/cloud infrastructure configuration
   - IQ-002: Verify database installation and version
   - IQ-003: Verify application deployment and version
   - IQ-004: Verify network connectivity and firewall rules
   - IQ-005: Verify SSL/TLS certificate installation
   - IQ-006: Verify integration endpoints accessible
   - IQ-007: Verify backup configuration
   - IQ-008: Verify audit trail mechanism is active
   - IQ-009: Verify time synchronization (NTP) for audit trails

6. Each test script contains:
   - Test ID, title, objective
   - Prerequisites
   - Step-by-step instructions
   - Expected results (specific, measurable)
   - Actual results (blank for execution)
   - Pass/Fail
   - Tester signature and date
   - Deviations section

7. Summary and Approval
```

### 3. Operational Qualification (OQ)

Verify the system operates correctly per functional specifications.

**OQ Protocol Structure:**

```
1. Protocol Header
   - Protocol ID: OQ-[SYSTEM]-[VERSION]-[SEQ]

2. Test Categories for EDC Systems:
   a. User Management & Access Control
      - OQ-001: User creation with role assignment
      - OQ-002: Role-based access control enforcement
      - OQ-003: Password policy enforcement (complexity, expiration, history)
      - OQ-004: Account lockout after failed attempts
      - OQ-005: Session timeout enforcement
      - OQ-006: Unique user identification verification

   b. Electronic Signatures (21 CFR Part 11)
      - OQ-010: Signature requires user ID + password
      - OQ-011: Signature meaning displayed (e.g., "approved", "reviewed")
      - OQ-012: Signed records cannot be modified without new signature
      - OQ-013: Signature linked to date/time
      - OQ-014: Consecutive signings require full credentials

   c. Audit Trail
      - OQ-020: All create/modify/delete actions logged
      - OQ-021: Audit trail captures who, what, when, why
      - OQ-022: Audit trail cannot be modified or disabled by users
      - OQ-023: Audit trail entries include previous and new values
      - OQ-024: Reason for change captured where required

   d. Data Entry and Edit Checks
      - OQ-030: Required field enforcement
      - OQ-031: Data type validation (numeric, date, text)
      - OQ-032: Range checks
      - OQ-033: Cross-field edit checks
      - OQ-034: Query generation for discrepancies

   e. Data Export and Reporting
      - OQ-040: Data export accuracy verification
      - OQ-041: Report generation correctness
      - OQ-042: CDISC format compliance (if applicable)

   f. System Administration
      - OQ-050: Study build and configuration
      - OQ-051: Backup and restore functionality
      - OQ-052: System availability monitoring

3. Each test script format: same as IQ above
4. Negative testing: confirm system rejects invalid operations
5. Boundary testing: edge cases for data entry fields
```

### 4. Performance Qualification (PQ)

Verify the system performs as intended in actual use conditions.

**PQ Protocol Structure:**

```
1. Protocol Header
   - Protocol ID: PQ-[SYSTEM]-[VERSION]-[SEQ]

2. Test Categories:
   a. End-to-End Clinical Workflows
      - PQ-001: Complete subject enrollment workflow
      - PQ-002: CRF data entry through to lock
      - PQ-003: Query resolution lifecycle
      - PQ-004: SAE reporting workflow
      - PQ-005: Medical coding workflow
      - PQ-006: Data review and sign-off

   b. Concurrent User Testing
      - PQ-010: Multiple simultaneous users performing data entry
      - PQ-011: System response time under load

   c. Business Continuity
      - PQ-020: Failover testing
      - PQ-021: Backup restoration verification
      - PQ-022: Disaster recovery procedure execution

3. Acceptance Criteria:
   - All critical test cases pass (zero critical failures)
   - Non-critical deviations documented with risk assessment
   - System response time within defined thresholds
   - No data integrity issues observed
```

## 21 CFR Part 11 Compliance Checklist

Every validation protocol must address these requirements:

| Requirement | CFR Section | Validation Test |
|---|---|---|
| Unique user IDs | 11.10(d) | OQ user management tests |
| Electronic signatures | 11.50, 11.70, 11.100, 11.200 | OQ signature tests |
| Audit trails | 11.10(e) | OQ audit trail tests |
| System access controls | 11.10(d), 11.10(g) | OQ access control tests |
| Authority checks | 11.10(g) | OQ role-based access tests |
| Device checks | 11.10(h) | IQ infrastructure tests |
| Training | 11.10(i) | Documented in VMP |
| Written policies | 11.10(j) | Referenced SOPs |
| System documentation | 11.10(k) | Validation deliverables |
| Open/closed system controls | 11.10, 11.30 | OQ security tests |
| Record retention | 11.10(c) | OQ data management tests |

## GAMP 5 Risk-Based Approach

### Software Category Classification

- **Category 1**: Infrastructure software (OS, databases) - minimal validation
- **Category 3**: Non-configured products - standard validation
- **Category 4**: Configured products - configuration verification focus
- **Category 5**: Custom applications - full lifecycle validation

Talosix EDC typically falls under Category 4 (configured) or Category 5 (custom modules).

### Risk-Based Testing Intensity

- **High risk** (patient safety, data integrity): Exhaustive testing, independent verification
- **Medium risk** (operational impact): Standard testing with review
- **Low risk** (minimal impact): Verification by inspection or reduced testing

## Traceability Requirements

Every test case must trace back to:

1. User Requirement Specification (URS) item
2. Functional Specification (FS) item
3. Design Specification (DS) item (where applicable)

Format: `URS-XXX → FS-XXX → DS-XXX → OQ-XXX`

## Deviation Management

During protocol execution:

1. Document deviation immediately on deviation form
2. Classify: Critical / Major / Minor
3. Assess impact on validation conclusion
4. Determine root cause
5. Define corrective action
6. Re-execute affected tests if required
7. Include in validation summary report

## Validation Summary Report

After protocol execution, generate a summary report containing:

- Execution summary (total tests, pass/fail counts)
- Deviation summary with dispositions
- Unresolved issues and risk assessment
- Conclusion statement on system validated state
- Recommendation for production release
- Approval signatures

## Output Format

When generating a validation protocol, produce:

1. Complete protocol document in markdown with formal structure
2. Individual test scripts as separate sections
3. Traceability matrix linking requirements to test cases
4. Deviation log template
5. Summary report template

## Regulatory References

- FDA 21 CFR Part 11 - Electronic Records; Electronic Signatures
- EU Annex 11 - Computerised Systems
- ISPE GAMP 5 - A Risk-Based Approach to Compliant GxP Computerized Systems
- ICH E6(R2) - Good Clinical Practice
- FDA Guidance: Part 11 Scope and Application (2003)
- ISPE GAMP Good Practice Guide: Testing of GxP Systems
