---
name: dependency-upgrade-planning
description: Plan and execute dependency upgrades for Talosix EDC systems with risk assessment, compatibility checking, regression testing, and change control for validated environments.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Dependency Upgrade Planning for Talosix EDC

## Domain Context

Talosix EDC systems are validated computerized systems used in clinical trials. Dependency upgrades in this context are not routine maintenance -- they are changes to a validated system that require formal change control, risk assessment, and regression testing per GxP requirements. An improperly managed upgrade can invalidate the system's qualification status, disrupt active clinical trials, and trigger regulatory findings.

## Upgrade Planning Process

### 1. Discovery and Inventory

#### Identify Current Dependencies
- Read package manifests: `package.json`, `Gemfile`, `requirements.txt`, `go.mod`, `pom.xml`, `Cargo.toml`, etc.
- Read lock files for exact installed versions: `package-lock.json`, `Gemfile.lock`, `poetry.lock`, `go.sum`, etc.
- Identify transitive dependencies and their version constraints.
- Catalog system-level dependencies: OS packages, runtime versions, database versions.
- Document container base images and their versions.

#### Classify Dependencies
- **Direct**: Explicitly declared in the project manifest.
- **Transitive**: Pulled in by direct dependencies.
- **Runtime**: Required for the application to run.
- **Development**: Used only for building, testing, or tooling.
- **Infrastructure**: Database engines, message brokers, cache systems.

### 2. Risk Assessment

#### Risk Categories

| Risk Level | Criteria | Change Control | Testing Required |
|-----------|----------|---------------|------------------|
| Critical | Major version bump of core framework, database engine, or runtime | Full change control with QA review | Full regression + IQ/OQ |
| High | Major version of significant library, security-critical dependency | Standard change control | Targeted regression + security testing |
| Medium | Minor version of direct dependency with breaking changes noted | Standard change control | Targeted regression |
| Low | Patch version, dev-only dependency update | Expedited change control | Automated test suite pass |

#### EDC-Specific Risk Factors
- **Audit trail impact**: Does the upgrade affect the ORM, database driver, or logging framework? Any change to audit trail generation is high risk.
- **Authentication/authorization**: Changes to auth libraries affect 21 CFR Part 11 compliance.
- **Data handling**: Upgrades to serialization, encryption, or data validation libraries affect data integrity.
- **PDF/report generation**: Changes affect regulatory submission outputs.
- **Date/time handling**: Clinical trial data is date-sensitive; timezone or date library changes are high risk.
- **Cryptography**: Changes to crypto libraries affect electronic signatures and data encryption.

#### Risk Assessment Template
```markdown
## Dependency Upgrade Risk Assessment

**Dependency**: [name]
**Current Version**: [x.y.z]
**Target Version**: [a.b.c]
**Change Type**: [major|minor|patch]

### Impact Analysis
- [ ] Affects audit trail generation
- [ ] Affects authentication or authorization
- [ ] Affects data persistence or serialization
- [ ] Affects electronic signature workflow
- [ ] Affects regulatory submission exports
- [ ] Affects date/time handling
- [ ] Has breaking changes per changelog

### Risk Rating: [Critical|High|Medium|Low]
### Justification: [explanation]

### Required Testing:
- [ ] Unit test suite
- [ ] Integration test suite
- [ ] Specific regression areas: [list]
- [ ] Security scan
- [ ] Performance benchmark comparison
```

### 3. Compatibility Checking

#### Pre-Upgrade Checks
- Read the dependency changelog and migration guide for the target version.
- Identify breaking changes and deprecated APIs.
- Check compatibility with the current runtime version (Node.js, Python, Ruby, etc.).
- Verify peer dependency compatibility.
- Check that transitive dependency resolution does not introduce conflicts.
- Search the codebase for usage of deprecated APIs that will be removed.

```bash
# Example: Search for deprecated API usage
grep -r "deprecatedFunction\|removedMethod" --include="*.ts" --include="*.js" src/
```

#### Compatibility Matrix
- Document known compatible version combinations.
- Test against the minimum supported versions of related dependencies.
- Verify database driver compatibility with the database server version.
- Check that container base image supports the new dependency version.

### 4. Upgrade Execution

#### Preparation
- Create a dedicated branch for the upgrade.
- Document the current state: dependency tree, test results, performance baselines.
- Take a snapshot of the lock file before changes.

#### Execution Steps
1. Update the dependency version in the manifest file.
2. Run the dependency resolver to update the lock file.
3. Review lock file changes to understand the full impact (transitive updates).
4. Fix any compilation or type errors introduced by the upgrade.
5. Update code to use new APIs where deprecated ones were removed.
6. Run the full test suite and document results.
7. Run security scans on the updated dependency tree.
8. Compare performance benchmarks before and after.

#### Handling Multiple Upgrades
- Upgrade one dependency at a time when possible, especially for high-risk changes.
- Group low-risk patch updates into a single change if they have no interdependencies.
- Never combine a framework major version upgrade with other major upgrades.
- Document each upgrade separately in the change control, even if deployed together.

### 5. Regression Testing Strategy

#### Test Tiers

**Tier 1 -- Automated (Required for all upgrades)**:
- Full unit test suite passes.
- Full integration test suite passes.
- No new linting or static analysis warnings.
- Security scan shows no new critical/high vulnerabilities.

**Tier 2 -- Targeted (Required for Medium+ risk)**:
- Test areas specifically affected by the upgraded dependency.
- For ORM upgrades: test all database operations, especially audit trail writes.
- For auth library upgrades: test login, session management, role enforcement, e-signatures.
- For HTTP library upgrades: test all external integrations and API endpoints.

**Tier 3 -- Full Regression (Required for Critical risk)**:
- End-to-end test suite covering all critical EDC workflows.
- Study setup and configuration.
- Subject enrollment and randomization.
- Data entry, edit checks, and query management.
- Medical coding.
- Data export and submission package generation.
- Audit trail completeness verification.
- Electronic signature workflow.
- User access control matrix verification.

**Tier 4 -- Performance (Required for Critical risk or infrastructure changes)**:
- Load testing with production-representative data volumes.
- Compare response times and throughput to baseline.
- Verify no memory leaks or resource exhaustion under sustained load.

### 6. Change Control for Validated Environments

#### Change Control Documentation
```markdown
## Change Control Record

**Change ID**: CC-[YYYY]-[NNN]
**Title**: Upgrade [dependency] from [old] to [new]
**Requested By**: [name]
**Date Requested**: [date]

### Description of Change
[What is being changed and why]

### Justification
- [ ] Security vulnerability remediation (CVE-XXXX-XXXXX)
- [ ] End of life / end of support for current version
- [ ] Required for new feature development
- [ ] Performance improvement
- [ ] Bug fix in dependency

### Risk Assessment
[Reference to risk assessment document]

### Test Plan
[Reference to test plan with tier level]

### Rollback Plan
[Steps to revert if the upgrade causes issues]

### Approvals
- [ ] Development Lead
- [ ] QA Lead
- [ ] Validation Lead (for Critical/High risk)
- [ ] IT Operations
```

#### Post-Deployment Verification
- Verify the deployed version matches the approved change.
- Run smoke tests in the validated environment.
- Confirm audit trail is functioning correctly.
- Update the system inventory and configuration documentation.
- Close the change control record with evidence of successful deployment.

### 7. Emergency Security Upgrades

For critical CVEs affecting production:
- Follow the expedited change control process.
- Minimum required: security scan pass, Tier 1 tests, and smoke tests.
- Document the expedited justification.
- Schedule full regression testing within an agreed timeframe post-deployment.
- Notify study teams if any service interruption is expected.

## Dependency Monitoring

### Ongoing Practices
- Enable automated dependency scanning (Dependabot, Snyk, Renovate).
- Review dependency alerts weekly.
- Maintain a dependency upgrade calendar for planned major upgrades.
- Track end-of-life dates for all critical dependencies.
- Subscribe to security advisories for key dependencies.

### Metrics to Track
- Number of dependencies with known vulnerabilities.
- Average age of dependencies (time since latest available version).
- Time from CVE disclosure to patch deployment.
- Upgrade failure rate and rollback frequency.

## When Applying This Skill

1. Identify which dependencies need upgrading by reading manifest and lock files.
2. Assess each upgrade using the risk framework above.
3. Search the codebase for usage of APIs that may change.
4. Propose an upgrade plan with appropriate testing tiers.
5. Draft change control documentation if the target is a validated environment.
6. Execute upgrades incrementally, verifying at each step.
