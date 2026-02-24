---
name: test-planning
description: >
  Plan test cycles with resource allocation, entry/exit criteria, environment
  requirements, defect management, and regression test selection for the
  Talosix EDC platform.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Test Planning

## Purpose

Create actionable test plans for specific test cycles within the Talosix EDC platform release process. While the test strategy defines the overall approach, the test plan specifies the concrete activities, assignments, schedules, and criteria for a particular test cycle.

## Context: Test Cycles in Clinical Trial EDC

Talosix typically runs these test cycles per release:

- **Smoke Test Cycle:** Quick validation that core functions work after deployment (30 min - 1 hour).
- **Regression Test Cycle:** Verify existing functionality is not broken by new changes (1 - 3 days).
- **Feature Test Cycle:** Thorough testing of new or modified features (varies by scope).
- **Integration Test Cycle:** Validate integrations with external systems (RTSM, ePRO, safety DB).
- **UAT Cycle:** Business user validation of workflows (3 - 5 days typically).
- **Pre-Production Validation:** Final verification in staging environment (1 - 2 days).

Each cycle has its own plan with specific scope, resources, and criteria.

## Workflow

1. **Identify the test cycle type** and its position in the release timeline.
2. **Define scope** by selecting test cases from Zephyr Scale based on the release content.
3. **Allocate resources** with named assignments where possible.
4. **Specify environment and data requirements.**
5. **Set entry/exit criteria** with measurable thresholds.
6. **Define defect management workflow** for the cycle.
7. **Select regression tests** using the risk-based approach.
8. **Output the plan** in the format below.

## Test Plan Document Structure

### 1. Test Cycle Identification

```
Cycle Name:        [e.g., "Release 4.2 System Test Cycle 1"]
Cycle Type:        [Smoke | Regression | Feature | Integration | UAT | Pre-Prod]
Release:           [Release version]
Planned Start:     [Date]
Planned End:       [Date]
Zephyr Test Cycle: [Link or key to Zephyr Scale test cycle]
Plan Author:       [Name]
Plan Version:      [Version with date]
```

### 2. Scope

#### Test Cases Included
List test case selections from Zephyr Scale:

| Folder / Suite | Test Case Count | Priority Breakdown | Selection Rationale |
|---------------|----------------|-------------------|-------------------|
| /CRF Data Entry/Lab Values | 24 | 8 Critical, 10 High, 6 Medium | Modified in this release |
| /Query Management | 18 | 5 Critical, 8 High, 5 Medium | Regression -- dependent module |
| /E-Signatures | 12 | 12 Critical | Regulatory -- always tested |
| /Audit Trail | 8 | 8 Critical | Regulatory -- always tested |

**Total Test Cases:** [Sum]
**Estimated Execution Time:** [Hours/days]

#### Exclusions
- Test cases excluded from this cycle with justification.
- Modules not affected by the release changes.

### 3. Resource Allocation

| Tester | Role | Assigned Suites | Availability | Estimated Effort |
|--------|------|----------------|-------------|-----------------|
| [Name/TBD] | QA Engineer | CRF Data Entry, Edit Checks | 100% | 3 days |
| [Name/TBD] | QA Engineer | Query Management, SDV | 100% | 2 days |
| [Name/TBD] | QA Lead | E-Signatures, Audit Trail, Oversight | 80% | 2 days |

**Notes:**
- Include backup assignments for critical suites.
- Account for defect retesting time (typically 20-30% of execution time).
- Include time for test environment setup and data preparation.

### 4. Entry Criteria

Define what must be true before the test cycle begins:

| Criterion | Verification Method | Owner |
|-----------|-------------------|-------|
| Build deployed to test environment | Deployment confirmation from DevOps | Release Manager |
| Smoke tests passing | Automated smoke suite green | QA Lead |
| Test data loaded and verified | Data verification checklist | QA Engineer |
| Test environment stable (no active deployments) | Environment calendar confirmed | DevOps |
| All test cases reviewed and up to date in Zephyr | Zephyr Scale audit | QA Lead |
| Previous cycle blocking defects resolved | Jira filter verification | QA Lead |
| Required integrations available | Integration health check | DevOps |

**Entry Decision:** All criteria must be met. QA Lead makes the go/no-go call.

### 5. Exit Criteria

Define what must be true for the test cycle to be considered complete:

| Criterion | Target | Measurement |
|-----------|--------|-------------|
| Test execution rate | >= 95% | (Executed / Planned) test cases |
| Critical test case pass rate | 100% | All Critical priority tests pass |
| High test case pass rate | >= 95% | High priority tests passing |
| Overall pass rate | >= 90% | All executed tests passing |
| Open Critical defects | 0 | No unresolved Critical defects |
| Open High defects | <= 2 | With documented workarounds |
| Regulatory test cases | 100% pass | All audit trail, e-sig, access control tests pass |
| Regression test cases | >= 95% pass | No unexpected regression failures |

**Exit Decision:** QA Lead reviews metrics and recommends cycle closure. Release Manager approves.

**Conditional Exit:** If exit criteria are not met, document deviations with risk assessment and obtain sign-off from the Release Manager and Product Owner.

### 6. Test Environment Requirements

```
Environment:       [QA / UAT / Staging]
URL:               [Environment URL]
Application Build: [Build number or commit SHA]
Database:          [Database version and instance]
Test Data Set:     [Data set identifier]

Browser Requirements:
  - Chrome (latest stable)
  - Firefox (latest stable)
  - Edge (latest stable)
  - Safari (latest stable, macOS only)

Required Integrations:
  - [Integration Name]: [Status: Available / Stubbed / Mock]
  - [Integration Name]: [Status]

Environment Restrictions:
  - [e.g., "No deployments during test execution window"]
  - [e.g., "Shared environment -- coordinate with team X"]
```

#### Test Data Requirements

| Data Category | Description | Setup Method |
|--------------|-------------|-------------|
| Studies | At least 2 active studies with different configurations | Seeded via setup script |
| Sites | 5+ sites per study across multiple countries | Seeded via setup script |
| Subjects | 50+ subjects per study at various stages | Seeded via setup script |
| Users | Users for each role type with appropriate permissions | Provisioned in environment |
| CRF Data | Completed CRFs with mix of clean and queried data | Generated via data loader |
| Queries | Mix of open, answered, and closed queries | Generated from edit checks |

### 7. Defect Management Process

#### Defect Workflow for This Cycle

```
New -> Triaged -> In Development -> Ready for Retest -> Retested -> Closed
                                                     -> Reopen (if retest fails)
       -> Deferred (with justification and risk note)
```

#### Severity Classification

| Severity | Definition (EDC Context) | Examples |
|----------|------------------------|----------|
| Critical | Data loss, security breach, regulatory non-compliance, system unusable | Audit trail not recording, e-sig bypass, data corruption |
| High | Major workflow blocked, no workaround or degraded workaround | Cannot save CRF, query workflow stuck, export produces wrong data |
| Medium | Feature impaired but workaround exists | Sorting not working on table, filter resets unexpectedly |
| Low | Cosmetic, minor usability, documentation | Misaligned label, typo in message, tooltip missing |

#### Defect Triage Process

- **Frequency:** Daily during active test execution.
- **Attendees:** QA Lead, Dev Lead, Product Owner.
- **Decisions:** Severity confirmation, assignment, fix-in-release vs. defer.
- **Defect fields required:** Steps to reproduce, expected result, actual result, severity, screenshots/video, environment, test case link.

#### Retest Protocol

- Retesting is performed by the original reporter or assigned tester.
- Retest must use the same environment and data conditions as the original test.
- If the fix introduces side effects, expand retest to include related test cases.
- Document retest evidence in Zephyr Scale execution results.

### 8. Regression Test Selection

#### Selection Strategy

Use a risk-based approach to select regression tests:

**Always Include (Core Regression):**
- Authentication and session management.
- Audit trail generation and viewing.
- E-signature workflows.
- Role-based access control verification.
- CRF data entry save and retrieve.
- Query create, respond, close lifecycle.
- Subject enrollment basic flow.
- Data export in primary format.

**Impact-Based Selection:**
1. Identify modules modified in the release.
2. Map module dependencies (use code analysis or architecture diagrams).
3. Select regression tests for directly modified modules (full coverage).
4. Select regression tests for dependent modules (critical and high priority only).
5. Select smoke-level tests for indirectly affected modules.

**Historical Defect Analysis:**
- Include tests for areas with high defect density in recent releases.
- Include tests for previously deferred defects that may resurface.
- Prioritize tests that caught regressions in the past 3 releases.

#### Regression Suite Maintenance

- After each release, review failed regression tests for inclusion in the automated suite.
- Remove redundant or outdated tests quarterly.
- Add new regression tests for every Critical/High defect found in production.

### 9. Schedule

| Day | Activity | Owner |
|-----|----------|-------|
| Day 0 | Entry criteria verification, environment setup | QA Lead, DevOps |
| Day 1 | Smoke test execution, critical path testing | QA Lead |
| Day 1-2 | Feature test execution (new/modified features) | QA Engineers |
| Day 2-3 | Regression test execution | QA Engineers |
| Day 3 | Regulatory test execution (audit trail, e-sig) | QA Lead |
| Day 3 | Defect retest cycle | QA Engineers |
| Day 4 | Exit criteria review, metrics compilation | QA Lead |
| Day 4 | Cycle closure and reporting | QA Lead |

Adjust timeline based on scope. Include buffer days for defect fix-and-retest loops.

### 10. Reporting

Generate the following reports from Zephyr Scale at cycle completion:

- **Test Execution Summary:** Pass/Fail/Blocked/Not Executed counts by suite.
- **Defect Summary:** Open/Closed/Deferred by severity.
- **Requirement Coverage:** Percentage of Jira requirements covered by executed tests.
- **Risk Items:** Any exit criteria not met with documented risk acceptance.
- **Cycle Metrics:** Execution rate, pass rate, defect find rate, retest rate.

### 11. Communication

| Event | Channel | Audience | Frequency |
|-------|---------|----------|-----------|
| Cycle kickoff | Meeting | QA team, Dev leads, PM | Once at start |
| Daily status | Slack / Email | QA team, Dev leads, PM | Daily during execution |
| Blocker escalation | Slack + Meeting | QA Lead, Dev Lead, RM | As needed |
| Cycle completion | Email + Report | All stakeholders | Once at end |

## Guidelines

- Always check the codebase for recent changes (git log, modified files) to inform regression selection.
- Reference specific Zephyr Scale test cycle keys and folder paths when available.
- Adjust scope and timeline based on the actual release content -- do not copy a previous plan verbatim.
- For hotfix releases, use a condensed plan focusing on the fix verification plus targeted regression.
- Include contingency plans for common blockers (environment issues, resource unavailability).
