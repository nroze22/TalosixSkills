---
name: sales-forecast
description: "Forecast Talosix pipeline and revenue with weighted pipeline analysis, historical conversion rates, and segmentation by deal type, customer type, and study phase."
---

# Sales Forecast & Pipeline Analytics

## Purpose

Generate accurate, data-driven sales forecasts for Talosix EDC revenue. Move beyond subjective sales rep predictions by applying historical conversion rates, deal health scoring, and segment-specific analysis to produce reliable pipeline and revenue projections.

## When to Use

- Weekly pipeline reviews and forecast updates
- Monthly and quarterly revenue forecasting
- Board reporting and financial planning
- Sales capacity planning and territory optimization
- Identifying pipeline gaps and coverage risks

## Information Gathering

### Required Data

1. **Active Pipeline** - All open opportunities with stage, value, close date, owner
2. **Historical Data** - Closed-won and closed-lost deals from the last 12-24 months
3. **Stage Definitions** - Current sales stages with entry/exit criteria
4. **Forecast Categories** - Current rep-submitted forecast commitments
5. **Current Quarter Targets** - Quota by rep, team, and total company
6. **Bookings to Date** - Closed-won in the current period

### Helpful Additional Data

- Average sales cycle length by segment
- Historical seasonality patterns
- Marketing pipeline generation trends
- Customer expansion/upsell pipeline
- Renewal pipeline and churn data

## Sales Stage Definitions for Clinical Trial EDC

Define clear stages with objective entry criteria specific to EDC sales:

| Stage | Entry Criteria | Typical Duration | Historical Win Rate |
|-------|---------------|-----------------|-------------------|
| **Lead/MQL** | Inbound interest or outbound response, not yet qualified | 1-2 weeks | 5-10% |
| **Qualification** | BANT confirmed (Budget, Authority, Need, Timeline) | 2-3 weeks | 10-15% |
| **Discovery** | Discovery call completed, pain points documented, stakeholders mapped | 2-4 weeks | 15-25% |
| **Demo/Evaluation** | Product demo delivered, evaluation criteria agreed | 3-6 weeks | 25-40% |
| **POC/Pilot** | Proof of concept or pilot study in progress | 4-8 weeks | 40-60% |
| **Proposal** | Formal proposal delivered with pricing | 2-4 weeks | 50-65% |
| **Negotiation** | Legal/commercial negotiation, MSA redlines in progress | 2-6 weeks | 65-80% |
| **Verbal Commit** | Verbal agreement, awaiting signature | 1-2 weeks | 85-95% |
| **Closed Won** | Contract fully executed | - | 100% |

Note: Win rates above are starting benchmarks. Replace with Talosix's actual historical data as it accumulates.

## Forecasting Methodologies

### Method 1: Weighted Pipeline

The foundational forecast method. Apply stage-based probabilities to deal values.

```
Weighted Value = Deal Amount x Stage Win Rate

Pipeline Coverage = Total Weighted Pipeline / Remaining Quota
Target Coverage Ratio: 3x-4x for early quarter, 1.5x-2x for late quarter
```

**Segment Adjustments:**

Apply multipliers to base stage probabilities based on deal characteristics:

| Segment | Multiplier | Rationale |
|---------|-----------|-----------|
| Large Pharma (>$5B revenue) | 0.8x | Longer cycles, more stakeholders, procurement complexity |
| Mid-Size Biotech | 1.0x | Baseline |
| Small Biotech (<50 employees) | 1.1x | Faster decisions but watch for funding risk |
| CRO - Enterprise | 0.85x | Multiple decision makers, platform standardization complexity |
| CRO - Mid-size | 1.0x | Baseline |
| Academic/Government | 0.7x | Grant funding cycles, procurement bureaucracy |
| New Logo | 0.9x | No existing relationship |
| Expansion (Existing Customer) | 1.2x | Existing trust and platform adoption |

### Method 2: Historical Conversion Analysis

Analyze historical stage-to-stage conversion rates and apply to current pipeline.

```
For each stage, calculate:
  Conversion Rate = Deals advancing to next stage / Total deals entering this stage
  Average Time in Stage = Mean days spent in stage for converted deals
  Stage Velocity = Deals advancing per month

Project current pipeline forward:
  Expected conversions from Stage N = Deals in Stage N x Conversion Rate(N to N+1)
  Apply successively through remaining stages to Closed Won
  Sum expected Closed Won values = Forecast
```

**Time-Based Decay:**
Deals that exceed the average time in a stage by more than 50% should have their conversion probability reduced:

| Time Over Average | Probability Adjustment |
|------------------|----------------------|
| 50-100% over | Reduce by 25% |
| 100-200% over | Reduce by 50% |
| 200%+ over | Reduce by 75% |

### Method 3: Rep Forecast with Bias Adjustment

Use rep-submitted forecasts but adjust for historical accuracy.

```
Adjusted Forecast = Rep Forecast x Historical Accuracy Ratio

Where:
  Historical Accuracy Ratio = Actual Closed / Forecasted (trailing 4 quarters)
```

| Rep Accuracy Pattern | Adjustment |
|---------------------|-----------|
| Consistently optimistic (forecasts 20%+ above actual) | Multiply by 0.75-0.85 |
| Reasonably accurate (within 10%) | Use as submitted |
| Consistently conservative (forecasts 10%+ below actual) | Multiply by 1.10-1.15 |

### Method 4: Composite Forecast

Combine methods for highest accuracy:

```
Composite Forecast = (Weighted Pipeline x 0.30) +
                     (Historical Conversion x 0.30) +
                     (Adjusted Rep Forecast x 0.40)
```

The rep forecast gets the highest weight because it incorporates qualitative signals that data alone cannot capture, but it's tempered by the data-driven methods.

## Segmented Analysis

### By Customer Type

| Customer Type | Pipeline Value | Weighted Value | Avg Deal Size | Avg Cycle (Days) | Win Rate |
|--------------|---------------|---------------|--------------|------------------|----------|
| Large Pharma | $X | $Y | $Z | N | X% |
| Mid Biotech | $X | $Y | $Z | N | X% |
| Small Biotech | $X | $Y | $Z | N | X% |
| CRO | $X | $Y | $Z | N | X% |
| Academic | $X | $Y | $Z | N | X% |

### By Study Phase

| Study Phase | Pipeline Value | Avg Deal Size | Notes |
|------------|---------------|--------------|-------|
| Phase I | $X | $Y | Smaller studies, faster decisions, lower ACV |
| Phase II | $X | $Y | Growing complexity, moderate ACV |
| Phase III | $X | $Y | Largest deals, longest cycles, highest ACV |
| Phase IV / PMS | $X | $Y | Often multi-year, moderate ACV |
| Registry / RWE | $X | $Y | Growing segment, variable sizing |

### By Deal Type

| Deal Type | Pipeline Value | Weighted Value | Win Rate |
|-----------|---------------|---------------|----------|
| New Logo | $X | $Y | X% |
| Expansion (new studies) | $X | $Y | X% |
| Upsell (new modules) | $X | $Y | X% |
| Renewal | $X | $Y | X% |

## Pipeline Health Metrics

### Coverage Analysis

```
Quota Remaining = Quarterly Target - Closed Won to Date
Pipeline Coverage = Total Weighted Pipeline / Quota Remaining

Coverage Thresholds:
  Green:  >= 2.5x coverage
  Yellow: 1.5x - 2.5x coverage
  Red:    < 1.5x coverage
```

### Pipeline Generation Velocity

```
Monthly Pipeline Generated = New pipeline created in the last 30 days
Required Monthly Generation = (Quarterly Target / Historical Win Rate) / 3

Velocity Health:
  Green:  Generation >= Required
  Yellow: Generation 75-100% of Required
  Red:    Generation < 75% of Required
```

### Stage Distribution Health

A healthy pipeline should have a roughly pyramidal shape:

```
Ideal Distribution (by deal count):
  Qualification:      30-35% of deals
  Discovery:          20-25% of deals
  Demo/Evaluation:    15-20% of deals
  POC/Pilot:          10-15% of deals
  Proposal+:          10-15% of deals
```

Flag imbalances:
- **Top-heavy** (too many early-stage) - Conversion problem, need sales enablement
- **Bottom-heavy** (too many late-stage) - Pipeline generation problem, need more marketing/outbound
- **Bulge in middle** (stuck in evaluation) - Demo/POC effectiveness issue, need sales engineering support

### Pipeline Aging

```
Average Pipeline Age = Mean days since opportunity creation for all open deals
Aged Deals = Deals open > 2x average sales cycle for their segment

Action: Review all aged deals for close-lost candidacy or re-engagement strategy
```

## Forecast Accuracy Tracking

Track forecast accuracy over time to improve the model:

```
Forecast Accuracy = 1 - |Actual - Forecast| / Actual

Track by:
  - Overall accuracy (quarterly)
  - Accuracy by rep
  - Accuracy by segment
  - Accuracy by method (weighted, conversion, rep, composite)
```

| Accuracy Level | Rating | Action |
|---------------|--------|--------|
| > 90% | Excellent | Maintain current approach |
| 80-90% | Good | Minor tuning, review outlier deals |
| 70-80% | Fair | Recalibrate conversion rates, review stage criteria |
| < 70% | Poor | Fundamental review of methodology and data quality |

## Seasonal Patterns in Clinical Trial EDC

Account for industry-specific seasonality:

- **Q1** - Budget flush from new fiscal year, new program starts. Often strong for new deals.
- **Q2** - Conference season (ASCO, DIA, ACDM). Pipeline generation peaks. Evaluations active.
- **Q3** - Summer slowdowns in Europe. Decision delays. Push deals to close before summer.
- **Q4** - Year-end budget deadlines drive urgency. Strongest close quarter for many. But also deal push risk if budget is depleted.

Adjust quarterly forecasts based on observed seasonal patterns in Talosix's historical data.

## Output Format

1. **Forecast Summary** - One-page view with current quarter forecast (commit, best case, upside), pipeline coverage, and key risks
2. **Deal-Level Detail** - Table of all deals with stage, value, weighted value, close date, health score, and forecast category
3. **Segment Analysis** - Breakdown by customer type, study phase, deal type, and geography
4. **Pipeline Health Dashboard** - Coverage ratio, velocity, stage distribution, aging analysis
5. **Trend Charts** - Pipeline created vs. closed over time, forecast accuracy trend, stage conversion trends
6. **Risk Callouts** - Top 5 deals with highest risk of slipping and recommended actions
7. **Gap Analysis** - Shortfall between forecast and target with recommendations for closing the gap
