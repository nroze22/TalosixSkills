---
name: competitive-intelligence-scraper
description: Gather and analyze competitive intelligence on EDC vendors in the clinical trial space. Monitors Medidata Rave, Veeva Vault EDC, Oracle Clinical One, Castor, IQVIA, Signant Health, and others. Produces structured intel reports with strategic implications for Talosix.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
argument-hint: "[competitor-name-or-all]"
---

# Competitive Intelligence Scraper

## Purpose

Gather, analyze, and synthesize competitive intelligence on EDC (Electronic Data Capture) vendors that compete with Talosix. Produce actionable intel reports that inform product strategy, sales positioning, and marketing messaging.

## Target Competitors

### Primary Competitors (direct EDC competition)

| Competitor | Parent/Owner | Key Product | Market Position |
|------------|-------------|-------------|-----------------|
| Medidata Rave | Dassault Systemes | Rave EDC / Rave Clinical Cloud | Market leader, enterprise pharma |
| Veeva Vault EDC | Veeva Systems | Vault EDC (part of Vault Clinical Suite) | Fast-growing, unified suite play |
| Oracle Clinical One | Oracle Health Sciences | Clinical One Platform | Enterprise, integrated with Oracle ecosystem |
| Castor EDC | Castor | Castor EDC | SMB/mid-market, ease of use |
| IQVIA | IQVIA | IQVIA Clinical Technologies (Orchestrated Clinical Trials) | Full-service CRO + tech |
| Signant Health | Signant Health | SmartSignals Platform | eCOA/ePRO focused, expanding to EDC |

### Secondary Competitors (adjacent/emerging)

| Competitor | Key Product | Notes |
|------------|-------------|-------|
| Climedo | Climedo EDC | EU-focused, hybrid trials |
| Datatrak | Datatrak ONE | Mid-market unified platform |
| ArisGlobal | LifeSphere EDC | Safety-focused company expanding |
| OpenClinica | OpenClinica | Open-source heritage, academic |
| REDCap | REDCap | Academic/research, free |

## Workflow

### Step 1: Determine Scope

Parse the input:
- If a specific competitor name is provided, run deep analysis on that competitor only.
- If "all" is specified, run a comprehensive sweep across all primary competitors.
- If no input, default to all primary competitors with summary-level analysis.

### Step 2: Intelligence Gathering

For each competitor in scope, conduct systematic research across these categories:

#### 2A: Recent News and Press Releases

Search queries:
- `"[competitor]" press release 2025 2026`
- `"[competitor]" announcement clinical trial EDC`
- `"[competitor]" news partnership collaboration`

Capture:
- Product launches and major updates
- Partnership and integration announcements
- Customer wins (new logos)
- Leadership changes
- Financial results (for public companies)
- Regulatory approvals or certifications

#### 2B: Product and Feature Intelligence

Search queries:
- `"[competitor]" new feature OR update OR release OR launch EDC`
- `"[competitor]" product roadmap OR innovation OR AI OR machine learning`
- `"[competitor]" decentralized trial OR DCT OR hybrid trial`
- `"[competitor]" integration OR API OR interoperability`

Capture:
- New features announced or released
- Technology direction (AI/ML, cloud migration, mobile)
- Integration capabilities added
- Decentralized trial support
- Regulatory compliance updates (21 CFR Part 11, EU Annex 11)
- User experience changes

#### 2C: Pricing and Packaging Intelligence

Search queries:
- `"[competitor]" pricing OR cost OR license model`
- `"[competitor]" subscription OR per-study OR per-site`
- `"[competitor]" G2 OR Gartner OR Forrester review pricing`

Capture:
- Pricing model changes
- Packaging/bundling strategies
- Free tier or trial offerings
- Competitive pricing moves

#### 2D: Customer Wins and Losses

Search queries:
- `"[competitor]" selected OR chosen OR implemented OR deployed`
- `"[competitor]" replaced OR migrated from OR switched from`
- `"[competitor]" case study clinical trial`
- `"[competitor]" customer success OR testimonial`

Capture:
- New customer announcements
- Customer churn signals (companies switching away)
- Case studies and success metrics
- Customer satisfaction indicators (G2, Capterra reviews)

#### 2E: Growth Signals (Job Postings)

Search queries:
- `"[competitor]" careers OR hiring OR jobs`
- `"[competitor]" site:linkedin.com jobs`
- `"[competitor]" engineering OR product OR sales hiring`

Analyze:
- Total open positions (proxy for growth/investment)
- Engineering vs. sales ratio (product-led vs. sales-led growth)
- Geographic expansion (new office locations)
- Key hires (product leaders, sales leaders from notable companies)
- Technology stack clues from engineering job descriptions

#### 2F: Conference and Thought Leadership

Search queries:
- `"[competitor]" DIA OR SCOPE OR SCDM OR ACDM 2025 2026 presentation`
- `"[competitor]" webinar OR white paper OR thought leadership`
- `"[competitor]" "electronic data capture" OR "clinical data management" conference`

Capture:
- Conference presentations and booth presence
- Webinar topics and frequency
- White papers and research reports
- Thought leadership positioning (what themes are they pushing)

#### 2G: Market Positioning and Messaging

Use WebFetch to visit competitor homepages and key product pages:
- Homepage headline and value proposition
- Product page messaging hierarchy
- Customer logos displayed
- Analyst recognition (Gartner, Forrester, IDC)
- Differentiators they claim

### Step 3: Strategic Analysis

Synthesize the raw intelligence into strategic insights:

#### Competitive Positioning Map

Assess each competitor on two axes and place them conceptually:

**Axis 1: Market Focus** (Enterprise <---> Mid-Market/SMB)
**Axis 2: Product Strategy** (Suite/Platform <---> Best-of-Breed EDC)

#### SWOT for Each Competitor

For each competitor, produce a concise SWOT:

| | Positive | Negative |
|---|----------|----------|
| **Internal** | Strengths | Weaknesses |
| **External** | Opportunities | Threats |

#### "So What" Analysis

For every piece of intelligence, answer: **What does this mean for Talosix?**

Categories:
- **Opportunity**: Competitor weakness or gap Talosix can exploit
- **Threat**: Competitor strength or move Talosix needs to respond to
- **Neutral**: Market intelligence to monitor but no immediate action needed

### Step 4: Battlecard Updates

For each competitor, generate or update battlecard-ready content:

**Quick Comparison**:
| Dimension | [Competitor] | Talosix |
|-----------|-------------|---------|
| Deployment | [Cloud/On-prem/Hybrid] | Cloud-native |
| Study build time | [Estimated] | [Talosix benchmark] |
| Pricing model | [Per-study/Per-user/etc.] | [Talosix model] |
| Target market | [Enterprise/Mid/SMB] | [Talosix target] |
| Key differentiator | [Their claim] | [Talosix counter] |

**Common Objections and Responses**:
- "Why not [Competitor]?" -- Structured response for sales team
- "[Competitor] is the industry standard" -- How to reframe
- "[Competitor] has more customers" -- Redirect to relevance and outcomes

**Win Themes Against [Competitor]**:
- When Talosix wins: [Scenarios and reasons]
- When Talosix loses: [Scenarios and reasons]
- Key proof points: [Evidence to cite in competitive deals]

### Step 5: Trend Analysis

Identify market-level trends emerging from competitive activity:

- **Technology trends**: What are multiple competitors investing in? (AI, DCT, patient-centricity)
- **Market consolidation**: Acquisitions, mergers, partnerships signaling consolidation
- **Pricing pressure**: Direction of pricing -- up, down, or restructuring
- **Regulatory drivers**: New regulations creating feature requirements across vendors
- **Buyer behavior shifts**: Changes in how customers evaluate and buy EDC

### Step 6: Output Format

#### Single Competitor Deep Dive

```markdown
# Competitive Intelligence Report: [Competitor Name]

**Date**: [Date]
**Analyst**: Talosix Competitive Intelligence
**Classification**: Internal Use Only

## Executive Summary
[3-5 sentences on the most important developments and implications]

## Company Overview
- **Parent**: [parent company]
- **Founded**: [year]
- **HQ**: [location]
- **Employees**: [count, with trend]
- **Revenue**: [if available]
- **Market Position**: [description]

## Recent Developments (Last 90 Days)
| Date | Development | Category | Talosix Implication |
|------|-------------|----------|---------------------|
| | | | |

## Product Intelligence
### Current Capabilities
[Assessment of current product strengths and weaknesses]

### Recent Updates
[New features, releases, announcements]

### Direction
[Where they appear to be heading based on signals]

## Market Activity
### Customer Wins
[New logos, expansion deals]

### Customer Losses
[Churn signals, replacements]

### Pricing Intelligence
[Any pricing data or changes]

## Growth Indicators
### Hiring Activity
[Job posting analysis]

### Geographic Expansion
[New markets or regions]

### Investment Areas
[Where they're putting resources]

## Competitive Positioning
### Their Claimed Differentiators
[What they say about themselves]

### Actual Strengths
[Our honest assessment]

### Vulnerabilities
[Where they are weak]

## Battlecard
### When We Win Against Them
[Scenarios and talking points]

### When We Lose Against Them
[Scenarios and lessons]

### Key Objection Handlers
[Common objections and responses]

## Recommendations for Talosix
1. [Specific action item with rationale]
2. [Specific action item with rationale]
3. [Specific action item with rationale]

## Sources
[Numbered list with URLs]
```

#### All-Competitor Sweep

```markdown
# Competitive Intelligence Sweep

**Date**: [Date]
**Coverage Period**: [Last 30/60/90 days]

## Market Summary
[Overview of the competitive landscape and key shifts]

## Competitor-by-Competitor Summary

### [Competitor 1]
**Status**: [Growing/Stable/Declining]
**Top Signal**: [Most important recent development]
**Talosix Action**: [What to do about it]

### [Competitor 2]
[Same format]

[...for each competitor...]

## Cross-Competitor Trends
[Patterns observed across multiple competitors]

## Opportunities for Talosix
[Specific gaps or openings in the market]

## Threats to Monitor
[Competitive moves that could impact Talosix]

## Recommended Actions
| Action | Priority | Owner | Deadline |
|--------|----------|-------|----------|
| | | | |
```

## Intelligence Quality Standards

- Distinguish between confirmed facts and inferences. Label inferences explicitly.
- Date-stamp all intelligence. Undated intelligence has limited value.
- Source every claim. Include URLs for verification.
- Avoid bias. Report competitor strengths honestly, not just weaknesses.
- Focus on actionable intelligence. Every section should answer "So what?" for Talosix.
- Check the local workspace for any existing competitive intel before starting, to build on prior work.
- Flag any intelligence that may be outdated (older than 6 months) or from low-reliability sources.
