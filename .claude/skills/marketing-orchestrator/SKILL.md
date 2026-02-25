---
name: marketing-orchestrator
description: Meta-skill that analyzes marketing goals and routes to the optimal sequence of marketing sub-skills. Orchestrates multi-step campaigns by chaining research, content creation, transformation, and distribution skills together using a three-layer architecture.
argument-hint: "[goal-or-campaign-type]"
---

# Marketing Orchestrator for Talosix EDC

## Purpose

This is the command center for Talosix marketing operations. It analyzes user intent, recommends the optimal combination of marketing skills, defines execution order, and ensures consistent output across the entire content supply chain. Think of it as the marketing project manager that breaks any campaign goal into an ordered sequence of specialized skills.

## Three-Layer Architecture

Every marketing initiative flows through three layers. The orchestrator ensures no layer is skipped.

### Layer 1: Research (Intelligence Gathering)

Before any content is created, gather intelligence using available MCP integrations and research tools.

**Actions at this layer:**
- Use WebSearch to analyze competitor content, industry trends, and audience conversations
- Use WebFetch to pull reference material from clinical trial publications, FDA guidance, and industry blogs
- Use PubMed MCP to find recent clinical trial methodology publications relevant to the topic
- Check Confluence (Atlassian MCP) for existing Talosix content assets, brand guidelines, and messaging docs
- Review Jira (Atlassian MCP) for product roadmap items that inform marketing messaging

**Research outputs:**
- Competitive content landscape summary
- Audience pain points and trending topics
- Regulatory and compliance context
- Existing Talosix assets available for repurposing
- Product capabilities and differentiators to reference

### Layer 2: Methodology (Framework Selection)

Select the right content frameworks based on the campaign goal, audience segment, and funnel stage.

**Framework mapping:**
| Goal | Primary Framework | Supporting Frameworks |
|------|------------------|----------------------|
| Brand awareness | Thought leadership, SEO content | E-E-A-T signals, hook-story-insight |
| Lead generation | Lead magnet + gate strategy | PAS/AIDA copy, nurture sequence |
| Pipeline acceleration | Case study, ROI proof | Direct response, ABM personalization |
| Product launch | Feature-benefit messaging | Multi-channel atomization |
| Event promotion | Event sequence + content tease | Social proof, countdown urgency |
| Customer expansion | Success story + use case | Value realization, new capability |

### Layer 3: Process (Execution Sequence)

Execute skills in the correct order, passing outputs from one skill as inputs to the next.

**Handoff protocol between skills:**
1. Each skill produces a structured output with clearly labeled sections
2. The next skill in the chain receives the relevant sections as input
3. Brand voice skill is loaded as background context for every content-producing skill
4. Final outputs are collected and assembled into a campaign package

## Pre-Built Campaign Workflows

### Workflow 1: Launch Campaign

**When to use:** New feature release, product launch, major platform update.

**Skill chain:**
```
1. [Research]       WebSearch + PubMed MCP -> competitive landscape, audience needs
2. [Lead Magnet]    lead-magnet-creator -> gated asset (checklist, guide, or template)
3. [Direct Copy]    direct-response-copy -> landing page, email headers, ad copy
4. [Email]          email-campaign (existing skill) -> nurture sequence
5. [Social]         social-media-transformer -> platform-specific posts
6. [Calendar]       content-calendar (existing skill) -> publication schedule
```

**Orchestrator actions:**
- Pass product feature details and competitive context from Step 1 into Step 2
- Use lead magnet topic to inform landing page copy in Step 3
- Align email sequence messaging with landing page in Step 4
- Extract key messages from all content for social atomization in Step 5
- Schedule all outputs across a 4-6 week launch window in Step 6

### Workflow 2: Content Blitz

**When to use:** Filling the content pipeline, establishing thought leadership on a topic, SEO content push.

**Skill chain:**
```
1. [Research]       WebSearch -> content gap analysis, keyword opportunities
2. [Blog]           blog-post-engine -> SEO-optimized long-form content
3. [Atomize]        content-atomizer -> 20+ content pieces from the blog post
4. [Social]         social-media-transformer -> platform-optimized variants
5. [Calendar]       content-calendar (existing skill) -> drip schedule over 2-4 weeks
```

**Orchestrator actions:**
- Research phase identifies the highest-value topic and keyword cluster
- Blog post becomes the pillar content piece
- Atomizer breaks it into LinkedIn posts, Twitter threads, email snippets, one-pager, slides
- Social transformer optimizes each piece for platform-specific engagement
- Calendar spreads content across 2-4 weeks to maximize sustained visibility

### Workflow 3: Prospect Outreach

**When to use:** ABM campaigns, targeted prospect engagement, conference follow-up.

**Skill chain:**
```
1. [Research]       linkedin-prospect-research -> prospect profiles and pain points
2. [Account]        abm-account-research -> account intelligence and entry points
3. [Outreach]       outreach-sequence-generator -> personalized multi-touch sequence
4. [Content]        direct-response-copy -> personalized landing page or one-pager
5. [Social]         social-media-transformer -> social touches between emails
```

**Orchestrator actions:**
- Prospect research identifies decision-makers, their content engagement, and stated challenges
- Account research maps the organization's clinical trial portfolio and technology stack
- Outreach generator creates a 5-7 touch sequence mixing email, LinkedIn, and content sharing
- Direct response copy creates a personalized asset addressing their specific pain points
- Social transformer creates engagement touches (comments, shares, connection messages)

### Workflow 4: Thought Leadership Sprint

**When to use:** Executive visibility, conference preparation, industry positioning.

**Skill chain:**
```
1. [Research]       WebSearch + PubMed MCP -> trending topics, regulatory changes, data
2. [Blog]           blog-post-engine -> authoritative long-form article
3. [One-Pager]      one-pager-generator -> executive summary version
4. [Atomize]        content-atomizer -> social content, email snippets, slides
5. [Social]         social-media-transformer -> LinkedIn articles, Twitter threads
```

### Workflow 5: Lead Generation Engine

**When to use:** Building top-of-funnel pipeline, filling webinar registrations, growing email list.

**Skill chain:**
```
1. [Research]       WebSearch -> audience pain points and content consumption patterns
2. [Lead Magnet]    lead-magnet-creator -> high-value gated asset
3. [Direct Copy]    direct-response-copy -> landing page and gate copy
4. [Email]          email-campaign (existing skill) -> post-download nurture
5. [Atomize]        content-atomizer -> promotional content pieces
6. [Social]         social-media-transformer -> paid and organic promotion
```

### Workflow 6: Customer Story Amplification

**When to use:** Maximizing the reach of a new case study or customer win.

**Skill chain:**
```
1. [Source]          case-study-creation (existing skill) -> full case study
2. [One-Pager]      one-pager-generator -> one-page summary for sales
3. [Atomize]        content-atomizer -> multi-format content pieces
4. [Direct Copy]    direct-response-copy -> email and ad copy with social proof
5. [Social]         social-media-transformer -> platform-optimized posts
6. [Email]          email-campaign (existing skill) -> prospect-targeted sends
```

## Intent Analysis Protocol

When a user describes a marketing goal, classify it using this decision tree:

```
User Goal
  |
  |-- Contains "launch" / "release" / "announce" / "new feature"
  |     -> Workflow 1: Launch Campaign
  |
  |-- Contains "blog" / "content" / "SEO" / "article" / "thought leadership"
  |     -> Workflow 2: Content Blitz
  |
  |-- Contains "prospect" / "outreach" / "ABM" / "account" / "sales"
  |     -> Workflow 3: Prospect Outreach
  |
  |-- Contains "executive" / "conference" / "speaking" / "visibility"
  |     -> Workflow 4: Thought Leadership Sprint
  |
  |-- Contains "leads" / "pipeline" / "webinar" / "checklist" / "download"
  |     -> Workflow 5: Lead Generation Engine
  |
  |-- Contains "case study" / "customer story" / "testimonial" / "win"
  |     -> Workflow 6: Customer Story Amplification
  |
  |-- Does not match any pattern
  |     -> Ask clarifying questions, then recommend workflow
```

If the goal spans multiple workflows, recommend a combined execution plan with clear phasing.

## Execution Protocol

### Step 1: Classify and Confirm

Present the recommended workflow to the user:
- Which workflow applies and why
- The skill chain that will execute
- Estimated outputs (number and type of content pieces)
- Any inputs needed from the user before starting

### Step 2: Research Phase

Execute Layer 1 research using available tools:
- Load brand-voice skill as background context
- Use WebSearch for competitive and market intelligence
- Use relevant MCP tools for specialized research
- Compile research brief to feed into content skills

### Step 3: Sequential Skill Execution

Run each skill in the chain, passing outputs forward:
- Load each skill's methodology
- Apply brand voice constraints throughout
- Generate all specified outputs
- Flag any decision points that need user input

### Step 4: Quality Assurance

Before delivering final outputs, verify:
- [ ] Brand voice consistency across all pieces (check against brand-voice skill)
- [ ] Clinical trial industry accuracy (terminology, regulatory references)
- [ ] Cross-channel message alignment (same core message, adapted per channel)
- [ ] CTA consistency (all pieces drive toward the same conversion goal)
- [ ] No contradictions between content pieces
- [ ] Compliance language accurate and not overstated

### Step 5: Delivery Package

Assemble all outputs into a structured delivery:
- Campaign summary (goal, audience, timeline, channels)
- All content pieces organized by channel
- Publication schedule recommendation
- Performance metrics to track
- Follow-up recommendations

## Custom Workflow Builder

If none of the pre-built workflows fit, build a custom chain:

1. **Define the goal** -- What outcome does the user need?
2. **Identify the audience** -- Which segment(s) are we targeting?
3. **Select the skills** -- Pick from available marketing skills
4. **Order the chain** -- Research first, then creation, then transformation, then distribution
5. **Define handoffs** -- What output from skill A feeds into skill B?
6. **Set the timeline** -- When does each piece need to be ready?

## Available Marketing Skills Registry

| Skill | Type | Input | Primary Output |
|-------|------|-------|---------------|
| brand-voice | Background | Auto-loaded | Voice constraints |
| blog-post-engine | Creation | Topic/keyword | SEO blog post + distribution checklist |
| content-atomizer | Transformation | Any long-form content | 20+ multi-format pieces |
| one-pager-generator | Creation | Topic/feature | Professional one-pager |
| social-media-transformer | Transformation | Any content | Platform-specific posts |
| direct-response-copy | Creation | Asset type + audience | High-converting copy |
| lead-magnet-creator | Creation | Topic/audience | Gated asset + landing page copy |
| email-campaign | Creation | Audience + goal | Email sequence + A/B variants |
| case-study-creation | Creation | Customer story | Full case study + assets |
| content-calendar | Planning | Content inventory | Publication schedule |
| landing-page-copy | Creation | Product/feature | Landing page copy |
| ad-copy | Creation | Campaign + audience | Ad variants |
| seo-content | Creation | Keyword cluster | SEO-optimized content |
| competitive-battlecard | Research | Competitor | Battlecard document |
| white-paper | Creation | Topic | Long-form white paper |

## Output Format

When orchestrating, always present:

```
## Campaign Plan: [Goal Name]

### Workflow: [Workflow Name]
### Audience: [Target Segment(s)]
### Timeline: [Estimated Duration]

### Execution Chain:
Step 1: [Skill Name] -- [What it produces] -- [Est. time]
Step 2: [Skill Name] -- [What it produces] -- [Est. time]
...

### Total Deliverables:
- X blog posts
- X social posts (LinkedIn + Twitter)
- X email sequences
- X one-pagers
- X ad variants
- [other outputs]

### Inputs Needed From You:
- [List of information or decisions needed]
```
