---
name: linkedin-prospect-research
description: Research and analyze LinkedIn profiles and companies for Talosix sales targeting. Builds comprehensive prospect dossiers with ICP scoring, buying signals, and personalized talking points for clinical trial EDC sales.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
argument-hint: "[company-name-or-linkedin-url]"
---

# LinkedIn Prospect Research

## Purpose

Build a comprehensive prospect dossier for Talosix sales targeting. This skill researches companies and key decision makers in the clinical trial space, scores them against the Ideal Customer Profile, identifies buying signals, and generates personalized talking points for outreach.

## Workflow

### Step 1: Identify the Target

Parse the input to determine whether it is a company name, a LinkedIn URL, or a person name.

- If a LinkedIn URL is provided, extract the company or person identifier from the URL path.
- If a company name is provided, use it directly for research.
- If ambiguous, default to treating the input as a company name.

### Step 2: Company Research

Use WebSearch to gather intelligence across multiple dimensions. Run these searches:

1. **Company overview**: `"[company name]" clinical trials site:linkedin.com`
2. **Recent news**: `"[company name]" news OR announcement OR press release 2025 2026`
3. **Clinical trial pipeline**: `"[company name]" site:clinicaltrials.gov`
4. **Funding and financials**: `"[company name]" funding OR revenue OR acquisition OR IPO`
5. **Technology stack**: `"[company name]" EDC OR "data management" OR "clinical technology" OR eClinical`
6. **Job postings (buying signals)**: `"[company name]" hiring OR careers "data manager" OR "clinical data" OR "EDC"`
7. **Leadership changes**: `"[company name]" appointed OR hired OR promoted VP OR Director "clinical operations" OR "data management"`
8. **Regulatory activity**: `"[company name]" FDA OR EMA submission OR approval OR NDA OR BLA`

Use WebFetch to pull key pages when search results indicate high-value content (press releases, about pages, career pages).

### Step 3: Key Decision Maker Identification

Search for stakeholders in these persona categories:

| Persona | Titles to Search | Priority |
|---------|------------------|----------|
| Clinical Operations | VP/SVP/Director Clinical Operations | Primary |
| Data Management | VP/Director Clinical Data Management, Head of CDM | Primary |
| IT / Technology | CTO, VP IT, Director Clinical Systems | Secondary |
| Procurement | VP Procurement, Director Vendor Management | Influencer |
| Quality / Regulatory | VP Quality, Director Regulatory Affairs | Influencer |
| C-Suite | CEO, COO, CMO (for smaller companies) | Executive Sponsor |

For each identified decision maker, note:
- Name and title
- Tenure in role (new hires are stronger signals)
- Background (previous companies, especially competitor EDC users)
- LinkedIn activity (posts, comments, shared content indicating priorities)
- Conference presentations or publications

### Step 4: Clinical Trial Pipeline Analysis

Search ClinicalTrials.gov for the company's active studies:

- Total number of active trials
- Phase distribution (Phase I, II, III, IV)
- Therapeutic areas
- Geographic spread (US, EU, global)
- Trial complexity indicators (adaptive designs, decentralized trials, rare disease)
- Recently started trials (last 6 months)
- Upcoming trials (registered but not yet recruiting)

### Step 5: Technology Landscape Assessment

Determine current technology stack:

- **Current EDC vendor**: Look for mentions of Medidata Rave, Veeva Vault EDC, Oracle Clinical One, Castor, IQVIA, Signant Health, or others.
- **Adjacent systems**: CTMS, eTMF, RTSM/IRT, safety databases, ePRO/eCOA.
- **Integration patterns**: Data warehouse, analytics platforms, regulatory submission tools.
- **Contract timing signals**: When current vendor contracts may be up for renewal (look for original implementation dates, typically 3-5 year cycles).

### Step 6: ICP Scoring

Score the prospect against Talosix's Ideal Customer Profile on a 0-100 scale:

| Criteria | Weight | Score Range | Indicators |
|----------|--------|-------------|------------|
| Company Type | 20% | 0-100 | CRO (100), Pharma Sponsor (90), Biotech (85), Academic (50), Device (40) |
| Trial Volume | 20% | 0-100 | 50+ active (100), 20-49 (80), 10-19 (60), 5-9 (40), <5 (20) |
| Trial Phase | 15% | 0-100 | Phase II-III heavy (100), Phase I (70), Phase IV (60), Mixed (80) |
| Company Size | 10% | 0-100 | 1000-10000 employees (100), 500-999 (80), 10000+ (70), 100-499 (60) |
| Technology Readiness | 15% | 0-100 | Legacy EDC/looking to switch (100), No EDC (80), Modern EDC but unhappy (70), Happy with current (20) |
| Buying Signals | 20% | 0-100 | Active RFP (100), Hiring data managers (80), New trials starting (70), Leadership change (60), No signals (10) |

Calculate weighted total. Classify:
- **90-100**: Tier 1 - Immediate priority, high fit
- **70-89**: Tier 2 - Strong fit, nurture actively
- **50-69**: Tier 3 - Moderate fit, monitor for signals
- **Below 50**: Tier 4 - Low priority, revisit quarterly

### Step 7: Buying Signal Detection

Flag the following high-value signals with evidence:

**Hot Signals (act within 1 week)**:
- Active RFP or vendor evaluation mentioned in job posts or industry forums
- New CDM/Clinical Ops leader hired (first 90 days = evaluation window)
- Public complaints about current EDC vendor
- Regulatory warning letter citing data integrity issues

**Warm Signals (act within 1 month)**:
- Job postings for clinical data managers or EDC specialists
- New trial registrations on ClinicalTrials.gov
- Funding round closed (expansion capital)
- Conference presentation about data management challenges

**Nurture Signals (add to long-term pipeline)**:
- General growth indicators (hiring across functions)
- New therapeutic area expansion
- Geographic expansion to new regions
- Partnership announcements

### Step 8: Generate Prospect Brief

Compile the final dossier in this format:

```markdown
# Prospect Brief: [Company Name]

## Quick Summary
- **ICP Score**: [X/100] - [Tier]
- **Company Type**: [CRO/Pharma/Biotech/etc.]
- **Size**: [employees] | **HQ**: [location]
- **Active Trials**: [count] across [therapeutic areas]
- **Current EDC**: [vendor if known, or "Unknown"]
- **Top Signal**: [strongest buying signal]

## Company Overview
[2-3 paragraph summary of the company, what they do, recent trajectory]

## Clinical Trial Pipeline
[Summary table of trials by phase and therapeutic area]

## Key Decision Makers
[Table with name, title, tenure, background, recommended approach]

## Technology Landscape
[Current systems, known pain points, integration environment]

## Buying Signals
[Ranked list of signals with evidence links]

## Competitive Context
[Current vendor relationship, known satisfaction/dissatisfaction, contract timing]

## Personalized Talking Points
1. [Specific talking point referencing a recent event or pain point]
2. [Talking point about their trial complexity and how Talosix addresses it]
3. [Talking point about industry trend relevant to their therapeutic area]
4. [Talking point about operational efficiency gains]
5. [Talking point referencing a peer company success story]

## Recommended Approach
- **Entry Point**: [Which persona to target first and why]
- **Opening Angle**: [What topic/value prop to lead with]
- **Timing**: [Why now is the right moment]
- **Potential Objections**: [Likely pushback and response strategies]
- **Multi-Threading Strategy**: [How to engage multiple stakeholders]
```

## Important Notes

- Always cite sources with URLs for every factual claim.
- Mark any inferred or uncertain information clearly with "[Estimated]" or "[Unconfirmed]".
- If limited information is available, state gaps clearly and suggest how the sales team can fill them.
- Prioritize recency: information from the last 6 months is most valuable.
- Cross-reference multiple sources to validate key facts.
- Do not fabricate information. If a data point cannot be found, say so.
- Check for any existing research in the local workspace before starting fresh.
