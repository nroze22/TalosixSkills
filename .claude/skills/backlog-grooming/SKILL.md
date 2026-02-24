---
name: Backlog Grooming
description: Assist with backlog refinement for Talosix EDC development, reviewing stories for completeness, adding acceptance criteria, and identifying dependencies.
allowed-tools:
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
  - mcp__claude_ai_Atlassian__getJiraIssue
  - mcp__claude_ai_Atlassian__editJiraIssue
  - mcp__claude_ai_Atlassian__addCommentToJiraIssue
argument-hint: "[Jira project key or specific ticket IDs to review] [Optional: 'full audit' or 'pre-sprint']"
---

# Backlog Grooming

Assist with backlog refinement for Talosix EDC teams by reviewing stories for completeness, suggesting acceptance criteria, identifying dependencies and blockers, and ensuring stories are ready for sprint planning.

## Workflow

1. **Pull Backlog Items**: Use Jira to get stories for review:
   - All ungroomed: `project = [KEY] AND status = "To Do" AND labels != "groomed" ORDER BY priority, created`
   - Specific epic: `project = [KEY] AND "Epic Link" = [EPIC-ID] AND status = "To Do"`
   - Pre-sprint candidates: `project = [KEY] AND sprint is EMPTY AND status = "Ready for Refinement" ORDER BY rank`
   - Get full details with `getJiraIssue` for each story

2. **Review Each Story** against the quality checklist.

3. **Generate Recommendations**: Produce a grooming report with improvements.

4. **Update Jira**: Offer to update stories with suggested improvements.

## Story Quality Checklist

Review each story against these criteria:

### Completeness
- [ ] **Title**: Clear, concise, follows "[Action] [Object] [Context]" format
- [ ] **Description**: Sufficient context for a developer to understand the need
- [ ] **User Story Format**: "As a [persona], I want [action] so that [outcome]"
- [ ] **Acceptance Criteria**: Defined, testable, comprehensive (see Acceptance Criteria skill)
- [ ] **Priority**: Set and justified
- [ ] **Story Points**: Estimated (or flagged for estimation)
- [ ] **Labels/Components**: Properly tagged for tracking

### EDC-Specific Completeness
- [ ] **Personas Identified**: Which EDC user roles are affected (CDM, CRC, Monitor, PI, Admin)?
- [ ] **Study Impact**: Does this affect active studies? New studies only? Both?
- [ ] **Regulatory Flag**: Does this touch 21 CFR Part 11 requirements (audit trail, e-signatures, access control)?
- [ ] **Validation Impact**: Will this require validation documentation updates?
- [ ] **Data Model Impact**: Does this change the data model or affect existing data?
- [ ] **Multi-tenant Consideration**: How does this work across different customer configurations?

### INVEST Criteria
- **I**ndependent: Can be developed without waiting on other stories
- **N**egotiable: Scope can be discussed and adjusted
- **V**aluable: Delivers clear value to a user or the business
- **E**stimable: Team can reasonably estimate the effort
- **S**mall: Can be completed in one sprint (if not, split it)
- **T**estable: Clear pass/fail criteria exist

## Common Story Problems and Fixes

### Problem: Story Too Large
**Indicator**: Estimated at 13+ points, or vague scope
**Fix**: Split by:
- User persona (CDM vs. site vs. monitor workflow)
- CRUD operations (create, read, update, delete as separate stories)
- Happy path vs. edge cases
- Backend API vs. frontend UI
- Core feature vs. configuration/admin

### Problem: Missing Acceptance Criteria
**Fix**: Generate criteria covering:
- Positive scenarios (happy path)
- Negative scenarios (error handling, validation)
- Edge cases (empty states, max limits, concurrent users)
- Regulatory scenarios (audit trail entries, permission checks)
- See the `acceptance-criteria` skill for detailed generation

### Problem: Hidden Dependencies
**Common EDC dependencies to check**:
- Audit trail infrastructure changes
- Database migration requirements
- API versioning for existing integrations
- Permission/role model changes
- Study configuration schema changes
- CDISC export format impacts
- Downstream reporting impacts

### Problem: Unclear Scope
**Fix**: Add explicit "In Scope" and "Out of Scope" sections:
```
**In Scope**:
- [Specific deliverable 1]
- [Specific deliverable 2]

**Out of Scope** (future stories):
- [Related but deferred item 1]
- [Related but deferred item 2]
```

## Grooming Report Template

```
## Backlog Grooming Report
**Date**: [Date]
**Stories Reviewed**: [N]
**Project**: [Jira project key]

### Summary
- Ready for sprint: [X] stories ([Y] points)
- Needs refinement: [X] stories
- Needs splitting: [X] stories
- Blocked: [X] stories

### Story-by-Story Review

#### [JIRA-123]: [Story Title]
- **Status**: Ready / Needs Work / Needs Split / Blocked
- **Issues Found**:
  - [Issue 1 and recommendation]
  - [Issue 2 and recommendation]
- **Suggested Acceptance Criteria** (if missing):
  - Given [context], when [action], then [expected result]
- **Dependencies Identified**: [List]
- **Regulatory Flag**: [Yes/No - explanation]
- **Estimated Points**: [Suggestion if unestimated]

### Dependencies Map
[JIRA-123] --> [JIRA-456] (needs API endpoint first)
[JIRA-789] --> [External: Lab vendor API update]

### Recommended Priority Adjustments
| Story | Current Priority | Suggested Priority | Rationale |
|-------|-----------------|-------------------|-----------|

### Action Items
- [ ] [Owner]: Split [JIRA-XXX] into [suggested sub-stories]
- [ ] [Owner]: Add acceptance criteria to [JIRA-XXX]
- [ ] [Owner]: Resolve dependency with [team/system]
- [ ] [Owner]: Get regulatory review for [JIRA-XXX]
```

## Post-Grooming Actions
Offer to:
- Update stories in Jira with suggested improvements via `editJiraIssue`
- Add comments with questions or suggestions via `addCommentToJiraIssue`
- Add "groomed" label to reviewed stories
- Create new stories for items that need splitting
