---
name: contract-review-for-clinical-trial-software
description: Review contract terms for clinical trial EDC software agreements, flag risk areas, suggest modifications, and ensure SLA compliance and proper data handling requirements.
---

# Contract Review for Clinical Trial Software

## Purpose

Review and analyze contracts for Talosix clinical trial EDC software agreements. Identify risk areas, suggest favorable modifications, compare terms to industry standards, and ensure compliance with regulatory requirements specific to clinical trial data systems.

## When to Use

- Reviewing a prospect's paper (their standard vendor agreement)
- Preparing Talosix's MSA/Order Form for a new customer
- Reviewing redlines from prospect's legal team
- Assessing renewal terms and amendments
- Reviewing subcontractor or partner agreements

## Important Disclaimer

This skill supports contract analysis but does not replace legal counsel. All recommendations should be reviewed by Talosix's legal team before any contractual commitments are made.

## Information Gathering

1. **Contract Document(s)** - MSA, Order Form, SOW, SLA, DPA, BAA as applicable
2. **Deal Context** - Deal value, customer type (sponsor/CRO/academic), strategic importance
3. **Customer Profile** - Company size, regulatory jurisdiction (US, EU, global)
4. **Talosix Standard Terms** - Current template MSA for comparison
5. **Prior Negotiations** - Any previously agreed deviations for this customer
6. **Risk Tolerance** - Standard deal vs. strategic account with higher flexibility

## Key Contract Sections to Review

### 1. Scope of Services

**Review for:**
- Clear definition of what is included (platform access, modules, support, services)
- What is explicitly excluded
- Change order process for scope additions
- Ambiguous language that could lead to scope disputes

**Red Flags:**
- Unlimited scope language ("all services necessary to support the study")
- No change order mechanism for out-of-scope requests
- Bundling of professional services with platform licensing without clear delineation

**Standard Position:**
- Services defined by reference to a specific Order Form or SOW
- Clear enumeration of included modules and user counts
- Documented change order process with pricing for additions

### 2. Data Ownership & Handling

**Review for:**
- Clear statement that customer owns their clinical trial data
- Talosix's rights to data (processing only, no secondary use of identifiable data)
- Data portability and export provisions
- Data retention and deletion obligations post-termination
- Data residency requirements (geographic restrictions on where data is stored)
- Subprocessor consent and notification requirements

**Red Flags:**
- Customer claims ownership of Talosix platform IP or configurations
- No data portability provisions (creates lock-in perception)
- Unlimited data retention obligations post-termination
- Restrictions on data residency that conflict with Talosix infrastructure
- Prohibition on all subprocessors without alternative

**Standard Position:**
- Customer owns all clinical trial data entered into the platform
- Talosix retains ownership of platform, algorithms, aggregate/anonymized analytics
- Data export in standard formats (CDISC, CSV) available at any time
- Data retained for [90-180 days] post-termination, then securely deleted
- Data Processing Agreement (DPA) governs GDPR and privacy requirements
- Subprocessor list provided with notification of changes

### 3. Service Level Agreement (SLA)

**Review for:**
- Uptime commitment (target: 99.9% or higher)
- How uptime is measured (excluding scheduled maintenance)
- Scheduled maintenance windows and notification requirements
- Response time commitments by severity level
- Remedies for SLA breach (service credits, termination rights)
- Reporting obligations (monthly uptime reports)

**Red Flags:**
- Uptime commitments above 99.99% (unrealistic and expensive to guarantee)
- SLA credits exceeding one month of fees (uncapped liability risk)
- No exclusions for customer-caused outages or force majeure
- Response time guarantees with financial penalties (vs. targets with credits)
- Customer right to terminate for a single SLA miss

**Standard Position:**

| Severity | Definition | Response Time | Resolution Target |
|----------|-----------|--------------|-------------------|
| Critical | System unavailable, data integrity risk | 1 hour | 4 hours |
| High | Major feature unavailable, workaround possible | 4 hours | 24 hours |
| Medium | Non-critical issue, limited impact | 8 business hours | 5 business days |
| Low | Minor issue, cosmetic, enhancement request | 2 business days | Next release |

- SLA credits: 5% of monthly fee per 0.1% below 99.9%, capped at 25% of monthly fee
- Credits are sole and exclusive remedy for SLA failures

### 4. Compliance & Regulatory

**Review for:**
- 21 CFR Part 11 compliance obligations and representations
- HIPAA BAA requirements (if PHI is involved)
- GDPR DPA requirements (if EU data subjects)
- GCP/ICH E6(R2) alignment commitments
- Audit rights (customer and regulatory authority)
- Validation support obligations
- Record retention for regulatory purposes (distinguish from general data retention)

**Red Flags:**
- Unlimited audit rights without reasonable notice or frequency limits
- Requirement to maintain compliance with future unknown regulations at no cost
- Guarantee of specific regulatory outcomes (e.g., "guarantee no FDA findings")
- No carve-out for regulatory record retention from general deletion obligations

**Standard Position:**
- Talosix represents and warrants 21 CFR Part 11 compliance for applicable functionality
- BAA executed for any engagement involving PHI
- Audit rights: once per year, 30 days notice, during business hours, at customer's expense
- Regulatory authority audit: reasonable cooperation with no frequency limit
- Validation documentation (IQ/OQ/PQ) provided as part of implementation
- Regulatory record retention: data retained as required by applicable law regardless of contract termination

### 5. Intellectual Property

**Review for:**
- Clear delineation: customer data vs. Talosix platform IP
- License grant scope (non-exclusive, limited to contract term)
- Custom development ownership (who owns custom features built for the customer)
- Feedback and suggestions clause
- Open source software disclosure

**Red Flags:**
- Customer claims IP rights to platform customizations or configurations
- Broad assignment of all IP created during the engagement to customer
- No license back to Talosix for general improvements inspired by customer work
- Work-for-hire language applied to standard platform configuration

**Standard Position:**
- Talosix owns all platform IP, including configurations and templates
- Customer receives a non-exclusive license to use the platform during the term
- Custom development: Talosix owns the code, customer has a perpetual license to the output
- Talosix may incorporate general learnings (not customer data) into the platform

### 6. Limitation of Liability

**Review for:**
- Cap on total liability (standard: 12 months of fees paid)
- Exclusion of consequential, indirect, and special damages
- Carve-outs from the cap (IP indemnification, confidentiality breach, data breach, willful misconduct)
- Mutual vs. one-sided limitations

**Red Flags:**
- Uncapped liability for any category
- Liability cap based on total contract value (multi-year) rather than annual fees
- No mutual limitation (only protects one party)
- Unlimited consequential damages for data breaches
- Customer indemnification obligations that exceed the liability cap

**Standard Position:**
- Mutual liability cap: 12 months of fees paid or payable
- Mutual exclusion of consequential, indirect, incidental, and punitive damages
- Carve-outs (higher cap, e.g., 2x annual fees): IP infringement, confidentiality breach, gross negligence
- Data breach liability: capped but at a reasonable elevated level (e.g., 2-3x annual fees)

### 7. Term, Termination & Transition

**Review for:**
- Initial term and renewal mechanism (auto-renew vs. manual)
- Termination for convenience rights and notice periods
- Termination for cause provisions and cure periods
- Transition assistance obligations and costs
- Data return/export timeline post-termination
- Survival clauses

**Red Flags:**
- Customer-only termination for convenience with short notice (< 90 days)
- No cure period for breach-based termination
- Extended free transition assistance obligations (> 90 days)
- No termination fee for early exit on multi-year deals
- Ambiguous data handling post-termination

**Standard Position:**
- Initial term: 1-3 years with auto-renewal for successive 1-year terms
- Termination for convenience: either party, 90 days written notice, effective at end of current term
- Termination for cause: 30-day cure period for material breach
- Transition assistance: 90 days at then-current rates
- Data export: available in standard formats for 90 days post-termination, then deleted

### 8. Insurance & Indemnification

**Review for:**
- Required insurance types and minimum coverage amounts
- Indemnification scope (IP, data breach, negligence, regulatory)
- Indemnification procedure (notice, control of defense, cooperation)
- Mutual vs. one-sided indemnification

**Standard Position:**
- Mutual indemnification for third-party IP claims
- Talosix indemnifies for platform IP infringement
- Customer indemnifies for data content and use of platform outside terms
- Insurance minimums: General liability ($1M), Professional liability/E&O ($2M), Cyber liability ($5M)

## Review Workflow

1. **Initial Read** - Read the full contract, noting all non-standard terms
2. **Comparison** - Compare against Talosix standard terms, flag deviations
3. **Risk Classification** - Categorize each deviation as:
   - **Accept** - Reasonable, low risk, common in industry
   - **Negotiate** - Unfavorable but not deal-breaking, propose alternative
   - **Reject** - Unacceptable risk, must be changed or removed
4. **Redline Preparation** - Draft specific alternative language for Negotiate and Reject items
5. **Legal Review** - Submit analysis to Talosix legal for final review before responding

## Output Format

1. **Contract Risk Summary** - One-page overview with risk rating (Low/Medium/High) and top issues
2. **Clause-by-Clause Analysis** - Table with each flagged clause, risk level, issue description, and recommended position
3. **Suggested Redlines** - Specific alternative language for each item requiring negotiation
4. **Comparison to Standard Terms** - Side-by-side showing deviations from Talosix template
5. **Negotiation Strategy** - Priority ordering of issues and recommended concession strategy
