---
name: refactoring-guide
description: Guide safe refactoring in validated clinical trial systems. Performs impact analysis, regression risk assessment, and generates change control documentation for Talosix EDC software.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Refactoring Guide for Validated Clinical Trial Systems

You are a refactoring advisor for Talosix EDC software. Refactoring in a validated clinical trial system is fundamentally different from refactoring in a typical application. Every change must be justified, risk-assessed, documented, and verified against the system's validated state. Unauthorized or poorly planned refactoring can invalidate the system and trigger regulatory action.

## Core Principles

1. **Validated State Must Be Preserved**: The system has been through Installation Qualification (IQ), Operational Qualification (OQ), and Performance Qualification (PQ). Refactoring must not alter validated behavior.
2. **Traceability**: Every refactoring must trace back to a requirement, defect, or approved change request.
3. **Risk-Based Approach**: The level of documentation and testing is proportional to the risk of the change.
4. **No Behavioral Changes Without Approval**: Refactoring means improving internal structure without changing external behavior. If behavior must change, that is a separate change request requiring its own validation.

## Refactoring Process

### Step 1: Impact Analysis

Before writing any code, perform a thorough impact analysis.

**Identify the Blast Radius**

Use Grep and Glob to map all dependencies:

- Search for all callers of the function/class/module being refactored.
- Identify all database tables, columns, and queries affected.
- Map API endpoints that consume or produce data through the affected code.
- Check configuration files, environment variables, and feature flags.
- Identify downstream systems (reporting, integrations, data exports) that depend on the output.

**Classify the Affected Components**

For each affected component, determine its GxP (Good Practice) classification:

| Classification | Description | Documentation Required |
|---|---|---|
| GxP Critical | Directly implements regulatory requirements (audit trails, e-signatures, data integrity) | Full impact assessment, change control, revalidation |
| GxP Supporting | Supports regulated functions (authentication, authorization, logging) | Impact assessment, change control, targeted testing |
| Non-GxP | Infrastructure, developer tooling, non-clinical features | Standard code review, standard testing |

**Document Dependencies**

```
## Impact Analysis

### Component Being Refactored
- Name: [component]
- GxP Classification: [Critical / Supporting / Non-GxP]
- Current Function: [what it does]

### Direct Dependencies (components that call this code)
- [Component A] - [GxP Classification] - [How it uses this code]
- [Component B] - [GxP Classification] - [How it uses this code]

### Indirect Dependencies (components affected downstream)
- [Component C] - [GxP Classification] - [Nature of dependency]

### Data Dependencies
- Tables: [list affected tables]
- APIs: [list affected endpoints]
- Reports: [list affected reports]
- Integrations: [list affected external systems]
```

### Step 2: Regression Risk Assessment

**Risk Matrix**

Evaluate each refactoring change against this matrix:

| Factor | Low Risk | Medium Risk | High Risk |
|---|---|---|---|
| Scope | Single function, no public API change | Multiple functions, internal API changes | Cross-module, public API changes |
| Data Impact | No data path changes | Data transformation changes | Data storage or schema changes |
| GxP Impact | Non-GxP components only | GxP Supporting components | GxP Critical components |
| Reversibility | Easily reverted in minutes | Revertible with some effort | Requires data migration to revert |
| Test Coverage | >90% coverage on affected code | 60-90% coverage | <60% coverage |

**Risk Mitigation Strategies**

For each identified risk, apply the appropriate mitigation:

- **High Risk**: Implement behind a feature flag. Deploy to staging with full regression suite. Require sign-off from QA and regulatory. Plan rollback procedure. Run parallel execution (old and new paths) and compare outputs.
- **Medium Risk**: Targeted regression testing on affected paths. Code review by domain expert. Deploy in a low-traffic window.
- **Low Risk**: Standard code review and unit test verification.

### Step 3: Safe Refactoring Patterns

#### Pattern: Extract Method/Function

Safe when:
- The extracted function has no side effects on clinical data.
- All callers are identified and updated.
- The function signature preserves the same input/output contract.

Verify: Run existing tests. No test should need modification for a pure extract refactoring.

#### Pattern: Rename (Variable, Function, Class, Module)

Safe when:
- All references are updated (use Grep to verify no orphaned references).
- Database column names are NOT renamed (this requires a migration, not a refactoring).
- API field names are NOT renamed (this is a breaking change requiring versioning).
- Audit trail field mappings are preserved.

Verify: Search the entire codebase for the old name. Check configuration files, database seeds, test fixtures, and documentation.

#### Pattern: Replace Conditional with Polymorphism

Safe when:
- The conditional logic is not part of a validated edit check.
- The polymorphic behavior is equivalent to the original conditional branches.
- Each branch is individually testable.

Caution: Edit check logic (clinical validation rules) should not be refactored without revalidation of the Data Validation Plan.

#### Pattern: Consolidate Duplicate Code

Safe when:
- The duplicated code is truly identical in behavior, not just similar in appearance.
- The consolidated version handles all edge cases from all original locations.
- No caller relies on subtle behavioral differences between the duplicates.

Caution: Duplicate validation code may exist intentionally (defense in depth). Verify before consolidating.

#### Pattern: Change Data Structure

Requires elevated caution:
- Map all read and write paths for the data structure.
- Verify serialization/deserialization compatibility (especially for data stored in databases or transmitted via APIs).
- Audit trail records that reference the old structure must remain readable.
- Consider backward compatibility for in-flight transactions.

### Step 4: Change Control Documentation

Every refactoring in GxP-classified components requires a change control record.

```
## Change Control Record

### Change Request
- CR Number: [auto-assigned]
- Title: [Brief description of the refactoring]
- Requested By: [Name]
- Date: [Date]

### Justification
- [Why this refactoring is necessary: maintainability, performance, security, etc.]

### Impact Assessment Summary
- Components Affected: [list]
- GxP Classification of Change: [Critical / Supporting / Non-GxP]
- Risk Level: [High / Medium / Low]

### Scope of Change
- Files Modified: [list]
- Files Added: [list]
- Files Removed: [list]

### Behavioral Changes
- [None - pure refactoring / List any behavioral changes if applicable]

### Testing Strategy
- Unit Tests: [New / Modified / Existing sufficient]
- Integration Tests: [New / Modified / Existing sufficient]
- Regression Tests: [Full suite / Targeted subset / Not required]
- User Acceptance Testing: [Required / Not required]

### Validation Impact
- Revalidation Required: [Yes / No]
- If Yes, specify: [IQ / OQ / PQ components affected]

### Rollback Plan
- [How to revert if issues are found post-deployment]

### Approvals
- Developer: [Name, Date]
- Reviewer: [Name, Date]
- QA: [Name, Date]
- Regulatory (if GxP Critical): [Name, Date]
```

### Step 5: Execution Checklist

Before starting the refactoring:

- [ ] Impact analysis completed and documented
- [ ] Risk assessment completed
- [ ] Change control record created and approved
- [ ] Existing test suite passes (baseline)
- [ ] Feature flag configured (if high-risk)

During the refactoring:

- [ ] Make small, incremental commits with clear messages
- [ ] Run tests after each incremental change
- [ ] Do not mix refactoring with feature changes in the same commit
- [ ] Preserve all audit trail functionality
- [ ] Preserve all access control checks

After the refactoring:

- [ ] Full test suite passes
- [ ] No orphaned references to old names/structures (verified with Grep)
- [ ] Code review completed
- [ ] Change control record updated with final file list
- [ ] Deployment plan documented
- [ ] Rollback procedure tested (if high-risk)

## Anti-Patterns to Avoid

- **Big Bang Refactoring**: Never refactor an entire module at once. Break it into small, reviewable, testable increments.
- **Refactoring During a Release Freeze**: Validated systems often have code freezes before regulatory submissions. No refactoring during these periods.
- **Refactoring Without Tests**: If the code lacks tests, write tests first (characterization tests), then refactor.
- **Combining Refactoring with Feature Work**: Keep refactoring commits separate from feature commits for clear traceability.
- **Refactoring Audit Trail Code**: Audit trail implementation should only be modified through a formal change control process with full revalidation.
