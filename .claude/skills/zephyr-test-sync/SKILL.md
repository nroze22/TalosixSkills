---
name: zephyr-test-sync
description: "Sync test artifacts with Zephyr Scale for the Talosix EDC platform. Map test cases to Jira requirements, generate execution reports, track coverage metrics, and manage test cycles and versions."
---

# Zephyr Scale Test Sync

## Purpose

Manage synchronization of test artifacts between the Talosix QA process and Zephyr Scale (Jira Test Management). This skill covers mapping test cases to Jira requirements, generating execution reports, tracking test coverage metrics, and managing test cycles and versions.

## Context: Zephyr Scale in Talosix QA

Talosix uses Zephyr Scale (formerly TM4J -- Test Management for Jira) as the central test management tool. It integrates directly with Jira and provides:

- **Test Cases:** Structured test case repository linked to Jira issues.
- **Test Cycles:** Groupings of test executions for a specific release or sprint.
- **Test Plans:** High-level plans linking cycles, environments, and test cases.
- **Traceability:** Bidirectional links between test cases and Jira stories/requirements.
- **Reporting:** Built-in and custom reports for coverage, execution, and defect metrics.

### Zephyr Scale Data Model

```
Project
  └── Test Case (T-NNN)
        ├── Steps (ordered list)
        ├── Preconditions
        ├── Labels / Components
        ├── Priority
        ├── Custom Fields
        └── Links to Jira Issues (traceability)

  └── Test Cycle (cycle key)
        ├── Test Executions
        │     ├── Status (Pass / Fail / Blocked / Not Executed)
        │     ├── Executed By
        │     ├── Execution Date
        │     ├── Defect Links
        │     └── Step-level Results
        ├── Environment
        ├── Version
        └── Folder structure

  └── Test Plan
        ├── Linked Test Cycles
        ├── Environment
        └── Version
```

## Capabilities

### 1. Test Case to Jira Requirement Mapping

#### Mapping Strategy

Every test case in Zephyr Scale must link to at least one Jira issue for traceability. The mapping follows these conventions:

| Jira Issue Type | Zephyr Link Type | Mapping Rule |
|----------------|-----------------|-------------|
| Story | "Tests" | Each story has at least one test case per acceptance criterion |
| Bug | "Tests" | Each bug fix has a regression test case |
| Epic | "Tests" (via stories) | Coverage tracked at epic level through story rollup |
| Requirement (custom) | "Verifies" | Regulatory requirements link directly to validation test cases |

#### Mapping Format

When generating test case mappings, output in this format for bulk import or API sync:

```json
{
  "testCases": [
    {
      "testCaseKey": "T-1234",
      "name": "Verify audit trail entry on CRF save",
      "folder": "/CRF Data Entry/Audit Trail",
      "priority": "Critical",
      "labels": ["regulatory", "audit-trail"],
      "jiraLinks": [
        {
          "issueKey": "PROJ-567",
          "linkType": "Tests"
        },
        {
          "issueKey": "REQ-089",
          "linkType": "Verifies"
        }
      ]
    }
  ]
}
```

#### Coverage Gap Analysis

Identify untested requirements by comparing:

1. All Jira stories in the release scope (from fix version or sprint).
2. All test cases linked to those stories in Zephyr Scale.
3. Report stories with zero test case links as coverage gaps.

Output format for coverage gaps:

```
Coverage Gap Report - Release X.Y
==================================

Stories with NO test coverage:
  PROJ-123: "Add batch query creation feature" (Priority: High)
  PROJ-456: "Update password policy rules" (Priority: Critical)

Stories with PARTIAL test coverage:
  PROJ-789: "Implement SDV workflow" - 3 of 7 ACs covered
    Missing: AC4 (partial SDV), AC5 (un-verify), AC7 (reporting)

Summary:
  Total stories in scope: 45
  Fully covered: 38 (84.4%)
  Partially covered: 5 (11.1%)
  Not covered: 2 (4.4%)
```

### 2. Test Execution Reports

#### Execution Summary Report

```
Test Execution Summary
======================
Cycle: [Cycle Name]
Version: [Release Version]
Environment: [Environment Name]
Period: [Start Date] - [End Date]

Execution Status:
  Total Test Cases:     120
  Passed:               105 (87.5%)
  Failed:                 8 (6.7%)
  Blocked:                4 (3.3%)
  Not Executed:           3 (2.5%)

By Priority:
  Critical (30):  28 Pass | 2 Fail | 0 Blocked | 0 Not Executed
  High (45):      40 Pass | 3 Fail | 1 Blocked | 1 Not Executed
  Medium (35):    30 Pass | 2 Fail | 2 Blocked | 1 Not Executed
  Low (10):        7 Pass | 1 Fail | 1 Blocked | 1 Not Executed

By Component:
  CRF Data Entry:     25/28 Pass (89.3%)
  Query Management:   18/20 Pass (90.0%)
  E-Signatures:       12/12 Pass (100.0%)
  Audit Trail:         8/8  Pass (100.0%)
  SDV:                14/16 Pass (87.5%)
  Reporting:          10/12 Pass (83.3%)
  User Management:    18/24 Pass (75.0%)

Failed Test Cases:
  T-1001: "Login with expired password" - PROJ-BUG-234 (High)
  T-1045: "Export data with special characters" - PROJ-BUG-235 (Medium)
  ...

Blocked Test Cases:
  T-2001: "RTSM integration randomization" - Blocked by: RTSM staging env down
  ...
```

#### Regulatory Compliance Report

For IQ/OQ/PQ validation cycles, generate a compliance-focused report:

```
Regulatory Test Execution Report
=================================
Validation Protocol: VP-2024-003
Cycle: OQ Cycle - Release X.Y

21 CFR Part 11 Compliance Tests:
  Audit Trail:        8/8   Pass (100%)
  E-Signatures:      12/12  Pass (100%)
  Access Control:    15/15  Pass (100%)
  Data Integrity:    10/10  Pass (100%)

  Status: ALL REGULATORY TESTS PASSED

Deviations: None

Conclusion: The system meets 21 CFR Part 11 requirements for electronic
records and electronic signatures as tested in this OQ cycle.
```

#### Trend Report

Track metrics across multiple test cycles:

```
Test Execution Trends - Last 5 Releases
========================================

Release  | Pass Rate | Defects Found | Critical Defects | Execution Rate
---------|-----------|---------------|------------------|---------------
v4.0     | 94.2%    | 12            | 1                | 100%
v4.1     | 96.1%    | 8             | 0                | 98%
v4.2     | 91.3%    | 15            | 2                | 100%
v4.3     | 97.5%    | 6             | 0                | 99%
v4.4     | 95.8%    | 9             | 0                | 100%

Observations:
  - v4.2 had lowest pass rate due to database migration issues.
  - Defect find rate trending downward (positive trend).
  - Execution rate consistently above 98%.
```

### 3. Test Coverage Metrics

#### Requirement Coverage

Track how thoroughly Jira requirements are covered by test cases:

```
Requirement Coverage Matrix
============================

Epic: PROJ-100 "CRF Data Entry Enhancements"

Story          | ACs | Test Cases | Coverage | Last Executed | Result
---------------|-----|-----------|----------|---------------|-------
PROJ-101       | 5   | 12        | 100%     | 2024-01-15    | Pass
PROJ-102       | 3   | 6         | 100%     | 2024-01-15    | Pass
PROJ-103       | 7   | 14        | 85.7%    | 2024-01-14    | 1 Fail
PROJ-104       | 4   | 0         | 0%       | Never         | N/A

Epic Coverage: 79.0% (15/19 ACs covered)
```

#### Test Type Coverage

```
Test Type Distribution
======================

            | Positive | Negative | Boundary | Edge | Regulatory | Total
------------|----------|----------|----------|------|-----------|------
CRF Entry   | 15       | 12       | 8        | 5    | 10        | 50
Queries     | 10       | 8        | 3        | 4    | 5         | 30
E-Sig       | 5        | 8        | 2        | 3    | 12        | 30
Reporting   | 8        | 5        | 4        | 2    | 3         | 22
------------|----------|----------|----------|------|-----------|------
Total       | 38       | 33       | 17       | 14   | 30        | 132

Distribution: 28.8% Positive | 25.0% Negative | 12.9% Boundary |
              10.6% Edge | 22.7% Regulatory
```

### 4. Test Cycle and Version Management

#### Creating Test Cycles

When a new release cycle begins, create the test cycle structure:

```
Test Cycle Structure for Release X.Y
=====================================

Test Plan: TP-Release-X.Y
  ├── Smoke Test Cycle
  │     Version: X.Y
  │     Environment: QA
  │     Test Cases: [Smoke test suite - ~20 cases]
  │
  ├── Feature Test Cycle
  │     Version: X.Y
  │     Environment: QA
  │     Test Cases: [New/modified feature tests - varies]
  │
  ├── Regression Test Cycle
  │     Version: X.Y
  │     Environment: QA
  │     Test Cases: [Selected regression suite - varies]
  │
  ├── Integration Test Cycle
  │     Version: X.Y
  │     Environment: QA
  │     Test Cases: [Integration tests - varies]
  │
  ├── UAT Cycle
  │     Version: X.Y
  │     Environment: UAT
  │     Test Cases: [UAT suite - varies]
  │
  └── Pre-Production Validation
        Version: X.Y
        Environment: Staging
        Test Cases: [Critical path + regulatory - ~40 cases]
```

#### Version Mapping

Map test cycles to Jira fix versions for consistent tracking:

| Jira Fix Version | Zephyr Test Plan | Cycles | Status |
|-----------------|-----------------|--------|--------|
| v4.4.0 | TP-v4.4.0 | 6 cycles | Active |
| v4.3.1-hotfix | TP-v4.3.1-hf | 2 cycles (smoke + targeted) | Complete |
| v4.3.0 | TP-v4.3.0 | 6 cycles | Complete |

#### Cloning Cycles for New Releases

When setting up a new release:

1. Clone the regression test cycle from the previous release as a baseline.
2. Add new test cases for features in the new release scope.
3. Remove test cases for deprecated or removed features.
4. Update test data references if environment data has changed.
5. Assign testers based on the test plan resource allocation.

### 5. Zephyr Scale API Integration Notes

Zephyr Scale provides a REST API for automation. Key endpoints for sync operations:

```
Base URL: https://{instance}.atlassian.net/rest/atm/1.0

Test Cases:
  GET    /testcase/{testCaseKey}           - Get test case
  POST   /testcase                         - Create test case
  PUT    /testcase/{testCaseKey}           - Update test case
  POST   /testcase/search                  - Search test cases

Test Cycles:
  GET    /testrun/{testRunKey}             - Get test cycle
  POST   /testrun                          - Create test cycle
  POST   /testrun/{testRunKey}/testcase    - Add test case to cycle

Test Executions:
  POST   /testrun/{testRunKey}/testcase/{testCaseKey}/testresult  - Record result

Traceability:
  GET    /traceability/testcase/{testCaseKey}  - Get linked issues
  POST   /testcase/{testCaseKey}/link          - Link to Jira issue
```

When generating sync artifacts, format data to be compatible with these API endpoints. For bulk operations, use the import/export CSV format supported by Zephyr Scale.

### 6. CSV Import/Export Format

For bulk test case import into Zephyr Scale:

```csv
Name,Objective,Precondition,Folder,Priority,Component,Labels,Step,Test Data,Expected Result
"Enter valid lab value","Verify valid lab entry saves correctly","User logged in as Site Coordinator","/CRF Data Entry/Lab Values","High","CRF Data Entry","positive,functional","Navigate to Subject > Visit > Lab Form","","Lab form opens in edit mode"
,,,,,,,"Enter Hemoglobin value","14.5 g/dL","Value accepted, no validation error"
,,,,,,,"Click Save","","Form saved, confirmation displayed"
```

Notes on CSV format:
- First row with Name starts a new test case.
- Subsequent rows with empty Name add steps to the current test case.
- Labels are comma-separated within the field.
- Folder paths create the folder hierarchy automatically on import.

## Guidelines

- Always verify Zephyr Scale folder structure exists before referencing it in output.
- Use consistent naming conventions: T-NNN for test cases, cycle names include release version.
- Keep traceability links bidirectional -- every Jira story should link to its tests and vice versa.
- Generate coverage reports before and after each test cycle to track progress.
- For regulatory releases, ensure 100% coverage of all GxP-impacting requirements before cycle start.
- Include defect links in test execution results for failed test cases to maintain traceability chain: Requirement -> Test Case -> Execution -> Defect.
- When reporting metrics, always include the measurement date and data source for auditability.
