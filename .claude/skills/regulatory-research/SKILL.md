---
name: regulatory-research
description: Research and summarize FDA guidance documents, ICH guidelines, 21 CFR Part 11, EU Annex 11, and GDPR for clinical trials with impact analysis on Talosix product features.
---

# Regulatory Research

## Purpose

Research, summarize, and cross-reference regulatory requirements applicable to Talosix clinical trial EDC systems. Provide impact analysis of regulations on product features and operational processes.

## When to Use

- Evaluating regulatory requirements for new features
- Responding to regulatory questions from clients or auditors
- Preparing regulatory compliance documentation
- Assessing impact of new or updated regulations
- Supporting pre-submission planning
- Training content development

## Core Regulatory Framework

### Tier 1: Directly Applicable Regulations

#### 21 CFR Part 11 - Electronic Records; Electronic Signatures (FDA)

**Scope**: Applies to records required by FDA predicate rules that are created, modified, maintained, archived, retrieved, or transmitted in electronic form.

**Key Requirements for EDC Systems:**

| Subpart | Section | Requirement | EDC Impact |
|---------|---------|-------------|------------|
| B | 11.10(a) | System validation | Full validation lifecycle required |
| B | 11.10(b) | Record legibility and retrieval | Records must be exportable and printable |
| B | 11.10(c) | Record protection | Backup, recovery, retention controls |
| B | 11.10(d) | Access limitation | Role-based access, unique user IDs |
| B | 11.10(e) | Audit trails | Computer-generated, timestamped, reason for change |
| B | 11.10(f) | Operational system checks | Sequence enforcement for workflows |
| B | 11.10(g) | Authority checks | Prevent unauthorized actions |
| B | 11.10(h) | Device checks | Source data verification capabilities |
| B | 11.10(i) | Training | Documented training for all users |
| B | 11.10(j) | Written policies | SOPs for system use and e-sig accountability |
| B | 11.10(k) | Documentation controls | Distribution, access, revision controls |
| C | 11.50 | Signature manifestations | Display signer name, date/time, meaning |
| C | 11.70 | Signature/record linking | Signatures inseparable from records |
| C | 11.100 | General e-sig requirements | Each signature unique to one individual |
| C | 11.200 | Components and controls | At minimum user ID + password |

**Key FDA Guidance Documents:**

1. Part 11, Electronic Records; Electronic Signatures -- Scope and Application (2003)
   - Clarifies FDA enforcement discretion
   - Narrowed scope to predicate rule records
   - Audit trails and record retention remain enforced

2. Guidance for Industry: Computerized Systems Used in Clinical Investigations (2007)
   - Source data at investigator sites
   - Sponsor responsibilities for electronic systems
   - Audit trail requirements for clinical data

3. Data Integrity and Compliance With Drug CGMP (2018)
   - ALCOA+ principles
   - Data governance expectations
   - Applicable concepts extend to clinical data

4. Use of Electronic Records and Electronic Signatures in Clinical Investigations Under 21 CFR Part 11 (2017)
   - Specific to clinical investigations
   - Remote access and electronic consent
   - Expectations for EDC systems

#### EU Annex 11 - Computerised Systems (EMA)

**Scope**: Applies to all forms of computerised systems used as part of GMP regulated activities.

**Key Sections and EDC Impact:**

| Section | Requirement | EDC Impact |
|---------|-------------|------------|
| 1 | Risk management applied throughout lifecycle | Risk-based validation approach |
| 2 | Personnel with appropriate qualifications | Training documentation required |
| 3 | Suppliers and service providers assessed | Vendor qualification needed |
| 4 | Validation proportionate to risk | GAMP 5 methodology |
| 5 | Data accuracy, integrity checks | Edit checks, data validation |
| 6 | Accuracy checks on data input | Input validation controls |
| 7 | Data storage with backup and recovery | Data retention and backup |
| 8 | Printouts of electronically stored data | Print and export capabilities |
| 9 | Audit trails for GMP relevant changes | Comprehensive audit trail |
| 10 | Change management procedures | Formal change control |
| 11 | Periodic evaluation of systems | Periodic review process |
| 12 | Security controls | Access management, encryption |
| 13 | Incident management | Issue tracking and CAPA |
| 14 | Electronic signatures | E-signature controls |
| 16 | Business continuity | DR and continuity plans |
| 17 | Archiving | Long-term data archiving |

#### ICH E6(R2) - Good Clinical Practice

**Scope**: International standard for design, conduct, recording, and reporting of clinical trials.

**Key Sections for EDC:**

- 1.11: Documentation definition includes electronic records
- 4.9: Records and reports requirements
- 5.5: Trial management, data handling, record keeping
- 5.5.3: Electronic data handling systems require validation
- 5.18.4(n): Monitoring includes computerized systems verification
- 8.1: Essential documents in electronic format

#### ICH E6(R3) - GCP Renovation (Emerging)

Key changes relevant to EDC:
- Proportionality principle for quality management
- Enhanced focus on risk-based approaches
- Technology-agnostic language
- Expanded guidance on electronic systems
- Greater emphasis on data governance

### Tier 2: Data Privacy Regulations

#### GDPR (EU General Data Protection Regulation)

**Applicability to Clinical Trials:**

| Article | Requirement | EDC Impact |
|---------|-------------|------------|
| 5 | Data processing principles | Lawfulness, purpose limitation, minimization |
| 6 | Lawful basis for processing | Consent or legitimate interest documentation |
| 9 | Special category data (health) | Explicit consent or regulatory basis needed |
| 15-22 | Data subject rights | Right of access, rectification, erasure considerations |
| 25 | Data protection by design | Privacy built into system architecture |
| 28 | Processor requirements | Data processing agreements with vendors |
| 30 | Records of processing | Processing activity documentation |
| 32 | Security of processing | Encryption, pseudonymization, access controls |
| 33-34 | Breach notification | 72-hour notification capability |
| 35 | Data Protection Impact Assessment | DPIA for clinical trial data processing |
| 44-49 | International transfers | Standard contractual clauses, adequacy decisions |

**Clinical Trial Regulation (EU) 536/2014 Interaction:**
- Article 28(2): GDPR rights vs. clinical data integrity
- Erasure right limited by data retention requirements
- Pseudonymization requirements for clinical data

### Tier 3: Regional and Supplementary Standards

#### MHRA (UK Post-Brexit)

- UK GDPR and Data Protection Act 2018
- MHRA GXP Data Integrity Guidance (2018)
- UK clinical trial regulation alignment

#### PMDA (Japan)

- ERES Guidelines (Electronic Records, Electronic Signatures)
- J-GCP requirements

#### TGA (Australia)

- Australian Privacy Act
- TGA GCP inspection requirements

#### Health Canada

- Division 5 of the Food and Drug Regulations
- Data integrity expectations per GAMP 5

## Research Methodology

### When Researching a Regulatory Question

1. **Identify the question scope**: Which regulations, which jurisdictions, which product features
2. **Primary source review**: Read the actual regulation text, not summaries
3. **Guidance document review**: Check FDA guidance, EMA Q&A documents, ICH guidelines
4. **Industry interpretation**: Review ISPE GAMP, DIA publications, regulatory authority presentations
5. **Cross-reference**: Identify conflicts or gaps between jurisdictions
6. **Impact assessment**: Map findings to specific Talosix features and processes

### Impact Analysis Template

```
Regulatory Finding Impact Assessment

Regulation: [Name and section]
Jurisdiction: [FDA/EMA/ICH/etc.]
Requirement Summary: [Plain language summary]

Impact on Talosix EDC:
- Feature(s) Affected: [List specific features]
- Current Compliance Status: Compliant / Partially Compliant / Gap
- Gap Description: [If applicable]
- Remediation Required: [Yes/No]
- Remediation Approach: [Brief description]
- Estimated Effort: [T-shirt size]
- Priority: [Based on regulatory risk]
- Timeline: [Recommended completion]

Cross-Reference:
- Related requirements from other regulations: [List]
- Existing URS items addressing this: [List]
- Existing test cases verifying this: [List]
```

### Regulatory Change Monitoring

Track regulatory updates relevant to clinical trial EDC:

```
| Date | Source | Change Description | Impact Level | Action Required | Owner | Status |
```

**Sources to Monitor:**
- FDA Federal Register notices
- EMA guideline publications
- ICH guideline updates
- ISPE GAMP publications
- DIA regulatory intelligence
- National competent authority publications

## Common Regulatory Questions for EDC

Maintain a knowledge base of frequently asked regulatory questions:

1. What constitutes an electronic record under Part 11?
2. When is an audit trail entry required?
3. What metadata must be captured with an electronic signature?
4. How long must clinical trial data be retained?
5. What are the requirements for data migration between systems?
6. When does a PDF constitute a "true copy" of an electronic record?
7. What are the expectations for cloud-hosted GxP systems?
8. How do GDPR subject access requests interact with clinical data?
9. What validation evidence is required for COTS vs custom software?
10. What are the expectations for mobile/ePRO data capture?

## Output Format

When performing regulatory research, deliver:

1. **Executive summary**: Key findings in plain language
2. **Regulatory citations**: Exact regulation text with section numbers
3. **Cross-reference table**: How requirements align across jurisdictions
4. **Impact assessment**: Specific impact on Talosix features
5. **Recommendations**: Prioritized action items
6. **References**: Complete list of sources consulted

## Regulatory References

- 21 CFR Part 11 (FDA)
- EU Annex 11 (EMA)
- ICH E6(R2) and E6(R3) (ICH)
- EU GDPR (Regulation 2016/679)
- EU Clinical Trials Regulation 536/2014
- ISPE GAMP 5 (Second Edition)
- FDA Guidance Documents (see individual citations above)
- MHRA GXP Data Integrity Guidance (2018)
- PIC/S Good Practices for Data Management (2021)
