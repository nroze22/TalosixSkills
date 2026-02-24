---
name: sprint-planning
description: "Help plan sprints for Talosix EDC development with story point estimation, capacity assessment, sprint goal definition, and Jira integration."
allowed-tools: mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql, mcp__claude_ai_Atlassian__getJiraIssue, mcp__claude_ai_Atlassian__editJiraIssue, mcp__claude_ai_Atlassian__addCommentToJiraIssue, mcp__claude_ai_Atlassian__getVisibleJiraProjects
argument-hint: "[Sprint name/number] [Optional: Jira project key] [Optional: sprint dates]"
---

# Sprint Planning

Assist with sprint planning for Talosix EDC development teams, including pulling stories from Jira, estimating effort, assessing capacity, and generating sprint commitments.

## Workflow

1. **Gather Sprint Context**: Ask the user for:
   - Sprint number/name and dates
   - Team name and composition
   - Jira project key and board
   - Any pre-committed items (carryover, urgent fixes, regulatory deadlines)

2. **Pull Candidate Stories from Jira**:
   - Backlog items: `project = [KEY] AND sprint is EMPTY AND status = "Ready for Dev" ORDER BY priority, rank`
   - Carryover: `project = [KEY] AND sprint = [previous sprint] AND status != Done`
   - Bugs: `project = [KEY] AND type = Bug AND status = Open ORDER BY priority`
   - Use `getJiraIssue` for details on top candidates

3. **Assess and Plan**: Apply the framework below.

4. **Output**: Generate sprint plan document.

## Capacity Assessment

### Team Capacity Template

| Team Member | Role | Available Days | Ceremonies (days) | Net Capacity (days) | Story Points |
|-------------|------|---------------|-------------------|---------------------|-------------|
| [Name] | Dev | [X] | 1 | [X-1] | [X * velocity] |
| [Name] | Dev | [X] | 1 | [X-1] | [X * velocity] |
| [Name] | QA | [X] | 1 | [X-1] | [X * velocity] |
| **Total** | | | | **[Sum]** | **[Sum]** |

**Adjustments**:
- PTO / holidays: subtract days
- On-call rotation: subtract ~20% for on-call engineer
- Interview duties: subtract 0.5 days per interview
- Sprint ceremonies: typically 1 day per 2-week sprint (planning, review, retro, standups)

### Capacity Buffer
- Reserve 15-20% for unplanned work (production issues, support escalations)
- Reserve additional buffer if active customer UATs or validation activities are in progress
- For sprints near a release: reserve 10% for regression testing support

## Story Point Estimation Guide (EDC-Specific)

| Points | Complexity | Example |
|--------|-----------|---------|
| 1 | Trivial | Config change, copy update, simple UI text fix |
| 2 | Small | Single-field addition to eCRF, simple API endpoint change |
| 3 | Medium | New edit check type, query workflow enhancement |
| 5 | Moderate | New report/dashboard, integration endpoint, form builder feature |
| 8 | Large | New module (e.g., lab data import), complex edit check engine change |
| 13 | Very Large | Major feature (e.g., ePRO module, randomization engine) |
| 21+ | Epic-sized | Break this down further before committing |

### EDC Estimation Considerations
- **Audit trail**: Add 1-2 points for features requiring comprehensive audit logging
- **Validation**: Add 2-3 points for features requiring IQ/OQ/PQ documentation
- **Migration**: Add points for data migration scripts affecting live studies
- **Multi-study impact**: Add points for features that must work across study configurations
- **Regulatory review**: Add 1 point if regulatory/compliance team review is needed

## Sprint Goal Template

A good sprint goal for Talosix EDC teams should:
- Be outcome-oriented, not task-oriented
- Connect to a user or business outcome
- Be achievable within the sprint

**Format**: "By the end of this sprint, [user/persona] will be able to [capability], enabling [outcome]."

**Examples**:
- "By the end of this sprint, Clinical Data Managers will be able to configure conditional edit checks using the visual builder, reducing setup time by 50%."
- "By the end of this sprint, the audit trail export will meet 21 CFR Part 11 requirements for customer X's upcoming FDA inspection."

## Sprint Plan Output

```
## Sprint [Number]: [Name]
**Dates**: [Start] - [End]
**Team**: [Team name]
**Capacity**: [X] story points ([Y] available after buffer)

### Sprint Goal
[Sprint goal statement]

### Committed Stories

| # | Key | Title | Points | Assignee | Type | Notes |
|---|-----|-------|--------|----------|------|-------|
| 1 | [JIRA-123] | [Title] | [X] | [Name] | Story/Bug | [Notes] |

**Total Committed**: [X] points / [Y] capacity ([Z]% utilization)

### Carryover from Previous Sprint
| Key | Title | Points | Remaining Effort | Reason |
|-----|-------|--------|-----------------|--------|

### Not Committed (Stretch / Next Sprint)
| Key | Title | Points | Reason for Deferral |
|-----|-------|--------|---------------------|

### Risks and Dependencies
- [Risk/dependency and mitigation]

### Definition of Done Reminders
- Code reviewed and merged
- Unit and integration tests passing
- Validation test scripts written (if applicable)
- Audit trail logging verified (if applicable)
- Documentation updated
- QA tested in staging environment
- Product Owner accepted
```

## Post-Planning Actions
Offer to:
- Update Jira stories with sprint assignment using `editJiraIssue`
- Add sprint goal as a comment on the sprint epic
- Identify stories that need further refinement before development starts
