---
name: retrospective
description: Facilitate sprint and release retrospectives for Talosix teams. Structured format covering what went well, what needs improvement, and action items. Captures learnings and generates improvement actions.
---

# Retrospective Skill

Facilitate and document sprint retrospectives, release retrospectives, and post-project reviews for Talosix teams. Generate structured retrospective documents that capture team insights and drive continuous improvement.

## When to Use

- At the end of a sprint
- After a release deployment
- After a project milestone or completion
- After a significant incident (post-incident review)
- When a team wants to reflect on a specific period or event

## Retrospective Formats

### Format 1: Standard Sprint Retrospective

```markdown
# Sprint Retrospective: Sprint <number>
Date: <date>
Team: <team name>
Facilitator: <name>
Attendees: <list>

## Sprint Summary
- Sprint Goal: <goal>
- Committed: <X> story points / <Y> tickets
- Completed: <X> story points / <Y> tickets
- Carry-over: <list of incomplete items>

## What Went Well
1. <item>
2. <item>
3. <item>

## What Didn't Go Well
1. <item>
2. <item>
3. <item>

## What Can We Improve
1. <item>
2. <item>
3. <item>

## Action Items
| # | Action | Owner | Due Date | Status |
|---|--------|-------|----------|--------|
| 1 | <action> | <name> | <date> | Open |
| 2 | <action> | <name> | <date> | Open |

## Follow-Up from Previous Retro
| # | Action | Owner | Status | Notes |
|---|--------|-------|--------|-------|
| 1 | <previous action> | <name> | <status> | <notes> |
```

### Format 2: Release Retrospective

```markdown
# Release Retrospective: v<version>
Date: <date>
Release Date: <release date>
Team: <team name>
Facilitator: <name>

## Release Summary
- Version: <version>
- Features delivered: <count>
- Bugs fixed: <count>
- Planned release date: <date>
- Actual release date: <date>
- Deployment duration: <time>
- Rollback required: Yes/No

## Planning & Scope
### What went well
- <items>

### What didn't go well
- <items>

## Development & Quality
### What went well
- <items>

### What didn't go well
- <items>

## Validation & Compliance
### What went well
- <items>

### What didn't go well
- <items>

## Deployment & Release
### What went well
- <items>

### What didn't go well
- <items>

## Metrics
| Metric | Target | Actual |
|--------|--------|--------|
| Defects found in validation | <target> | <actual> |
| Defects escaped to production | 0 | <actual> |
| Validation cycle time | <target> | <actual> |
| Deployment downtime | <target> | <actual> |
| Post-release incidents | 0 | <actual> |

## Key Learnings
1. <learning>
2. <learning>

## Action Items
| # | Action | Owner | Due Date | Category |
|---|--------|-------|----------|----------|
| 1 | <action> | <name> | <date> | Process |
| 2 | <action> | <name> | <date> | Technical |
```

### Format 3: Post-Incident Review

```markdown
# Post-Incident Review: INC-<number>
Date: <date>
Incident Date: <incident date>
Severity: SEV-<level>
Facilitator: <name>
Attendees: <list>

## Incident Summary
- Description: <what happened>
- Duration: <time to resolve>
- Impact: <users/studies affected>
- Clinical Impact: <assessment>

## Timeline Review
| Time | Event |
|------|-------|
| <time> | <event> |

## What Went Well in Response
1. <item>

## What Could Be Improved in Response
1. <item>

## Root Cause
<root cause summary>

## Prevention
| # | Preventive Action | Owner | Due Date | Type |
|---|-------------------|-------|----------|------|
| 1 | <action> | <name> | <date> | Technical/Process |

## Detection Improvement
How can we detect this faster?
1. <item>

## CAPA Required: Yes/No
If yes: CAPA-<number> initiated
```

## Facilitation Guidelines

### Before the Retrospective
1. Review previous retrospective action items and their status
2. Gather data: sprint metrics, deployment logs, incident reports
3. Send a brief pre-retro survey to collect initial thoughts
4. Prepare the retrospective template

### During the Retrospective
1. **Set the stage** (5 min): Review the purpose, establish psychological safety
2. **Gather data** (15 min): Have each participant share observations
3. **Generate insights** (15 min): Group similar items, identify patterns
4. **Decide what to do** (15 min): Prioritize and assign action items
5. **Close** (5 min): Summarize, confirm action items, gather feedback

### Facilitation Principles
- Blameless: Focus on systems and processes, not individuals
- Specific: "We need better testing" becomes "Add integration tests for the query workflow"
- Actionable: Every improvement becomes an action item with an owner
- Limited: Pick 2-3 action items maximum to avoid overload
- Tracked: Follow up on previous retro actions at the start of each retro

## Clinical Trial EDC Context

When facilitating retrospectives for Talosix teams, consider these domain-specific areas:

- **Validation efficiency**: Are validation cycles too long? Can we improve IQ/OQ automation?
- **Regulatory compliance**: Were there any compliance gaps or near-misses?
- **Audit readiness**: Would we be comfortable with a regulatory audit of this sprint/release?
- **Data integrity**: Were there any data integrity concerns raised?
- **Clinical workflow impact**: Did changes improve or complicate site user workflows?
- **Cross-functional collaboration**: How well did Engineering, QA, Clinical, and Regulatory work together?

## Action Item Best Practices

Good action items are:
- **Specific**: Clear description of what needs to happen
- **Measurable**: You can tell when it's done
- **Owned**: One person is accountable (not "the team")
- **Time-bound**: Has a due date (ideally within the next sprint)
- **Realistic**: Can be completed alongside sprint work

Examples:
- "Add automated smoke test for e-signature flow" (Owner: Jane, Due: Sprint 24)
- "Update deployment runbook with new database migration steps" (Owner: Bob, Due: Feb 28)
- "Schedule cross-team sync between Engineering and Clinical Ops for study launches" (Owner: Alice, Due: Next sprint planning)

## Output Format

Generate retrospective documents in markdown using the appropriate format above. Include all sections even if some have placeholder text, so nothing is missed. When processing raw notes from a user, organize them into the structured format and suggest action items based on the themes identified.
