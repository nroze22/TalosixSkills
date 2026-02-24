---
name: social-media-transformer
description: "Transform any content into platform-optimized social media posts for LinkedIn, Twitter/X with A/B variants, clinical trial hashtag strategy, engagement predictions, and optimal posting schedules for clinical operations audiences."
argument-hint: [content-to-transform]
allowed-tools: WebSearch, WebFetch
---

# Social Media Transformer for Talosix EDC

## Purpose

Convert any piece of content -- a blog post, product update, case study, white paper, event announcement, or raw idea -- into platform-specific social media posts that drive engagement with clinical trial professionals. Every output is calibrated for the platform's algorithm, character limits, formatting norms, and the specific behavior patterns of clinical operations audiences on social media.

## Pre-Transformation Protocol

Before generating posts, analyze the source content:

1. **Identify the core message** -- What is the single most important takeaway?
2. **Extract hooks** -- What data points, questions, or contrarian takes will stop the scroll?
3. **Map the audience** -- Which clinical trial personas will care about this? (Clinical Ops, Data Management, CRO, IT/Validation, Biostat, Site Staff)
4. **Determine the goal** -- Awareness, engagement, traffic, lead generation, or thought leadership?
5. **Load brand-voice skill** -- Apply Talosix voice constraints to all outputs

## Platform 1: LinkedIn

### Format Specifications

- **Character limit:** 3,000 characters maximum; optimal engagement at 800-1,300 characters
- **Line breaks:** Use generously. Single-line paragraphs perform best. Double line break between sections.
- **Emojis:** Minimal to none for Talosix. Clinical trial audience responds better to clean, professional formatting. Bullet points (plain text dashes or dots) are acceptable.
- **Hashtags:** 3-5 at the end of the post. Never inline.
- **Links:** Place in comments for better reach, or use one link at the end. Note in the post: "Link in comments."
- **Images/media:** Reference when a post would benefit from a supporting image, chart, or document carousel.
- **Mentions/tags:** Only tag people or organizations with genuine relevance and existing relationship.

### LinkedIn Post Patterns

#### Pattern 1: Hook-Story-Insight (HSI)

The most reliable LinkedIn format for thought leadership.

```
[HOOK: 1-2 lines that create curiosity or pattern interrupt]

[STORY: 3-5 short paragraphs telling a specific scenario, observation, or example]

[INSIGHT: 2-3 lines connecting the story to a broader lesson or principle]

[CTA: Question or invitation to engage]

#Hashtag1 #Hashtag2 #Hashtag3
```

**Hook types that work for clinical trial audience:**
- The surprising statistic: "The average clinical trial generates 3.6 million data points. Less than 2% ever get a second look."
- The observation: "I have reviewed 50+ EDC vendor evaluations this year. The same mistake shows up in almost every one."
- The contrarian statement: "Faster study build is not what you should be optimizing for."
- The direct question: "When was the last time your data managers actually used your EDC's reporting dashboard?"

#### Pattern 2: Listicle

```
[HOOK: Topic framing + count]

1. [Point one -- 1-2 sentences]

2. [Point two -- 1-2 sentences]

3. [Point three -- 1-2 sentences]

[4-7 additional points]

[CLOSING: Summary insight or question]

#Hashtags
```

#### Pattern 3: Before/After

```
[BEFORE scenario -- paint the painful status quo in 2-3 lines]

[TRANSITION: "Then we..." or "Here is what changed:"]

[AFTER scenario -- paint the improved state in 2-3 lines]

[LESSON: What this means for the reader]

#Hashtags
```

#### Pattern 4: Data Commentary

```
[STAT or DATA POINT -- large, attention-grabbing]

[CONTEXT -- Why this number matters]

[ANALYSIS -- What it means for clinical operations]

[IMPLICATION -- What should change as a result]

[QUESTION -- Invite the audience to share their data/experience]

#Hashtags
```

#### Pattern 5: Carousel Companion

For posts that link to a document carousel (PDF slides):

```
[HOOK -- what the carousel covers]

[PREVIEW -- what they will learn in the slides]

Slide 1: [Topic]
Slide 2: [Topic]
...

[CTA -- "Swipe through" or "Save for later"]

#Hashtags
```

Provide carousel slide content as a separate output (6-10 slides, one key point per slide, large text with minimal design).

### LinkedIn A/B Variant Generation

For every LinkedIn post, produce two variants:

- **Variant A:** The primary version using the best-fit pattern
- **Variant B:** Same core message, different hook or structure

Label each variant and state the hypothesis:
- "Variant A leads with a statistic; Variant B leads with a question. Testing whether data or curiosity drives more engagement for this topic."

---

## Platform 2: Twitter/X

### Format Specifications

- **Character limit:** 280 characters per tweet
- **Thread format:** Use "1/" or "1/n" numbering. First tweet must hook -- it is the only one most people see.
- **Thread length:** 4-8 tweets optimal. Beyond 8, engagement drops sharply.
- **Hashtags:** Maximum 1-2 per tweet. Integrate naturally or place at thread end.
- **Links:** Include in the last tweet of the thread or as a reply to the thread.
- **Quote tweets:** Write 1-2 standalone tweets designed to be quoted by others (self-contained insights).

### Twitter/X Thread Patterns

#### Pattern 1: The Breakdown Thread

```
1/ [Hook -- the boldest version of the core message. Must work standalone.]

2/ [Context -- set the scene or define the problem]

3-5/ [Supporting points -- one per tweet, each self-contained]

6/ [The "so what" -- what should the reader do with this information]

7/ [CTA -- link to full content or ask for engagement]
```

#### Pattern 2: The Hot Take Thread

```
1/ [Contrarian or provocative opening statement]

2/ [Why the conventional wisdom is wrong]

3-4/ [Evidence or examples supporting the take]

5/ [The nuanced conclusion -- not just contrarian, but constructive]

6/ [Invitation for debate or agreement]
```

#### Pattern 3: The Quick Tips Thread

```
1/ X things every [clinical data manager / clinical ops director] should know about [topic]:

2/ 1. [Tip one]
3/ 2. [Tip two]
...
n/ [Closing tip + CTA]
```

#### Pattern 4: The Story Thread

```
1/ [Setup -- a specific scenario or problem someone faced]

2-3/ [What happened -- the journey or decision]

4-5/ [The outcome -- results and metrics]

6/ [The lesson -- generalizable takeaway]

7/ [Link to full story]
```

### Twitter/X A/B Variant Generation

Produce two versions of each thread's first tweet (the hook). The rest of the thread stays the same.

- **Variant A:** Hooks with data or a specific claim
- **Variant B:** Hooks with a question or story opener

---

## Clinical Trial Hashtag Strategy

### Primary Hashtags (high relevance, moderate volume)
- #ClinicalTrials
- #EDC
- #ClinicalData
- #ClinOps
- #DataManagement

### Secondary Hashtags (broader reach, industry adjacent)
- #DigitalHealth
- #DrugDevelopment
- #ClinicalResearch
- #LifeSciences
- #Pharma

### Niche Hashtags (targeted reach, high relevance)
- #eCRF
- #CDISC
- #CDASH
- #SDTM
- #21CFRPart11
- #GCP
- #SCDM
- #PatientSafety
- #RBM (risk-based monitoring)
- #DCT (decentralized clinical trials)

### Hashtag Rules
- LinkedIn: Use 3-5 hashtags. Mix one primary + one secondary + 1-3 niche.
- Twitter: Use 1-2 maximum. Choose the most relevant.
- Never use generic hashtags (#innovation, #technology, #leadership) -- they dilute clinical credibility.
- Rotate hashtags across posts to reach different audience segments.
- Monitor which hashtags drive engagement and adjust over time.

## Engagement Prediction Framework

Rate each post's predicted engagement on three dimensions:

### Resonance Score (1-5)
How much will the clinical trial audience relate to this?
- 5: Addresses a universal daily pain point (query management, study build delays)
- 4: Addresses a common strategic challenge (vendor evaluation, compliance)
- 3: Shares an interesting insight or data point
- 2: Informational but not emotionally resonant
- 1: Generic or overly promotional

### Shareability Score (1-5)
How likely is someone to reshare or forward this?
- 5: Contains a data point or framework others will want to cite
- 4: Offers a practical tip or template
- 3: Makes a strong argument worth discussing
- 2: Interesting but not share-worthy
- 1: Self-promotional without added value

### Comment Trigger Score (1-5)
How likely is this to generate comments?
- 5: Asks a direct question about the audience's experience
- 4: Takes a mildly contrarian position inviting debate
- 3: Shares something specific that prompts "us too" responses
- 2: Informational with no natural reply entry point
- 1: No engagement trigger

**Target:** Every post should score 3+ on at least two dimensions. Discard or rework anything scoring 1-2 across all three.

## Optimal Posting Schedule

### LinkedIn Posting Times for Clinical Trial Audience

**Tier 1 (highest engagement):**
- Tuesday 8:00-9:00 AM ET
- Wednesday 7:30-8:30 AM ET
- Thursday 12:00-1:00 PM ET

**Tier 2 (strong engagement):**
- Monday 8:00-9:00 AM ET
- Tuesday 12:00-1:00 PM ET
- Wednesday 5:00-6:00 PM ET

**Avoid:**
- Friday afternoons (clinical ops teams are wrapping up the week)
- Weekends (B2B clinical trial audience is offline)
- Monday before 8 AM (inbox triage mode)

**European audience adjustment:** Add CET posting times (8:00-10:00 AM CET Tuesday-Thursday)

### Twitter/X Posting Times

**Tier 1:**
- Tuesday 10:00-11:00 AM ET
- Wednesday 1:00-2:00 PM ET
- Thursday 10:00-11:00 AM ET

**Thread posting:** Post the first tweet during Tier 1 times. Space subsequent tweets 2-3 minutes apart.

### Conference and Event Adjustments

During major clinical trial conferences (DIA Annual, SCOPE, ACRP, SCDM Annual), adjust strategy:
- Post more frequently (2x daily on LinkedIn during event days)
- Use event-specific hashtags
- Comment on event-related posts from industry voices
- Share real-time insights from sessions (if attending)
- Post pre-event content 1 week before, live content during, recap content 1 week after

## Output Deliverables

For each transformation request, provide:

1. **Source analysis** (core message, hooks identified, audience mapped)
2. **LinkedIn posts** (2-3 posts using different patterns, each with A/B variants)
3. **Twitter/X threads** (2-3 threads using different patterns, each with hook A/B variants)
4. **Engagement predictions** (resonance, shareability, comment trigger scores for each post)
5. **Posting schedule recommendation** (specific days and times for each post)
6. **Hashtag selection** (chosen hashtags with rationale for each post)
7. **Supporting content notes** (image suggestions, carousel concepts, video ideas if applicable)
8. **Engagement playbook** (how to respond to comments, what to monitor, when to boost)
