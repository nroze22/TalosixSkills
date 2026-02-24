---
name: reconcile-payments
description: Reconcile invoices and payments for Talosix. Flag anomalies and discrepancies, maintain audit trail documentation, and support compliance reporting.
---

# Reconcile Payments Skill

Assist with invoice and payment reconciliation for Talosix. Ensure financial records are accurate, discrepancies are flagged and resolved, and audit trail requirements are maintained.

## When to Use

- Monthly or periodic payment reconciliation
- When invoices need to be matched against payments received
- When discrepancies or anomalies are detected in financial records
- When preparing financial data for audit or compliance review
- When generating reconciliation reports

## Reconciliation Process

### Step 1: Data Collection

Gather the following data sources for the reconciliation period:
- Invoices issued (from billing system)
- Payments received (from bank/payment processor)
- Customer contracts and pricing agreements
- Credit notes and adjustments
- Previous period carryforward balances

### Step 2: Invoice-to-Payment Matching

Match payments to invoices using these criteria:

```markdown
## Matching Rules (in priority order)
1. Exact match: Payment amount = Invoice amount, same customer
2. Reference match: Payment reference contains invoice number
3. Customer match: Payment from known customer, amount within tolerance
4. Partial match: Payment covers portion of invoice (flag for review)
5. Overpayment: Payment exceeds invoice amount (flag for review)
6. Unmatched: No matching invoice found (flag for investigation)
```

### Step 3: Discrepancy Identification

Flag the following anomalies:

| Anomaly Type | Description | Severity | Action Required |
|-------------|-------------|----------|-----------------|
| Unmatched payment | Payment received with no matching invoice | High | Identify source, create invoice or refund |
| Unmatched invoice | Invoice with no payment beyond terms | High | Follow up with customer, assess collectibility |
| Partial payment | Payment less than invoice amount | Medium | Confirm intentional, track remaining balance |
| Overpayment | Payment exceeds invoice amount | Medium | Confirm with customer, issue credit or refund |
| Duplicate payment | Same amount from same customer twice | High | Verify, issue refund if confirmed duplicate |
| Amount variance | Payment differs from invoice by small amount | Low | Investigate (FX differences, fees, rounding) |
| Late payment | Payment received after due date | Low | Document, assess late fee applicability |
| Currency mismatch | Payment currency differs from invoice | Medium | Apply exchange rate, document variance |

### Step 4: Reconciliation Report

Generate a reconciliation report with the following structure:

```markdown
# Payment Reconciliation Report
Period: <start date> to <end date>
Prepared by: <name>
Date: <date>

## Summary
| Metric | Amount | Count |
|--------|--------|-------|
| Total invoiced | $<amount> | <count> |
| Total payments received | $<amount> | <count> |
| Matched (exact) | $<amount> | <count> |
| Matched (with variance) | $<amount> | <count> |
| Unmatched invoices | $<amount> | <count> |
| Unmatched payments | $<amount> | <count> |
| Net variance | $<amount> | - |

## Aged Receivables
| Aging Bucket | Amount | Count |
|-------------|--------|-------|
| Current (0-30 days) | $<amount> | <count> |
| 31-60 days | $<amount> | <count> |
| 61-90 days | $<amount> | <count> |
| 91+ days | $<amount> | <count> |

## Discrepancies Requiring Resolution
| # | Type | Customer | Invoice | Amount | Variance | Status |
|---|------|----------|---------|--------|----------|--------|
| 1 | <type> | <customer> | <inv#> | <amount> | <variance> | Open |

## Actions Taken
| # | Action | Responsible | Due Date | Status |
|---|--------|-------------|----------|--------|
| 1 | <action> | <name> | <date> | <status> |

## Notes
<any explanatory notes, FX rate used, special circumstances>
```

### Step 5: Resolution Tracking

For each discrepancy, track resolution:

```markdown
## Discrepancy Resolution Log
| Discrepancy # | Identified | Description | Root Cause | Resolution | Resolved Date | Resolved By |
|---------------|-----------|-------------|------------|------------|---------------|-------------|
| 1 | <date> | <description> | <root cause> | <resolution> | <date> | <name> |
```

## Clinical Trial EDC Billing Context

Talosix billing has domain-specific considerations:

### Revenue Models
- **Per-study licensing**: Fixed fee per study setup
- **Per-site fees**: Monthly fee per active clinical site
- **Per-subject fees**: Fee per enrolled or randomized subject
- **Professional services**: Implementation, configuration, training hours
- **Support tiers**: Standard, premium, enterprise SLA-based pricing

### Reconciliation Considerations
- **Study lifecycle**: Invoice adjustments when studies close early or extend
- **Site activation timing**: Pro-rated billing for mid-month activations/deactivations
- **Subject milestones**: Verify subject counts against EDC system data
- **Change orders**: Scope changes during study execution that modify pricing
- **Multi-currency**: Global trials often involve multiple currencies across regions

### Contract Verification
When reconciling, cross-check against:
- Master Service Agreement (MSA) terms
- Statement of Work (SOW) for each study
- Change orders and amendments
- Agreed payment terms (Net 30, Net 45, etc.)
- Volume discount thresholds

## Audit Trail Documentation

All reconciliation activities must maintain audit trails:

1. **Record every action**: Who did what, when, and why
2. **Preserve source data**: Original records before any adjustments
3. **Document adjustments**: Every adjustment with justification and approval
4. **Retain evidence**: Supporting documents for discrepancy resolutions
5. **Version control**: Track changes to reconciliation reports

### Audit-Ready Format
```markdown
## Audit Trail Entry
Date/Time: <timestamp>
Action: <description of action taken>
Performed by: <name>
Reason: <justification>
Supporting evidence: <reference to document/email/approval>
Before state: <original values>
After state: <adjusted values>
Approved by: <name, if applicable>
```

## Compliance Reporting

### SOX Compliance Considerations
- Segregation of duties: Different people for invoice creation, payment receipt, and reconciliation
- Timely reconciliation: Complete within defined period after month-end
- Management review: Reconciliation reviewed and approved by finance manager
- Exception documentation: All exceptions documented with resolution

### Revenue Recognition (ASC 606)
- Verify revenue recognition timing against contract performance obligations
- Multi-element arrangements: Allocate revenue across deliverables
- Variable consideration: Subject-based fees may require estimation
- Contract modifications: Account for scope changes properly

## Output Format

When performing reconciliation, always produce:
1. A reconciliation summary with key metrics
2. A list of discrepancies with severity ratings
3. Recommended actions for each discrepancy
4. An audit trail of the reconciliation process
5. Aged receivables analysis if applicable
