---
name: deploy-checklist
description: Pre-deployment verification checklist for validated clinical trial EDC systems. Covers environment checks, data backup, rollback readiness, validation documentation completeness, and stakeholder notification.
---

# Deploy Checklist Skill

Generate a comprehensive pre-deployment verification checklist for Talosix EDC system releases. Clinical trial systems are validated environments subject to 21 CFR Part 11 and GxP requirements. Every deployment must be verified before execution.

## When to Use

- Before any deployment to staging, UAT, or production environments
- When preparing for a validated system release
- As part of change control execution

## Checklist Generation

When asked to create a deploy checklist, produce a structured checklist covering all sections below. Tailor the checklist to the specific release scope provided by the user.

### 1. Change Control Verification

- [ ] Change request approved and documented in the change control system
- [ ] Change advisory board (CAB) approval obtained (production deployments)
- [ ] Risk assessment completed and reviewed
- [ ] All linked Jira tickets in "Ready for Deploy" status
- [ ] Deployment window confirmed and communicated

### 2. Validation Documentation Completeness

- [ ] Installation Qualification (IQ) protocol approved
- [ ] Operational Qualification (OQ) protocol approved
- [ ] Test scripts mapped to requirements (traceability matrix updated)
- [ ] All validation test cases executed and passed in pre-prod
- [ ] Deviations documented and dispositioned
- [ ] Validation summary report drafted (pending production execution)

### 3. Environment Readiness

- [ ] Target environment infrastructure verified (compute, memory, storage)
- [ ] Database migration scripts reviewed and tested in lower environment
- [ ] Environment configuration diff reviewed (env vars, feature flags)
- [ ] SSL certificates valid and not expiring within 30 days
- [ ] Third-party service dependencies confirmed operational
- [ ] Load balancer and DNS configuration verified
- [ ] Monitoring and alerting configured for new components

### 4. Data Backup and Recovery

- [ ] Full database backup completed and verified
- [ ] Backup restoration tested in isolated environment
- [ ] Audit trail data backed up separately
- [ ] Document/file storage snapshot taken
- [ ] Backup retention policy confirmed (minimum per regulatory requirements)
- [ ] Point-in-time recovery capability confirmed

### 5. Rollback Readiness

- [ ] Rollback plan documented and reviewed
- [ ] Rollback scripts tested in lower environment
- [ ] Database rollback procedure verified (forward-compatible schema changes preferred)
- [ ] Rollback decision criteria defined (go/no-go thresholds)
- [ ] Rollback communication plan prepared
- [ ] Maximum acceptable rollback window defined

### 6. Code and Artifact Verification

- [ ] Release branch/tag created and locked
- [ ] Build artifacts generated from tagged source
- [ ] Artifact checksums verified
- [ ] No unapproved commits since last validation
- [ ] Static analysis and security scans passed
- [ ] Dependency vulnerability scan completed
- [ ] Container images scanned (if applicable)

### 7. Stakeholder Notification

- [ ] Internal engineering team notified of deployment window
- [ ] QA team on standby for post-deploy verification
- [ ] Customer Success team notified (for customer-facing changes)
- [ ] Affected clinical sites notified (if downtime or workflow changes)
- [ ] Sponsor contacts notified (if study-impacting changes)
- [ ] Support team briefed on changes and known issues
- [ ] Release notes distributed to stakeholders

### 8. Go/No-Go Decision

- [ ] All checklist items above completed or explicitly waived with justification
- [ ] Go/no-go decision documented with approver names
- [ ] Deployment team roles assigned (deployer, verifier, rollback lead)
- [ ] War room / communication channel established
- [ ] Post-deployment verification plan ready

## Output Format

Generate the checklist as a markdown document with:
- Release identifier and version number at the top
- Date and target environment
- Each section with checkboxes
- Space for sign-off names and timestamps
- Notes section for any deviations or waivers

## Regulatory Context

Talosix EDC deployments must comply with:
- 21 CFR Part 11 (electronic records, electronic signatures)
- ICH E6(R2) GCP guidelines
- EU Annex 11 (computerized systems)
- GAMP 5 software lifecycle practices

Every deployment to a validated environment is a controlled change that requires documented evidence of verification.
