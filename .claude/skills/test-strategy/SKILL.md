---
name: test-strategy
description: "Create comprehensive test strategies for Talosix EDC platform releases. Covers risk-based testing, all test levels, regulatory validation (IQ/OQ/PQ), and resource planning for clinical trial software."
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Test Strategy Creation

## Purpose

Create comprehensive, risk-based test strategies for Talosix EDC platform releases. The strategy addresses all test levels, regulatory validation requirements, and provides actionable resource and timeline planning. The output is suitable for inclusion in validation documentation packages.

## Context: Regulated Clinical Trial Software

Talosix EDC is classified as a GAMP 5 Category 5 (custom application) computerized system used in GxP-regulated clinical trials. Test strategies must satisfy:

- **21 CFR Part 11:** Electronic records and electronic signatures.
- **ICH E6(R2) GCP:** Good Clinical Practice guidelines for trial conduct.
- **EU Annex 11:** Computerized systems used in EU-regulated environments.
- **GAMP 5:** Risk-based approach to compliant GxP computerized systems.
- **FDA Data Integrity Guidance:** ALCOA+ principles for data reliability.

Regulatory authorities may audit the test strategy and its execution evidence at any time during or after a clinical trial.

## Workflow

1. **Gather release scope:** Read the release plan, feature list, Jira epics, and changelogs.
2. **Assess risk:** Classify features by GxP impact and technical complexity.
3. **Define test levels and types:** Determine which test levels apply to each feature.
4. **Map regulatory requirements:** Identify IQ/OQ/PQ activities for the release.
5. **Plan resources and timeline:** Estimate effort by role and phase.
6. **Document the strategy** in the format below.

## Test Strategy Document Structure

### 1. Executive Summary
- Release identifier and target date.
- Scope summary (features, modules, fixes).
- Overall risk assessment (High / Medium / Low).
- Key testing milestones and decision gates.

### 2. Scope and Objectives

#### In Scope
- New features with Jira epic references.
- Modified features with change descriptions.
- Bug fixes requiring regression verification.
- Infrastructure or platform changes.

#### Out of Scope
- Features deferred to future releases.
- Third-party systems not modified in this release.
- Explicitly excluded test types with justification.

#### Objectives
- Validate all new and modified functionality meets requirements.
- Confirm no regression in existing functionality.
- Verify regulatory compliance for GxP-impacting changes.
- Achieve defined test coverage targets.

### 3. Risk-Based Testing Approach

Classify each feature using a risk matrix:

| Risk Factor | High | Medium | Low |
|-------------|------|--------|-----|
| **GxP Impact** | Directly affects patient safety or data integrity | Affects data management workflows | Administrative or cosmetic |
| **Technical Complexity** | New architecture, integrations, or data migration | Modifications to existing complex logic | Simple UI changes, text updates |
| **User Impact** | All users affected, critical workflows | Subset of users or non-critical workflows | Minimal user interaction changes |
| **Change Size** | Large codebase changes, new modules | Moderate changes to existing modules | Small, isolated changes |

**Risk Score Calculation:**
- High risk (any High factor): Full test coverage -- all test levels, all test types.
- Medium risk (no High, any Medium): Standard coverage -- functional, integration, and regression.
- Low risk (all Low): Targeted coverage -- functional verification and smoke testing.

### 4. Test Levels

#### Unit Testing
- **Responsibility:** Development team.
- **Scope:** All new and modified code.
- **Coverage Target:** Minimum 80% line coverage, 70% branch coverage for new code.
- **Tools:** Jest / Vitest for frontend, language-appropriate framework for backend.
- **Criteria:** All unit tests pass before code merge.

#### Integration Testing
- **Responsibility:** Development team + QA.
- **Scope:** API contracts, database interactions, third-party integrations.
- **Focus Areas:**
  - CRF data flow from UI to database and back.
  - Edit check execution pipeline.
  - Query generation and notification dispatch.
  - Authentication and authorization services.
  - CDISC data export pipeline.
  - External system integrations (RTSM, ePRO, safety database).
- **Tools:** API testing framework, database verification scripts.

#### System Testing
- **Responsibility:** QA team.
- **Scope:** End-to-end functional testing of all in-scope features.
- **Approach:** Execute test cases from Zephyr Scale organized by feature.
- **Test Types:**
  - Functional testing (positive, negative, boundary, edge cases).
  - Regulatory compliance testing (audit trail, e-signatures, access control).
  - Cross-browser testing (Chrome, Firefox, Edge, Safari).
  - Compatibility testing (screen resolutions, operating systems).
  - Data migration testing (if applicable).

#### User Acceptance Testing (UAT)
- **Responsibility:** CRO and Sponsor representatives.
- **Scope:** Business workflow validation using UAT test suites.
- **Approach:** Scripted UAT with business-language test cases.
- **Environment:** UAT environment with production-like configuration.
- **Sign-off:** Formal UAT sign-off required from each stakeholder group.

#### Performance Testing
- **Responsibility:** QA + DevOps.
- **Scope:** Features identified as performance-sensitive in risk assessment.
- **Test Types:**
  - **Load Testing:** Concurrent users performing typical workflows.
  - **Stress Testing:** Beyond expected capacity to find breaking points.
  - **Endurance Testing:** Sustained load over extended periods.
  - **Scalability Testing:** Increasing data volumes (subjects, sites, visits).
- **Key Metrics:** Response time (P50, P95, P99), throughput, error rate, resource utilization.
- **Benchmarks:**
  - CRF form load: < 2 seconds (P95).
  - Form save: < 3 seconds (P95).
  - Report generation: < 30 seconds for standard reports.
  - Concurrent users: Support 500+ simultaneous users.

#### Security Testing
- **Responsibility:** QA + Security team.
- **Scope:** All features with authentication, authorization, or data handling.
- **Test Types:**
  - Authentication and session management.
  - Role-based access control verification.
  - Input validation and injection prevention (SQL, XSS, CSRF).
  - Data encryption at rest and in transit.
  - PHI/PII data protection.
  - API security (authentication, rate limiting, input validation).
- **Tools:** OWASP ZAP, Burp Suite, custom security test scripts.

### 5. Regulatory Validation (IQ/OQ/PQ)

#### Installation Qualification (IQ)
- Verify the system is installed correctly in the target environment.
- Confirm software versions, configurations, and dependencies match specifications.
- Validate infrastructure components (servers, databases, network).
- Document evidence of successful deployment.

**IQ Checklist:**
- [ ] Application version matches release manifest.
- [ ] Database schema version is correct.
- [ ] Configuration files match environment specifications.
- [ ] SSL certificates are valid and properly configured.
- [ ] Integration endpoints are reachable.
- [ ] Backup and recovery mechanisms are operational.

#### Operational Qualification (OQ)
- Verify the system operates as intended under normal conditions.
- Execute functional test cases covering all GxP-impacting features.
- Confirm edit checks, validations, and business rules function correctly.
- Verify audit trail completeness and accuracy.
- Validate e-signature functionality per 21 CFR Part 11.

**OQ maps to:** System Testing (functional + regulatory test cases).

#### Performance Qualification (PQ)
- Verify the system performs as intended in the production-equivalent environment.
- Execute UAT test cases with representative clinical data.
- Confirm end-to-end workflows complete successfully.
- Validate reporting and data export accuracy.

**PQ maps to:** UAT + Performance Testing results.

### 6. Entry and Exit Criteria

#### Test Level Entry Criteria
| Level | Entry Criteria |
|-------|---------------|
| Unit | Code complete, peer reviewed |
| Integration | Unit tests passing, services deployed to integration env |
| System | Integration tests passing, test environment stable, test data loaded |
| UAT | System testing complete with no open Critical/High defects, UAT env provisioned |
| Performance | Functional testing complete, performance env provisioned with production-like data |
| Security | Functional testing complete, security tools configured |

#### Release Exit Criteria
- All Critical and High severity defects resolved and verified.
- Medium severity defects assessed and documented with mitigation plan.
- Test execution rate >= 95% (all planned tests executed).
- Test pass rate >= 98% for Critical/High priority test cases.
- No open regulatory compliance failures.
- UAT sign-off obtained from all stakeholder groups.
- All IQ/OQ/PQ protocols executed and deviations documented.
- Validation summary report approved.

### 7. Resource Planning

Provide estimates in this format:

| Role | Allocation | Phase | Duration |
|------|-----------|-------|----------|
| QA Lead | 100% | All phases | Full release cycle |
| QA Engineer (x N) | 100% | System Testing | X weeks |
| Automation Engineer | 100% | Script development + execution | X weeks |
| Performance Engineer | 50% | Performance testing phase | X weeks |
| UAT Coordinator | 100% | UAT preparation + execution | X weeks |
| Business Testers (x N) | 50% | UAT execution | X weeks |

### 8. Test Environment Requirements

| Environment | Purpose | Data | Refresh Cycle |
|------------|---------|------|---------------|
| DEV | Developer testing | Synthetic | On demand |
| QA / SIT | System integration testing | Synthetic + anonymized production | Per test cycle |
| UAT | User acceptance testing | Production-like anonymized | Per UAT cycle |
| PERF | Performance testing | Scaled production-like | Per performance cycle |
| STAGING | Pre-production validation | Production clone (anonymized) | Pre-release |

### 9. Defect Management

- **Tracking Tool:** Jira with Zephyr Scale integration.
- **Severity Levels:**
  - Critical: System crash, data loss, security breach, regulatory non-compliance.
  - High: Major feature broken, workaround exists but impacts workflow.
  - Medium: Minor feature issue, reasonable workaround available.
  - Low: Cosmetic, typo, minor UI inconsistency.
- **SLA for Resolution:**
  - Critical: Fix within 24 hours, retest within 48 hours.
  - High: Fix within 3 business days.
  - Medium: Fix before release or documented deferral.
  - Low: Fix in next release.

### 10. Risks and Mitigations

Document release-specific risks:

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Example: Third-party API changes | Medium | High | Integration tests with contract testing, vendor communication |
| Example: UAT resource availability | High | Medium | Early scheduling, backup testers identified |
| Example: Performance regression | Low | High | Baseline benchmarks established, automated performance suite |

## Guidelines

- Tailor the strategy to the release scope. A minor patch does not need the same depth as a major release.
- Always include regulatory validation sections for any release that touches GxP functionality.
- Reference specific Jira epics and stories throughout the strategy for traceability.
- Review the codebase for recent changes to understand technical risk areas.
- The strategy is a living document -- note that it should be updated as scope changes during the release.
