---
name: uat-test-generation
description: "Generate UAT test suites for CRO and sponsor users of the Talosix EDC platform. Produces business-language test scripts covering clinical workflows with clear pass/fail criteria. Formatted for Zephyr Scale."
allowed-tools:
  - Read
  - Grep
  - Glob
---

# UAT Test Suite Generation

## Purpose

Generate User Acceptance Test suites written in plain business language for CRO (Contract Research Organization) and sponsor users validating the Talosix EDC platform. UAT scripts must be executable by non-technical clinical operations staff.

## Context: UAT in Clinical Trials

UAT in the clinical trial EDC context serves a dual purpose:

1. **Business Validation:** Confirm the system meets study-specific requirements and supports clinical workflows.
2. **Regulatory Compliance:** Satisfy IQ/OQ/PQ validation requirements per GAMP 5 and 21 CFR Part 11. UAT evidence becomes part of the validation package submitted to regulatory authorities.

### Key UAT Stakeholders

| Role | Focus Area |
|------|-----------|
| Clinical Data Manager | CRF design, edit checks, query workflows, data export |
| CRA (Clinical Research Associate) | SDV, monitoring visit workflows, query resolution |
| Medical Monitor | Safety data review, SAE workflows, medical coding |
| Sponsor Project Lead | Reporting, dashboards, study configuration |
| Biostatistician | Data extraction, CDISC compliance, dataset generation |
| Regulatory Affairs | Audit trail, e-signatures, compliance features |

## Workflow

1. **Gather context:** Read the feature specification, Jira story, or release notes provided by the user.
2. **Identify UAT-relevant workflows:** Focus on end-to-end business processes, not isolated technical functions.
3. **Determine target user roles:** Map each workflow to the CRO/sponsor role that performs it.
4. **Write test scripts** in business language following the format below.
5. **Define pass/fail criteria** that are objective and observable.
6. **Organize into test suites** by workflow area.

## Clinical Workflows to Cover

### Patient Enrollment
- Screen and register a new subject.
- Assign randomization number.
- Verify eligibility criteria checks (inclusion/exclusion).
- Handle screen failures and re-screening.
- Transfer subjects between sites.

### CRF Data Entry
- Complete a CRF form for a scheduled visit.
- Handle unscheduled and missed visits.
- Enter partial data and save as draft.
- Submit completed CRF for review.
- Enter data with edit check violations and resolve.

### Query Management
- System-generated auto-queries from edit check failures.
- Manual query creation by CRA or Data Manager.
- Site response to a query with supporting data.
- Query closure by CRA or Data Manager.
- Query re-opening and escalation.
- Bulk query operations.

### Source Data Verification (SDV)
- CRA marks CRF fields as source-verified.
- Partial and complete SDV at visit and subject level.
- SDV status tracking and reporting.
- Un-verify and re-verify after data changes.

### Medical Coding
- Code adverse events using MedDRA.
- Code concomitant medications using WHODrug.
- Review and approve auto-coded terms.
- Handle uncoded or ambiguous terms.

### Safety Reporting
- SAE (Serious Adverse Event) entry and notification.
- SUSAR (Suspected Unexpected Serious Adverse Reaction) workflow.
- Safety narrative generation.
- Medical monitor review and sign-off.

### Data Management
- Database lock at subject, site, and study level.
- Data export in CDISC formats (CDASH, SDTM).
- Discrepancy listing generation.
- Data review and sign-off workflows.

### Study Administration
- Study build and configuration (non-production).
- User provisioning and role assignment.
- Site activation and deactivation.
- Study amendment deployment.

## UAT Test Script Format

Write each test script in this structure for Zephyr Scale compatibility:

```
Test Case Key: UAT-[NNN]
Name: [Business-friendly name describing the workflow]
Suite: [Workflow area, e.g., "Patient Enrollment", "Query Management"]
Role: [CRO/Sponsor role executing this test]
Priority: [Critical | High | Medium | Low]
Traceability: [Jira Key or Requirement ID]

Prerequisites:
  - [Plain language description of required setup]
  - [e.g., "A study with at least one active site and enrolled subject"]
  - [e.g., "User has Data Manager role for Study XYZ"]

Test Script:
  Step 1: [Action in plain business language]
    Expected Result: [What the user should observe]
    Pass/Fail Criteria: [Specific, measurable criterion]

  Step 2: [Next action]
    Expected Result: [Observable outcome]
    Pass/Fail Criteria: [Criterion]

  ...

Overall Pass Criteria:
  - [Summary criteria for the entire test case to pass]
  - [All steps must pass unless noted otherwise]

Notes:
  - [Any additional context for the tester]
  - [Known limitations or workarounds]
```

## Writing Style Guidelines

- **Use business language:** "Open the subject's Visit 3 lab form" not "Navigate to /study/subjects/{id}/visits/3/forms/lab."
- **Be specific about data:** "Enter patient weight as 75.5 kg" not "Enter a value."
- **Describe observable outcomes:** "A green checkmark appears next to the visit" not "The database record is updated."
- **Avoid technical jargon:** "The system shows an error message" not "A 422 validation error is returned."
- **Include exact expected messages:** When the system displays a specific message, quote it exactly.
- **One action per step:** Each step should contain a single user action and its expected result.
- **Keep steps sequential:** A non-technical user should be able to follow without skipping around.

## Pass/Fail Criteria Guidelines

Pass/fail criteria must be:

- **Binary:** Clearly pass or fail, no subjective judgment.
- **Observable:** The tester can see/verify the result in the UI.
- **Specific:** "Subject status changes to 'Enrolled'" not "Subject is enrolled."
- **Complete:** Include data verification, not just action completion.

Examples of good criteria:
- "The subject list shows the new subject with status 'Screened' and the assigned Subject ID follows the format SITE-NNN."
- "The query counter on the dashboard increments by 1 and the query appears in the 'Open Queries' table with status 'Answered'."
- "The exported CSV file contains exactly the fields defined in the export template, with dates in ISO 8601 format."

## Suite Organization

Organize test cases into suites by workflow area:

```
/UAT
  /Patient Enrollment
    UAT-001: Screen a new subject
    UAT-002: Randomize an eligible subject
    UAT-003: Handle screen failure
    ...
  /CRF Data Entry
    UAT-010: Complete a scheduled visit CRF
    UAT-011: Enter data with edit check violation
    ...
  /Query Management
    UAT-020: Create manual query
    UAT-021: Respond to auto-query at site
    ...
```

## Regulatory Considerations for UAT

- Every UAT test case that modifies data should include a step to verify the audit trail entry.
- E-signature workflows must verify the signature manifest is complete and accurate.
- Access control tests should confirm users cannot perform actions outside their role.
- UAT test evidence (screenshots, results) becomes part of the validation deliverable -- instruct testers to capture evidence at each step.

## Example

```
Test Case Key: UAT-021
Name: Site user responds to an auto-query on a lab value
Suite: Query Management
Role: Site Coordinator
Priority: High
Traceability: PROJ-789 - "Query resolution workflow"

Prerequisites:
  - Study ABC-123 is in data collection phase
  - Subject 001 has an open auto-query on Visit 2 Hemoglobin field
    (triggered by value outside normal range)
  - User is logged in as Site Coordinator for Site 01

Test Script:
  Step 1: Open the Query Inbox from the main navigation menu.
    Expected Result: The Query Inbox displays with a list of open queries.
    Pass/Fail Criteria: The auto-query for Subject 001, Visit 2, Hemoglobin
    is visible in the list with status "Open."

  Step 2: Click on the query to open the query detail panel.
    Expected Result: The query detail shows the original value, the
    validation rule that triggered the query, and a response text box.
    Pass/Fail Criteria: The query detail displays the Hemoglobin value,
    the rule "Value outside normal range (12.0-17.5 g/dL)," and the
    response field is editable.

  Step 3: Enter a response explaining the value is correct per source.
    Type: "Value confirmed per source document. Patient has known
    condition causing elevated hemoglobin."
    Expected Result: The response text is accepted.
    Pass/Fail Criteria: The response text appears in the response field
    with no error messages.

  Step 4: Click "Submit Response."
    Expected Result: The query status changes and a confirmation appears.
    Pass/Fail Criteria: Query status changes to "Answered." A confirmation
    message "Query response submitted successfully" is displayed.

  Step 5: Return to the Query Inbox.
    Expected Result: The query is no longer in the Open queries list.
    Pass/Fail Criteria: The query for Subject 001 Visit 2 Hemoglobin
    appears under "Answered" filter, not under "Open."

Overall Pass Criteria:
  - All 5 steps pass.
  - Audit trail shows the query response action with user, timestamp,
    and response text.

Notes:
  - If the query does not exist in prerequisites, the Data Manager
    can trigger one by entering a Hemoglobin value of 22.0 g/dL.
```
