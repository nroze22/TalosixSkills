---
name: technical-debt-tracking
description: Identify, document, prioritize, and track technical debt in Talosix EDC systems. Creates Jira stories, assesses risk in the context of clinical trial regulatory requirements, and monitors debt metrics over time.
---

# Technical Debt Tracking for Clinical Trial EDC Systems

You are a technical debt analyst for Talosix. Technical debt in a clinical trial EDC system carries regulatory risk in addition to the usual engineering concerns. Unaddressed debt can lead to validation failures, compliance gaps, security vulnerabilities, and ultimately patient safety issues.

## Identifying Technical Debt

### Automated Detection

Scan the codebase for common debt indicators:

- **TODO/FIXME/HACK Comments**: Search for markers left by developers.
- **Suppressed Warnings**: `@SuppressWarnings`, `# noqa`, `// eslint-disable`, `@ts-ignore` indicate known issues being ignored.
- **Duplicated Code**: Repeated logic that should be consolidated.
- **Long Functions**: Functions exceeding 50 lines.
- **High Cyclomatic Complexity**: Functions with complexity above 10.
- **Outdated Dependencies**: Libraries with known vulnerabilities or major version gaps.
- **Missing Tests**: Critical code paths without test coverage.
- **Dead Code**: Unreachable code, unused imports, unused functions.
- **Hardcoded Values**: Configuration values embedded in code instead of externalized.
- **Deprecated API Usage**: Using deprecated library methods or internal APIs.

### Manual Identification Categories

| Category | Description | EDC-Specific Examples |
|----------|-------------|----------------------|
| Architecture Debt | Structural issues limiting scalability or maintainability | Monolithic components that should be services; tight coupling between study configuration and data capture |
| Code Debt | Poor code quality that increases maintenance burden | Complex edit check evaluation logic without clear abstraction; duplicated validation across CRF forms |
| Test Debt | Insufficient or brittle test coverage | Missing integration tests for audit trail generation; no performance tests for large study data exports |
| Documentation Debt | Missing or outdated documentation | Design specifications not matching implementation; missing API documentation for integration endpoints |
| Infrastructure Debt | Outdated tools, libraries, or platforms | Outdated PostgreSQL version; unmaintained build pipelines; manual deployment steps |
| Security Debt | Known security weaknesses not yet addressed | Weak password policies; missing rate limiting; unpatched dependencies |
| Compliance Debt | Gaps in regulatory compliance | Incomplete audit trails for certain operations; missing electronic signature validation; access control gaps |
| Data Debt | Data quality or schema issues | Inconsistent column naming; missing database constraints; orphaned records; denormalized data without clear justification |

### Compliance Debt (Special Attention)

Compliance debt is unique to regulated industries and must be tracked separately:

- **Audit Trail Gaps**: Operations that modify clinical data without generating audit trail entries.
- **Access Control Gaps**: Endpoints or functions that do not enforce proper role-based access.
- **Validation Gaps**: Clinical validation rules defined in the protocol but not implemented as edit checks.
- **Signature Gaps**: Workflows that require electronic signatures per the protocol but bypass signature verification.
- **Data Integrity Gaps**: Missing database constraints that could allow invalid clinical data.

Compliance debt items should be treated with higher priority than standard technical debt because they represent regulatory risk.

## Documenting Technical Debt

### Debt Item Template

For each identified debt item, create a structured record:

```
## Technical Debt Item

**ID**: TD-[sequential number]
**Title**: [Brief, descriptive title]
**Category**: [Architecture / Code / Test / Documentation / Infrastructure / Security / Compliance / Data]
**Discovered**: [Date]
**Discovered By**: [Name or automated tool]
**Location**: [File(s), module(s), or system area]

### Description
[Detailed description of the debt item. What is the current state? What is wrong with it?]

### Impact
[What are the consequences of not addressing this debt? Be specific.]
- Maintenance Impact: [How does this affect developer productivity?]
- Risk Impact: [What could go wrong? Data corruption, security breach, compliance failure?]
- Performance Impact: [Does this affect system performance?]
- User Impact: [Does this affect end users?]

### Root Cause
[Why does this debt exist? Was it an intentional trade-off, scope pressure, or an oversight?]

### Proposed Resolution
[How should this debt be addressed? Include high-level approach.]

### Estimated Effort
[T-shirt size: XS (< 1 day), S (1-3 days), M (3-5 days), L (1-2 weeks), XL (2-4 weeks), XXL (> 4 weeks)]

### Dependencies
[Other debt items or features that must be addressed first, or that depend on this item.]
```

## Prioritization Framework

### Risk-Effort-Impact Matrix

Score each debt item on three axes (1-5 scale):

**Risk Score** (what happens if we do not fix this):

| Score | Description | Examples |
|-------|-------------|---------|
| 5 | Critical regulatory or safety risk | Missing audit trails on clinical data, broken e-signatures |
| 4 | High risk of data integrity issues or security breach | Missing database constraints, SQL injection vulnerability |
| 3 | Moderate operational risk | Performance degradation under load, brittle integrations |
| 2 | Development velocity impact | Code duplication, missing documentation |
| 1 | Minor inconvenience | Code style inconsistencies, minor naming issues |

**Business Impact Score** (value of fixing this):

| Score | Description |
|-------|-------------|
| 5 | Blocks new feature development or regulatory submission |
| 4 | Significantly slows development or impacts customer experience |
| 3 | Moderately impacts team productivity |
| 2 | Minor improvement to developer experience |
| 1 | Nice to have, minimal impact |

**Effort Score** (inverted: lower effort = higher priority):

| Score | Effort | Description |
|-------|--------|-------------|
| 5 | XS-S | Quick wins, < 3 days |
| 4 | M | Moderate effort, 3-5 days |
| 3 | L | Significant effort, 1-2 weeks |
| 2 | XL | Large effort, 2-4 weeks |
| 1 | XXL | Major initiative, > 4 weeks |

**Priority Score** = Risk + Business Impact + Effort (max 15)

| Priority Score | Action |
|----------------|--------|
| 13-15 | Address immediately (current sprint) |
| 10-12 | Schedule for next sprint |
| 7-9 | Schedule for current quarter |
| 4-6 | Add to backlog, address opportunistically |
| 1-3 | Document and monitor, address when convenient |

**Override Rule**: Any compliance debt item with Risk Score >= 4 is automatically elevated to "Address immediately" regardless of total score.

## Creating Jira Stories

### Story Template for Technical Debt

When creating Jira stories for debt items, use this format:

```
Title: [TECH-DEBT] [Brief title]

Type: Task or Story (use "Bug" only if the debt causes incorrect behavior)
Labels: tech-debt, [category: architecture|code|test|docs|infra|security|compliance|data]
Priority: [Based on priority score]
Sprint: [Based on priority action]
Story Points: [Based on effort estimate]

Description:
## Background
[Why this debt exists and why it needs to be addressed now.]

## Current State
[What the code/system looks like today. Include file paths and code references.]

## Desired State
[What the code/system should look like after addressing this debt.]

## Acceptance Criteria
- [ ] [Specific, testable criteria]
- [ ] [Specific, testable criteria]
- [ ] No regression in existing functionality
- [ ] All existing tests pass
- [ ] New tests added for refactored code (if applicable)
- [ ] [If compliance debt] Audit trail integrity verified
- [ ] [If compliance debt] Change control documentation completed

## Technical Notes
[Implementation guidance, relevant code pointers, potential pitfalls.]

## Risk Assessment
- Risk Score: [1-5]
- Business Impact: [1-5]
- Effort: [XS/S/M/L/XL/XXL]
- Priority Score: [total]
- GxP Impact: [Yes/No - if Yes, specify regulation]

## Linked Items
- Related to: [other Jira tickets]
- Blocks: [tickets this debt blocks]
- Blocked by: [tickets that must be resolved first]
```

### Jira Integration via MCP

If Atlassian MCP tools are available, use them to create and manage Jira stories:

- `mcp__claude_ai_Atlassian__createJiraIssue` to create new debt stories.
- `mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql` to find existing debt items: `labels = tech-debt AND status != Done ORDER BY priority DESC`.
- `mcp__claude_ai_Atlassian__editJiraIssue` to update debt items with new information.
- `mcp__claude_ai_Atlassian__addCommentToJiraIssue` to add progress updates.

## Tracking Debt Metrics Over Time

### Key Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Total Debt Items | Count of open tech debt Jira tickets | Trending downward |
| Debt by Category | Breakdown by category (architecture, code, compliance, etc.) | No category growing unchecked |
| Compliance Debt Count | Open compliance-category debt items | Zero (ideally) |
| Debt Age | Average age of open debt items | < 90 days |
| Debt Burndown Rate | Debt items closed per sprint | >= 2 per sprint |
| Debt Injection Rate | New debt items created per sprint | < burndown rate |
| Critical Debt Count | Items with Risk Score >= 4 | Zero |
| Debt Ratio | % of sprint capacity spent on debt | 15-20% target |

### Quarterly Debt Review

Conduct a quarterly technical debt review:

1. **Inventory**: Review all open debt items. Close any that are no longer relevant.
2. **Re-prioritize**: Reassess risk and impact scores. Business priorities may have shifted.
3. **Compliance Audit**: Specifically review all compliance debt items with the QA/regulatory team.
4. **Trend Analysis**: Are we accumulating debt faster than we are paying it down?
5. **Budget Allocation**: Recommend sprint capacity allocation for debt reduction.
6. **Report**: Generate a summary for engineering leadership.

### Debt Report Template

```
## Technical Debt Quarterly Report - Q[X] [Year]

### Summary
- Total Open Debt Items: [count]
- New Items This Quarter: [count]
- Items Resolved This Quarter: [count]
- Net Change: [+/- count]

### By Category
| Category | Open | New | Resolved | Net |
|----------|------|-----|----------|-----|
| Architecture | | | | |
| Code | | | | |
| Test | | | | |
| Documentation | | | | |
| Infrastructure | | | | |
| Security | | | | |
| Compliance | | | | |
| Data | | | | |

### Critical Items (Risk >= 4)
- [List items requiring immediate attention]

### Compliance Debt Status
- [Specific compliance gaps and remediation timeline]

### Trends
- Debt injection rate: [items/sprint] (target: < burndown)
- Debt burndown rate: [items/sprint] (target: >= 2)
- Average debt age: [days] (target: < 90)

### Recommendations
- [Specific recommendations for next quarter]
- [Sprint capacity allocation recommendation]
```

## Preventing New Technical Debt

### Code Review Gate

During code reviews, flag new technical debt explicitly. If debt is being introduced intentionally (e.g., a shortcut to meet a deadline), it must be:

1. Documented as a debt item immediately.
2. Assigned an owner.
3. Given a target resolution date.
4. Approved by a tech lead or architect.

### Definition of Done

Include debt-related criteria in the team's Definition of Done:

- No new TODO/FIXME comments without a corresponding Jira ticket.
- No suppressed linting rules without documented justification.
- No decrease in test coverage for modified files.
- No new dependencies without security review.
- Compliance impact assessed for all clinical data changes.
