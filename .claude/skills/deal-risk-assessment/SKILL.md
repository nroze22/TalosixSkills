---
name: Deal Risk Assessment
description: Assess deal health and risk factors for Talosix EDC opportunities, score deals across multiple dimensions, identify red flags, and recommend actions to improve pipeline forecast accuracy.
---

# Deal Risk Assessment

## Purpose

Systematically evaluate the health of active deals in the Talosix sales pipeline. Identify risks early, recommend corrective actions, and improve forecast accuracy by replacing gut-feel assessments with structured analysis grounded in clinical trial EDC sales patterns.

## When to Use

- Weekly pipeline review and deal inspection
- Before updating forecast commitments
- When a deal shows signs of stalling or going silent
- Before requesting executive involvement or resources
- Quarterly pipeline health assessments

## Information Gathering

For each deal being assessed, collect:

1. **Opportunity Details** - Deal name, prospect company, deal value, stage, close date
2. **Timeline** - Days in current stage, total days in pipeline, original vs. current close date
3. **Stakeholders** - Identified contacts, their roles, engagement level
4. **Activities** - Recent meetings, emails, demos, evaluations
5. **Competitive Landscape** - Known competitors, prospect's incumbent vendor
6. **Next Steps** - Agreed-upon next action and date
7. **Champion Status** - Who is the internal champion, how active are they
8. **Technical Evaluation** - Status of any POC, pilot, or technical review
9. **Procurement** - Status of legal/procurement engagement, MSA review

## Scoring Framework

Score each deal across these dimensions on a 1-5 scale (1 = high risk, 5 = low risk):

### 1. Champion Strength (Weight: 25%)

| Score | Criteria |
|-------|----------|
| 5 | Active champion with decision authority, personally invested in Talosix winning |
| 4 | Strong advocate with influence, regularly provides internal intel |
| 3 | Supportive contact but limited influence or unclear decision authority |
| 2 | Contact is engaged but not actively championing internally |
| 1 | No identified champion, or champion has left/changed roles |

### 2. Decision Process Clarity (Weight: 20%)

| Score | Criteria |
|-------|----------|
| 5 | Full buying process mapped: decision makers, criteria, timeline, budget confirmed |
| 4 | Most elements known, one or two gaps |
| 3 | General understanding but key elements (budget, final approver) unclear |
| 2 | Vague process, prospect has not shared specifics |
| 1 | No visibility into decision process, single-threaded relationship |

### 3. Compelling Event (Weight: 20%)

| Score | Criteria |
|-------|----------|
| 5 | Hard deadline (study start date, contract expiry, regulatory requirement) driving urgency |
| 4 | Strong business driver with general timeline pressure |
| 3 | Acknowledged need but no specific deadline or consequence of delay |
| 2 | "Nice to have" evaluation, no urgency |
| 1 | No identified reason to change or act now |

### 4. Technical Fit (Weight: 15%)

| Score | Criteria |
|-------|----------|
| 5 | Requirements fully met, successful POC/pilot, technical team supportive |
| 4 | Strong fit with minor gaps easily addressed (configuration, roadmap items) |
| 3 | Good fit with some gaps requiring workarounds or custom development |
| 2 | Significant gaps, prospect has concerns about technical capability |
| 1 | Major requirement gaps, technical team has raised blockers |

### 5. Competitive Position (Weight: 10%)

| Score | Criteria |
|-------|----------|
| 5 | Sole source or strongly preferred, competitor eliminated |
| 4 | Leading position, clear advantages acknowledged by prospect |
| 3 | Competitive, no clear leader, evaluation ongoing |
| 2 | Behind a competitor, prospect leaning elsewhere |
| 1 | Incumbent advantage is strong, Talosix is the underdog with no clear differentiator |

### 6. Engagement Recency (Weight: 10%)

| Score | Criteria |
|-------|----------|
| 5 | Active engagement in last 7 days, next meeting scheduled |
| 4 | Contact within last 14 days, positive communication |
| 3 | Last meaningful contact 15-30 days ago |
| 2 | 30-60 days since meaningful engagement despite outreach |
| 1 | 60+ days of silence, emails/calls unanswered |

## Overall Score Calculation

```
Deal Health Score = (Champion x 0.25) + (Decision Process x 0.20) +
                   (Compelling Event x 0.20) + (Technical Fit x 0.15) +
                   (Competitive Position x 0.10) + (Engagement x 0.10)
```

### Score Interpretation

| Score Range | Rating | Forecast Category |
|-------------|--------|------------------|
| 4.5 - 5.0 | Strong | Commit |
| 3.5 - 4.4 | Healthy | Best Case |
| 2.5 - 3.4 | At Risk | Pipeline |
| 1.5 - 2.4 | Troubled | Upside Only |
| 1.0 - 1.4 | Critical | Omit from Forecast |

## Red Flags

Automatically flag deals exhibiting any of these patterns:

### Immediate Action Required

- **Ghost Champion** - Champion has not responded in 14+ days
- **Close Date Slippage** - Close date pushed more than twice
- **Single-Threaded** - Only one contact engaged, no multi-stakeholder access
- **No Next Step** - No agreed-upon next action with a date
- **Procurement Stall** - Legal/procurement engaged but no progress in 21+ days

### Warning Signs

- **Expanding Evaluation** - Prospect adds new competitors late in the process
- **Stakeholder Turnover** - Key contact leaves or changes roles during evaluation
- **Scope Creep** - Requirements expanding without corresponding budget discussion
- **Delayed POC** - Technical evaluation keeps getting postponed
- **Budget Uncertainty** - No budget confirmed or "still working on budget" after discovery
- **Committee Decision** - Decision shifted from individual to committee
- **Unusual Silence After Demo** - No follow-up questions after a demo (often means they're not serious or chose a competitor)

### Clinical Trial EDC-Specific Red Flags

- **Study Already Started** - Prospect's study has already begun enrollment with another EDC (switching mid-study is rare)
- **CRO Mandate** - Prospect's CRO has a preferred/mandated EDC vendor
- **Enterprise Agreement** - Prospect has an enterprise license with incumbent that covers EDC
- **Regulatory Filing Dependency** - Prospect is mid-submission and reluctant to change systems
- **Validation Concerns** - IT/QA team has raised CSV burden as a significant concern

## Recommended Actions by Risk Level

### Strong (4.5-5.0)
- Maintain momentum, don't over-manage
- Prepare for contracting and implementation planning
- Ask champion to confirm timeline with procurement
- Schedule executive alignment meeting

### Healthy (3.5-4.4)
- Address the lowest-scoring dimension specifically
- Expand stakeholder engagement to reduce single-thread risk
- Confirm compelling event and quantify cost of inaction
- Request access to procurement/legal to start MSA review

### At Risk (2.5-3.4)
- Conduct an honest assessment of whether this deal is real
- Develop a champion or find a new one
- Create urgency through a time-bound offer or reference visit
- Involve sales leadership for strategy session
- Consider requesting an executive-to-executive meeting

### Troubled (1.5-2.4)
- Escalate to sales leadership for go/no-go decision
- Reduce time investment unless a clear path to recovery exists
- Attempt a "pattern interrupt" (new stakeholder, new angle, executive outreach)
- Move to nurture if no improvement within 2 weeks

### Critical (1.0-1.4)
- Remove from active forecast immediately
- Move to long-term nurture or close-lost
- Document lessons learned for similar future opportunities
- Redirect resources to healthier deals

## Pipeline Forecast Methodology

### Weighted Pipeline Calculation

```
Weighted Value = Deal Value x Stage Probability x (Health Score / 5)
```

Stage probabilities for clinical trial EDC sales:

| Stage | Base Probability |
|-------|-----------------|
| Qualification | 10% |
| Discovery Complete | 20% |
| Demo/Evaluation | 35% |
| POC/Pilot | 50% |
| Proposal Delivered | 60% |
| Negotiation | 75% |
| Verbal Commit | 90% |
| Closed Won | 100% |

Adjust base probability up or down by the health score ratio to get a more realistic weighted pipeline.

## Output Format

Deliver deal risk assessments as:

1. **Individual Deal Scorecard** - One-page scoring with dimension breakdown, red flags, and recommended actions
2. **Pipeline Risk Summary** - Table of all active deals with scores, ratings, and top risk per deal
3. **Forecast Adjustment Recommendations** - Which deals to move between forecast categories based on assessment
4. **Action Plan** - Prioritized list of actions across the pipeline, sorted by deal value and urgency
