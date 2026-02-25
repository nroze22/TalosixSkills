---
name: find-skill
description: Find the right Talosix skill for any task. Describes what you want to do and get a recommendation for which skill(s) to use. Say "list all" to see every available skill organized by department.
argument-hint: "[what-you-want-to-do]"
---

# Find the Right Skill

Analyze what the user wants to accomplish and recommend the best skill(s).

## How to Respond

1. Understand the user's goal from $ARGUMENTS
2. Recommend 1-3 skills with a one-sentence explanation of each
3. If skills should be chained, show the recommended order
4. Offer to invoke the first skill immediately

## If User Says "list all" or "show all"

Display this catalog:

### Product Management (12)
`/product-prd` - Generate PRDs | `/user-interview-analysis` - Analyze interviews | `/roadmap-planning` - Create roadmaps | `/business-case` - Build business cases | `/feature-scoring` - Prioritize features | `/stakeholder-alignment` - RACI & alignment | `/competitor-analysis` - Competitive analysis | `/customer-feedback-synthesis` - Synthesize feedback | `/sprint-planning` - Plan sprints | `/backlog-grooming` - Refine backlog | `/release-notes` - Generate release notes | `/acceptance-criteria` - Write acceptance criteria

### Engineering (14)
`/code-review` - Review code | `/refactoring-guide` - Safe refactoring | `/technical-design` - Design docs | `/api-design` - API design | `/database-migration` - DB migrations | `/technical-debt-tracking` - Track debt | `/performance-optimization` - Optimize perf | `/security-audit` - Security review | `/cicd-pipeline` - CI/CD design | `/documentation-generation` - Generate docs | `/dependency-upgrade` - Upgrade deps | `/error-handling-strategy` - Error patterns | `/debugging-guide` - Debug methodology | `/load-testing` - Load test plans

### QA & Testing (6)
`/test-case-generation` - Test cases from stories | `/uat-test-generation` - UAT for CROs | `/playwright-test-generation` - Playwright scripts | `/test-strategy` - Test strategies | `/test-planning` - Plan test cycles | `/zephyr-test-sync` - Zephyr Scale sync

### Sales (10)
`/sales-rfp-response` - RFP responses | `/demo-script` - Demo scripts | `/sales-proposal` - Proposals | `/competitive-battlecard` - Battlecards | `/discovery-call-prep` - Call prep | `/deal-risk-assessment` - Deal health | `/roi-calculator` - ROI models | `/contract-review` - Review contracts | `/reference-matching` - Match references | `/sales-forecast` - Pipeline forecast

### Marketing - Content (8)
`/marketing-orchestrator` - Routes to right marketing skill | `/brand-voice` - Voice consistency (auto) | `/content-atomizer` - One piece -> many formats | `/one-pager-generator` - Sell sheets | `/social-media-transformer` - Social posts | `/blog-post-engine` - SEO blog posts | `/direct-response-copy` - Conversion copy | `/lead-magnet-creator` - Lead magnets

### Marketing - Core (10)
`/marketing-content` - General content | `/case-study-creation` - Case studies | `/email-campaign` - Email sequences | `/landing-page-copy` - Landing pages | `/webinar-planning` - Webinars | `/seo-content` - SEO content | `/white-paper` - White papers | `/press-release` - Press releases | `/ad-copy` - Ad copy | `/content-calendar` - Content calendars

### Lead Gen & Outreach (6)
`/linkedin-prospect-research` - Prospect research | `/outreach-sequence-generator` - Outreach emails | `/abm-account-research` - ABM deep dives | `/competitive-intelligence-scraper` - Competitive intel | `/seo-audit` - SEO audits | `/lead-research-assistant` - Find & qualify leads

### Regulatory & Compliance (8)
`/validation-protocol` - IQ/OQ/PQ protocols | `/change-control` - Change control | `/risk-assessment` - FMEA risk assessment | `/traceability-matrix` - RTM | `/regulatory-research` - FDA/ICH research | `/sop-generation` - Write SOPs | `/audit-preparation` - Audit prep | `/deploy-plan` - GxP deployment plans

### Release & Deployment (4)
`/deploy-checklist` - Pre-deploy checks | `/rollback-plan` - Rollback plans | `/release-monitoring` - Post-release monitoring | `/hotfix-procedure` - Emergency hotfixes

### Operations (3)
`/incident-response` - Incident management | `/retrospective` - Sprint retros | `/meeting-agent` - Meeting notes & actions

### Design & UX (7)
`/talosix-design-system` - Design tokens & patterns (auto) | `/prd-to-wireframe` - PRD to prototype | `/figma-to-code` - Figma to React | `/user-flow-generator` - User flow diagrams | `/design-qa` - Design QA checks | `/figma-component-generator` - Component specs | `/design-sprint-prototype` - Rapid prototyping

### Finance (1)
`/reconcile-payments` - Payment reconciliation

### HR (3)
`/job-description` - Job descriptions | `/onboarding-checklist` - Onboarding | `/policy-document` - Policy docs
