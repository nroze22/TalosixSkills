---
name: seo-audit
description: Comprehensive SEO audit for Talosix web properties. Analyzes technical SEO, content SEO, on-page optimization, competitive positioning, and Generative Engine Optimization (GEO). Produces prioritized action items with effort/impact scoring tailored to the clinical trial EDC market.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
argument-hint: "[url-or-domain]"
---

# SEO Audit

## Purpose

Conduct a comprehensive SEO audit of a web property (typically talosix.com or a competitor site) across technical, content, on-page, competitive, and generative AI dimensions. Produce a prioritized action plan with effort/impact scoring relevant to the clinical trial EDC market.

## Workflow

### Step 1: Site Discovery and Baseline

Parse the input URL or domain. Establish the scope:

- Primary domain and subdomains to audit
- Current indexed page count estimate
- Site technology stack (use WebFetch on the homepage and check response headers, meta tags, script references)

**Initial reconnaissance**:
- WebFetch the homepage and 3-5 key pages (product, about, blog, pricing, contact)
- WebSearch: `site:[domain]` to estimate indexed pages
- WebSearch: `"[domain]" OR "[brand name]"` to assess brand search presence
- WebSearch: `[domain] site:ahrefs.com OR site:semrush.com OR site:moz.com` for any publicly available SEO data

### Step 2: Technical SEO Audit

Analyze the site's technical foundation:

#### 2A: Crawlability and Indexation

Use WebFetch to check:
- `[domain]/robots.txt` -- Review directives, blocked paths, sitemap references
- `[domain]/sitemap.xml` -- Verify existence, structure, freshness, page count
- HTTP status codes on key pages (200, 301, 404, 500)
- Canonical tag implementation (check 5-10 pages)
- Meta robots directives (index/noindex, follow/nofollow)

**Findings format**:
| Check | Status | Issue | Priority |
|-------|--------|-------|----------|
| robots.txt | [Pass/Fail/Warning] | [Description] | [High/Med/Low] |
| Sitemap | [Pass/Fail/Warning] | [Description] | [High/Med/Low] |
| Canonical tags | [Pass/Fail/Warning] | [Description] | [High/Med/Low] |

#### 2B: Site Architecture

Analyze from crawled pages:
- URL structure (clean, hierarchical, keyword-informed)
- Internal linking patterns
- Navigation depth (clicks from homepage to deepest content)
- Breadcrumb implementation
- Pagination handling (rel=next/prev, or alternative)
- Faceted navigation issues (if applicable)

#### 2C: Page Speed and Core Web Vitals

Use WebSearch to find:
- `site:pagespeed.web.dev "[domain]"` or publicly reported scores
- Any Lighthouse or PageSpeed data available

Assess from page source:
- Large unoptimized images
- Render-blocking JavaScript/CSS
- Third-party script bloat
- Lazy loading implementation
- CDN usage indicators

#### 2D: Mobile and Accessibility

From WebFetch responses, check:
- Viewport meta tag
- Responsive design indicators
- Touch target sizing (from source inspection)
- Font size readability
- Mobile-specific navigation

#### 2E: Security and Trust

- HTTPS implementation (redirect from HTTP)
- HSTS header presence
- Content Security Policy
- Trust signals visible on site (certifications, compliance badges)

### Step 3: Content SEO Audit

Analyze the site's content strategy and execution:

#### 3A: Clinical Trial Keyword Universe

Evaluate coverage of the core keyword universe for clinical trial EDC:

**Tier 1 Keywords (highest intent)**:
| Keyword | Monthly Search Vol (est.) | Page Targeting It? | Current Ranking (if known) |
|---------|--------------------------|---------------------|---------------------------|
| electronic data capture | High | | |
| EDC clinical trials | High | | |
| eCRF software | Medium | | |
| clinical data management system | High | | |
| CDMS software | Medium | | |
| 21 CFR Part 11 compliant EDC | Medium | | |
| clinical trial software | High | | |
| eClinical platform | Medium | | |

**Tier 2 Keywords (informational/awareness)**:
| Keyword | Category |
|---------|----------|
| clinical trial data collection | Process |
| study build EDC | Process |
| electronic case report form | Definition |
| clinical data management best practices | Educational |
| EDC system comparison | Comparison |
| Medidata alternative | Competitive |
| Veeva EDC alternative | Competitive |
| decentralized clinical trial technology | Trend |
| risk-based monitoring EDC | Feature |
| clinical trial audit trail | Compliance |

**Tier 3 Keywords (long-tail/niche)**:
| Keyword | Category |
|---------|----------|
| EDC for phase I trials | Segment |
| EDC for rare disease trials | Segment |
| EDC implementation timeline | Buyer journey |
| clinical data management outsourcing vs. in-house | Buyer journey |
| GxP cloud validation | Technical |
| CDISC standards EDC | Technical |
| edit check programming | Technical |

Use WebSearch to check:
- Which keywords the site currently appears for (search each and look for the domain)
- What content exists targeting each keyword cluster
- Gaps where no content exists

#### 3B: Content Quality and E-E-A-T

Evaluate content against Google's E-E-A-T signals (Experience, Expertise, Authoritativeness, Trustworthiness):

**Experience**:
- Does content reflect real-world clinical trial experience?
- Are case studies and examples from actual implementations?
- Author bios with relevant clinical/regulatory background?

**Expertise**:
- Technical depth appropriate for the audience (clinical ops, data managers)
- Accurate use of regulatory terminology (ICH, GCP, 21 CFR Part 11)
- Detailed enough to be genuinely useful vs. surface-level marketing

**Authoritativeness**:
- Author credentials and industry recognition
- External citations and backlinks from authoritative sources
- Mentions in industry publications (Applied Clinical Trials, SCDM, DIA)

**Trustworthiness**:
- Contact information visible
- Privacy policy, terms of service
- Customer testimonials with real names/companies
- Security and compliance certifications displayed

#### 3C: Content Gap Analysis

Identify content that should exist but does not:

- **vs. competitor pages**: "[competitor] vs Talosix" or alternative comparison pages
- **use case pages**: By therapeutic area, trial phase, organization type
- **resource hub**: Guides, templates, calculators, glossaries
- **regulatory content**: Compliance guides for different regulatory frameworks
- **integration content**: Pages for each integration partner or system type
- **industry trend content**: Decentralized trials, AI in clinical data, risk-based monitoring

### Step 4: On-Page SEO Audit

For each key page examined via WebFetch, evaluate:

#### 4A: Meta Tags

| Page | Title Tag | Meta Description | Issues |
|------|-----------|------------------|--------|
| Homepage | [content, char count] | [content, char count] | [too long/short, missing keyword, duplicate] |
| Product | | | |
| Blog Index | | | |
| [Other key pages] | | | |

**Title tag rules**: 50-60 characters, primary keyword near front, brand at end.
**Meta description rules**: 150-160 characters, includes CTA, unique per page.

#### 4B: Heading Structure

For each key page:
- H1 tag (should be exactly one, contain primary keyword)
- H2-H6 hierarchy (logical nesting, keyword variation)
- Heading content quality (descriptive vs. generic)

#### 4C: Schema / Structured Data

Check for implementation of:
- Organization schema
- Product schema (for EDC product pages)
- Article schema (for blog posts)
- FAQ schema (for FAQ pages or sections)
- BreadcrumbList schema
- Review/Rating schema (if applicable)
- HowTo schema (for process-oriented content)

Use WebFetch to examine page source for JSON-LD, microdata, or RDFa.

#### 4D: Internal Linking

- Do key pages receive sufficient internal links?
- Is anchor text descriptive and keyword-relevant?
- Are there orphaned pages (no internal links pointing to them)?
- Is the blog linking to product/conversion pages?

#### 4E: Image Optimization

- Alt text presence and quality
- File naming conventions
- Format optimization (WebP, AVIF)
- Responsive image implementation

### Step 5: Competitive SEO Analysis

Compare the target site against 3-5 competitors:

#### 5A: Keyword Overlap and Gaps

For each major keyword cluster, determine which competitors rank and where gaps exist:

Use WebSearch for each Tier 1 keyword and record which competitors appear in results.

#### 5B: Content Strategy Comparison

| Dimension | Target Site | Medidata | Veeva | Oracle | Castor |
|-----------|------------|----------|-------|--------|--------|
| Blog frequency | | | | | |
| Content types | | | | | |
| Keyword focus | | | | | |
| E-E-A-T signals | | | | | |
| Resource hub | | | | | |

#### 5C: Backlink Profile Indicators

Use WebSearch to find:
- Sites linking to competitors but not to the target
- Industry directories and listings presence
- Analyst report mentions
- Conference/event page links
- Educational institution links

### Step 6: Generative Engine Optimization (GEO)

Evaluate readiness for AI-generated search results (Google AI Overviews, Bing Copilot, Perplexity, ChatGPT search):

#### 6A: AI Overview Presence

Test with WebSearch:
- Search key queries and note if the site appears in AI-generated summaries
- `what is the best EDC for clinical trials`
- `electronic data capture comparison`
- `clinical trial software for biotech`
- `21 CFR Part 11 compliant EDC systems`
- `EDC system selection criteria`

#### 6B: GEO Optimization Factors

Evaluate content against factors that influence AI citation:

| Factor | Status | Recommendation |
|--------|--------|----------------|
| **Structured data** | | Schema markup for products, articles, FAQs |
| **Clear definitions** | | Does content directly answer "What is [concept]?" queries |
| **Authoritative sourcing** | | Citations to regulations, standards, research |
| **Concise summaries** | | TL;DR sections that AI can extract |
| **Comparison content** | | Structured comparisons AI can reference |
| **FAQ format** | | Question-and-answer format sections |
| **Data and statistics** | | Quotable metrics and benchmarks |
| **Unique insights** | | Original research, proprietary data, expert opinions |

#### 6C: AI Discoverability Strategy

Recommendations for increasing presence in AI-generated results:
- Create "definitive guide" content for key topics
- Structure content with clear Q&A sections
- Include data tables and structured comparisons
- Publish original research and benchmarks
- Build topical authority through comprehensive content clusters
- Ensure factual accuracy and cite authoritative sources

### Step 7: Prioritized Action Plan

Score every finding on two dimensions:

**Impact**: How much will fixing this improve SEO performance?
- 5 = Transformative (can significantly improve rankings/traffic)
- 4 = High (meaningful improvement expected)
- 3 = Medium (moderate improvement)
- 2 = Low (incremental improvement)
- 1 = Minimal (nice-to-have, best practice)

**Effort**: How much work is required to implement?
- 1 = Quick win (< 1 day, no dev resources)
- 2 = Easy (1-3 days, minimal dev)
- 3 = Medium (1-2 weeks, some dev)
- 4 = Significant (2-4 weeks, dev + content)
- 5 = Major project (1+ months, cross-functional)

### Step 8: Output Format

```markdown
# SEO Audit Report: [Domain]

**Date**: [Date]
**Auditor**: Talosix SEO Audit Skill
**Pages Analyzed**: [count]
**Overall SEO Health Score**: [X/100]

## Executive Summary
[5-7 sentences summarizing the most important findings and top 3 priorities]

## Health Scorecard

| Category | Score | Grade | Key Issue |
|----------|-------|-------|-----------|
| Technical SEO | /25 | [A-F] | |
| Content SEO | /25 | [A-F] | |
| On-Page SEO | /25 | [A-F] | |
| Competitive Position | /15 | [A-F] | |
| GEO Readiness | /10 | [A-F] | |
| **Total** | **/100** | | |

## Technical SEO Findings
[Detailed findings organized by subcategory]

## Content SEO Findings
[Keyword coverage, E-E-A-T assessment, content gaps]

## On-Page SEO Findings
[Meta tags, headings, schema, internal linking]

## Competitive Analysis
[Positioning vs. competitors with keyword overlap]

## GEO Assessment
[AI overview presence, optimization recommendations]

## Prioritized Action Plan

### Quick Wins (High Impact, Low Effort)
| # | Action | Impact | Effort | Details |
|---|--------|--------|--------|---------|
| 1 | | 5 | 1 | |

### Strategic Priorities (High Impact, Medium-High Effort)
| # | Action | Impact | Effort | Details |
|---|--------|--------|--------|---------|
| 1 | | 5 | 3 | |

### Foundation Work (Medium Impact, Builds Over Time)
| # | Action | Impact | Effort | Details |
|---|--------|--------|--------|---------|
| 1 | | 3 | 3 | |

### Backlog (Lower Priority)
| # | Action | Impact | Effort | Details |
|---|--------|--------|--------|---------|
| 1 | | 2 | 2 | |

## 90-Day SEO Roadmap
### Month 1: Foundation
[Priority actions for immediate implementation]

### Month 2: Content
[Content creation and optimization priorities]

### Month 3: Authority
[Link building, PR, and authority development]

## Sources and Methodology
[How the audit was conducted, tools used, limitations]
```

## Audit Standards

- Base recommendations on observable evidence, not assumptions.
- Distinguish between critical issues (blocking SEO performance), important issues (limiting performance), and nice-to-haves.
- Tailor recommendations to the clinical trial EDC market context -- generic SEO advice is less valuable than industry-specific guidance.
- Be honest about limitations: this audit uses publicly available data and does not have access to Google Search Console, analytics, or server logs.
- Check the local workspace for any prior SEO work or content strategy documents before starting.
