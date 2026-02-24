---
name: traceability-matrix
description: Create requirements traceability matrices mapping user requirements through functional specs, design specs, and test cases with gap analysis in regulatory submission ready format.
---

# Requirements Traceability Matrix

## Purpose

Create and maintain Requirements Traceability Matrices (RTMs) for Talosix EDC systems to demonstrate complete, bidirectional traceability from user needs through implementation and verification. RTMs are essential for regulatory submissions and audit readiness.

## When to Use

- New system or module validation
- Change control impact assessment
- Gap analysis during validation
- Regulatory submission preparation
- Audit preparation and response
- Periodic validation review

## Traceability Chain

The full traceability chain maps four levels:

```
User Requirements (URS)
    ↓
Functional Specifications (FS)
    ↓
Design Specifications (DS)
    ↓
Test Cases (IQ/OQ/PQ)
```

Every requirement at each level must trace forward and backward through the chain.

## Document Identification Standards

### Naming Conventions

```
User Requirements:    URS-[MODULE]-[SEQ]     e.g., URS-AUTH-001
Functional Specs:     FS-[MODULE]-[SEQ]       e.g., FS-AUTH-001
Design Specs:         DS-[MODULE]-[SEQ]       e.g., DS-AUTH-001
IQ Test Cases:        IQ-[SEQ]                e.g., IQ-001
OQ Test Cases:        OQ-[MODULE]-[SEQ]       e.g., OQ-AUTH-001
PQ Test Cases:        PQ-[SEQ]                e.g., PQ-001
Risk Assessment:      RA-[MODULE]-[SEQ]       e.g., RA-AUTH-001
```

### Module Codes

```
AUTH   - Authentication and User Management
SIG    - Electronic Signatures
AUDIT  - Audit Trail
CRF    - Case Report Form / Data Entry
QUERY  - Query Management
SAE    - Safety Reporting
CODE   - Medical Coding
EXPORT - Data Export
RPT    - Reporting
ADMIN  - System Administration
INT    - Integrations
INFRA  - Infrastructure
```

## RTM Structure

### Primary Traceability Matrix

```
| URS ID | URS Description | Priority | FS ID(s) | DS ID(s) | Test Case ID(s) | Test Type | Risk Level | Status |
|--------|----------------|----------|----------|----------|-----------------|-----------|------------|--------|
```

**Column Definitions:**

- **URS ID**: Unique identifier for the user requirement
- **URS Description**: Brief requirement statement
- **Priority**: Critical / High / Medium / Low (Critical = patient safety or regulatory)
- **FS ID(s)**: Functional specification items that implement this requirement
- **DS ID(s)**: Design specification items detailing the implementation
- **Test Case ID(s)**: All test cases verifying this requirement
- **Test Type**: IQ, OQ, PQ, or combination
- **Risk Level**: From risk assessment (Critical/High/Medium/Low)
- **Status**: Draft / Approved / Tested-Pass / Tested-Fail / Deferred

### Reverse Traceability Matrix

```
| Test Case ID | Test Type | DS ID(s) | FS ID(s) | URS ID(s) | Execution Status | Result |
|-------------|-----------|----------|----------|-----------|-----------------|--------|
```

This confirms every test case traces back to a requirement, identifying any orphan tests.

### Regulatory Requirements Mapping

```
| Regulation | Section | Requirement Summary | URS ID(s) | Test Case ID(s) | Compliance Status |
|-----------|---------|--------------------|-----------|-----------------|--------------------|
```

Map each applicable regulatory requirement to specific URS items and test cases:

**21 CFR Part 11 Requirements:**
- 11.10(a) - System validation
- 11.10(b) - Legible, suitable for inspection
- 11.10(c) - Record protection and retention
- 11.10(d) - Limiting system access
- 11.10(e) - Audit trails
- 11.10(g) - Authority checks
- 11.10(h) - Device checks
- 11.10(k) - Documentation controls
- 11.50 - Signature manifestations
- 11.70 - Signature/record linking
- 11.100 - General requirements for e-signatures
- 11.200 - E-signature components and controls

**EU Annex 11 Requirements:**
- Section 1 - Risk management
- Section 2 - Personnel
- Section 4 - Validation
- Section 5 - Data
- Section 7 - Data storage
- Section 9 - Audit trails
- Section 10 - Change management
- Section 12 - Security
- Section 14 - Electronic signatures

## Building the RTM

### Step 1: Extract Requirements

For each URS item, capture:
- Unique ID
- Requirement text (shall statement)
- Priority classification
- Regulatory driver (if applicable)
- Associated risk assessment item

### Step 2: Map to Functional Specifications

For each URS item, identify:
- Which FS items address it
- Whether the FS items collectively fulfill the URS item
- Any URS items without FS coverage (gap)

### Step 3: Map to Design Specifications

For each FS item, identify:
- Which DS items detail the implementation
- Whether technical approach satisfies the functional requirement
- Any FS items without DS coverage (gap)

### Step 4: Map to Test Cases

For each DS item, identify:
- Which test cases verify the implementation
- Whether test coverage is adequate for the risk level
- Any DS items without test coverage (gap)

### Step 5: Cross-Reference Risk Assessment

Link risk assessment items to requirements:
- High-risk items must have thorough test coverage
- Critical requirements must have multiple test cases
- Risk mitigation controls must be verified by tests

## Gap Analysis

### Forward Traceability Gaps

Identify requirements not fully covered:

```
| Gap ID | Type | Item ID | Description | Impact | Resolution |
|--------|------|---------|-------------|--------|------------|
```

**Gap Types:**
- **URS-FS Gap**: User requirement with no functional specification
- **FS-DS Gap**: Functional spec with no design specification
- **DS-TC Gap**: Design spec with no test case
- **Coverage Gap**: Test cases exist but do not fully verify the requirement

### Reverse Traceability Gaps

Identify orphan items:

```
| Gap ID | Type | Item ID | Description | Resolution |
|--------|------|---------|-------------|------------|
```

**Orphan Types:**
- **Orphan Test**: Test case not linked to any requirement
- **Orphan DS**: Design spec not linked to any functional spec
- **Orphan FS**: Functional spec not linked to any user requirement

### Gap Resolution

For each gap, document:
1. Gap identified date
2. Responsible person
3. Resolution approach (add requirement, add test, remove orphan, accept with justification)
4. Resolution date
5. Verification that gap is closed

## Coverage Metrics

Calculate and report:

```
Requirement Coverage:
- Total URS items: [N]
- URS items with FS mapping: [N] ([%])
- URS items with test coverage: [N] ([%])
- Critical URS items with test coverage: [N] ([%]) - Target: 100%

Test Coverage:
- Total test cases: [N]
- Test cases linked to requirements: [N] ([%])
- Orphan test cases: [N]

Gap Summary:
- Open gaps: [N]
- Critical gaps: [N] - Target: 0
- Gaps resolved this period: [N]
```

## Regulatory Submission Format

For regulatory submissions, format the RTM as:

1. **Cover page** with system name, version, document ID, approval signatures
2. **Table of contents**
3. **Scope and methodology** section
4. **Primary forward traceability matrix** (URS to test cases)
5. **Regulatory requirements mapping** (regulations to URS to test cases)
6. **Reverse traceability matrix** (test cases to URS)
7. **Gap analysis results** with resolutions
8. **Coverage metrics summary**
9. **Conclusion** stating completeness of coverage
10. **Approval signatures**

## Maintenance

- Update RTM whenever requirements, specifications, or test cases change
- RTM update is a mandatory deliverable in the change control process
- Version control the RTM with revision history
- Review RTM completeness during periodic validation reviews
- RTM must be current prior to any regulatory submission or audit

## Regulatory References

- FDA 21 CFR Part 11
- EU Annex 11
- ISPE GAMP 5 - Specification and Verification
- ICH E6(R2) - GCP
- IEEE 829 - Standard for Software Test Documentation
- ISO/IEC 29148 - Requirements Engineering
