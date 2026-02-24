# Talosix - Clinical Trial EDC Platform

## Company Context
Talosix builds Electronic Data Capture (EDC) software for clinical trials. Our platform is used by CROs (Contract Research Organizations), pharmaceutical sponsors, and clinical sites.

## Regulatory Environment
- **21 CFR Part 11**: FDA regulations for electronic records and electronic signatures
- **EU Annex 11**: European equivalent for computerised systems in GxP environments
- **ICH E6(R2) GCP**: Good Clinical Practice guidelines
- **HIPAA**: Health Insurance Portability and Accountability Act (PHI protection)
- **GDPR**: General Data Protection Regulation (EU clinical trial data)
- **GAMP 5**: Risk-based validation framework for computerized systems
- **ALCOA+**: Data integrity principles (Attributable, Legible, Contemporaneous, Original, Accurate + Complete, Consistent, Enduring, Available)

## Key Domain Terms
- **eCRF**: Electronic Case Report Form - primary data entry interface
- **EDC**: Electronic Data Capture system
- **CDMS**: Clinical Data Management System
- **SDV**: Source Data Verification
- **SAE**: Serious Adverse Event
- **CDISC**: Clinical Data Interchange Standards Consortium (CDASH, SDTM, ODM)
- **CRO**: Contract Research Organization
- **CRA**: Clinical Research Associate (monitor)
- **PI**: Principal Investigator
- **CDM**: Clinical Data Manager

## Tool Stack
- **Project Management**: Jira (Atlassian)
- **Documentation**: Confluence (Atlassian)
- **Test Management**: Zephyr Scale
- **Version Control**: GitHub/Bitbucket
- **Communication**: Microsoft Teams, Slack
- **Database**: PostgreSQL
- **Research**: NotebookLM, PubMed
- **CI/CD**: GitHub Actions

## MCP Integrations
- Atlassian (Jira + Confluence): `https://talosix.atlassian.net`
- PostgreSQL: Direct database access
- Zephyr Scale: Test case management
- Microsoft Teams (planned)

## Skills Library
72 skills organized by department in `.claude/skills/`. Type `/skill-name` to invoke.

### Departments Covered
- Product Management (12 skills)
- Engineering & Development (14 skills)
- QA & Testing (6 skills)
- Sales & Business Development (10 skills)
- Marketing & Communications (10 skills)
- Regulatory & Clinical Ops (8 skills)
- Release & Deployment (4 skills)
- Operations (3 skills)
- Design & UX (1 skill)
- Finance (1 skill)
- HR & People Ops (3 skills)

## Code Standards
- All clinical data operations must maintain audit trails (who, what, when, why)
- Soft deletes only - never hard delete clinical data
- Electronic signatures require re-authentication per 21 CFR Part 11
- All database tables with clinical data must include: created_by, created_at, updated_by, updated_at, is_deleted, version columns
- PHI must never appear in logs, error messages, or non-encrypted storage
