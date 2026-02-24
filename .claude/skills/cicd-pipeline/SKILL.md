---
name: CI/CD Pipeline Design
description: Design and optimize CI/CD pipelines for Talosix EDC systems with GxP-compliant quality gates, automated testing, security scanning, and validated deployment strategies.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# CI/CD Pipeline Design for Talosix EDC

## Domain Context

Talosix is a clinical trial Electronic Data Capture (EDC) company. All CI/CD pipelines must account for GxP (Good Practice) regulatory requirements, 21 CFR Part 11 compliance, and the critical nature of clinical trial data. Pipeline failures or misconfigurations can impact patient safety, data integrity, and regulatory submissions.

## Pipeline Architecture Principles

### Validation-First Design
- Every pipeline must enforce the principle that no artifact reaches production without passing through all quality gates.
- Pipelines are themselves validated artifacts. Changes to pipeline definitions require change control documentation.
- Pipeline configurations should be version-controlled and treated as regulated code.

### Traceability
- Every build must produce a traceable artifact linked to a specific commit, ticket, and approver.
- Audit logs of all pipeline executions must be retained per data retention policies (typically 15+ years for clinical data systems).
- Deploy logs must capture who triggered, what was deployed, and the validation status.

## Pipeline Stages

### 1. Source Stage
- Trigger on pull request creation or merge to protected branches.
- Enforce branch protection rules: require reviews, passing status checks, signed commits.
- Validate commit messages reference a ticket or change control number.

```yaml
# Example: GitHub Actions trigger
on:
  pull_request:
    branches: [main, release/*]
  push:
    branches: [main]
```

### 2. Build Stage
- Use reproducible builds with pinned dependencies and locked versions.
- Generate Software Bill of Materials (SBOM) for every build.
- Tag artifacts with build metadata: commit SHA, build number, timestamp.
- Store build artifacts in an immutable artifact repository.

### 3. Static Analysis & Linting
- Run language-specific linters (ESLint, Pylint, RuboCop, etc.).
- Enforce code style consistency across the codebase.
- Run static application security testing (SAST) tools.
- Fail the pipeline if critical or high severity findings are detected.

### 4. Unit & Integration Testing
- Execute unit tests with minimum coverage thresholds (aim for 80%+ on critical paths).
- Run integration tests against isolated test databases and service mocks.
- For EDC-specific logic, test:
  - Edit check execution and validation rules.
  - Audit trail generation for data modifications.
  - Role-based access control enforcement.
  - Electronic signature workflows.
- Generate test reports in a format suitable for validation documentation.

### 5. Security Scanning
- **Dependency scanning**: Check all dependencies against CVE databases (Snyk, Dependabot, Trivy).
- **Container scanning**: Scan Docker images for vulnerabilities before pushing to registry.
- **Secret detection**: Scan for hardcoded credentials, API keys, connection strings.
- **DAST (Dynamic Application Security Testing)**: Run against staging deployments.
- Block promotion if critical vulnerabilities are found. Document accepted risks for lower-severity items.

### 6. Compliance & Validation Gates
- Verify that change control documentation exists and is approved.
- Check that required test evidence is attached (IQ/OQ/PQ artifacts).
- Validate that the deployment target environment is in a qualified state.
- Enforce separation of duties: the person who wrote the code cannot be the sole approver.
- Gate deployments to production behind manual approval from QA or validation lead.

```yaml
# Example: Manual approval gate
deploy-production:
  needs: [deploy-staging, validation-gate]
  environment:
    name: production
    url: https://app.talosix.com
  steps:
    - name: Require QA Approval
      uses: trstringer/manual-approval@v1
      with:
        approvers: qa-team,validation-lead
        minimum-approvals: 2
```

### 7. Deployment Strategies

#### Blue-Green Deployment
- Maintain two identical production environments (blue and green).
- Deploy to the inactive environment, run smoke tests, then switch traffic.
- Keep the previous environment available for instant rollback.
- Preferred for major releases or database migration deployments.

```
[Load Balancer] --> [Blue (current)]
                    [Green (new deploy, testing)]
# After validation:
[Load Balancer] --> [Green (now current)]
                    [Blue (standby for rollback)]
```

#### Canary Deployment
- Route a small percentage of traffic (5-10%) to the new version.
- Monitor error rates, latency, and business metrics.
- Gradually increase traffic if metrics remain healthy.
- Suitable for feature releases and non-breaking changes.

#### Rolling Deployment
- Update instances one at a time behind the load balancer.
- Ensure backward compatibility of database schemas and API contracts.
- Use for routine patches and minor updates.

### 8. Post-Deployment Verification
- Run automated smoke tests against the production environment.
- Verify critical EDC workflows:
  - User login and authentication.
  - Form data entry and save.
  - Audit trail recording.
  - Query generation and resolution.
  - Data export functionality.
- Monitor application metrics for anomalies in the first 30 minutes.
- Generate deployment verification report for validation records.

## GxP Compliance in Deployment

### Computer System Validation (CSV) Integration
- Map pipeline stages to GAMP 5 categories.
- Maintain a requirements traceability matrix linking requirements to tests.
- Generate Installation Qualification (IQ) evidence from deployment logs.
- Generate Operational Qualification (OQ) evidence from automated test results.
- Support Performance Qualification (PQ) through production monitoring data.

### Change Control Process
- Every deployment to a validated environment must have an approved change control.
- Pipeline should verify change control status before allowing promotion.
- Emergency changes follow an expedited process but still require documentation.
- Post-deployment, the change control is closed with evidence artifacts.

### Environment Management
- Development, QA, Staging, and Production environments must be documented.
- Environment configurations should be managed as code (Terraform, Pulumi).
- Environment parity: staging must mirror production configuration.
- Access to production pipelines restricted to authorized personnel.

## Pipeline Configuration Patterns

### Monorepo Pipeline
- Use path-based triggers to only build affected services.
- Share common pipeline steps via reusable workflow definitions.
- Coordinate deployments of interdependent services.

### Microservices Pipeline
- Each service has its own pipeline with independent versioning.
- Contract testing between services at the integration stage.
- Service mesh configuration changes go through the same quality gates.

### Database Migration Pipeline
- Migrations are forward-only in production; no destructive rollbacks.
- Test migrations against a production-like dataset before deploying.
- Backup verification before and after migration execution.
- Audit trail tables must never be altered or dropped.

## Monitoring & Alerting

- Pipeline failure notifications to the responsible team channel.
- Track pipeline metrics: build time, failure rate, deployment frequency.
- Alert on security scan findings even if the build passes.
- Dashboard showing deployment history and current environment state.

## Disaster Recovery

- Pipeline definitions are backed up and recoverable.
- Artifact repositories are replicated across regions.
- Runbook for manual deployment if pipeline infrastructure is unavailable.
- Regular DR drills for the deployment process.

## When Applying This Skill

1. Examine the existing pipeline configuration files (Jenkinsfile, .github/workflows, .gitlab-ci.yml, etc.).
2. Identify gaps against the stages and quality gates described above.
3. Prioritize adding compliance gates if they are missing, as these are non-negotiable for GxP.
4. Recommend incremental improvements rather than full pipeline rewrites.
5. Ensure all pipeline changes are documented and go through change control.
