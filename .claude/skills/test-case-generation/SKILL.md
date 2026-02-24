---
name: test-case-generation
description: "Generate comprehensive test cases from Jira stories for Talosix clinical trial EDC platform. Produces positive, negative, boundary, and edge cases with regulatory coverage. Output is formatted for direct import into Zephyr Scale."
allowed-tools: Read, Grep, Glob
---

# Test Case Generation from Jira Stories

## Purpose

Generate thorough, traceable test cases from Jira user stories and requirements for the Talosix EDC platform. Every test case must satisfy clinical trial regulatory expectations (21 CFR Part 11, ICH E6(R2) GCP, EU Annex 11) and be importable into Zephyr Scale.

## Context: Talosix EDC Domain

Talosix is a clinical trial Electronic Data Capture system. Key domain concepts:

- **CRF (Case Report Form):** Structured forms for collecting clinical data per protocol.
- **Edit Checks:** Programmatic validations that fire on data entry (range checks, cross-field, cross-form, cross-visit).
- **Queries:** Discrepancies raised against data points, requiring site resolution.
- **Audit Trail:** Immutable, timestamped log of every data change with reason.
- **E-Signatures:** 21 CFR Part 11 compliant electronic signatures binding a user to a record.
- **SDV (Source Data Verification):** CRA review comparing EDC data against source documents.
- **RBQM (Risk-Based Quality Management):** Centralized monitoring using KRIs and data analytics.
- **Roles:** Site users, CRAs, Data Managers, Medical Monitors, Sponsors, Admins.

## Workflow

1. **Read the Jira story** provided by the user (text, screenshot, or reference).
2. **Identify testable requirements** -- extract every acceptance criterion, business rule, and implicit expectation.
3. **Classify each requirement** by type: functional, regulatory, security, performance, usability.
4. **Generate test cases** across all categories (see below).
5. **Add traceability** linking each test case back to the Jira story key and specific acceptance criterion.
6. **Format output** for Zephyr Scale import.

## Test Case Categories

### Positive / Happy Path
- Verify the feature works as described in the acceptance criteria.
- Cover each distinct user role that interacts with the feature.
- Validate correct system responses, messages, and state transitions.

### Negative
- Invalid input (wrong data types, special characters, SQL injection attempts).
- Unauthorized access (wrong role, expired session, locked account).
- Missing mandatory fields, incomplete workflows.
- Concurrent modification conflicts.

### Boundary
- Min/max values for numeric fields (e.g., lab ranges, visit windows).
- Character limits on text fields.
- Date boundaries (first/last visit, randomization windows).
- Pagination limits, file size limits for attachments.

### Edge Cases
- Timezone differences across global trial sites.
- Daylight saving transitions for timestamp-sensitive operations.
- Unicode and multi-byte characters in free-text fields.
- Very long running studies with large data volumes.
- Browser back/forward button during form submission.
- Session timeout mid-operation.

### Regulatory Test Scenarios

#### Audit Trail (21 CFR Part 11.10(e))
- Every create, update, and delete action generates an audit trail entry.
- Audit trail captures: user, timestamp (UTC), old value, new value, reason for change.
- Audit trail entries are immutable -- no edit or delete capability.
- Audit trail is accessible to authorized users only.

#### E-Signatures (21 CFR Part 11.50, 11.70, 11.100)
- Signature requires username + password (two-component authentication).
- Signature is linked to the specific record and meaning (e.g., "reviewed", "approved").
- Signing a modified record invalidates the previous signature.
- Signature manifest is available and includes signer identity, date/time, and meaning.

#### Data Integrity (ALCOA+ Principles)
- **Attributable:** Every entry traceable to the person who made it.
- **Legible:** Data is readable and permanent.
- **Contemporaneous:** Data recorded at the time of the activity.
- **Original:** First-recorded data is preserved.
- **Accurate:** Data is correct, complete, and consistent.

#### Access Control
- Role-based permissions enforced at UI and API level.
- Study-level and site-level data segregation.
- Account lockout after failed login attempts.
- Password complexity and expiration policies.

## Output Format (Zephyr Scale Compatible)

Each test case must include these fields:

```
Test Case Key: [Auto-generated or placeholder, e.g., TC-001]
Name: [Concise descriptive name]
Objective: [What this test case validates]
Preconditions: [Required state before execution]
Priority: [Critical | High | Medium | Low]
Component: [EDC module, e.g., CRF Designer, Query Management]
Labels: [positive | negative | boundary | edge | regulatory | audit-trail | e-signature | security]
Traceability: [Jira Story Key] - [Specific AC reference]
Folder: /[Module]/[Feature]/[Category]

Steps:
  1. Step description
     Test Data: [Specific data to use]
     Expected Result: [What should happen]
  2. ...

Postconditions: [Expected system state after test]
```

## Zephyr Scale Import Notes

- Use the field names above to map to Zephyr Scale custom fields.
- Folder paths map to Zephyr Scale test case folders.
- Labels enable filtering by test type during cycle planning.
- Traceability field maps to the "Issues" link in Zephyr Scale for requirement coverage.

## Guidelines

- Write step descriptions in imperative mood ("Navigate to...", "Enter...", "Verify...").
- Include specific test data values, not placeholders, wherever possible.
- Each test case should be independently executable (no dependency on other test cases unless explicitly noted).
- Keep step count between 3 and 15. Split complex scenarios into multiple test cases.
- For regulatory tests, reference the specific regulation section in the Objective field.
- Always include at least one audit trail verification test for any data-modifying feature.
- Always include role-based access tests for any feature with permission controls.

## Example

Given a Jira story about adding a new lab value field to a CRF:

```
Test Case Key: TC-001
Name: Enter valid lab value within normal range
Objective: Verify a site user can enter a lab value within the defined normal range and the value is saved correctly.
Preconditions: User is logged in as Site Investigator; Study ABC-123 is active; Subject 001 has Visit 2 CRF available.
Priority: Critical
Component: CRF Data Entry
Labels: positive
Traceability: PROJ-456 - AC1 "Site user can enter numeric lab values"
Folder: /CRF Data Entry/Lab Values/Positive

Steps:
  1. Navigate to Subject 001 > Visit 2 > Lab Results CRF
     Test Data: N/A
     Expected Result: Lab Results CRF opens in edit mode
  2. Enter value in Hemoglobin field
     Test Data: 14.5 g/dL
     Expected Result: Value accepted, no validation error displayed
  3. Click Save
     Test Data: N/A
     Expected Result: Form saved successfully, confirmation message displayed
  4. Reopen the CRF and verify persisted value
     Test Data: N/A
     Expected Result: Hemoglobin field displays 14.5 g/dL

Postconditions: Audit trail contains entry for Hemoglobin field creation with user, timestamp, and value.
```

## Codebase Reference

When generating test cases, scan the codebase for additional context:

- Look for validation rules, edit check definitions, and business logic in source code.
- Check existing test cases for patterns and naming conventions.
- Review API schemas for field constraints (min, max, required, data types).
- Examine role/permission configuration files for access control requirements.
