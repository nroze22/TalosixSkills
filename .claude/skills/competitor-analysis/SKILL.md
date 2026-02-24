---
name: competitor-analysis
description: "Analyze competitors in the EDC and clinical trial technology space including feature comparison, pricing, market positioning, and SWOT analysis."
allowed-tools: WebSearch, mcp__claude_ai_Atlassian__searchConfluenceUsingCql, mcp__claude_ai_Atlassian__getConfluencePage, mcp__claude_ai_Atlassian__createConfluencePage
argument-hint: "[Competitor name(s) or 'full landscape'] [Optional: specific area to compare, e.g. 'ePRO', 'edit checks', 'pricing']"
---

# Competitor Analysis

Analyze competitors in the EDC and clinical trial technology space to inform Talosix product strategy and positioning.

## Workflow

1. **Scope the Analysis**: Ask user for:
   - Specific competitor(s) or full landscape review
   - Focus area (specific feature, pricing, market position, or comprehensive)
   - Purpose (sales battle card, strategic planning, feature gap analysis)

2. **Research**: Pull from available sources:
   - Confluence for existing competitive intel
   - Web search for public information, press releases, analyst reports
   - User's input on win/loss data and customer feedback

3. **Generate Analysis** in the appropriate format.

## Key Competitors in EDC/Clinical Trial Technology

### Tier 1 (Enterprise / Market Leaders)
- **Medidata Rave (Dassault Systemes)**: Market leader, broad platform, strong pharma penetration
- **Oracle Clinical One / InForm**: Enterprise suite, strong in large pharma, legacy presence
- **Veeva Vault EDC**: Growing fast, unified Vault platform, strong in mid-market

### Tier 2 (Established Challengers)
- **Signant Health (ClinCapture)**: ePRO/eCOA focus, decentralized trials
- **Clario (Merge/ERT)**: eCOA, cardiac safety, imaging
- **Medrio**: Mid-market cloud EDC, ease-of-use focus
- **Castor EDC**: European market, ease of use, academic-friendly
- **OpenClinica**: Open-source heritage, flexible, cost-effective

### Tier 3 (Niche / Emerging)
- **Climedo**: Decentralized trials, hybrid approach
- **REDCap**: Academic/investigator-initiated, free, limited for commercial
- **Florence / Slope**: Site-focused tools, eISF, eConsent
- **Science 37 / Curebase**: Decentralized trial platforms

## Analysis Templates

### Feature Comparison Matrix

| Feature Area | Talosix | Medidata | Veeva | Oracle | Castor |
|-------------|---------|----------|-------|--------|--------|
| **eCRF Design** | | | | | |
| Visual form builder | [Rating] | [Rating] | [Rating] | [Rating] | [Rating] |
| CDASH library | | | | | |
| Conditional logic | | | | | |
| **Data Management** | | | | | |
| Edit checks | | | | | |
| Query management | | | | | |
| CDISC ODM export | | | | | |
| Medical coding (MedDRA/WHODrug) | | | | | |
| **Monitoring** | | | | | |
| SDV workflows | | | | | |
| Remote monitoring | | | | | |
| Risk-based monitoring | | | | | |
| **Integrations** | | | | | |
| Lab data import | | | | | |
| IRT/IWRS | | | | | |
| Safety (E2B) | | | | | |
| **Compliance** | | | | | |
| 21 CFR Part 11 | | | | | |
| Audit trail | | | | | |
| e-Signatures | | | | | |
| **Patient-Centric** | | | | | |
| ePRO/eCOA | | | | | |
| eConsent | | | | | |
| Patient portal | | | | | |

Use ratings: Leading / Strong / Adequate / Weak / Absent

### SWOT Analysis Template

For each competitor:

```
## [Competitor Name] SWOT

### Strengths
- [Market position, technology, customer base, integrations]

### Weaknesses
- [Known limitations, customer complaints, technical debt, pricing]

### Opportunities (for Talosix)
- [Where we can win against them, gaps we can exploit]

### Threats (from this competitor)
- [Where they threaten our position, upcoming capabilities]
```

### Pricing Analysis

| Vendor | Pricing Model | Per-Study Range | Per-Site/Per-Subject | Platform Fee | Notes |
|--------|-------------|-----------------|---------------------|-------------|-------|
| Medidata | Per-subject + modules | $$$$ | High | Yes | Premium pricing, bundling |
| Veeva | Per-user + modules | $$$ | Medium | Yes | Growing, competitive deals |
| Oracle | Enterprise license | $$$$ | High | Yes | Complex pricing |
| Castor | Per-study tiers | $$ | Low-Medium | No | Transparent pricing |
| Talosix | [Our model] | [Range] | [Rate] | [Y/N] | [Notes] |

### Win/Loss Analysis Format

| Deal | Competitor | Outcome | Key Factors | Lessons |
|------|-----------|---------|-------------|---------|
| [Customer] | [Competitor] | Won/Lost | [Top 3 decision factors] | [What to repeat/improve] |

### Market Positioning Map

```
                    Enterprise
                        |
         Oracle    Medidata
                   |
    Full Platform--+--------Point Solution
                   |
         Veeva   Talosix?
                   |
         Castor   Medrio
                        |
                    Mid-Market
```

Define where Talosix aims to position and the strategic moves to get there.

## Sales Battle Card Format

For each competitor, produce a concise battle card:

```
## Battle Card: Talosix vs. [Competitor]

### When We Win
- [Scenario 1]
- [Scenario 2]

### When We Lose
- [Scenario 1]
- [Scenario 2]

### Key Differentiators
- [Differentiator 1]: Our advantage explanation
- [Differentiator 2]: Our advantage explanation

### Objection Handling
- "[Objection]" -> [Response]
- "[Objection]" -> [Response]

### Landmines to Set
- Ask the prospect about [topic] to highlight competitor weakness

### Proof Points
- [Customer quote or case study reference]
```

## Output
After generating the analysis, offer to:
- Publish to Confluence for team access
- Format as a sales battle card
- Create an executive summary for leadership
