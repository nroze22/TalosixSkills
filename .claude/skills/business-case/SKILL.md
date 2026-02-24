---
name: business-case-builder
description: Build comprehensive business cases for new Talosix EDC features and investments, including market analysis, ROI projections, and clinical trial industry metrics.
allowed-tools:
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
  - mcp__claude_ai_Atlassian__searchConfluenceUsingCql
  - mcp__claude_ai_Atlassian__getConfluencePage
  - mcp__claude_ai_Atlassian__createConfluencePage
  - WebSearch
argument-hint: "[Feature or investment to evaluate, e.g. 'eConsent module', 'RTSM integration', 'AI-powered edit checks']"
---

# Business Case Builder

Build rigorous business cases for Talosix EDC feature investments, grounded in clinical trial industry economics and competitive dynamics.

## Workflow

1. **Gather Input**: Ask the user for:
   - Feature/investment being evaluated
   - Target customer segment (pharma sponsors, CROs, academic sites, biotechs)
   - Investment scope (build vs. buy vs. partner)
   - Any existing data (customer requests, win/loss data, competitor moves)

2. **Research**: Pull from available sources:
   - Jira: related customer requests, feature votes
   - Confluence: prior analyses, customer feedback, market research
   - Use industry knowledge of the clinical trial EDC market

3. **Generate the Business Case** document.

## Business Case Template

### Executive Summary
- 3-5 sentence overview: what, why, expected return, recommendation
- Go / No-Go / Defer recommendation with confidence level

### 1. Opportunity Description
- **Feature/Investment**: Clear description
- **Problem Being Solved**: For whom, how severe
- **Strategic Alignment**: How this fits Talosix product strategy
- **Trigger**: What prompted this evaluation (customer request, competitive pressure, regulatory change, market trend)

### 2. Market Analysis

#### Market Size
- **TAM**: Total addressable market for EDC/clinical trial technology
- **SAM**: Serviceable addressable market for this specific capability
- **SOM**: Realistic serviceable obtainable market for Talosix

#### Industry Context
Reference these clinical trial market benchmarks:
- Global clinical trial market: ~$80B+ and growing ~6% CAGR
- EDC market segment: ~$2-3B
- Average clinical trial cost by phase: Phase I ($4M), Phase II ($13M), Phase III ($20M+)
- Average EDC cost as % of trial budget: 2-5%
- Key trends: decentralized trials, risk-based monitoring, AI/ML adoption, patient-centricity

#### Customer Segments
| Segment | Size | Willingness to Pay | Talosix Penetration |
|---------|------|-------------------|---------------------|
| Top 20 Pharma | [X] | High | [Current %] |
| Mid-size Pharma/Biotech | [X] | Medium-High | [Current %] |
| CROs | [X] | Medium | [Current %] |
| Academic/Government | [X] | Low-Medium | [Current %] |

### 3. Competitive Landscape
- **Medidata Rave**: [Current capability and positioning]
- **Veeva Vault EDC**: [Current capability and positioning]
- **Oracle InForm / Clinical One**: [Current capability and positioning]
- **Castor / Decentralized**: [Current capability and positioning]
- **Others**: REDCap, OpenClinica, Climedo, etc.
- **Competitive gap**: What advantage does this give Talosix?

### 4. Financial Analysis

#### Revenue Impact
- **New ARR potential**: Number of deals influenced x average deal size uplift
- **Retention impact**: Churn reduction from addressing unmet need
- **Upsell potential**: Expansion revenue from existing customers
- **Deal win rate impact**: Competitive win rate improvement

#### Cost Estimate
| Category | One-Time | Recurring (Annual) |
|----------|----------|-------------------|
| Engineering (FTEs x months) | $[X] | $[X] |
| Design/UX | $[X] | -- |
| QA/Validation | $[X] | $[X] |
| Infrastructure | $[X] | $[X] |
| Regulatory/Compliance | $[X] | $[X] |
| Training/Enablement | $[X] | $[X] |
| **Total** | **$[X]** | **$[X]** |

#### ROI Calculation
- **Payback Period**: Months to recover investment
- **3-Year NPV**: Net present value over 3 years
- **IRR**: Internal rate of return
- **Break-even**: Number of customers/deals needed

### 5. Risk Assessment
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Regulatory complexity underestimated | [H/M/L] | [H/M/L] | [Strategy] |
| Customer adoption slower than projected | [H/M/L] | [H/M/L] | [Strategy] |
| Competitive response | [H/M/L] | [H/M/L] | [Strategy] |
| Technical complexity | [H/M/L] | [H/M/L] | [Strategy] |
| Validation burden | [H/M/L] | [H/M/L] | [Strategy] |

### 6. Implementation Approach
- **Phase 1 (MVP)**: Scope, timeline, cost
- **Phase 2 (Enhancement)**: Additional capabilities
- **Phase 3 (Scale)**: Full feature parity with market leaders
- Build vs. buy vs. partner recommendation

### 7. Clinical Trial Regulatory Considerations
- 21 CFR Part 11 implications and validation effort
- Impact on existing validated state (regression risk)
- Customer re-validation requirements and communication
- GxP documentation deliverables

### 8. Success Metrics and Tracking
- Leading indicators (pipeline mentions, demo requests, trial signups)
- Lagging indicators (revenue, adoption, retention)
- Review cadence: 30/60/90 day checkpoints post-launch

### 9. Recommendation
- Clear Go / No-Go / Defer with rationale
- If Go: requested budget, timeline, team allocation
- If Defer: conditions that would change the recommendation
- Next steps and decision owners
