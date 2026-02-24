---
name: meeting-agent
description: Process meeting notes and transcripts. Extract action items with owners and due dates, create Jira tickets from action items, and generate meeting summaries for distribution.
---

# Meeting Agent Skill

Process meeting notes, transcripts, and recordings summaries for Talosix teams. Extract structured information, generate action items, and prepare summaries for distribution.

## When to Use

- After any team meeting to process notes or transcripts
- When raw meeting notes need to be organized
- When action items need to be extracted and tracked
- When a meeting summary needs to be distributed to stakeholders

## Processing Meeting Notes

When given raw meeting notes or a transcript, perform the following:

### Step 1: Meeting Metadata

Extract or request:
- Meeting title/type (standup, sprint planning, design review, etc.)
- Date and time
- Attendees
- Meeting organizer/facilitator

### Step 2: Meeting Summary

Generate a concise summary (3-7 bullet points) covering:
- Key topics discussed
- Decisions made (with rationale if available)
- Important updates shared
- Risks or concerns raised
- Open questions that need follow-up

### Step 3: Decisions Log

Extract all decisions made during the meeting:

```markdown
## Decisions
| # | Decision | Context | Decided By |
|---|----------|---------|------------|
| 1 | <decision> | <why this was decided> | <who decided> |
```

### Step 4: Action Items

Extract all action items with maximum specificity:

```markdown
## Action Items
| # | Action Item | Owner | Due Date | Priority | Related Topic |
|---|-------------|-------|----------|----------|---------------|
| 1 | <specific action> | <name> | <date> | High/Med/Low | <topic> |
```

Rules for action item extraction:
- Every action item must have an owner (if not mentioned, flag as "TBD - needs owner")
- Every action item must have a due date (if not mentioned, suggest one based on context)
- Be specific: "Look into the issue" becomes "Investigate intermittent 500 errors on form save endpoint and report findings"
- Split compound actions: "Fix the bug and update the docs" becomes two action items
- Include context so the action is understandable standalone

### Step 5: Jira Ticket Drafts

For action items that warrant Jira tickets, generate ticket drafts:

```markdown
## Jira Ticket Drafts

### Ticket 1
- **Summary**: <concise title, max 80 chars>
- **Type**: Story / Task / Bug / Spike
- **Description**:
  <Detailed description>

  **Context from meeting (<date>):**
  <relevant discussion context>

  **Acceptance Criteria:**
  - [ ] <criterion 1>
  - [ ] <criterion 2>
- **Assignee**: <name>
- **Priority**: <Critical / High / Medium / Low>
- **Labels**: <suggested labels>
- **Sprint**: <current or next sprint>
```

Guidelines for ticket creation:
- Not every action item needs a Jira ticket (e.g., "Send an email to X" does not)
- Group related action items into a single ticket when appropriate
- Include enough context that someone not in the meeting can understand the ticket
- For clinical/regulatory items, include compliance context

## Meeting Types and Templates

### Sprint Planning Summary
```markdown
# Sprint Planning Summary - Sprint <number>
Date: <date>
Team: <team name>

## Sprint Goal
<goal statement>

## Committed Work
| Ticket | Summary | Points | Assignee |
|--------|---------|--------|----------|
| XX-123 | <summary> | <pts> | <name> |

## Total Commitment: <X> points
## Capacity: <Y> points
## Carry-over from Previous Sprint: <list>

## Risks and Dependencies
- <risk or dependency>

## Action Items
<action items table>
```

### Design Review Summary
```markdown
# Design Review Summary
Date: <date>
Feature: <feature name>
Presenter: <name>

## Design Overview
<brief description of what was reviewed>

## Feedback Summary
| Area | Feedback | Resolution |
|------|----------|------------|
| <area> | <feedback> | Accepted / Deferred / Rejected |

## Clinical Workflow Considerations
- <considerations discussed>

## Decisions
<decisions table>

## Action Items
<action items table>
```

### Stakeholder/Sponsor Meeting Summary
```markdown
# Stakeholder Meeting Summary
Date: <date>
Study: <study name/protocol>
Attendees: <internal and external>

## Discussion Topics
1. <topic and key points>

## Sponsor Requests/Feedback
- <request or feedback>

## Commitments Made
| # | Commitment | Owner | Timeline |
|---|-----------|-------|----------|
| 1 | <commitment> | <name> | <date> |

## Follow-Up Required
<action items table>

## Next Meeting: <date>
```

## Distribution Format

When generating a meeting summary for distribution (email/Slack), use this format:

```markdown
**Meeting: <Title> | <Date>**
Attendees: <names>

**Key Takeaways:**
- <point 1>
- <point 2>
- <point 3>

**Decisions:**
- <decision 1>
- <decision 2>

**Action Items:**
- [ ] <action> (@owner, due <date>)
- [ ] <action> (@owner, due <date>)

**Next Steps:**
<what happens next>

Full notes: <link to detailed notes>
```

## Clinical Trial EDC Context

When processing meetings related to clinical trial topics, pay special attention to:
- **Regulatory commitments**: Any commitment to a sponsor or regulatory body must be clearly captured
- **Protocol changes**: Note any discussion of protocol amendments and their system impact
- **Validation requirements**: Capture any new validation requirements discussed
- **Timeline commitments**: Clinical trial timelines are often contractual; flag any timeline discussions
- **Data integrity items**: Any data quality or integrity discussion points are high priority
- **Audit findings**: If audit findings are discussed, capture required responses and deadlines

## Output

Always produce:
1. A structured meeting summary (using the appropriate template)
2. A standalone action items list
3. Jira ticket drafts for engineering/product action items
4. A distribution-ready summary (shorter format for email/Slack)
