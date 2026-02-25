---
name: product-brief
description: "Create a product brief with market research, user needs analysis, competitive landscape, and problem definition that feeds into PRD creation. The first step in the product lifecycle before writing a full PRD."
argument-hint: "[feature-idea-or-problem-statement]"
---

# Product Brief

Generate a structured product brief that captures the initial vision, research, and justification for a new feature or product initiative. This is the pre-PRD artifact that aligns stakeholders on WHAT we're solving and WHY before investing in detailed requirements.

## When to Use

- New feature idea needs validation and structure before PRD
- Stakeholder wants a concise overview before committing resources
- Need to research market/competitive landscape for an initiative
- Translating customer feedback or support trends into a product opportunity

## Workflow

### Step 1: Problem Discovery

Gather context about the problem:
- What triggered this idea? (customer request, support trend, competitive pressure, internal observation)
- Who is affected? (which user roles: CRAs, Data Managers, Site Coordinators, Sponsors)
- How are users solving this today? (workarounds, manual processes, competitor tools)
- What is the cost of NOT solving this? (time lost, errors, compliance risk, churn risk)

### Step 2: Research

If WebSearch is available, research:
- How competitors handle this (Medidata, Veeva, Oracle, Castor)
- Industry trends and standards (CDISC, ICH guidelines, FDA guidance)
- Market sizing and demand signals
- Published best practices or white papers

Pull existing context from Confluence via Atlassian MCP if available:
- Prior discussions or decisions on this topic
- Related customer feedback or support tickets
- Existing technical documentation

### Step 3: Generate Product Brief

Structure the brief as follows:

```
# Product Brief: [Feature/Initiative Name]

## Status: Draft | Under Review | Approved | Declined
## Author: [Name]
## Date: [Date]
## Stakeholders: [List]

---

## 1. Problem Statement
One paragraph. What problem are we solving and for whom?

## 2. Background & Context
- What triggered this initiative
- Current state and pain points
- Customer evidence (quotes, ticket counts, feedback themes)
- Market/industry context

## 3. Target Users
| User Role | Need | Current Workaround | Pain Level (1-5) |
|-----------|------|--------------------|-------------------|
| [Role]    | ...  | ...                | ...               |

## 4. Competitive Landscape
| Competitor | Has This? | Their Approach | Gap/Opportunity |
|------------|-----------|----------------|-----------------|
| Medidata   | ...       | ...            | ...             |
| Veeva      | ...       | ...            | ...             |
| Oracle     | ...       | ...            | ...             |

## 5. Proposed Solution (High Level)
2-3 paragraphs describing the envisioned solution at a conceptual level.
NOT detailed requirements - save that for the PRD.

## 6. Success Metrics
| Metric | Current State | Target | How Measured |
|--------|--------------|--------|--------------|
| ...    | ...          | ...    | ...          |

## 7. Regulatory Considerations
- 21 CFR Part 11 impact (audit trails, e-signatures, data integrity)
- Validation requirements (new IQ/OQ/PQ needed?)
- HIPAA/GDPR implications

## 8. Rough Sizing
- Estimated effort: [T-shirt size: S/M/L/XL]
- Key dependencies
- Risks and unknowns

## 9. Recommendation
Go / No-Go / Needs More Research
Rationale for recommendation.

## 10. Next Steps
If approved:
- [ ] Assign PM for full PRD (use /product-prd)
- [ ] Schedule stakeholder alignment (use /stakeholder-alignment)
- [ ] Begin technical feasibility assessment
```

### Step 4: Offer Next Actions

After generating the brief, offer:
- "Shall I create a full PRD from this brief? (use /product-prd)"
- "Want me to create a business case with ROI? (use /business-case)"
- "Should I do a deeper competitive analysis? (use /competitor-analysis)"
