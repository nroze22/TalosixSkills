---
name: customer-feedback-synthesis
description: Synthesize customer feedback from multiple sources, categorize by theme and severity, and generate actionable recommendations for the Talosix EDC platform.
allowed-tools:
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
  - mcp__claude_ai_Atlassian__getJiraIssue
  - mcp__claude_ai_Atlassian__searchConfluenceUsingCql
  - mcp__claude_ai_Atlassian__getConfluencePage
  - mcp__claude_ai_Atlassian__createConfluencePage
argument-hint: "[Paste feedback, provide file paths, or specify Jira filter/label for customer-reported issues]"
---

# Customer Feedback Synthesis

Synthesize customer feedback from multiple sources into structured, actionable insights for Talosix EDC product decisions.

## Workflow

1. **Collect Feedback**: Gather from one or more sources:
   - Direct input: user pastes feedback text (emails, call notes, survey responses)
   - Jira: search for customer-reported issues using `searchJiraIssuesUsingJql` (e.g., `labels = "customer-feedback" OR type = Bug AND reporter in (externalUsers)`)
   - Confluence: search for meeting notes, QBR summaries using `searchConfluenceUsingCql`
   - File paths: read transcript files or exported survey data

2. **Categorize and Analyze**: Process all feedback through the framework below.

3. **Generate the Synthesis Report**.

4. **Publish**: Offer to create a Confluence page.

## Feedback Categorization

### By Source Type
- **Support Tickets**: Reactive, often bug-focused
- **Feature Requests**: Proactive, strategic needs
- **QBR / Account Reviews**: Strategic, relationship-driven
- **User Interviews**: Deep qualitative insights
- **Surveys (NPS/CSAT)**: Quantitative with qualitative comments
- **Sales Feedback**: Prospect objections, competitive intel
- **Onboarding Feedback**: First impressions, setup friction
- **Audit/Inspection Feedback**: Regulatory and compliance gaps

### By Customer Segment
- **Pharma Sponsors**: Enterprise needs, complex studies, integrations
- **CROs**: Multi-sponsor, efficiency, configurability, scalability
- **Biotech**: Speed, simplicity, cost-effectiveness
- **Academic / Investigator-Initiated**: Budget constraints, flexibility
- **Sites**: Usability, speed, minimal training burden

### By Product Area
- eCRF Design and Data Entry
- Edit Checks and Validation
- Query Management
- Data Export / CDISC Compliance
- Monitoring and SDV
- User Administration and Permissions
- Reporting and Dashboards
- Integrations (Labs, IRT, Safety, CTMS)
- ePRO / eCOA
- Performance and Reliability
- Compliance and Audit Trail

### By Severity / Urgency
- **Critical**: Blocking customer operations, data integrity risk, regulatory non-compliance
- **High**: Significant workflow disruption, workaround required, affecting multiple users
- **Medium**: Inconvenience, minor workaround, affects efficiency
- **Low**: Cosmetic, nice-to-have, minor annoyance

## Synthesis Report Template

### Executive Summary
- Total feedback items analyzed: [N]
- Date range: [Start] to [End]
- Sources: [List]
- Top 3 themes and recommended actions

### Theme Analysis

For each identified theme:

#### Theme: [Theme Name]
- **Description**: What customers are telling us
- **Frequency**: Number of mentions / unique customers
- **Severity Distribution**: Critical [X], High [X], Medium [X], Low [X]
- **Customer Segments Affected**: [List segments]
- **Representative Quotes**:
  - "[Quote]" -- [Customer type, size]
  - "[Quote]" -- [Customer type, size]
- **Current State**: How Talosix handles this today
- **Recommendation**: Proposed action and priority
- **Estimated Impact**: Revenue at risk, satisfaction improvement, competitive positioning

### Feedback Volume Trends

| Theme | This Quarter | Last Quarter | Trend |
|-------|-------------|-------------|-------|
| [Theme 1] | [Count] | [Count] | Up/Down/Stable |

### Customer Sentiment Summary

| Segment | Overall Sentiment | Top Positive | Top Negative |
|---------|------------------|-------------|-------------|
| Pharma | [Positive/Neutral/Negative] | [Theme] | [Theme] |
| CRO | [Positive/Neutral/Negative] | [Theme] | [Theme] |
| Biotech | [Positive/Neutral/Negative] | [Theme] | [Theme] |

### Actionable Recommendations

Ranked by impact and urgency:

| Priority | Recommendation | Themes Addressed | Customer Impact | Effort | Owner |
|----------|---------------|-----------------|----------------|--------|-------|
| 1 | [Action] | [Themes] | [Impact] | [S/M/L] | [Team] |

### Risk Flags
- **Churn Risk**: Customers showing frustration patterns or escalation
- **Regulatory Risk**: Feedback indicating compliance gaps
- **Competitive Risk**: Customers mentioning competitor capabilities

### Gaps in Feedback
- Areas with insufficient data to draw conclusions
- Customer segments underrepresented
- Recommended research to fill gaps

## Multi-Period Comparison

When analyzing feedback over time, include:
- Theme emergence and resolution tracking
- Sentiment trend by segment
- Effectiveness of past product changes (did feedback decrease after a fix?)
- Emerging themes that warrant early attention
