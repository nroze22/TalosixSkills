---
name: lead-research-assistant
description: Identify and qualify leads for the Talosix sales team. Finds companies matching target criteria, qualifies them against the ICP, scores them as Hot/Warm/Cool, and produces a prioritized lead list with outreach recommendations for clinical trial EDC sales.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
argument-hint: "[target-criteria]"
---

# Lead Research Assistant

## Purpose

Systematically identify, research, and qualify potential leads for the Talosix sales team. Takes target criteria as input (company size, therapeutic area, geography, trial phase) and produces a scored, prioritized lead list with actionable outreach recommendations.

## Workflow

### Step 1: Parse Target Criteria

Extract the targeting parameters from the input. If criteria are vague or missing, use sensible defaults for Talosix's ICP:

| Parameter | User Input | Default (if not specified) |
|-----------|------------|----------------------------|
| Company Type | [CRO, Pharma, Biotech, All] | All clinical trial organizations |
| Company Size | [employee range] | 100-10,000 employees |
| Therapeutic Area | [specific area or All] | All (with preference for oncology, CNS, rare disease, immunology) |
| Geography | [region/country] | US, EU, Global |
| Trial Phase | [I, II, III, IV, All] | Phase II-III preferred |
| Trial Volume | [minimum active trials] | 5+ active trials |
| Technology Signal | [legacy EDC, no EDC, any] | Any (flag legacy EDC users) |
| Lead Count | [how many leads to find] | 15-25 qualified leads |

### Step 2: Lead Discovery

Use a multi-pronged search strategy to identify candidate companies:

#### 2A: ClinicalTrials.gov Mining

Search for companies with active clinical trials matching criteria:

- WebSearch: `site:clinicaltrials.gov "[therapeutic area]" sponsor recruiting 2025 2026`
- WebSearch: `site:clinicaltrials.gov "[therapeutic area]" phase [X] recruiting`
- WebSearch: `clinicaltrials.gov [therapeutic area] new trials 2025 2026`

Extract sponsor and collaborator names from trial listings.

#### 2B: Industry Directory Search

Search for companies in clinical trial industry directories:

- WebSearch: `"contract research organization" OR CRO [therapeutic area] [geography] list OR directory`
- WebSearch: `biotech [therapeutic area] clinical trials [geography] company`
- WebSearch: `pharmaceutical company [therapeutic area] pipeline`
- WebSearch: `top CRO clinical research 2025 2026 ranking`

#### 2C: Funding and Growth Signal Search

Find companies with recent funding that suggests trial expansion:

- WebSearch: `biotech funding round 2025 2026 [therapeutic area] series OR IPO`
- WebSearch: `pharmaceutical acquisition clinical trial [therapeutic area]`
- WebSearch: `biotech [therapeutic area] phase II OR "phase 2" 2025 2026`

#### 2D: EDC Vendor Signal Search

Find companies that may be evaluating or switching EDC vendors:

- WebSearch: `"EDC" OR "electronic data capture" RFP OR evaluation OR selection clinical trial`
- WebSearch: `"data management" OR "clinical data" hiring [therapeutic area] company`
- WebSearch: `Medidata OR Rave OR Veeva migration OR switch OR replacement`

#### 2E: Conference and Event Mining

Identify companies active in the clinical trial technology space:

- WebSearch: `DIA 2025 2026 exhibitor OR attendee OR presenter clinical trial`
- WebSearch: `SCOPE summit 2025 2026 clinical operations speaker`
- WebSearch: `SCDM annual conference 2025 2026 data management`

### Step 3: Initial Screening

For each candidate company discovered, conduct rapid screening:

**Screen In (proceed to qualification)**:
- Runs clinical trials (confirmed via ClinicalTrials.gov or company website)
- Matches company type criteria
- Appears to be within size range
- Active in target therapeutic area or geography

**Screen Out (exclude)**:
- No clinical trial activity found
- Too small (fewer than 20 employees, pre-clinical only)
- Too large and entrenched (top 10 pharma with long-term enterprise EDC contracts, unless showing switch signals)
- Academic-only with no commercial operations
- Already a Talosix customer (check workspace for existing customer lists)
- Acquired or merged into another entity

### Step 4: Lead Qualification

For each screened-in company, conduct qualification research:

#### 4A: Company Profile

Use WebSearch and WebFetch to gather:
- Company name and legal entity
- HQ location
- Employee count (LinkedIn, company website, Crunchbase)
- Company type (CRO, Pharma Sponsor, Biotech, Medical Device)
- Therapeutic focus areas
- Public/private status
- Recent funding or financial events
- Website URL

#### 4B: Clinical Trial Activity

- Total active trials on ClinicalTrials.gov
- Phase distribution
- Therapeutic areas of active trials
- Geographic distribution of trial sites
- Recent trial starts (last 6 months)
- Trial complexity indicators

#### 4C: Technology Assessment

Search for current EDC/technology vendor:
- WebSearch: `"[company name]" Medidata OR Rave OR Veeva OR "Oracle Clinical" OR Castor OR IQVIA OR Signant EDC`
- Check job postings for required system experience
- Look for vendor press releases mentioning the company

Classify:
- **Known Legacy EDC**: Using an older system (strong signal)
- **Known Modern EDC**: Using a current competitor product
- **Unknown**: Could not determine current EDC
- **No EDC**: Small enough or new enough to not have an EDC yet

#### 4D: Buying Signal Detection

Check for active buying signals:

**Hot Signals** (assign if any are present):
- Active RFP or vendor evaluation (found in searches, industry forums, or job posts)
- New VP/Director of Clinical Data Management hired in last 90 days
- Public dissatisfaction with current vendor
- Regulatory finding citing data management or data integrity issues

**Warm Signals** (assign if 2+ are present):
- Job postings for EDC administrators, clinical data managers, or clinical programmers
- New trial registrations on ClinicalTrials.gov (last 3 months)
- Funding round closed in last 6 months
- Geographic or therapeutic area expansion
- Conference presentations about clinical data challenges

**Cool Signals** (assign if only these are present):
- General hiring activity
- Stable trial pipeline with no major changes
- No technology-related news
- Existing vendor relationship appears stable

#### 4E: Key Contacts Identification

For each qualified lead, identify 1-3 key contacts:

| Priority | Persona | Search Strategy |
|----------|---------|-----------------|
| Primary | VP/Director Clinical Data Management | WebSearch: `"[company]" "data management" OR CDM director OR VP site:linkedin.com` |
| Secondary | VP/Director Clinical Operations | WebSearch: `"[company]" "clinical operations" director OR VP site:linkedin.com` |
| Tertiary | CTO / Director IT | WebSearch: `"[company]" CTO OR "clinical systems" OR "clinical technology" site:linkedin.com` |

For each contact, note: name, title, and any personalization anchors (recent posts, conference talks, publications).

### Step 5: Lead Scoring

Score each qualified lead on a composite scale:

| Criteria | Weight | Score (0-10) | Description |
|----------|--------|-------------|-------------|
| ICP Fit | 25% | | How closely does the company match Talosix's ideal customer profile |
| Trial Volume | 20% | | Number and complexity of active trials |
| Buying Signals | 25% | | Strength and recency of buying signals detected |
| Technology Gap | 15% | | Likelihood of EDC evaluation (legacy system, no system, known pain) |
| Accessibility | 15% | | Ability to reach decision makers, mutual connections, event overlap |

**Weighted Score Calculation**: Sum of (weight x score) for each criterion.

**Lead Classification**:
- **Hot (80-100)**: Active evaluation signals, strong ICP fit, clear technology need. Prioritize for immediate outreach.
- **Warm (50-79)**: Good ICP fit with moderate signals. Add to active nurture sequence.
- **Cool (25-49)**: Future potential but no immediate triggers. Monitor for signal changes.
- **Disqualified (<25)**: Poor fit, remove from list.

### Step 6: Outreach Recommendations

For each qualified lead, provide a specific outreach recommendation:

**For Hot Leads**:
- Recommended first touch: Personalized email from sales rep
- Opening angle: Reference specific buying signal
- Urgency: Engage within 1 week
- Suggested sequence: Full 7-touch personalized sequence (use outreach-sequence-generator skill)

**For Warm Leads**:
- Recommended first touch: LinkedIn connection + value content share
- Opening angle: Share relevant industry insight
- Timeline: Engage within 2-4 weeks
- Suggested nurture: Monthly value touchpoints until signal strengthens

**For Cool Leads**:
- Recommended action: Add to marketing nurture list
- Content: Subscribe to newsletter, target with relevant content
- Review cadence: Re-evaluate quarterly for signal changes

### Step 7: Output Format

```markdown
# Qualified Lead List for Talosix

**Date**: [Date]
**Criteria**: [Summary of targeting criteria used]
**Total Companies Screened**: [count]
**Qualified Leads**: [count]
**Hot**: [count] | **Warm**: [count] | **Cool**: [count]

## Executive Summary
[3-5 sentences summarizing the lead landscape, top opportunities, and recommended priorities]

## Hot Leads (Prioritize Immediately)

### 1. [Company Name] -- Score: [X/100]
| Field | Details |
|-------|---------|
| **Company Type** | [CRO/Pharma/Biotech] |
| **HQ** | [Location] |
| **Size** | [Employees] |
| **Therapeutic Focus** | [Areas] |
| **Active Trials** | [Count, phase distribution] |
| **Current EDC** | [Vendor if known] |
| **Top Signal** | [Most compelling buying signal with evidence] |
| **Key Contact** | [Name, Title] |
| **Secondary Contact** | [Name, Title] |
| **Recommended Approach** | [Specific outreach strategy] |
| **Talking Point** | [Personalized angle for first touch] |

### 2. [Company Name] -- Score: [X/100]
[Same format]

---

## Warm Leads (Active Nurture)

### [Number]. [Company Name] -- Score: [X/100]
| Field | Details |
|-------|---------|
| **Company Type** | |
| **HQ** | |
| **Size** | |
| **Active Trials** | |
| **Current EDC** | |
| **Signal Summary** | |
| **Key Contact** | |
| **Nurture Strategy** | |

---

## Cool Leads (Monitor and Revisit)

| # | Company | Type | Size | Trials | Signal | Score | Next Review |
|---|---------|------|------|--------|--------|-------|-------------|
| | | | | | | | |

---

## Market Observations
[Trends noticed during research: common pain points, technology shifts, budget cycles, competitive dynamics]

## Recommended Next Steps
1. [Immediate action for hot leads]
2. [Nurture setup for warm leads]
3. [Monitoring plan for cool leads]
4. [Criteria refinement suggestions based on what was found]

## Sources
[Key sources used during research]
```

## Research Quality Standards

- Every lead must have verifiable clinical trial activity. Do not include companies based solely on industry association.
- Buying signal claims must have specific evidence (URL, date, description). Do not infer signals without evidence.
- Contact names must be verifiable via public sources. Do not guess or fabricate contact information.
- Scoring must be consistent. Apply the same criteria and thresholds to every lead.
- If the requested number of leads cannot be found at the quality threshold, report fewer leads rather than padding with low-quality ones.
- Clearly distinguish between confirmed data and estimates. Label uncertain data points as "[Estimated]".
- Check the local workspace for any existing lead lists, CRM exports, or customer data before starting to avoid duplicating effort or recommending existing customers.
- Recency matters: prioritize companies with activity in the last 6 months over older data.
