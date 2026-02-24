---
name: Product Requirements Document Generator
description: Generate comprehensive PRDs for Talosix clinical trial EDC features, including regulatory considerations, user stories, and technical requirements.
allowed-tools:
  - mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql
  - mcp__claude_ai_Atlassian__getJiraIssue
  - mcp__claude_ai_Atlassian__searchConfluenceUsingCql
  - mcp__claude_ai_Atlassian__getConfluencePage
  - mcp__claude_ai_Atlassian__createConfluencePage
argument-hint: "[Feature name or area, e.g. 'ePRO module', 'edit check builder', 'randomization engine']"
---

# Product Requirements Document Generator

Generate structured, comprehensive PRDs for Talosix EDC platform features. All PRDs must account for clinical trial regulatory requirements and the validated system environment.

## Workflow

1. **Gather Context**: Ask the user for the feature name/area. Then pull relevant context:
   - Search Jira for related epics, stories, and feature requests using `searchJiraIssuesUsingJql`
   - Search Confluence for existing specs, research, or prior PRDs using `searchConfluenceUsingCql`
   - Ask the user for any additional context (customer requests, competitive pressure, regulatory drivers)

2. **Generate the PRD** using the template below.

3. **Offer to publish** the PRD to Confluence using `createConfluencePage`.

## PRD Template

Structure every PRD with these sections:

### Header
- **Title**: PRD: [Feature Name]
- **Author**: [User name]
- **Date**: [Current date]
- **Status**: Draft | In Review | Approved
- **Target Release**: [Version/Quarter]
- **Stakeholders**: List key stakeholders (PM, Engineering, QA, Regulatory, Clinical)

### 1. Problem Statement
- What problem does this solve for clinical trial stakeholders (sponsors, CROs, sites, monitors)?
- Who experiences this problem and how frequently?
- What is the cost of not solving it (delays, errors, compliance risk)?

### 2. Goals and Success Metrics
- Primary goal and measurable outcomes
- Key metrics: adoption rate, time savings, error reduction, compliance improvement
- EDC-specific metrics: data entry time, query resolution time, database lock timeline

### 3. User Stories
Write user stories for each persona:
- **Site Coordinator**: "As a site coordinator, I want to... so that..."
- **Clinical Data Manager**: "As a CDM, I want to... so that..."
- **Medical Monitor**: "As a medical monitor, I want to..."
- **Sponsor/CRO Project Manager**: "As a sponsor PM, I want to..."
- **System Administrator**: "As an admin, I want to..."

### 4. Functional Requirements
- Numbered list of requirements (REQ-001, REQ-002, etc.)
- Each requirement: description, priority (Must/Should/Could), and rationale
- Include data model changes, API requirements, UI changes

### 5. Regulatory Considerations
Address these for every feature:
- **21 CFR Part 11 compliance**: Electronic signatures, audit trails, access controls
- **ICH E6(R2) GCP**: Data integrity, source data verification
- **EU Annex 11**: Computerized systems validation requirements
- **GDPR/Privacy**: Personal data handling, consent, data subject rights
- **Audit trail requirements**: What actions must be logged with timestamps and user IDs
- **Electronic signature requirements**: Does this feature require Part 11 compliant e-signatures?
- **Data integrity (ALCOA+)**: Attributable, Legible, Contemporaneous, Original, Accurate

### 6. Technical Requirements
- System architecture impacts
- Integration points (CDASH, CDISC ODM, HL7 FHIR, third-party labs, IRT)
- Performance requirements (concurrent users, response times)
- Data migration considerations
- API specifications (REST endpoints, payload schemas)

### 7. UX/UI Requirements
- Wireframes or mockup descriptions
- Accessibility requirements (WCAG 2.1 AA)
- Multi-language/localization needs
- Mobile/tablet considerations for site users

### 8. Non-Functional Requirements
- Performance: page load times, concurrent user support
- Security: role-based access, encryption, session management
- Scalability: multi-study, multi-site deployment
- Availability: uptime SLA targets
- Validation: IQ/OQ/PQ requirements, test documentation

### 9. Dependencies and Risks
- Technical dependencies (infrastructure, third-party services)
- Cross-team dependencies
- Regulatory risks and mitigation strategies
- Timeline risks

### 10. Release and Rollout Plan
- Phased rollout strategy
- Validation and UAT requirements
- Training needs for site users, data managers, monitors
- Documentation updates (user guides, validation docs, SOPs)

### 11. Open Questions
- List unresolved questions with owners and target resolution dates

## Domain Guidance

When generating PRDs for common EDC features, include these domain-specific considerations:

- **eCRF Design**: CDASH standards, visit scheduling, conditional branching, edit checks
- **Data Import/Export**: CDISC ODM, SAS datasets, CSV, API integrations
- **Query Management**: auto-queries, manual queries, query workflows, resolution tracking
- **Randomization**: IWRS integration, stratification, blinding requirements
- **ePRO/eCOA**: device provisioning, offline capability, compliance windows
- **Safety/SAE**: MedDRA coding, expedited reporting timelines, E2B(R3)
- **Monitoring**: remote monitoring, SDV workflows, risk-based monitoring support
- **Reporting**: study dashboards, enrollment tracking, data quality metrics
