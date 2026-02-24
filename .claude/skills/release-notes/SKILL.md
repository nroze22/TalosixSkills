---
name: release-notes
description: "Generate release notes from Jira tickets and git history in multiple formats (internal, customer-facing, regulatory) with compliance and validation impact statements."
allowed-tools: mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql, mcp__claude_ai_Atlassian__getJiraIssue, mcp__claude_ai_Atlassian__searchConfluenceUsingCql, mcp__claude_ai_Atlassian__createConfluencePage
argument-hint: "[Version number or Jira fixVersion, e.g. 'v2.5.0' or 'Release 2025-Q1']"
---

# Release Notes Generator

Generate professional release notes for Talosix EDC platform releases, pulling data from Jira and git history. Produces multiple formats appropriate for different audiences.

## Workflow

1. **Gather Release Scope**:
   - Search Jira for tickets in the release: `fixVersion = "[version]" AND status = Done ORDER BY type, priority`
   - Get details on each ticket using `getJiraIssue`
   - Ask user for the version number, release date, and any highlights to emphasize
   - Optionally: ask user to run `git log --oneline [previous-tag]..[current-tag]` and provide the output

2. **Categorize Changes**: Group tickets by type and impact.

3. **Generate Release Notes** in the requested format(s).

4. **Publish**: Offer to create Confluence page(s).

## Change Categorization

Group all changes into these categories:

### New Features
- Major new capabilities added to the platform
- New modules or workflows

### Enhancements
- Improvements to existing features
- UX/UI improvements
- Performance improvements

### Bug Fixes
- Defects resolved
- Categorize by severity: Critical, High, Medium, Low

### Regulatory / Compliance
- Changes affecting validated system state
- Audit trail modifications
- 21 CFR Part 11 related changes
- Security patches

### API Changes
- New endpoints
- Modified endpoints (backward compatible)
- Deprecated endpoints
- Breaking changes (major version only)

### Known Issues
- Issues identified but not resolved in this release
- Workarounds if available

## Format 1: Customer-Facing Release Notes

```
# Talosix EDC Release Notes - Version [X.Y.Z]
**Release Date**: [Date]

## Highlights
[2-3 sentence summary of the most impactful changes]

## New Features

### [Feature Name]
[1-2 paragraph description written for clinical users: CDMs, CRCs, monitors]
- What it does and why it matters
- How to access/enable it
- Which user roles can use it

## Enhancements

### [Enhancement Name]
- [Clear description of the improvement]
- [User benefit]

## Bug Fixes
- **[Area]**: [Description of fix in user-friendly language]. ([Ticket ID])

## Validation Impact Statement
The following changes affect the validated state of the system and may
require updates to your site-specific validation documentation:

| Change | Affected Area | Validation Impact | Action Required |
|--------|--------------|-------------------|-----------------|
| [Change] | [Area] | [IQ/OQ/PQ impact] | [Customer action] |

**No validation impact**: The following changes do not affect the validated
state: [list minor fixes/cosmetic changes].

## Compliance Notes
- All changes in this release maintain compliance with 21 CFR Part 11,
  ICH E6(R2) GCP, and EU Annex 11 requirements.
- Audit trail coverage: [Confirm all new features include audit logging]
- Electronic signature workflows: [Any changes noted]

## Upgrade Notes
- **Downtime Required**: [Yes/No, expected duration]
- **Data Migration**: [Yes/No, details]
- **Configuration Changes**: [Any admin actions needed post-upgrade]
- **Browser Compatibility**: [Any changes to supported browsers]

## Known Issues
- [Issue description] -- Workaround: [workaround]. Fix targeted for [version].

## Getting Help
Contact Talosix Support at [support channel] for questions about this release.
```

## Format 2: Internal Release Notes

```
# Internal Release Notes - Version [X.Y.Z]
**Release Date**: [Date]
**Release Manager**: [Name]
**QA Sign-off**: [Name, Date]
**Regulatory Sign-off**: [Name, Date]

## Release Summary
- Total tickets: [N]
- New features: [N]
- Enhancements: [N]
- Bug fixes: [N] (Critical: [N], High: [N], Medium: [N], Low: [N])
- Story points delivered: [N]

## Detailed Change Log

### [JIRA-123]: [Title]
- **Type**: Story / Bug / Task
- **Priority**: [Priority]
- **Component**: [Component]
- **Developer**: [Name]
- **QA**: [Name]
- **Description**: [Technical description]
- **Validation**: [IQ/OQ/PQ scripts updated: Yes/No, Script IDs]
- **Rollback Plan**: [How to revert if needed]

## Deployment Checklist
- [ ] Database migrations executed
- [ ] Configuration changes applied
- [ ] Feature flags set
- [ ] Cache cleared
- [ ] Integration endpoints verified
- [ ] Monitoring alerts configured
- [ ] Smoke tests passed

## Customer Communication Plan
- [ ] Release notes published to help center
- [ ] Email notification sent to admins
- [ ] CS team briefed
- [ ] Training materials updated

## Metrics to Watch Post-Release
- Error rates (compare to pre-release baseline)
- Page load times
- Support ticket volume
- Feature adoption (for new features)
```

## Format 3: Regulatory Release Documentation

```
# Software Release Record - Version [X.Y.Z]

## Release Identification
- **Version**: [X.Y.Z]
- **Build**: [Build number]
- **Release Date**: [Date]
- **Previous Version**: [X.Y.Z-1]

## Change Summary
[List all changes with traceability to requirements]

| Change ID | Description | Requirement | Risk Assessment | Test Evidence |
|-----------|-------------|-------------|-----------------|---------------|
| [JIRA-ID] | [Description] | [REQ-ID] | [Risk level] | [Test script IDs] |

## Validation Summary
- **Test Scripts Executed**: [N]
- **Test Scripts Passed**: [N]
- **Test Scripts Failed**: [N] (with deviation references)
- **Regression Tests**: [Scope and results]

## Risk Assessment
- Impact on data integrity: [Assessment]
- Impact on electronic records/signatures: [Assessment]
- Impact on audit trail: [Assessment]
- Impact on access controls: [Assessment]

## Approval
| Role | Name | Signature | Date |
|------|------|-----------|------|
| QA Manager | | | |
| Development Lead | | | |
| Regulatory | | | |
| Product Owner | | | |
```

## Tips
- Write customer-facing notes in plain language; avoid technical jargon
- Always include the validation impact statement for regulated customers
- Flag breaking changes prominently
- For security fixes, describe the improvement without exposing vulnerability details
- Include Jira ticket IDs for traceability
