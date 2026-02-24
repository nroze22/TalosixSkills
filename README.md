# Talosix Skills Library

**94 Claude Code & Claude Desktop skills for clinical trial EDC teams.**

Built for [Talosix](https://talosix.com) — covering product management, engineering, QA, sales, marketing, lead generation, regulatory compliance, design, operations, finance, and HR.

---

## Quick Start

### Option A: Clone into your project (Claude Code)

```bash
# Clone this repo
git clone https://github.com/nroze22/TalosixSkills.git

# Copy skills into your project
cp -r TalosixSkills/.claude/skills/ /path/to/your/project/.claude/skills/
```

Every team member using Claude Code in that project instantly gets all 94 skills.

### Option B: Install as personal skills (all projects)

```bash
# Clone and copy to your personal skills directory
git clone https://github.com/nroze22/TalosixSkills.git
cp -r TalosixSkills/.claude/skills/ ~/.claude/skills/
```

Skills are now available in every project on your machine.

### Option C: Claude Teams Admin (org-wide)

Upload individual skills via the Claude Teams admin UI at [console.anthropic.com](https://console.anthropic.com). Each skill is a standalone `SKILL.md` file in its own directory under `.claude/skills/`.

---

## How Skills Work

Type `/skill-name` in Claude Code or Claude Desktop to invoke a skill. For example:
- `/product-prd` — Generate a Product Requirements Document
- `/code-review` — Review code for quality, security, and compliance
- `/test-case-generation` — Generate test cases from Jira stories

**Don't know which skill to use?** Type `/find-skill [what you want to do]` and Claude will recommend the right skill(s).

**Auto-routing:** The `skill-router` runs silently in the background — just describe what you need and Claude will automatically pick the right skill without you asking.

---

## All 94 Skills by Department

### Product Management (12 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Product PRD | `/product-prd` | Generate comprehensive PRDs with regulatory considerations |
| User Interview Analysis | `/user-interview-analysis` | Analyze interview transcripts, extract themes and personas |
| Roadmap Planning | `/roadmap-planning` | Create roadmaps for different audiences |
| Business Case | `/business-case` | Build business cases with ROI projections |
| Feature Scoring | `/feature-scoring` | Prioritize features using RICE, MoSCoW, weighted scoring |
| Stakeholder Alignment | `/stakeholder-alignment` | RACI matrices, decision logs, communication plans |
| Competitor Analysis | `/competitor-analysis` | Analyze EDC competitors (Medidata, Veeva, Oracle, etc.) |
| Customer Feedback | `/customer-feedback-synthesis` | Synthesize feedback from multiple sources |
| Sprint Planning | `/sprint-planning` | Plan sprints with story point estimation |
| Backlog Grooming | `/backlog-grooming` | Refine backlog, add acceptance criteria |
| Release Notes | `/release-notes` | Generate release notes from Jira/git history |
| Acceptance Criteria | `/acceptance-criteria` | Detailed acceptance criteria with test scenarios |

### Engineering (14 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Code Review | `/code-review` | Review for quality, security, 21 CFR Part 11 compliance |
| Refactoring Guide | `/refactoring-guide` | Safe refactoring in validated systems |
| Technical Design | `/technical-design` | Architecture and design documents |
| API Design | `/api-design` | RESTful API design with clinical patterns |
| Database Migration | `/database-migration` | Zero-downtime PostgreSQL migrations |
| Tech Debt Tracking | `/technical-debt-tracking` | Identify, document, and prioritize tech debt |
| Performance Optimization | `/performance-optimization` | Analyze and optimize application performance |
| Security Audit | `/security-audit` | OWASP, HIPAA, 21 CFR Part 11 security review |
| CI/CD Pipeline | `/cicd-pipeline` | Design GxP-compliant CI/CD pipelines |
| Documentation Generation | `/documentation-generation` | Generate technical docs from code |
| Dependency Upgrade | `/dependency-upgrade` | Plan and execute dependency upgrades |
| Error Handling | `/error-handling-strategy` | Design error handling patterns |
| Debugging Guide | `/debugging-guide` | Systematic debugging methodology |
| Load Testing | `/load-testing` | Design and execute load tests for EDC systems |

### QA & Testing (6 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Test Case Generation | `/test-case-generation` | Generate test cases from Jira stories (Zephyr Scale format) |
| UAT Test Generation | `/uat-test-generation` | UAT suites for CRO and sponsor users |
| Playwright Tests | `/playwright-test-generation` | Automated Playwright test scripts |
| Test Strategy | `/test-strategy` | Comprehensive test strategies with IQ/OQ/PQ |
| Test Planning | `/test-planning` | Plan test cycles with resource allocation |
| Zephyr Test Sync | `/zephyr-test-sync` | Sync test artifacts with Zephyr Scale |

### Sales & Business Development (10 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| RFP Response | `/sales-rfp-response` | Generate RFP responses for clinical trial EDC |
| Demo Script | `/demo-script` | Tailored demo scripts by audience |
| Sales Proposal | `/sales-proposal` | Professional proposals with ROI projections |
| Competitive Battlecard | `/competitive-battlecard` | Sales-focused competitive battlecards |
| Discovery Call Prep | `/discovery-call-prep` | Prepare for discovery calls |
| Deal Risk Assessment | `/deal-risk-assessment` | Assess deal health and risk |
| ROI Calculator | `/roi-calculator` | Custom ROI calculators for prospects |
| Contract Review | `/contract-review` | Review clinical trial software contracts |
| Reference Matching | `/reference-matching` | Match reference customers to prospects |
| Sales Forecast | `/sales-forecast` | Pipeline forecast and analysis |

### Marketing — Advanced Content (8 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Marketing Orchestrator | `/marketing-orchestrator` | Routes to the right marketing sub-skill chain |
| Brand Voice | *(auto-loaded)* | Maintains consistent Talosix voice |
| Content Atomizer | `/content-atomizer` | Transform one piece into 20+ formats |
| One-Pager Generator | `/one-pager-generator` | Professional sell sheets and one-pagers |
| Social Media Transformer | `/social-media-transformer` | Platform-optimized social posts |
| Blog Post Engine | `/blog-post-engine` | SEO-optimized blog posts with distribution plan |
| Direct Response Copy | `/direct-response-copy` | High-converting copy (PAS, AIDA, StoryBrand) |
| Lead Magnet Creator | `/lead-magnet-creator` | Create lead magnets with landing page + nurture sequence |

### Marketing — Core (10 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Marketing Content | `/marketing-content` | General marketing content creation |
| Case Study | `/case-study-creation` | Customer case studies |
| Email Campaign | `/email-campaign` | Email sequences and nurture campaigns |
| Landing Page Copy | `/landing-page-copy` | Conversion-optimized landing pages |
| Webinar Planning | `/webinar-planning` | Plan and promote webinars |
| SEO Content | `/seo-content` | Keyword-optimized content |
| White Paper | `/white-paper` | Research-backed white papers |
| Press Release | `/press-release` | AP style press releases |
| Ad Copy | `/ad-copy` | Google, LinkedIn, and publication ads |
| Content Calendar | `/content-calendar` | Monthly/quarterly content planning |

### Lead Generation & Outreach (6 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| LinkedIn Prospect Research | `/linkedin-prospect-research` | Research and score prospects |
| Outreach Sequence | `/outreach-sequence-generator` | Personalized multi-touch outreach |
| ABM Account Research | `/abm-account-research` | Deep account-based marketing research |
| Competitive Intel | `/competitive-intelligence-scraper` | Gather competitive intelligence |
| SEO Audit | `/seo-audit` | Comprehensive SEO + GEO audit |
| Lead Research | `/lead-research-assistant` | Find and qualify leads |

### Regulatory & Compliance (8 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Validation Protocol | `/validation-protocol` | IQ/OQ/PQ protocols (GAMP 5) |
| Change Control | `/change-control` | Change control for validated systems |
| Risk Assessment | `/risk-assessment` | FMEA risk assessment |
| Traceability Matrix | `/traceability-matrix` | Requirements traceability (URS → FS → DS → TC) |
| Regulatory Research | `/regulatory-research` | FDA guidance, ICH guidelines research |
| SOP Generation | `/sop-generation` | Standard Operating Procedures |
| Audit Preparation | `/audit-preparation` | FDA, EMA, sponsor audit prep |
| Deploy Plan | `/deploy-plan` | GxP deployment plans |

### Release & Deployment (4 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Deploy Checklist | `/deploy-checklist` | Pre-deployment verification |
| Rollback Plan | `/rollback-plan` | Rollback plans with data integrity checks |
| Release Monitoring | `/release-monitoring` | Post-release monitoring plan |
| Hotfix Procedure | `/hotfix-procedure` | Emergency hotfix process |

### Operations (3 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Incident Response | `/incident-response` | Production incident management |
| Retrospective | `/retrospective` | Sprint and release retros |
| Meeting Agent | `/meeting-agent` | Process meeting notes, extract action items |

### Design & UX (7 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Talosix Design System | *(auto-loaded)* | Full design tokens, component patterns, Stitch specs |
| PRD to Wireframe | `/prd-to-wireframe` | Convert PRDs into interactive prototypes |
| Figma to Code | `/figma-to-code` | Convert Figma designs to React/TypeScript |
| User Flow Generator | `/user-flow-generator` | Mermaid user flow diagrams from Jira stories |
| Design QA | `/design-qa` | Compare implementation vs Figma specs |
| Component Generator | `/figma-component-generator` | Figma-ready component specifications |
| Design Sprint Prototype | `/design-sprint-prototype` | Rapid prototyping for design sprints |

### Finance (1 skill)

| Skill | Command | Description |
|-------|---------|-------------|
| Payment Reconciliation | `/reconcile-payments` | Reconcile invoices and flag anomalies |

### HR & People Ops (3 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Job Description | `/job-description` | Clinical trial tech job descriptions |
| Onboarding Checklist | `/onboarding-checklist` | Role-customized onboarding with GxP training |
| Policy Document | `/policy-document` | HR policies and internal documents |

### Meta / Discovery (2 skills)

| Skill | Command | Description |
|-------|---------|-------------|
| Skill Router | *(auto-loaded)* | Automatically detects intent and picks the right skill |
| Find Skill | `/find-skill` | Ask for help finding the right skill |

---

## Recommended Skill Chains

These multi-skill workflows are pre-configured in the orchestrator skills:

| Workflow | Skills (in order) |
|----------|-------------------|
| **Launch Campaign** | `/marketing-orchestrator` → `/lead-magnet-creator` → `/direct-response-copy` → `/email-campaign` → `/content-atomizer` → `/social-media-transformer` |
| **Content Blitz** | `/blog-post-engine` → `/content-atomizer` → `/social-media-transformer` → `/content-calendar` |
| **Prospect Outreach** | `/linkedin-prospect-research` → `/abm-account-research` → `/outreach-sequence-generator` |
| **Full Deployment** | `/deploy-plan` → `/deploy-checklist` → `/rollback-plan` → `/release-monitoring` |
| **Audit Prep** | `/audit-preparation` → `/traceability-matrix` → `/validation-protocol` → `/regulatory-research` |
| **New Feature (Full Cycle)** | `/product-prd` → `/acceptance-criteria` → `/technical-design` → `/prd-to-wireframe` → `/test-case-generation` |
| **Release QA** | `/test-strategy` → `/test-planning` → `/test-case-generation` → `/uat-test-generation` → `/zephyr-test-sync` |

---

## MCP Server Integrations

These skills are designed to work with the following MCP servers:

| MCP Server | Used By | Purpose |
|------------|---------|---------|
| **Atlassian** (Jira + Confluence) | Product, Engineering, QA, Operations | Pull stories, create tickets, read PRDs |
| **Figma** (official remote MCP) | Design skills | Read designs, push Code to Canvas |
| **GitHub** | Engineering, Release | PRs, code review, branch management |
| **PostgreSQL** | Engineering, Finance | Direct database queries |
| **Zephyr Scale** | QA skills | Test case management |
| **Slack / Teams** | Operations, Marketing | Notifications, channel posting |
| **WebSearch** | Marketing, Sales, Lead Gen | Research, competitive intel |
| **PubMed / ClinicalTrials.gov** | Regulatory, Sales | Clinical trial research |

---

## Regulatory Context

All skills are built with clinical trial regulatory awareness:

- **21 CFR Part 11** — Electronic records and signatures
- **EU Annex 11** — Computerised systems in GxP environments
- **ICH E6(R2) GCP** — Good Clinical Practice
- **HIPAA** — PHI protection
- **GDPR** — EU data protection
- **GAMP 5** — Risk-based validation
- **ALCOA+** — Data integrity principles

---

## Directory Structure

```
.claude/
  skills/
    product-prd/
      SKILL.md          ← Each skill is a directory with a SKILL.md file
    code-review/
      SKILL.md
    test-case-generation/
      SKILL.md
    ... (94 total)
CLAUDE.md                ← Project context loaded automatically
README.md                ← This file
```

---

## Contributing

1. Create a new directory under `.claude/skills/your-skill-name/`
2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`) and markdown instructions
3. Keep skills under 500 lines — move detailed references to separate files
4. Test your skill by typing `/your-skill-name` in Claude Code
5. Submit a PR

---

## License

Internal use — Talosix team only.
