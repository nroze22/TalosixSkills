---
name: reference-matching
description: "Match Talosix reference customers to prospect profiles based on therapeutic area, study phase, company size, and geography, with talking points and usage tracking."
---

# Reference Customer Matching

## Purpose

Strategically match the most relevant Talosix reference customers to each prospect's profile to maximize the impact of reference calls and case studies during the sales process. A well-matched reference accelerates trust-building and reduces perceived risk.

## When to Use

- When a prospect requests a reference call (typically late in the evaluation)
- When building a proposal and selecting case studies to include
- When a deal is stalled and a peer reference could provide momentum
- When preparing for a competitive bake-off where social proof matters
- Proactively during discovery to mention "a customer like you"

## Information Gathering

### Prospect Profile

Collect these attributes for matching:

| Attribute | Description | Priority |
|-----------|------------|----------|
| Therapeutic Area | Primary indication(s) under study | High |
| Study Phase | Phase I, II, III, IV, or observational | High |
| Company Type | Large pharma, mid-size biotech, CRO, academic | High |
| Company Size | Revenue range or employee count | Medium |
| Geography | HQ location and clinical operations regions | Medium |
| Previous EDC | Incumbent EDC vendor being replaced | Medium |
| Key Concern | Primary objection or hesitation to address | High |
| Use Case | Specific capability to validate (e.g., ePRO, integrations, RBM) | Medium |

### Reference Customer Database

Maintain a reference database with these fields for each reference customer:

```
- Customer Name
- Company Type (pharma, biotech, CRO, academic)
- Company Size (small/mid/large, employee count, revenue range)
- Headquarters / Regions
- Therapeutic Areas (list all)
- Study Phases Using Talosix
- Number of Studies on Talosix
- Number of Sites / Subjects
- Previous EDC Vendor (what they switched from)
- Go-Live Date (how long they've been a customer)
- Key Use Cases (standard EDC, ePRO, integrations, RBM, CDISC, etc.)
- Satisfaction Score (NPS or CSAT if available)
- Reference Contact Name and Role
- Reference Contact Willingness (enthusiastic / willing / reluctant)
- Last Used as Reference (date)
- Times Used as Reference (count, trailing 12 months)
- Topics Comfortable Discussing
- Topics to Avoid
- Notable Success Metrics (quantified outcomes)
```

## Matching Algorithm

### Scoring Criteria

Score each potential reference match on a 0-3 scale per attribute:

| Score | Meaning |
|-------|---------|
| 3 | Exact match (same therapeutic area, same phase, same company type) |
| 2 | Close match (related therapeutic area, adjacent phase, similar company type) |
| 1 | Partial relevance (different area but similar complexity, different but complementary experience) |
| 0 | No match on this dimension |

### Matching Dimensions and Weights

| Dimension | Weight | Rationale |
|-----------|--------|-----------|
| Therapeutic Area | 30% | Prospects trust peers in their disease area most |
| Company Type | 20% | Operational context must be similar |
| Study Phase | 15% | Complexity and regulatory context alignment |
| Previous EDC Vendor | 15% | Migration stories from the same incumbent are very powerful |
| Geography | 10% | Regional regulatory and operational similarity |
| Key Concern | 10% | Reference should directly address the prospect's primary objection |

### Match Score Calculation

```
Match Score = Sum of (dimension score x weight) for all dimensions
Maximum possible: 3.0
```

| Score Range | Match Quality |
|-------------|-------------|
| 2.5 - 3.0 | Excellent match - prioritize this reference |
| 2.0 - 2.4 | Strong match - good option |
| 1.5 - 1.9 | Moderate match - use if better options unavailable |
| Below 1.5 | Weak match - avoid unless no alternatives |

### Additional Filters

After scoring, apply these filters:

- **Willingness Gate** - Only include references rated "enthusiastic" or "willing"
- **Freshness Check** - Deprioritize if used as reference more than 3 times in the last 90 days
- **Recency Check** - Prioritize customers live on Talosix for 6+ months (enough experience to speak credibly)
- **Relationship Health** - Exclude any customer with open critical support tickets or known dissatisfaction

## Reference Talking Points

Generate talking points for each matched reference, tailored to what the prospect needs to hear:

### For the Sales Rep (Pre-Reference Call Briefing)

```
Reference: [Customer Name]
Contact: [Name, Title]
Match Reason: [Why this reference was selected]
Relationship Note: [Any context on the relationship, preferences]

Suggested Topics for Prospect to Ask:
1. [Topic aligned with prospect's key concern]
2. [Topic related to therapeutic area experience]
3. [Topic about migration from previous EDC]
4. [Topic about measurable outcomes]

Topics to Steer Toward:
- [Specific success metric the reference can share]
- [Feature the reference is enthusiastic about]

Topics to Avoid:
- [Any known issue or sensitive area]
- [Feature the reference hasn't used]
```

### For the Reference Customer (Pre-Call Prep)

Prepare a brief for the reference customer:

```
Thank you for agreeing to be a reference for Talosix.

Prospect Overview:
- Company: [Name], a [type] focused on [therapeutic area]
- They are evaluating Talosix for [use case]
- They are currently using [competitor] and are looking to [migrate/add/replace]

What They'll Likely Ask About:
1. Your experience with [specific feature or workflow]
2. How the migration from [previous EDC] went
3. Measurable outcomes you've seen
4. What you wish you'd known before starting

We Suggest Highlighting:
- [Specific success metric]
- [Specific positive experience]

Estimated Call Duration: 30 minutes
```

## Reference Usage Tracking

Track reference usage to prevent burnout and maintain reference willingness:

### Usage Limits

| Reference Willingness | Max Uses per Quarter | Cool-Down After Use |
|----------------------|---------------------|-------------------|
| Enthusiastic | 4 | 2 weeks |
| Willing | 2 | 4 weeks |
| Reluctant | 1 (only if critical) | 8 weeks |

### Tracking Fields

After each reference engagement, record:

- Date of reference call/visit
- Prospect company name
- Topics discussed
- Outcome (positive/neutral/negative impact on deal)
- Reference's feedback on the experience
- Any follow-up needed (thank you, gift, discount consideration)

### Reference Appreciation

Maintain reference willingness through:

- Thank-you communication after every reference engagement
- Quarterly appreciation (gift cards, conference invitations, feature previews)
- Annual reference program benefits (advisory board membership, early access)
- Reduced reference requests for customers showing fatigue

## Case Study Generation

When a reference engagement leads to a strong story, propose creating a formal case study:

**Case Study Structure:**
1. Customer profile (industry, size, therapeutic focus)
2. Challenge (what problem they faced with their previous approach)
3. Solution (how Talosix addressed the challenge)
4. Results (quantified outcomes: time saved, cost reduced, quality improved)
5. Quote from the reference contact

## Output Format

1. **Matched References List** - Ranked list of 3-5 references with match scores and rationale
2. **Top Recommendation** - Detailed briefing for the #1 match including talking points
3. **Reference Prep Brief** - Document to send to the reference customer before the call
4. **Usage Report** - Current utilization of each reference in the database, flagging overused or stale references
