---
name: documentation-generation
description: "Generate technical documentation from Talosix EDC codebases including API docs, architecture docs, runbooks, and GxP compliance documentation with IQ/OQ/PQ references."
allowed-tools: Read, Grep, Glob, Bash
---

# Documentation Generation for Talosix EDC

## Domain Context

Talosix builds Electronic Data Capture systems for clinical trials. Documentation is not optional -- it is a regulatory requirement. FDA 21 CFR Part 11, ICH E6(R2) GCP guidelines, and GAMP 5 all mandate thorough documentation of computerized systems used in clinical trials. Poor documentation can result in FDA warning letters, trial delays, or data rejection.

## Documentation Categories

### 1. API Documentation

#### Approach
- Scan the codebase for API route definitions, controllers, and endpoint handlers.
- Extract request/response schemas, HTTP methods, URL patterns, and authentication requirements.
- Document query parameters, path parameters, request bodies, and response formats.
- Include error response codes and their meanings.

#### EDC-Specific API Concerns
- Document which endpoints trigger audit trail entries.
- Note endpoints that require electronic signatures (21 CFR Part 11).
- Identify endpoints that handle Protected Health Information (PHI) and their access controls.
- Document rate limits, especially for data export and integration endpoints used by clinical sites.
- Flag endpoints used by external systems (CTMS, IVRS/IWRS, lab systems).

#### Output Format
```markdown
## POST /api/v1/studies/{studyId}/subjects/{subjectId}/forms/{formId}/data

**Description**: Submit or update form data for a subject visit.

**Authentication**: Bearer token (JWT). Requires role: DataEntry or Investigator.

**Audit Trail**: Yes. Records field-level changes with old/new values, user, timestamp.

**Parameters**:
| Name | In | Type | Required | Description |
|------|-----|------|----------|-------------|
| studyId | path | UUID | yes | Study identifier |
| subjectId | path | string | yes | Subject number |
| formId | path | UUID | yes | Form definition ID |

**Request Body**: JSON object with field values keyed by field OID.

**Responses**:
| Code | Description |
|------|-------------|
| 200 | Data saved successfully |
| 409 | Conflict - data modified by another user |
| 422 | Validation errors - edit checks failed |
```

### 2. Architecture Documentation

#### Approach
- Analyze project structure, configuration files, and infrastructure code.
- Identify services, databases, message queues, caches, and external integrations.
- Map data flows between components.
- Document technology stack and version requirements.

#### Structure
- **System Overview**: High-level description of the EDC platform and its role in clinical trials.
- **Component Diagram**: Services, databases, and their interactions.
- **Data Flow Diagrams**: How clinical data moves from site entry to database to export.
- **Integration Points**: Connections to external systems (CTMS, randomization, lab, safety).
- **Security Architecture**: Authentication, authorization, encryption at rest and in transit.
- **Infrastructure**: Cloud provider, regions, availability zones, disaster recovery.
- **Audit Architecture**: How the audit trail is implemented, stored, and protected from modification.

#### EDC-Specific Architecture Concerns
- Document the audit trail implementation (append-only tables, triggers, application-level).
- Describe the electronic signature workflow and its compliance with 21 CFR Part 11.
- Map the data flow for regulatory submissions (define.xml, CDISC datasets).
- Document multi-tenancy approach and data isolation between sponsors/studies.
- Describe backup and recovery procedures with recovery time/point objectives.

### 3. Runbooks

#### Approach
- Identify operational procedures from deployment scripts, monitoring configurations, and incident history.
- Document step-by-step procedures for common operational tasks.
- Include troubleshooting decision trees.

#### Standard Runbook Structure
```markdown
# Runbook: [Procedure Name]

## Purpose
What this procedure accomplishes and when to use it.

## Prerequisites
- Access requirements
- Tools needed
- Approvals required (change control for production)

## Procedure
1. Step-by-step instructions
2. Include exact commands with placeholders
3. Expected output at each step
4. Verification steps

## Rollback
How to reverse the procedure if something goes wrong.

## Escalation
Who to contact if the procedure fails.
```

#### Essential EDC Runbooks
- **Database failover**: Steps to promote a replica, with data integrity verification.
- **Study environment provisioning**: Creating a new study database and configuration.
- **Data export for regulatory submission**: Generating compliant export packages.
- **User access review**: Periodic review of active users and their roles.
- **Incident response**: Steps for investigating and containing data integrity issues.
- **Audit trail verification**: Confirming audit trail completeness and integrity.
- **Certificate rotation**: Updating SSL/TLS certificates without downtime.
- **Backup restoration and verification**: Testing backup restores with data integrity checks.

### 4. Compliance Documentation

#### IQ (Installation Qualification)
- Documents that the system is installed correctly per specifications.
- Generate from: deployment logs, infrastructure-as-code outputs, environment configurations.
- Content includes:
  - Hardware/infrastructure specifications and verification.
  - Software component versions and checksums.
  - Network configuration and connectivity verification.
  - Security configuration (firewalls, access controls, encryption).
  - Backup configuration verification.

#### OQ (Operational Qualification)
- Documents that the system operates correctly under expected conditions.
- Generate from: automated test results, integration test reports.
- Content includes:
  - Functional test results mapped to requirements.
  - Security testing results (authentication, authorization, session management).
  - Audit trail verification test results.
  - Electronic signature workflow test results.
  - Data validation and edit check test results.
  - Boundary and negative test results.

#### PQ (Performance Qualification)
- Documents that the system performs as expected in its production environment.
- Generate from: production monitoring data, user acceptance test results.
- Content includes:
  - Performance benchmarks under expected load.
  - End-to-end workflow verification by business users.
  - Data migration verification (if applicable).
  - Integration verification with external systems.

#### Traceability Matrix
- Map user requirements to functional specifications to test cases to test results.
- Generate by scanning requirement IDs in code comments, test names, and documentation.
- Format:

```markdown
| Requirement ID | Description | Test Case(s) | Result | Evidence |
|---------------|-------------|--------------|--------|----------|
| REQ-AUTH-001 | Users must authenticate with username and password | TC-AUTH-001, TC-AUTH-002 | Pass | test-report-v2.3.pdf |
| REQ-AUDIT-001 | All data changes must be recorded in audit trail | TC-AUDIT-001 through TC-AUDIT-015 | Pass | test-report-v2.3.pdf |
```

## Generation Workflow

### Step 1: Codebase Analysis
- Scan for route/controller files to identify API endpoints.
- Read configuration files (docker-compose, k8s manifests, terraform) for architecture.
- Find test files and extract test descriptions and coverage.
- Locate existing documentation and assess currency.

### Step 2: Content Extraction
- Parse code comments, docstrings, and annotations.
- Extract type definitions, interfaces, and schemas.
- Read migration files for database schema understanding.
- Analyze error handling for error code documentation.

### Step 3: Documentation Assembly
- Organize extracted information into the appropriate template.
- Cross-reference between documents for consistency.
- Flag gaps where documentation cannot be generated from code alone.
- Mark sections that require manual review or human input.

### Step 4: Quality Checks
- Verify all API endpoints are documented.
- Check that documented interfaces match actual code.
- Validate that compliance references are accurate.
- Ensure runbook commands are syntactically correct.

## Style Guidelines

- Use clear, unambiguous language. Avoid jargon that regulators may not understand.
- Include version numbers and dates on all documents.
- Use consistent terminology (align with CDISC, ICH, and FDA glossaries).
- Mark draft documents clearly. Only finalized documents enter the quality system.
- Keep paragraphs short. Use tables and lists for structured information.
- Include diagrams described in text (Mermaid, PlantUML) that can be rendered.

## When Applying This Skill

1. Determine which documentation type is needed.
2. Scan the relevant parts of the codebase using Grep and Glob.
3. Read key files to extract detailed information.
4. Assemble the documentation following the templates and structure above.
5. Note any gaps or areas requiring manual input.
6. If generating compliance documentation, explicitly state which regulatory requirements are addressed.
