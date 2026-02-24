---
name: roadmap-planning
description: Create product roadmaps for Talosix EDC platform tailored to different audiences (engineering, leadership, customers) with timelines, dependencies, and milestones.
allowed-tools:
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
  - mcp__claude_ai_Atlassian__getJiraIssue
  - mcp__claude_ai_Atlassian__getVisibleJiraProjects
  - mcp__claude_ai_Atlassian__searchConfluenceUsingCql
  - mcp__claude_ai_Atlassian__createConfluencePage
argument-hint: "[Audience: 'engineering' | 'leadership' | 'customer'] [Time horizon: 'quarterly' | 'annual']"
---

# Roadmap Planning

Create tailored product roadmaps for the Talosix EDC platform. Roadmaps are adapted for different audiences with appropriate levels of detail and framing.

## Workflow

1. **Gather Data**: Pull current state from Jira:
   - Search for epics and their statuses using `searchJiraIssuesUsingJql` (e.g., `type = Epic AND project = [PROJECT] ORDER BY priority`)
   - Get details on key epics using `getJiraIssue`
   - Search Confluence for strategic documents or prior roadmaps
   - Ask user for: time horizon, audience, strategic priorities, any fixed commitments

2. **Organize by Theme**: Group initiatives into strategic themes relevant to EDC:
   - **Core EDC Platform**: eCRF builder, data entry, edit checks, query management
   - **Data Management**: CDISC compliance, data exports, coding, reconciliation
   - **Monitoring & Oversight**: SDV, remote monitoring, risk-based monitoring
   - **Patient Engagement**: ePRO/eCOA, eConsent, patient portals
   - **Integrations**: Labs, IRT/IWRS, safety, CTMS, imaging
   - **Compliance & Security**: 21 CFR Part 11, audit trails, validation, SOC 2
   - **Platform & Infrastructure**: Performance, scalability, API, multi-tenancy

3. **Generate the Roadmap** in the audience-appropriate format.

## Audience Formats

### Engineering Roadmap
Focus: What to build and when, technical dependencies, capacity needs.

```
## Q[X] [Year] - Engineering Roadmap

### Theme: [Theme Name]

#### Epic: [Epic Name] (JIRA-123)
- **Status**: Not Started | In Progress | Done
- **Target Sprint/PI**: [Sprint range]
- **Story Points**: [Estimate]
- **Team**: [Team name]
- **Dependencies**: [List blockers and upstream/downstream dependencies]
- **Tech Notes**: [Architecture decisions, tech debt, infrastructure needs]
- **Risks**: [Technical risks and mitigation]

### Capacity Allocation
| Team | Capacity (pts) | Committed | Buffer |
|------|---------------|-----------|--------|
| [Team] | [X] | [Y] | [Z] |

### Dependency Map
[List cross-team and external dependencies with dates]

### Tech Debt / Infrastructure
[Dedicated section for non-feature work]
```

### Leadership Roadmap
Focus: Strategic alignment, business value, investment decisions, milestone dates.

```
## [Year] Product Roadmap - Executive Summary

### Strategic Pillars
1. [Pillar]: [1-sentence description and business rationale]

### Quarterly Overview
| Quarter | Key Deliverables | Business Impact | Investment |
|---------|-----------------|-----------------|------------|
| Q1 | [Features] | [Impact] | [Team/cost] |

### Initiative Detail
#### [Initiative Name]
- **Business Case**: Why this matters (revenue, retention, compliance, competitive)
- **Target Customers**: [Segments impacted]
- **Key Milestones**: [Dates and deliverables]
- **Success Metrics**: [KPIs]
- **Status**: On Track | At Risk | Delayed
- **Decision Needed**: [Any executive decisions required]

### Risk Register
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
```

### Customer-Facing Roadmap
Focus: Value delivery, no internal details, commitment-safe language.

```
## Talosix EDC Product Roadmap - [Quarter/Year]

### Recently Delivered
- [Feature]: [User benefit description]

### Coming Soon (Current Quarter)
- [Feature]: [User benefit, no dates, no technical detail]

### On the Horizon (Next 1-2 Quarters)
- [Feature]: [Directional description]

### Exploring (Future)
- [Area]: [High-level direction]

**Note**: Roadmap is directional and subject to change.
Regulatory and compliance features are prioritized to maintain
validated system status and 21 CFR Part 11 compliance.
```

## EDC-Specific Milestones to Track
- Database lock readiness features
- CDISC standards updates (CDASH, ODM, Define-XML)
- Regulatory submission support timelines
- Validation documentation releases
- SOC 2 audit cycles
- Customer UAT windows
- Major conference demos (DIA, SCOPE, SCDM)

## Dependencies to Flag
- Regulatory changes requiring platform updates
- CDISC standard version transitions
- Third-party integration partner timelines (lab vendors, IRT providers)
- Infrastructure/cloud provider changes
- Validation cycle requirements (changes require re-validation at customer sites)

## Output
Offer to:
- Publish to Confluence
- Export as a formatted table for slides
- Generate a Mermaid diagram for visual timeline
