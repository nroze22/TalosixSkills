---
name: ROI Calculator Builder
description: Build custom ROI calculators for Talosix EDC prospects, factoring in study setup time, data query reduction, monitoring efficiency, and industry benchmarks to quantify the business case.
---

# ROI Calculator Builder

## Purpose

Create prospect-specific ROI analyses that quantify the financial and operational benefits of adopting Talosix EDC. The calculator should use a combination of the prospect's own data (when available) and validated industry benchmarks to build a credible, defensible business case.

## When to Use

- During or after discovery to quantify pain points financially
- As part of a formal proposal to justify investment
- When a deal is stalled and needs a business case to move forward
- For executive-level conversations where financial justification is required
- At renewal to demonstrate realized value

## Information Gathering

### Prospect-Specific Inputs (Preferred)

Request these from the prospect when possible:

| Input | Description | If Unknown, Use Benchmark |
|-------|-------------|--------------------------|
| Number of active studies | Concurrent studies in the portfolio | Varies by company type |
| Average sites per study | Sites across all active studies | 50-150 (Phase III) |
| Average subjects per study | Enrolled subjects per study | 200-500 (Phase III) |
| Average eCRF pages per subject | Data volume indicator | 80-150 pages |
| Average study build time | Weeks from protocol final to go-live | 10-16 weeks |
| Current query rate | Queries per 1000 eCRF fields | 8-15% |
| Average query resolution time | Days from query open to close | 5-14 days |
| SDV percentage | Source data verification rate | 80-100% |
| Cost per monitoring visit | Loaded cost per on-site visit | $1,500-$3,000 |
| Annual EDC spend | Current EDC licensing + services | Varies |
| Time from LPLV to DB lock | Weeks after last patient last visit | 4-8 weeks |
| FTE data management cost | Fully loaded annual cost per DM | $85,000-$130,000 |
| Study amendments per study | Average protocol amendments | 2-3 per study |
| Time to implement amendment | Weeks to deploy EDC amendment | 2-6 weeks |

### Industry Benchmarks

Use these peer-reviewed and industry-sourced benchmarks as defaults:

**Study Setup:**
- Average EDC study build time: 12 weeks (Tufts CSDD)
- Cost of one week delay in study start: $37,000-$600,000 depending on therapeutic area and phase (Tufts CSDD)
- Average cost to build and validate an EDC study: $50,000-$200,000

**Data Quality:**
- Industry average query rate: 10-15 queries per 100 eCRF pages (SCDM benchmarks)
- Cost per query lifecycle (open, send, resolve, close): $50-$75 (industry estimates)
- Percentage of queries that are avoidable with better edit checks: 30-50%

**Monitoring:**
- Average monitoring visit cost (on-site): $2,000-$3,000 including travel
- Average monitoring visits per site per year: 6-10
- RBM can reduce on-site visits by 25-50% (TransCelerate benchmarks)

**Database Lock:**
- Average time from LPLV to DB lock: 6-8 weeks (traditional), 2-4 weeks (modern EDC)
- Cost per day of delayed NDA/BLA submission: varies by indication, can exceed $1M/day for blockbuster drugs
- Revenue at risk per month of delayed approval: drug-specific

## ROI Model Components

### 1. Study Setup Acceleration

```
Current setup time: [X] weeks
Talosix projected setup time: [Y] weeks (typically 40-60% reduction)
Time saved: [X - Y] weeks per study
Studies per year: [N]

Annual time saved: (X - Y) x N weeks
Financial impact: (X - Y) x N x [cost per week of delay]
```

**Value drivers:**
- Form library with reusable components reduces design time
- No-code configuration eliminates programming bottleneck
- Integrated validation reduces separate testing cycles
- Template-based study setup for similar protocols

### 2. Data Query Reduction

```
Current query rate: [X] per 100 eCRF pages
Talosix projected query rate: [Y] per 100 eCRF pages (typically 30-50% reduction)
Total eCRF pages per study: [subjects] x [pages per subject]
Queries avoided per study: (X - Y) / 100 x [total pages]

Cost per query: $[50-75]
Annual savings: Queries avoided x N studies x cost per query

FTE impact: Queries avoided x [time per query] / [annual work hours] = FTEs redeployed
```

**Value drivers:**
- Real-time edit checks catch errors at the point of entry
- Conditional logic prevents impossible data combinations
- Smart defaults reduce transcription errors
- Better site UX reduces training-related entry errors

### 3. Monitoring Efficiency

```
Current monitoring visits per site per year: [X]
Sites across all studies: [N]
Total annual monitoring visits: X x N
Cost per visit: $[2,000-3,000]
Current annual monitoring cost: X x N x cost per visit

With Talosix RBM support:
Projected visit reduction: [25-40%]
Visits eliminated: X x N x reduction percentage
Annual savings: Visits eliminated x cost per visit
```

**Value drivers:**
- Real-time data quality dashboards enable central monitoring
- KRI tracking identifies sites needing attention vs. routine visits
- Remote data review reduces need for on-site SDV
- Automated compliance checks flag issues before monitor visits

### 4. Database Lock Acceleration

```
Current LPLV-to-lock time: [X] weeks
Talosix projected time: [Y] weeks (typically 30-50% reduction)
Time saved per study: (X - Y) weeks

For submission-critical studies:
Revenue impact per week of earlier approval: $[estimate based on drug revenue projections]
Probability-adjusted value: Revenue per week x (X - Y) x probability of approval
```

**Value drivers:**
- Continuous data cleaning during the study (not batch at end)
- Automated CDISC mapping reduces post-lock data transformation
- Real-time data quality metrics enable rolling database lock
- Fewer outstanding queries at LPLV

### 5. Amendment Agility

```
Amendments per study per year: [X]
Current time to implement amendment in EDC: [Y] weeks
Talosix projected implementation time: [Z] weeks
Time saved per amendment: (Y - Z) weeks

Annual time saved: X x N studies x (Y - Z) weeks
Impact: Reduced enrollment delays, faster protocol compliance
Financial value: Time saved x [cost per week of enrollment delay]
```

### 6. Operational Efficiency (FTE Optimization)

```
Current DM FTEs dedicated to EDC-related tasks: [X]
Tasks automated or reduced by Talosix:
  - Query management: [X%] time reduction
  - Report generation: [X%] time reduction
  - Study build: [X%] time reduction
  - Amendment implementation: [X%] time reduction

FTE time recaptured: Sum of reductions
Financial value: FTEs recaptured x loaded cost per FTE
```

## ROI Summary Table

| Category | Current Annual Cost | Projected w/ Talosix | Annual Savings |
|----------|-------------------|---------------------|---------------|
| Study Setup | $X | $Y | $Z |
| Data Queries | $X | $Y | $Z |
| Monitoring | $X | $Y | $Z |
| Database Lock | $X | $Y | $Z |
| Amendments | $X | $Y | $Z |
| FTE Optimization | $X | $Y | $Z |
| **Total Operational Savings** | | | **$Z** |
| **Talosix Investment** | | | **($A)** |
| **Net Annual Benefit** | | | **$B** |
| **ROI** | | | **X%** |
| **Payback Period** | | | **X months** |

```
ROI = (Net Annual Benefit / Total Talosix Investment) x 100
Payback Period = Total Talosix Investment / (Net Annual Benefit / 12)
```

## Sensitivity Analysis

Present three scenarios to account for uncertainty:

| Scenario | Assumptions | ROI | Payback |
|----------|-----------|-----|---------|
| Conservative | Low-end improvements (25th percentile) | X% | X months |
| Expected | Mid-range improvements (50th percentile) | X% | X months |
| Optimistic | High-end improvements (75th percentile) | X% | X months |

## Credibility Guidelines

- **Cite sources** for all industry benchmarks (Tufts CSDD, SCDM, TransCelerate)
- **Use prospect's own data** whenever available; benchmarks are fallbacks
- **Be conservative** in projections; under-promise and over-deliver
- **Exclude soft benefits** from the primary ROI calculation (improved site satisfaction, reduced audit risk) but mention them qualitatively
- **Show your math** -- every number should be traceable to an input assumption
- **Acknowledge limitations** -- state that actual results depend on implementation quality and adoption

## Output Format

1. **Executive ROI Summary** - One-page visual summary with headline ROI, payback period, and top 3 value drivers
2. **Detailed Model** - Full calculation workbook with all inputs, formulas, and outputs
3. **Assumptions Document** - Every input, its source (prospect-provided or benchmark), and confidence level
4. **Sensitivity Analysis** - Three-scenario view showing ROI range
5. **Presentation Slides** - 3-5 slides suitable for executive review
