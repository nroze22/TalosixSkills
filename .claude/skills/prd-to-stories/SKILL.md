---
name: prd-to-stories
description: "Break down a PRD into individual SMART Jira stories with acceptance criteria, story points, dependencies, and sprint-ready detail. Creates epic structure and story hierarchy from product requirements."
argument-hint: "[prd-content-or-confluence-url]"
---

# PRD to Jira Stories

Break down a Product Requirements Document into well-structured, sprint-ready Jira stories organized under epics with clear acceptance criteria, dependencies, and estimates.

## When to Use

- PRD is approved and ready for engineering breakdown
- Need to create a backlog of stories from requirements
- Converting a feature spec into actionable development work
- Preparing stories for sprint planning

## Workflow

### Step 1: Ingest the PRD

Accept PRD from one of:
- Pasted text in the conversation
- Confluence page (pull via Atlassian MCP if available)
- File path to a local document

### Step 2: Analyze and Decompose

Read the entire PRD and identify:
- **Functional requirements** - what the system must do
- **Non-functional requirements** - performance, security, compliance
- **User roles** involved and their specific workflows
- **Data model changes** required
- **Integration points** with other systems
- **Regulatory requirements** (audit trails, e-signatures, validation)

### Step 3: Create Epic Structure

Group requirements into epics:

```
Epic: [Feature Name] - [Component/Module]
  Description: High-level summary of this epic's scope
  Labels: feature-name, component
  Fix Version: [Target release]
```

### Step 4: Generate Individual Stories

For EACH story, produce:

```
## Story: [EPIC-PREFIX]-[Number]
**Title:** As a [role], I want to [action] so that [benefit]

**Type:** Story | Task | Bug | Spike
**Story Points:** [1, 2, 3, 5, 8, 13]
**Priority:** Critical | High | Medium | Low
**Labels:** [feature], [component], [regulatory] if applicable
**Sprint Candidate:** Yes/No (is it self-contained enough for one sprint?)

### Description
[2-3 sentences describing what needs to be built and why.
Include relevant context from the PRD.]

### Acceptance Criteria
Given [precondition]
When [action]
Then [expected result]

- [ ] AC1: [Specific, testable criterion]
- [ ] AC2: [Specific, testable criterion]
- [ ] AC3: [Specific, testable criterion]
- [ ] AC4: [Regulatory criterion if applicable]

### Technical Notes
- Database changes: [if any]
- API changes: [if any]
- UI components: [if any]
- Migration considerations: [if any]

### Dependencies
- Blocked by: [story IDs]
- Blocks: [story IDs]

### Definition of Done
- [ ] Code complete and passes code review
- [ ] Unit tests written (>80% coverage for new code)
- [ ] Integration tests passing
- [ ] Acceptance criteria verified
- [ ] Documentation updated
- [ ] No critical/high security findings
- [ ] Audit trail requirements met (if clinical data)
```

### Story Sizing Guidelines

| Points | Complexity | Example |
|--------|-----------|---------|
| 1 | Trivial | Config change, copy update, add a field label |
| 2 | Simple | Add a form field with validation, new API parameter |
| 3 | Moderate | New form section, new API endpoint, simple query |
| 5 | Complex | New page/view, multi-step workflow, data migration |
| 8 | Very complex | New module, cross-system integration, complex business logic |
| 13 | Epic-level | Should be broken down further |

### Story Quality Checklist

Every story MUST be SMART:
- **S**pecific: Clear what needs to be built
- **M**easurable: Acceptance criteria are testable
- **A**chievable: Can be completed in one sprint
- **R**elevant: Traces back to a PRD requirement
- **T**ime-bound: Story pointed and sprint-assignable

### Regulatory Story Patterns

For clinical trial EDC features, ensure these story types are included:

**Audit Trail Stories:**
- Capture who changed what, when, and why
- Store previous values for all clinical data changes
- Reason for change required on edits to completed forms

**E-Signature Stories:**
- Re-authentication required (username + password)
- Signature meaning displayed before signing
- Signed records locked from further editing without re-signature

**Access Control Stories:**
- Role-based permissions for each new feature
- Study-level and site-level access restrictions
- Privilege escalation prevention

**Validation Stories:**
- Field-level validation (data types, ranges, formats)
- Cross-field validation (edit checks)
- Form-level validation before completion

### Step 5: Generate Summary

After all stories, provide:

```
## Breakdown Summary

| Epic | Story Count | Total Points | Dependencies |
|------|------------|--------------|--------------|
| ...  | ...        | ...          | ...          |

**Total Stories:** [count]
**Total Story Points:** [sum]
**Estimated Sprints:** [points / team velocity]
**Critical Path:** [list stories that block others]
**Recommended Sprint 1:** [stories with no dependencies, foundational work]
```

### Step 6: Offer Next Actions

- "Want me to create these stories in Jira? (I'll use the Atlassian MCP)"
- "Shall I generate test cases for these stories? (use /requirements-test-matrix)"
- "Need acceptance criteria refined? (use /acceptance-criteria)"
- "Ready to plan the sprint? (use /sprint-planning)"
