---
name: requirements-test-matrix
description: "Generate a comprehensive test case matrix from PRD requirements or Jira stories. Categorizes each test as automated, manual, or UAT candidate. Outputs a spreadsheet-ready format with full traceability to requirements."
argument-hint: "[prd-or-story-reference]"
---

# Requirements Test Matrix

Generate a comprehensive, categorized test case matrix from product requirements. Each test case is mapped to its source requirement and categorized by test type (automated, manual, UAT) for planning and execution.

## When to Use

- PRD approved and stories created - need test coverage plan
- QA needs a master test list before sprint execution
- Preparing UAT test suites for CRO/sponsor users
- Building regression test suite for a new feature
- Validation planning (mapping tests to IQ/OQ/PQ)

## Workflow

### Step 1: Ingest Requirements

Accept input from:
- PRD text or Confluence URL
- Jira story keys (pull via Atlassian MCP)
- Pasted requirements or acceptance criteria

### Step 2: Extract Testable Requirements

For each requirement, identify:
- Functional behavior to verify
- Edge cases and boundary conditions
- Error handling scenarios
- Regulatory/compliance requirements
- Performance expectations
- Security requirements

### Step 3: Generate Test Case Matrix

Output as a table (CSV/spreadsheet-ready):

```
| TC-ID | Requirement Ref | Test Category | Test Title | Preconditions | Steps | Expected Result | Test Type | Priority | Automation Candidate | UAT Candidate | Validation Phase |
|-------|----------------|---------------|------------|---------------|-------|-----------------|-----------|----------|---------------------|---------------|-----------------|
```

#### Column Definitions

- **TC-ID**: Unique identifier (TC-[Feature]-[Number], e.g., TC-ENROLL-001)
- **Requirement Ref**: PRD section or Jira story key this traces to
- **Test Category**: Functional | Negative | Boundary | Security | Performance | Compliance | Integration | Accessibility
- **Test Title**: Clear, concise description of what is being tested
- **Preconditions**: System state required before test execution
- **Steps**: Numbered steps to execute (keep concise, 3-7 steps)
- **Expected Result**: Specific, observable, pass/fail outcome
- **Test Type**: Unit | Integration | System | E2E | Regression
- **Priority**: P1-Critical | P2-High | P3-Medium | P4-Low
- **Automation Candidate**: Yes | No | Partial (with reason)
- **UAT Candidate**: Yes | No (with audience: CRO, Sponsor, Site)
- **Validation Phase**: IQ | OQ | PQ | N/A

### Categorization Rules

#### Automation Candidates (mark Yes when):
- Repeatable with consistent inputs/outputs
- Data-driven (same test, different data sets)
- Regression risk (core functionality that must always work)
- High-frequency execution (run every build)
- No subjective judgment required
- UI interactions follow predictable patterns
- API/service layer tests
- Database validation queries

#### Manual Testing Required (mark No when):
- Requires subjective visual assessment
- Complex multi-system workflow with timing dependencies
- Exploratory testing needed
- First-time validation of new functionality
- Usability evaluation
- Complex clinical workflow requiring domain judgment

#### UAT Candidates (mark Yes when):
- Business-critical workflow that end users must validate
- Clinical data entry patterns that site users perform daily
- Query management workflows (Data Managers)
- Monitoring and SDV workflows (CRAs)
- Report generation and data export (Sponsors, Biostatisticians)
- Any workflow where incorrect behavior impacts patient safety

### Step 4: Generate Category Summaries

```
## Test Matrix Summary

### By Test Category
| Category     | Count | Auto | Manual | UAT |
|-------------|-------|------|--------|-----|
| Functional  | ...   | ...  | ...    | ... |
| Negative    | ...   | ...  | ...    | ... |
| Boundary    | ...   | ...  | ...    | ... |
| Security    | ...   | ...  | ...    | ... |
| Performance | ...   | ...  | ...    | ... |
| Compliance  | ...   | ...  | ...    | ... |
| Integration | ...   | ...  | ...    | ... |

### By Priority
| Priority    | Count | Auto | Manual | UAT |
|-------------|-------|------|--------|-----|
| P1-Critical | ...   | ...  | ...    | ... |
| P2-High     | ...   | ...  | ...    | ... |
| P3-Medium   | ...   | ...  | ...    | ... |
| P4-Low      | ...   | ...  | ...    | ... |

### Automation Coverage
- Total test cases: [N]
- Automation candidates: [N] ([%])
- Manual only: [N] ([%])
- UAT required: [N] ([%])

### Traceability Coverage
- Requirements with test coverage: [N/total] ([%])
- Requirements without test coverage: [list any gaps]
```

### Step 5: Regulatory Test Patterns

For clinical trial EDC, always include these test categories:

**Audit Trail Tests:**
- TC: Verify audit trail records user ID, timestamp, old value, new value for every data change
- TC: Verify audit trail captures reason for change
- TC: Verify audit trail is read-only (cannot be modified or deleted)
- TC: Verify audit trail is viewable by authorized users only

**Electronic Signature Tests (21 CFR Part 11):**
- TC: Verify signature requires username and password re-entry
- TC: Verify signature meaning is displayed before signing
- TC: Verify signed record is locked from editing
- TC: Verify re-signature is required after any change to signed data
- TC: Verify signature timestamp is system-generated (not user-editable)

**Data Integrity Tests (ALCOA+):**
- TC: Verify data is Attributable (who entered it)
- TC: Verify data is Legible (readable, permanent)
- TC: Verify data is Contemporaneous (timestamped at entry)
- TC: Verify data is Original (first recording preserved)
- TC: Verify data is Accurate (validation rules enforced)

**Access Control Tests:**
- TC: Verify role-based access for each user role
- TC: Verify site-level data segregation
- TC: Verify study-level access restrictions
- TC: Verify unauthorized access attempts are blocked and logged

### Step 6: Output Formats

Offer the test matrix in multiple formats:
1. **Markdown table** (default - paste into Confluence or Jira)
2. **CSV format** (copy into Excel/Google Sheets)
3. **Zephyr Scale import format** (if using Zephyr for test management)

### Step 7: Offer Next Actions

- "Want me to generate detailed Playwright test scripts for the automation candidates? (use /playwright-test-generation)"
- "Shall I create UAT test scripts in business language? (use /uat-test-generation)"
- "Need a full test strategy document? (use /test-strategy)"
- "Want to sync these to Zephyr Scale? (use /zephyr-test-sync)"
- "Should I create the Jira stories for test execution? (use /prd-to-stories)"
