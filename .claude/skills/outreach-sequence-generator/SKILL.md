---
name: outreach-sequence-generator
description: Generate personalized multi-touch outreach sequences for Talosix clinical trial EDC sales. Creates 5-7 touch sequences across email and LinkedIn with genuine personalization, consultative tone, and clinical-trial-specific value propositions.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
argument-hint: "[prospect-name-or-company]"
---

# Outreach Sequence Generator

## Purpose

Generate a personalized, multi-touch outreach sequence for a specific prospect or company. Every touch must be genuinely personalized -- not templated mail-merge content. The tone is consultative and value-first, positioning Talosix as a thought partner rather than a vendor pitching features.

## Workflow

### Step 1: Gather Prospect Intelligence

Before writing any outreach, collect the necessary context:

1. **Check for existing dossier**: Search the local workspace for any prior prospect research.
   - Use Glob to find files matching `*[company-name]*` or `*[prospect-name]*` in the workspace.
   - Read any existing research files.

2. **If no dossier exists**, use WebSearch to quickly gather:
   - Company overview and recent news
   - Prospect's role, tenure, and background
   - Company's clinical trial pipeline (ClinicalTrials.gov)
   - Current EDC/technology stack if discoverable
   - Recent LinkedIn activity or conference appearances
   - Any published content (articles, podcast appearances, presentations)

3. **Identify personalization anchors** -- specific, verifiable facts that prove this outreach was crafted for them:
   - A specific trial they registered recently
   - A conference talk they gave
   - A blog post or article they published
   - A company milestone (funding, acquisition, new indication)
   - A regulatory event (FDA approval, submission)
   - A job posting that reveals internal priorities
   - A mutual connection or shared experience

### Step 2: Define the Narrative Arc

Every sequence tells a story across touches. Define:

- **Core insight**: One observation about their business that demonstrates understanding (not a product pitch).
- **Pain hypothesis**: The specific operational challenge they likely face, based on research.
- **Value bridge**: How Talosix's approach (not features) addresses the pain.
- **Social proof angle**: A relevant peer company or industry trend that validates the approach.
- **Call to action progression**: From soft (share a resource) to medium (brief conversation) to direct (meeting request).

### Step 3: Map Clinical Trial Pain Points

Select 2-3 pain points most relevant to this prospect based on their profile:

| Pain Point | Relevant When | Talosix Angle |
|------------|---------------|---------------|
| Study build time | Company running multiple trials, tight timelines, competitive enrollment | Talosix reduces study build from weeks to days with intelligent form libraries and configuration-driven design |
| Data quality issues | Large/complex trials, multiple sites, regulatory scrutiny | Real-time edit checks, centralized data review dashboards, automated query management |
| Regulatory audit readiness | FDA/EMA submission stage, history of 483s or warning letters | Complete audit trail, 21 CFR Part 11 compliance built-in, audit-ready exports |
| Remote monitoring challenges | Global trials, decentralized elements, post-COVID operational model | Cloud-native platform with role-based access, real-time data visibility, risk-based monitoring support |
| Vendor lock-in / flexibility | Using legacy EDC with high switching costs | Open architecture, data portability, transparent pricing, rapid migration tools |
| Integration complexity | Multiple systems (CTMS, IRT, safety, ePRO) that don't talk to each other | API-first design, pre-built connectors, unified data model |
| Scaling operations | Growing trial portfolio, entering new therapeutic areas | Multi-study platform, reusable standards, global deployment capability |
| Cost of ownership | Budget pressure, value scrutiny from procurement | Transparent pricing, no hidden per-study fees, fast time-to-value |

### Step 4: Generate the Sequence

Create a 5-7 touch sequence following this cadence:

---

#### Touch 1: Initial Email (Day 1)

**Purpose**: Open the conversation with insight, not a pitch. Demonstrate you understand their world.

**Structure**:
- Subject line (2 A/B variants, under 50 characters, no spam triggers)
- Opening line: Reference a specific, recent event about them or their company (NOT "I hope this email finds you well")
- Insight paragraph: Share an observation about a challenge in their space that connects to their situation
- Bridge: One sentence connecting the insight to how companies like theirs are solving it
- Soft CTA: Offer a resource, perspective, or brief exchange -- not a demo request
- Signature: Keep it clean, include a relevant credential or social proof element

**Rules**:
- Under 150 words total
- No feature lists
- No "we" in the first two sentences
- First sentence must be about THEM, not Talosix
- Subject line must be specific enough that it could only apply to this prospect

---

#### Touch 2: LinkedIn Connection Request (Day 3)

**Purpose**: Open a second channel. The connection note should feel natural, not salesy.

**Structure**:
- Connection note (under 300 characters for LinkedIn limit)
- Reference a shared interest, mutual connection, or specific content they posted
- Do NOT pitch Talosix in the connection request
- If possible, reference the email ("I also sent a note about [topic]")

---

#### Touch 3: Email Follow-Up (Day 5)

**Purpose**: Add value, not pressure. Share something genuinely useful.

**Structure**:
- Subject line: Reply to original thread (Re: [original subject]) OR new angle (2 variants)
- Opening: Brief, acknowledge they're busy, no guilt-tripping
- Value add: Share a specific resource -- industry benchmark, regulatory update, case study, or insight relevant to their therapeutic area or trial phase
- Bridge: One sentence connecting the resource to a challenge they likely face
- CTA: Slightly more direct -- "Would a 15-minute conversation on [specific topic] be useful?"

**Rules**:
- Under 120 words
- The resource or insight must be genuinely valuable even if they never respond
- Do not repeat the same messaging from Touch 1

---

#### Touch 4: Value Share via LinkedIn or Email (Day 8)

**Purpose**: Demonstrate expertise. Position Talosix team as thought leaders.

**Structure**:
- If connected on LinkedIn: Send a message sharing a relevant article, report, or perspective
- If not connected: Send via email
- Content options:
  - A relevant industry report or benchmark (clinical trial timelines, data quality metrics)
  - A perspective on a regulatory change that affects their operations
  - A case study from a similar company (anonymized if needed)
  - Commentary on a trend in their therapeutic area
- Brief note (3-4 sentences) explaining why you thought of them specifically
- No CTA or a very soft one ("Thought this might be relevant to what you're working on")

---

#### Touch 5: Direct Email (Day 12)

**Purpose**: Make a clear, respectful ask. This is the "meeting request" touch.

**Structure**:
- Subject line: New thread, direct and specific (2 variants)
- Opening: Reference the value you've shared, acknowledge the outreach
- Hypothesis: State your hypothesis about a challenge they face (based on research)
- Offer: Propose a specific, time-bound conversation ("20 minutes to compare notes on [specific topic]")
- Easy out: "If this isn't a priority right now, no worries -- happy to reconnect when timing is better"

**Rules**:
- Under 100 words
- Be direct but respectful
- Include a specific proposed time or scheduling link
- Make it easy to say yes OR no

---

#### Touch 6: LinkedIn Engagement (Day 15)

**Purpose**: Stay visible without being pushy. Engage authentically.

**Structure**:
- Comment thoughtfully on one of their recent LinkedIn posts (if available)
- OR share a post tagging their company about a relevant industry topic
- OR react to their company's content
- This is a "warm presence" touch, not a direct outreach

---

#### Touch 7: Break-Up Email (Day 20)

**Purpose**: Final touch that often generates the highest response rate. Give them an easy way back in.

**Structure**:
- Subject line: Simple, personal (2 variants). Examples: "Should I close the loop?" / "[First name] -- one last thought"
- Tone: Genuinely gracious, zero pressure
- Brief summary of what you've shared (one sentence)
- Acknowledge timing may not be right
- Leave the door open: "If [pain point] becomes a priority, I'm an easy person to reach"
- Optional: Share one final compelling data point or insight

**Rules**:
- Under 80 words
- Must feel like a real human closing out, not a guilt trip
- No "I've tried reaching you X times"
- End on a positive, forward-looking note

---

### Step 5: Quality Checks

Before delivering the sequence, verify:

1. **Personalization audit**: Every touch references something specific to this prospect. Remove any line that could apply to any company in the industry.
2. **Tone check**: Read each email aloud. Does it sound like a knowledgeable peer, or a salesperson? Revise if salesy.
3. **Length check**: No email exceeds its word limit.
4. **Subject line check**: No spam trigger words (free, guarantee, limited time, act now). No ALL CAPS. No excessive punctuation.
5. **CTA progression**: CTAs escalate naturally from soft to direct across the sequence.
6. **Factual accuracy**: Every referenced fact (trial, news, publication) is real and correctly attributed.
7. **Compliance**: No misleading claims about Talosix capabilities. No disparagement of competitors by name in outreach.

### Step 6: Output Format

Deliver the complete sequence in this format:

```markdown
# Outreach Sequence: [Prospect Name] at [Company]

## Prospect Context
- **Name**: [Full name]
- **Title**: [Current title]
- **Company**: [Company name]
- **ICP Score**: [If available from prior research]
- **Key Signal**: [Primary buying signal driving this outreach]
- **Core Pain Hypothesis**: [The challenge this sequence addresses]
- **Personalization Anchors**: [List of specific facts used for personalization]

## Sequence Overview
| Touch | Day | Channel | Purpose | CTA Level |
|-------|-----|---------|---------|-----------|
| 1 | 1 | Email | Insight-led open | Soft |
| 2 | 3 | LinkedIn | Connection request | None |
| 3 | 5 | Email | Value add follow-up | Medium |
| 4 | 8 | LinkedIn/Email | Thought leadership share | Soft |
| 5 | 12 | Email | Direct meeting request | Direct |
| 6 | 15 | LinkedIn | Engagement/visibility | None |
| 7 | 20 | Email | Gracious close | Open door |

## Touch 1: Initial Email (Day 1)
**Subject A**: [subject line]
**Subject B**: [subject line]

[Full email body]

---

## Touch 2: LinkedIn Connection (Day 3)
[Connection note, under 300 characters]

---

[...continue for all touches...]

## Sequence Notes
- **If they respond positively at any touch**: Stop the sequence, transition to live conversation.
- **If they respond with objection**: Address it directly, do not continue automated sequence.
- **If they connect on LinkedIn but don't respond to email**: Shift primary channel to LinkedIn.
- **Recommended follow-up after sequence**: Re-engage in 90 days with new trigger event.
```

## Talosix Messaging Guidelines

When referencing Talosix, adhere to these principles:

- **Lead with outcomes, not features**: "Reduce study build time by 60%" not "Our drag-and-drop form builder"
- **Use peer language**: "Companies like [similar org] have found..." not "Our customers say..."
- **Clinical credibility**: Reference specific regulatory standards (21 CFR Part 11, ICH E6 R2) to demonstrate domain expertise
- **Differentiation themes**: Modern cloud-native architecture, rapid study configuration, purpose-built for clinical trials, transparent pricing
- **Avoid**: Feature dump, competitive bashing, urgency tactics, generic value propositions
