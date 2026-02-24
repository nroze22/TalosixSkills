---
name: abm-account-research
description: Deep Account-Based Marketing research for Talosix target accounts. Compiles comprehensive account intelligence including clinical trial pipeline, technology landscape, organizational structure, stakeholder mapping, and generates account-specific messaging pillars.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
argument-hint: "[company-name]"
---

# ABM Account Research

## Purpose

Produce a comprehensive Account-Based Marketing intelligence package for a target account. This goes deeper than prospect research -- it maps the entire account landscape to support a coordinated, multi-stakeholder sales campaign. The output enables sales, marketing, and executive leadership to engage the account with precision.

## Workflow

### Step 1: Account Identification and Scoping

Parse the input company name. Determine:

- Legal entity name vs. common/trade name
- Parent company vs. subsidiary structure (important for large pharma -- e.g., Roche vs. Genentech)
- Geographic scope (global HQ, regional operations)
- Business units relevant to clinical trials (some conglomerates have non-clinical divisions)

Use WebSearch with queries like:
- `"[company name]" about overview clinical trials`
- `"[company name]" subsidiaries divisions business units`
- `"[company name]" annual report 2025 2026`

### Step 2: Company Deep Dive

Compile a thorough company profile:

**Corporate Overview**:
- Founded, HQ location, employee count
- Public/private, market cap (if public), recent financial performance
- Mission, therapeutic focus areas, strategic priorities
- Recent M&A activity, partnerships, licensing deals

**Business Model**:
- Revenue model (CRO services, drug development, contract manufacturing)
- Customer base (who are THEY selling to, if a CRO)
- Geographic footprint and expansion trajectory
- Growth rate and trajectory

Use WebSearch and WebFetch to pull from:
- Company website (About, Investors, News pages)
- SEC filings or annual reports (for public companies)
- Industry databases and rankings
- Recent news articles and press releases

### Step 3: Clinical Trial Pipeline Analysis

This is the most critical section for Talosix positioning. Conduct thorough trial research:

**Search Strategy**:
- WebSearch: `"[company name]" site:clinicaltrials.gov`
- WebSearch: `"[company name]" clinical trial pipeline 2025 2026`
- WebSearch: `"[company name]" phase III OR "phase 3" trial`
- WebFetch: Pull ClinicalTrials.gov results pages for detailed trial data

**Compile**:

| Metric | Data |
|--------|------|
| Total active trials | [count] |
| Phase I | [count] |
| Phase II | [count] |
| Phase III | [count] |
| Phase IV / Post-market | [count] |
| Therapeutic areas | [list with counts] |
| Geographic distribution | [countries/regions] |
| Trial complexity indicators | [adaptive, decentralized, rare disease, pediatric] |
| Recently registered (last 6 months) | [count and key trials] |
| Estimated annual new trial starts | [count] |

**Trial Complexity Assessment**:
- Average sites per trial
- Adaptive or platform trial designs
- Decentralized/hybrid elements (ePRO, remote monitoring, direct-to-patient)
- Rare disease or pediatric populations (complex regulatory requirements)
- Global trials with regional regulatory requirements
- Biomarker-driven or precision medicine trials (complex data capture)

### Step 4: Technology Landscape

Map the account's clinical technology ecosystem:

**Current EDC / CDMS**:
- Search for: `"[company name]" Medidata OR Rave OR Veeva OR "Oracle Clinical" OR Castor OR IQVIA OR Signant OR EDC`
- Check job postings for required system experience
- Look for vendor press releases mentioning the company as a customer
- Check conference presentations where they may reference their systems

**Adjacent Systems** (map the ecosystem Talosix would integrate with):
- CTMS (Clinical Trial Management System)
- eTMF (Electronic Trial Master File)
- RTSM/IRT (Randomization and Trial Supply Management)
- Safety / Pharmacovigilance database
- ePRO/eCOA (Electronic Patient-Reported Outcomes)
- LIMS (Laboratory Information Management)
- Data analytics / visualization tools
- Regulatory submission tools (eCTD)

**Technology Maturity Assessment**:
- Cloud adoption level (cloud-native, cloud-hosted, on-premise)
- Integration maturity (point-to-point, middleware, API-driven)
- Data strategy (centralized data lake, federated, siloed)
- Innovation indicators (AI/ML adoption, RPA, advanced analytics)

**Vendor Contract Intelligence**:
- Estimated contract start dates (from press releases, implementation announcements)
- Typical renewal cycles (3-5 years for enterprise EDC)
- Estimated renewal window
- Multi-vendor vs. single-vendor strategy

### Step 5: Organizational Structure and Stakeholder Map

Build a comprehensive stakeholder map organized by influence and function:

**Stakeholder Tiers**:

**Tier 1 -- Decision Makers** (budget authority, final sign-off):
| Name | Title | Tenure | Background | Priorities | Approach |
|------|-------|--------|------------|------------|----------|
| | VP/SVP Clinical Operations | | | | |
| | VP/SVP Clinical Data Management | | | | |
| | CTO / VP IT | | | | |

**Tier 2 -- Influencers** (shape requirements, evaluate vendors):
| Name | Title | Tenure | Background | Priorities | Approach |
|------|-------|--------|------------|------------|----------|
| | Director Data Management | | | | |
| | Director Clinical Systems | | | | |
| | Head of Quality/Compliance | | | | |

**Tier 3 -- Champions** (daily users, internal advocates):
| Name | Title | Tenure | Background | Priorities | Approach |
|------|-------|--------|------------|------------|----------|
| | Clinical Data Managers | | | | |
| | Biostatisticians | | | | |
| | Clinical Research Associates | | | | |

**Tier 4 -- Blockers / Gatekeepers**:
| Name | Title | Tenure | Background | Priorities | Approach |
|------|-------|--------|------------|------------|----------|
| | Procurement / Vendor Management | | | | |
| | IT Security | | | | |
| | Legal | | | | |

For each stakeholder, use WebSearch to find:
- LinkedIn profile information
- Published content (articles, presentations, interviews)
- Conference appearances
- Professional background and previous companies
- Known technology preferences or vendor relationships

### Step 6: Competitive Vendor Analysis (Account-Specific)

Determine which vendors currently serve this account and assess the competitive landscape:

- **Incumbent EDC vendor**: Identify with confidence level (Confirmed/Likely/Unknown)
- **Relationship health**: Look for public praise, complaints, case studies, or neutral mentions
- **Other clinical technology vendors**: Map the full vendor ecosystem
- **Vendor consolidation trends**: Is the account consolidating vendors or diversifying?
- **Budget environment**: Expanding tech spend or cost-cutting?

### Step 7: Pain Points and Opportunities

Synthesize research into specific, evidence-based pain points:

**Operational Pain Points**:
- Study build timelines (are they publicly discussing speed-to-database-lock?)
- Data quality metrics (any regulatory findings, audit observations?)
- Site burden complaints (from investigator surveys, conference talks)
- Scaling challenges (rapid trial portfolio growth straining current systems)

**Strategic Pain Points**:
- Digital transformation initiatives (mentioned in annual reports, press releases)
- Regulatory compliance gaps (warning letters, 483 observations)
- Competitive pressure (losing trials to faster competitors)
- Cost reduction mandates (public statements about operational efficiency)

**Technology Pain Points**:
- Integration challenges (manual data transfers, reconciliation issues)
- User adoption issues (high training burden, workflow friction)
- Scalability limitations (system performance, multi-study management)
- Innovation gaps (inability to support decentralized trials, advanced analytics)

For each pain point, provide:
- Evidence (source and link)
- Severity estimate (Critical / High / Medium / Low)
- Talosix relevance (how specifically Talosix addresses this)

### Step 8: Account-Specific Messaging Pillars

Based on the research, define 3-4 messaging pillars tailored to this account:

Each pillar should include:
- **Pillar name**: A concise theme (e.g., "Accelerating Database Lock")
- **Account evidence**: Why this resonates for THIS account specifically
- **Key message**: One sentence that captures the value proposition
- **Supporting proof points**: 2-3 data points, case studies, or capabilities that substantiate the message
- **Persona relevance**: Which stakeholders this pillar resonates with most

### Step 9: Engagement Strategy

Provide a recommended engagement plan:

**Entry Strategy**:
- Recommended first contact and why
- Opening angle (which pain point or opportunity to lead with)
- Channel preference (email, LinkedIn, event, mutual introduction)

**Multi-Threading Plan**:
- Sequence for engaging additional stakeholders
- How to create internal momentum (e.g., arm a champion with content)
- Executive-to-executive engagement opportunities

**Content and Event Plays**:
- Relevant upcoming conferences or events where engagement could happen
- Content assets to share with each persona
- Custom content to create for this account (one-pager, ROI analysis, technical brief)

**Timeline**:
- Week 1-2: Initial outreach and research validation
- Week 3-4: Multi-stakeholder engagement
- Month 2: Value demonstration (pilot discussion, technical deep dive)
- Month 3: Proposal and evaluation support

### Step 10: Output Format

Deliver the complete ABM package:

```markdown
# ABM Intelligence Package: [Company Name]

**Prepared**: [Date]
**Account Tier**: [1/2/3 based on ICP fit and opportunity size]
**Estimated Deal Size**: [Range based on trial volume and scope]
**Engagement Stage**: [New/Aware/Engaged/Evaluating/Negotiating]

## Executive Summary
[3-5 sentences capturing the opportunity, key signals, and recommended approach]

## Company Profile
[Comprehensive overview]

## Clinical Trial Pipeline
[Detailed analysis with tables]

## Technology Landscape
[Current stack, integration map, vendor relationships]

## Stakeholder Map
[Tiered stakeholder tables with approach recommendations]

## Competitive Position
[Incumbent analysis, relationship health, switching triggers]

## Pain Points & Opportunities
[Evidence-based analysis with severity and Talosix relevance]

## Messaging Pillars
[3-4 tailored pillars with evidence and persona mapping]

## Engagement Strategy
[Entry strategy, multi-threading plan, timeline]

## Open Questions
[What we still need to learn, and how to learn it]

## Sources
[Numbered list of all sources consulted with URLs]
```

## Research Quality Standards

- Every factual claim must have a source. Unsourced claims must be flagged as "[Estimated]" or "[Inferred]".
- Distinguish between confirmed information and educated inferences.
- Prioritize recent information (last 12 months). Flag anything older than 2 years.
- If critical information is unavailable, explicitly state the gap and suggest how to fill it.
- Cross-reference multiple sources for key facts (especially current EDC vendor and trial counts).
- Check for any existing research in the local workspace before starting fresh to build on prior work.
