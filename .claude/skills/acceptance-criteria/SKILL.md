---
name: Acceptance Criteria Generator
description: Generate detailed acceptance criteria for Talosix EDC user stories including positive/negative test cases, edge cases, and regulatory compliance criteria.
allowed-tools:
  - mcp__claude_ai_Atlassian__getJiraIssue
  - mcp__claude_ai_Atlassian__editJiraIssue
  - mcp__claude_ai_Atlassian__addCommentToJiraIssue
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
argument-hint: "[Jira ticket ID or user story description]"
---

# Acceptance Criteria Generator

Generate comprehensive acceptance criteria for Talosix EDC user stories. Criteria cover functional requirements, edge cases, error handling, and regulatory compliance for validated clinical trial systems.

## Workflow

1. **Get the Story**: Either:
   - Pull from Jira using `getJiraIssue` with the ticket ID
   - Accept a user story description directly from the user

2. **Understand Context**: Identify:
   - Which EDC module this belongs to (eCRF, queries, monitoring, admin, etc.)
   - Which user personas are involved
   - Whether this touches regulated/validated functionality
   - Dependencies on other features or systems

3. **Generate Acceptance Criteria** using the framework below.

4. **Update Jira**: Offer to add criteria to the ticket via `editJiraIssue` or `addCommentToJiraIssue`.

## Acceptance Criteria Format

Use Given-When-Then (Gherkin) syntax for clarity and testability:

```
**AC-[N]: [Short description]**
Given [precondition/context]
When [action performed by user]
Then [expected observable result]
And [additional expected result]
```

## Criteria Categories

For every story, generate criteria in these categories:

### 1. Happy Path (Positive Scenarios)
The primary intended workflow succeeds as expected.
- Standard use case with valid inputs
- Expected data flow from start to completion
- Correct UI feedback (success messages, state changes)

### 2. Validation and Error Handling (Negative Scenarios)
The system handles invalid inputs and error conditions gracefully.
- Required field validation
- Data type validation (numeric, date, text length)
- Business rule validation
- Meaningful error messages displayed to the user
- System state remains consistent after errors

### 3. Edge Cases
Boundary conditions and unusual but valid scenarios.
- Empty states (no data, first-time use)
- Maximum limits (max characters, max rows, max concurrent)
- Boundary values (date ranges, numeric limits)
- Special characters and Unicode handling
- Concurrent user actions on the same data

### 4. Permission and Access Control
Role-based access requirements (critical for EDC systems).
- Which roles CAN perform this action
- Which roles CANNOT perform this action
- Behavior when user lacks permission (graceful denial, not a system error)
- Permission inheritance across study/site hierarchy

### 5. Audit Trail Requirements
Required for 21 CFR Part 11 compliance in validated EDC systems.
- What actions are logged in the audit trail
- Audit trail entry includes: timestamp, user ID, action, old value, new value, reason for change (if applicable)
- Audit trail entries are immutable
- Audit trail is viewable by authorized users

### 6. Data Integrity
ALCOA+ principles for clinical data.
- **Attributable**: Action is linked to the user who performed it
- **Legible**: Data is readable and permanent
- **Contemporaneous**: Timestamps reflect when the action occurred
- **Original**: Original data is preserved (no silent overwrites)
- **Accurate**: Data is correct and consistent

### 7. Integration Criteria (if applicable)
When the feature interacts with external systems.
- Successful data exchange with expected format
- Handling of integration failures (timeout, invalid response)
- Retry behavior and error logging
- Data consistency between systems

### 8. Performance Criteria (if applicable)
- Page load time within [X] seconds
- Action completion within [X] seconds
- Behavior under expected concurrent load

## EDC-Specific Criteria Templates

### For eCRF/Data Entry Features

```
AC: Field-level audit trail
Given a user modifies a data point on an eCRF
When the change is saved
Then an audit trail entry is created with:
  - Original value
  - New value
  - User ID and timestamp
  - Reason for change (if configured)
And the audit trail is visible to authorized users

AC: Edit check firing
Given an edit check rule is configured for this field
When the user enters a value that violates the rule
Then the appropriate query/warning is displayed
And the user can proceed (soft check) or is blocked (hard check)
```

### For Query Management Features

```
AC: Query state transitions
Given a query is in [state] status
When a [role] user performs [action]
Then the query transitions to [new state]
And all stakeholders are notified per configuration
And the state change is recorded in the audit trail

AC: Query auto-generation
Given an edit check is triggered by data entry
When the data violates the edit check rule
Then a query is automatically generated
And it is assigned to the appropriate site user
And the query includes the discrepancy details
```

### For Monitoring/SDV Features

```
AC: Source data verification
Given a monitor is reviewing a subject's eCRF data
When they mark a field as SDV-verified
Then the field displays the verified indicator
And the verification is recorded with monitor ID and timestamp
And the SDV status is reflected in monitoring reports
```

### For User Administration Features

```
AC: Role-based access assignment
Given an admin assigns a role to a user for a study
When the user logs in
Then they see only the studies and functions permitted by their role
And no unauthorized data or functions are accessible
And the role assignment is recorded in the audit trail
```

### For Electronic Signature Features

```
AC: 21 CFR Part 11 compliant e-signature
Given a user initiates a signing action
When they provide their credentials (username + password)
Then the system verifies the credentials match the logged-in user
And the signature includes: meaning of signature, date/time, printed name
And the signed record is locked from further editing (unless re-opened with audit trail)
And the signature event is recorded in the audit trail
```

## Output Format

Present all criteria grouped by category, numbered for reference:

```
## Acceptance Criteria for [JIRA-ID]: [Story Title]

### Happy Path
- **AC-1**: [Description]
  Given... When... Then...

### Error Handling
- **AC-2**: [Description]
  Given... When... Then...

### Edge Cases
- **AC-3**: [Description]
  Given... When... Then...

### Permissions
- **AC-4**: [Description]
  Given... When... Then...

### Audit Trail
- **AC-5**: [Description]
  Given... When... Then...

### Regulatory Compliance
- **AC-6**: [Description]
  Given... When... Then...

---
**Total Criteria**: [N]
**Regulatory Criteria**: [N] (flagged for validation test scripts)
```

## Tips
- Every criterion must be independently testable -- no ambiguous language
- Use specific values in examples, not vague terms ("enters 'abc' in numeric field" not "enters invalid data")
- Always include audit trail criteria for any data-modifying action in the EDC
- Flag criteria that require validation test script creation
- Keep criteria atomic: one behavior per criterion
