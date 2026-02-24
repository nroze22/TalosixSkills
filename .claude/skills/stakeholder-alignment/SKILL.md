---
name: stakeholder-alignment
description: Create stakeholder alignment documents including RACI matrices, decision logs, and communication plans for cross-functional clinical trial feature development.
allowed-tools:
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
  - mcp__claude_ai_Atlassian__searchConfluenceUsingCql
  - mcp__claude_ai_Atlassian__createConfluencePage
  - mcp__claude_ai_Atlassian__getConfluencePage
argument-hint: "[Initiative or feature name requiring cross-functional alignment]"
---

# Stakeholder Alignment

Create structured alignment documents for cross-functional initiatives at Talosix. Clinical trial EDC features often require coordination across engineering, regulatory, clinical operations, customer success, and commercial teams.

## Workflow

1. **Identify the Initiative**: Get the feature/initiative name and scope from the user.
2. **Map Stakeholders**: Identify all involved teams and individuals.
3. **Generate Documents**: Produce the requested alignment artifacts.
4. **Publish**: Offer to create a Confluence page for shared access.

## Document Types

### 1. RACI Matrix

Define Responsible, Accountable, Consulted, Informed for each workstream.

#### Talosix Standard Stakeholder Roles
- **Product Management**: Feature definition, prioritization, go-to-market
- **Engineering**: Design, development, code review, deployment
- **QA / Validation**: Test planning, IQ/OQ/PQ execution, validation documentation
- **Regulatory / Compliance**: 21 CFR Part 11 review, audit readiness, SOP updates
- **Clinical Operations**: Domain expertise, workflow validation, customer requirements
- **Customer Success**: Customer communication, training, UAT coordination
- **Commercial / Sales**: Positioning, pricing, competitive messaging
- **Security / IT**: Infrastructure, access control, penetration testing
- **Documentation**: User guides, release notes, help center updates
- **Executive Sponsor**: Budget approval, strategic decisions, escalation path

#### RACI Template

| Activity | Product | Engineering | QA | Regulatory | Clinical Ops | CS | Commercial |
|----------|---------|-------------|-----|-----------|-------------|-----|-----------|
| Requirements definition | R/A | C | C | C | C | I | I |
| Technical design | C | R/A | C | I | I | -- | -- |
| Development | I | R/A | I | -- | -- | -- | -- |
| Test planning | C | C | R/A | C | C | -- | -- |
| Validation documentation | C | C | R/A | A | C | -- | -- |
| Regulatory review | I | I | C | R/A | C | -- | -- |
| UAT coordination | C | I | C | -- | C | R/A | -- |
| Customer communication | A | -- | -- | C | C | R | C |
| Training materials | C | -- | -- | -- | C | R/A | I |
| Go-to-market | A | -- | -- | -- | C | C | R |

### 2. Decision Log

Track key decisions for audit trail and alignment.

| # | Date | Decision | Context/Rationale | Decision Maker | Stakeholders Consulted | Impact | Status |
|---|------|----------|-------------------|---------------|----------------------|--------|--------|
| 1 | [Date] | [Decision] | [Why] | [Name/Role] | [Names] | [Impact] | Final/Pending |

#### Decision Categories for EDC Features
- **Regulatory**: Compliance approach, validation strategy, audit trail scope
- **Technical**: Architecture, data model, integration approach
- **Product**: Scope, phasing, MVP definition, UX approach
- **Commercial**: Pricing, packaging, customer rollout order
- **Operational**: Training, support model, documentation

### 3. Communication Plan

| Audience | Message Focus | Channel | Frequency | Owner | Start Date |
|----------|--------------|---------|-----------|-------|------------|
| Engineering team | Technical progress, blockers | Standup / Slack | Daily | Tech Lead | [Date] |
| Product + Eng leads | Milestone tracking | Weekly sync | Weekly | PM | [Date] |
| Leadership | Status, risks, decisions needed | Executive update | Bi-weekly | PM | [Date] |
| Customer Success | Feature details, timeline, talking points | CS briefing | Monthly | PM + CS Lead | [Date] |
| Customers (beta) | Preview, feedback request | Email + webinar | At milestones | CS | [Date] |
| All customers | Release announcement | Release notes + email | At GA | Marketing | [Date] |
| Regulatory | Compliance review gates | Review meeting | At gates | Regulatory Lead | [Date] |

### 4. Alignment Meeting Agenda Template

```
## [Initiative] Alignment Meeting - [Date]

### Attendees
[List with roles]

### Agenda (30 min)
1. (5 min) Status update - what's changed since last meeting
2. (10 min) Open decisions requiring input
3. (10 min) Risks and blockers
4. (5 min) Action items and next steps

### Pre-Read
- [Link to PRD / design doc]
- [Link to Jira board]
- [Link to decision log]

### Decision(s) Needed
- [ ] [Decision 1]: Options A vs B, recommendation
- [ ] [Decision 2]: Go/no-go on [scope item]
```

### 5. Stakeholder Map

Categorize stakeholders by influence and interest:

```
         High Influence
              |
   Manage     |    Collaborate
   Closely    |    Closely
              |
Low Interest -+------------- High Interest
              |
   Monitor    |    Keep
              |    Informed
              |
         Low Influence
```

For each stakeholder:
- **Name / Role**: Who they are
- **Quadrant**: Collaborate / Manage / Inform / Monitor
- **Key Concern**: What they care about most (e.g., timeline, compliance, cost, UX)
- **Communication Preference**: Email, Slack, meeting, async doc review
- **Potential Blocker?**: Yes/No and mitigation

## EDC-Specific Alignment Considerations

- **Validation gates**: QA and Regulatory must sign off before customer-facing releases
- **Customer re-validation**: Changes to validated features require customer notification and potential re-validation effort
- **Audit trail**: All decisions affecting data handling must be documented for regulatory inspections
- **Change control**: Feature changes after requirements freeze must go through formal change control
- **Multi-study impact**: Changes can affect active studies; rollout must be coordinated with customer study timelines
