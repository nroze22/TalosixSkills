---
name: feature-scoring-and-prioritization
description: Data-driven feature prioritization for Talosix EDC using RICE, MoSCoW, and weighted scoring frameworks with regulatory impact consideration.
allowed-tools:
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
  - mcp__claude_ai_Atlassian__getJiraIssue
  - mcp__claude_ai_Atlassian__searchConfluenceUsingCql
argument-hint: "[List of features to score, or 'pull from Jira' with project/filter details]"
---

# Feature Scoring and Prioritization

Perform structured, data-driven feature prioritization for the Talosix EDC platform. Incorporates clinical trial regulatory considerations alongside standard product metrics.

## Workflow

1. **Gather Feature List**: Either:
   - Accept a list of features from the user
   - Pull from Jira using `searchJiraIssuesUsingJql` (e.g., `type = Story AND status = "To Do" AND project = [X]`)
   - Get details on each feature using `getJiraIssue`

2. **Select Scoring Framework**: Ask the user which framework to use, or recommend one:
   - **RICE**: Best for comparing many features with quantitative data
   - **MoSCoW**: Best for release scoping and stakeholder alignment
   - **Weighted Scoring**: Best when multiple custom criteria matter
   - **Combined**: Use RICE for initial ranking, MoSCoW for release slotting

3. **Score Features** using the selected framework with EDC-specific criteria.

4. **Present Results** with ranked output and recommendations.

## RICE Framework (EDC-Adapted)

### Reach (1-10)
How many users/customers are impacted per quarter?
- 10: All customers, all users
- 7: Most customers, specific user role (e.g., all CDMs)
- 5: Subset of customers (e.g., Phase III studies only)
- 3: Few customers, specific use case
- 1: Single customer request

### Impact (0.25 - 3)
How much does this improve the user's workflow?
- 3: Massive -- eliminates a major pain point or enables new capability
- 2: High -- significant time savings or error reduction
- 1: Medium -- noticeable improvement
- 0.5: Low -- minor improvement
- 0.25: Minimal -- nice to have

### Confidence (20% - 100%)
How certain are we about reach and impact estimates?
- 100%: Strong data (customer interviews, usage analytics, regulatory requirement)
- 80%: Good evidence (multiple customer requests, competitive gap)
- 50%: Some signal (a few requests, team intuition)
- 20%: Speculation (no direct evidence)

### Effort (person-months)
Total engineering, QA, and validation effort:
- Include: development, testing, validation documentation, regulatory review
- Account for: 21 CFR Part 11 audit trail requirements, IQ/OQ/PQ test scripts
- Factor in: customer re-validation communication and support

### RICE Score = (Reach x Impact x Confidence) / Effort

## MoSCoW Framework (EDC-Adapted)

### Must Have
- Regulatory requirements (FDA, EMA mandates)
- Critical defects affecting data integrity
- Contractual commitments to customers
- Security vulnerabilities
- Features required for validated system status

### Should Have
- High-demand customer requests (5+ customers)
- Competitive table-stakes features
- Significant efficiency improvements for site users
- Features that reduce support burden

### Could Have
- Nice-to-have UX improvements
- Features requested by 1-2 customers
- Internal tooling improvements
- Performance optimizations (non-critical)

### Won't Have (This Release)
- Low demand, high effort features
- Features with unresolved regulatory questions
- Items that require infrastructure not yet in place

## Weighted Scoring Matrix

Score each feature 1-5 on these criteria, with EDC-specific weights:

| Criterion | Weight | Description |
|-----------|--------|-------------|
| Customer Demand | 20% | Number and size of requesting customers |
| Revenue Impact | 15% | ARR growth, churn prevention, deal acceleration |
| Competitive Necessity | 15% | Gap vs. Medidata, Veeva, Oracle |
| Regulatory Compliance | 15% | Required for 21 CFR Part 11, GCP, Annex 11 |
| User Experience | 10% | Impact on site/CDM/monitor daily workflows |
| Technical Complexity | 10% | Engineering effort (inverse: lower = better) |
| Validation Effort | 10% | IQ/OQ/PQ documentation and testing burden |
| Strategic Alignment | 5% | Fit with product vision and company strategy |

**Weighted Score** = Sum of (Score x Weight) for each criterion.

## Output Format

### Ranked Feature List

| Rank | Feature | RICE Score | MoSCoW | Weighted Score | Recommendation |
|------|---------|------------|--------|---------------|----------------|
| 1 | [Feature] | [X] | Must | [Y] | Build in Q[X] |
| 2 | [Feature] | [X] | Should | [Y] | Build in Q[X] |

### Scoring Detail (per feature)
- Feature name and Jira link
- Score breakdown by criterion
- Key justification notes
- Dependencies or blockers
- Regulatory impact flag (Yes/No + explanation)

### Prioritization Recommendations
- **Immediate (this sprint/PI)**: Top-priority items
- **Next quarter**: High-value items requiring planning
- **Backlog**: Lower priority, revisit next cycle
- **Decline/Defer**: Items not worth pursuing now, with rationale

### Sensitivity Analysis
- Which features change rank if weights are adjusted?
- What new information would change the prioritization?
- Flag features where confidence is low and more research is needed

## Tips
- Always flag regulatory-driven features separately -- these often override scoring
- Consider the "validation tax": features touching validated workflows require extra QA effort
- Group related features that share infrastructure investment
- Note when a feature is a prerequisite for higher-scored future features
