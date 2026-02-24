---
name: EDC Demo Script Generator
description: Create tailored demo scripts for Talosix EDC product demonstrations, customized by audience type, therapeutic area, and competitive context with objection handling.
---

# EDC Demo Script Generator

## Purpose

Create structured, audience-specific demo scripts for Talosix EDC that highlight the most relevant features, address likely objections, and position competitively against known alternatives. Every demo should tell a story grounded in the prospect's clinical trial reality.

## When to Use

- Preparing for a scheduled product demo with a prospect
- Creating demo templates for a specific audience segment
- Customizing an existing script for a new therapeutic area or study phase
- Training new sales engineers on demo delivery

## Information Gathering

Collect before generating:

1. **Audience** - Who is attending (roles, seniority, technical depth)
2. **Company Type** - Sponsor, CRO, academic, site network
3. **Therapeutic Area** - Primary indication and study type
4. **Study Phase** - Phase I-IV, or registry/observational
5. **Pain Points** - Known issues with current EDC or processes
6. **Competitive Context** - Current/previous EDC vendor, vendors also being evaluated
7. **Demo Duration** - Typically 30, 45, or 60 minutes
8. **Demo Environment** - Which Talosix demo instance/study to use

## Demo Script Structure

### Opening (5 minutes)

```
1. Welcome and introductions
2. Confirm agenda and time
3. Ask: "What would make this demo successful for you today?"
4. Brief Talosix positioning (2-3 sentences, not a company pitch)
5. Transition: "Let me show you how this works in practice..."
```

### Story-Based Flow (Core Demo)

Structure the demo around a realistic clinical trial scenario, not a feature walkthrough. Use a study that mirrors the prospect's work.

**Example narrative arc:**
```
"Let's walk through a Phase III [therapeutic area] study from setup to database lock."

Scene 1: Study Build & Configuration
Scene 2: Site Activation & First Patient In
Scene 3: Day-in-the-Life of a Site Coordinator
Scene 4: Data Management & Query Resolution
Scene 5: Medical Review & Coding
Scene 6: Reporting & Oversight
Scene 7: Database Lock & Export
```

### Closing (5 minutes)

```
1. Recap key differentiators shown
2. Ask: "How does this compare to what you're doing today?"
3. Propose next steps (technical deep-dive, reference call, pilot)
4. Provide timeline for follow-up
```

## Audience Customization

### CRO Audience

**Priority Features:**
- Multi-sponsor study management and tenant isolation
- Rapid study build with form libraries (emphasize time-to-first-patient)
- Flexible workflows that adapt to different sponsor SOPs
- White-labeling and branding options
- Volume licensing and portfolio pricing

**Key Messages:**
- "Reduce study build time by X% with reusable libraries"
- "Support multiple sponsor standards within one platform"
- "Your data managers will spend less time on queries and more on oversight"

**Likely Concerns:** Switching costs, retraining staff, sponsor acceptance

### Sponsor Audience

**Priority Features:**
- Real-time data visibility and oversight dashboards
- CDISC compliance and submission-ready data
- Risk-based monitoring support (KRIs, central monitoring)
- Cross-study analytics and portfolio view
- Vendor oversight and audit capabilities

**Key Messages:**
- "See your data the moment it's entered, not weeks later"
- "Reduce time from database lock to submission"
- "Built-in compliance reduces audit findings"

**Likely Concerns:** Validation burden, IT integration, data migration

### Site/Investigator Audience

**Priority Features:**
- Intuitive eCRF interface (minimize clicks, smart defaults)
- Offline capability for sites with connectivity issues
- Query resolution workflow simplicity
- Patient visit scheduling and reminders
- eSource and EHR integration

**Key Messages:**
- "Designed for clinicians, not just data managers"
- "Complete a visit form in under X minutes"
- "Reduce screen failures with real-time eligibility checks"

**Likely Concerns:** Training burden, another system to learn, IT requirements at site

## Therapeutic Area Customization

### Oncology Demo
- Show RECIST tumor measurement forms with auto-calculations
- Demonstrate complex visit windows with treatment cycles
- Highlight dose modification tracking and safety reporting
- Show randomization stratification by stage/biomarker
- Include PRO/QoL instrument integration

### Rare Disease Demo
- Show adaptive design support and protocol amendment agility
- Demonstrate natural history data collection
- Highlight small-N analytics and individual patient narratives
- Show patient/caregiver portal for ePRO
- Include long-term follow-up scheduling

### Vaccine Demo
- Show large-scale enrollment tracking and site dashboards
- Demonstrate reactogenicity diary collection (electronic)
- Highlight lot number tracking and randomization
- Show safety surveillance and signal detection dashboards
- Include multi-country regulatory submission data formatting

## Competitive Differentiators

Weave these into the demo naturally (show, don't tell):

| Differentiator | How to Demonstrate |
|---|---|
| Study build speed | Build a form live in the demo, show library drag-and-drop |
| Modern UX | Side-by-side comparison with legacy EDC screenshots |
| Real-time analytics | Show live dashboard updating as data is entered |
| Edit check transparency | Show edit check logic in plain language, not SAS code |
| API-first architecture | Show integration marketplace, webhook configuration |
| Configuration flexibility | Make a mid-demo change to show no-code adaptability |

## Objection Handling

Prepare responses for common objections and weave preemptive answers into the demo:

**"We're happy with our current EDC."**
- "Many of our customers felt the same way. What changed was [specific pain point]. Let me show you how we handle that differently."

**"Switching EDC mid-program is too risky."**
- Show migration tools and parallel-run capabilities
- Reference a successful migration case study in their therapeutic area

**"You're too small / not established enough."**
- Show customer logos, study volume, and growth metrics
- Emphasize agility and responsiveness as advantages
- Offer a reference call with a similar-sized organization

**"We need [specific feature] that you probably don't have."**
- If available: demonstrate it immediately
- If roadmap: share timeline and show API extensibility
- If not planned: acknowledge honestly, pivot to platform strengths

**"How do we know you'll be around in 5 years?"**
- Share growth metrics, funding status, and customer retention rates
- Emphasize clinical data portability and standards compliance (CDISC)

**"Our IT team will never approve this."**
- Show security architecture, compliance certifications
- Offer a dedicated IT/security deep-dive session
- Provide security whitepaper and SOC 2 report under NDA

## Demo Tips

- **Never demo a feature without context.** Always tie it to a clinical trial workflow.
- **Use their terminology.** If they say "case report form" not "eCRF," match their language.
- **Pause after showing something impressive.** Let it land.
- **Ask questions throughout.** "Is this similar to how you handle this today?" keeps engagement.
- **Have a backup plan.** If the demo environment has issues, have screenshots/recordings ready.
- **End 5 minutes early.** Use the time for discussion rather than rushing.

## Output Format

Deliver the demo script as:

1. **Script Document** - Minute-by-minute script with talking points, click paths, and transition phrases
2. **Objection Cheat Sheet** - One-page reference for the demo presenter
3. **Follow-Up Template** - Email template summarizing key points shown, next steps, and relevant resources to share
