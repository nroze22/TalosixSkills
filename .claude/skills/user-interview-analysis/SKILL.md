---
name: user-interview-analysis
description: "Analyze user interview transcripts from clinical trial stakeholders (CROs, sponsors, sites) to extract themes, pain points, opportunities, and persona insights."
allowed-tools: mcp__claude_ai_Atlassian__createConfluencePage, mcp__claude_ai_Atlassian__searchConfluenceUsingCql
argument-hint: "[Paste transcript text, or provide path to transcript file]"
---

# User Interview Analysis

Analyze user interview transcripts from clinical trial EDC stakeholders to produce structured, actionable insights for the Talosix product team.

## Workflow

1. **Receive Input**: The user provides one or more interview transcripts (pasted text or file paths). Read the content carefully.

2. **Identify the Persona**: Classify the interviewee into one or more Talosix persona categories:
   - **Site Coordinator / CRC**: Enters data, manages visits, resolves queries
   - **Principal Investigator (PI)**: Reviews and signs off on data, medical decisions
   - **Clinical Data Manager (CDM)**: Designs CRFs, manages data cleaning, locks databases
   - **Clinical Monitor / CRA**: Performs SDV, conducts monitoring visits
   - **Sponsor Project Manager**: Oversees trial execution, tracks enrollment/timelines
   - **CRO Operations Lead**: Manages multi-study operations, resource allocation
   - **Regulatory/Quality**: Ensures compliance, manages validation documentation
   - **Biostatistician**: Consumes clean datasets, defines derivations
   - **IT/System Administrator**: Manages system configuration, integrations, user access

3. **Analyze and Generate** the structured output below.

## Output Structure

### Interview Summary
- **Interviewee**: Role, organization type (sponsor/CRO/site), therapeutic area
- **Interview Date**: [date if available]
- **Interviewer**: [name if available]
- **Duration**: [if available]
- **Key Context**: Study phase, study size, current EDC system (if mentioned)

### Themes Identified
Group findings into themes. For each theme:
- **Theme Name**: Brief label (e.g., "Query Resolution Burden", "eCRF Navigation Friction")
- **Description**: 1-2 sentence summary
- **Supporting Quotes**: Direct quotes from the transcript (with attribution)
- **Frequency/Intensity**: How prominently this theme appeared
- **Relevance to Talosix**: How this maps to current or planned features

### Pain Points
For each pain point:
- **Pain Point**: Clear description
- **Severity**: Critical / High / Medium / Low
- **Frequency**: Daily / Weekly / Per-study / Occasional
- **Current Workaround**: What does the user do today?
- **Quote**: Verbatim supporting quote
- **Product Implication**: What Talosix feature or improvement could address this

### Opportunities
- **Opportunity**: Description of unmet need or improvement area
- **User Value**: What would change for the user
- **Business Value**: Revenue, retention, competitive differentiation
- **Feasibility Estimate**: Quick win / Medium effort / Large initiative

### Workflow Observations
- Document the user's current workflow step-by-step
- Note friction points, inefficiencies, and manual steps
- Identify where Talosix EDC fits (or could fit) in their workflow

### Competitive Intelligence
Extract any mentions of:
- Other EDC systems (Medidata Rave, Veeva Vault EDC, Oracle InForm, Castor, REDCap)
- Specific likes/dislikes about competitor products
- Switching triggers or barriers

### Recommendations
- **Immediate Actions**: Quick wins the product team should pursue
- **Feature Requests**: Specific features or improvements suggested
- **Further Research**: Areas that need deeper investigation
- **Persona Updates**: New insights about this persona's needs, goals, and frustrations

## Multi-Interview Synthesis

When analyzing multiple transcripts together, also produce:

### Cross-Interview Theme Matrix
| Theme | Interview 1 | Interview 2 | Interview 3 | Prevalence |
|-------|-------------|-------------|-------------|------------|
| [Theme] | Mentioned/Not | Mentioned/Not | Mentioned/Not | X/N |

### Persona Refinement
- Behavioral patterns observed across interviews
- Segment differences (sponsor vs. CRO vs. site perspective)
- Priority ranking of needs by persona

### Actionable Summary
- Top 3 product priorities based on the research
- Confidence level (strong signal / emerging signal / weak signal)
- Recommended next steps (more research, prototype, build)

## Tips
- Preserve exact quotes -- do not paraphrase when citing evidence
- Flag contradictions between interviewees
- Note emotional intensity (frustration, excitement, indifference)
- Distinguish between stated needs and observed/implied needs
- Pay attention to clinical trial-specific regulatory pain points (audit trail fatigue, validation burden, 21 CFR Part 11 friction)
