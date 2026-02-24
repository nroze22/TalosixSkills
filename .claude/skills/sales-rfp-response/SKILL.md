---
name: sales-rfp-response
description: "Generate comprehensive RFP responses for Talosix clinical trial EDC, addressing security questionnaires, vendor assessments, compliance certifications, and therapeutic area tailoring."
---

# Sales RFP Response Generator

## Purpose

Generate thorough, accurate RFP responses for clinical trial EDC procurement processes. Responses must demonstrate Talosix's regulatory compliance, technical capabilities, and domain expertise while being tailored to the specific prospect's therapeutic focus and study requirements.

## When to Use

- Responding to formal RFPs/RFIs from sponsors, CROs, or academic research organizations
- Completing security questionnaires and vendor assessment forms
- Preparing compliance documentation packages for procurement teams
- Addressing due diligence requests during vendor selection

## Information Gathering

Before generating a response, collect the following:

1. **RFP Document** - The full RFP/RFI with all questions and requirements
2. **Prospect Profile** - Company name, type (sponsor/CRO/academic), size, therapeutic areas
3. **Study Details** - Phase (I-IV), indication, estimated site count, subject count, geography
4. **Deadline** - Submission date and any page/format constraints
5. **Competitive Context** - Known competing vendors if available

## Confluence Integration

Pull standard company information from Confluence for consistent responses:

- **Company Overview** - Search Confluence for "Talosix Company Overview" or "About Talosix" pages
- **Compliance Certifications** - Search for "Compliance Certifications", "21 CFR Part 11", "SOC 2 Report", "HIPAA Compliance" pages
- **Technical Architecture** - Search for "Platform Architecture", "Infrastructure Overview" pages
- **Security Documentation** - Search for "Security Whitepaper", "Data Security", "Penetration Testing" pages
- **Customer References** - Search for "Customer References", "Case Studies" pages
- **Implementation Methodology** - Search for "Implementation Process", "Onboarding" pages

When Confluence access is available, use the Atlassian MCP tools to search and retrieve these pages. If unavailable, note which sections need manual input from the team.

## Response Structure

### 1. Executive Summary (1-2 pages)

Tailor to the prospect's specific needs:

```
- Opening: Acknowledge the prospect's mission and therapeutic focus
- Value proposition: Why Talosix is the right EDC partner for THIS study/program
- Key differentiators: 3-4 points most relevant to their requirements
- Compliance summary: Certifications and regulatory alignment
- Call to action: Next steps and point of contact
```

### 2. Company Overview

Include:
- Talosix founding, mission, and clinical trial focus
- Number of studies supported, subjects enrolled, sites active
- Therapeutic area experience relevant to the prospect
- Financial stability and growth trajectory
- Key leadership team members and their clinical/regulatory backgrounds

### 3. Compliance & Regulatory Section

Address each certification with specific detail:

**21 CFR Part 11 Compliance:**
- Electronic signatures with meaning and manifestation
- Audit trail implementation (who, what, when, why for every change)
- System access controls and user authentication
- Record retention and retrieval capabilities
- Validation documentation (IQ/OQ/PQ protocols and reports)

**HIPAA Compliance:**
- BAA execution process
- PHI handling, encryption at rest (AES-256) and in transit (TLS 1.2+)
- Access controls and minimum necessary standard
- Breach notification procedures
- Workforce training program

**SOC 2 Type II:**
- Trust service criteria covered (Security, Availability, Confidentiality, Processing Integrity, Privacy)
- Audit period and most recent report date
- How to request report under NDA

**Additional Compliance:**
- GDPR readiness for EU studies
- ICH E6(R2) GCP alignment
- GAMP 5 software category and validation approach
- ISO 27001 alignment

### 4. Technical Capabilities

Map Talosix features to RFP requirements:

- **Study Build** - eCRF design, library of standard forms, edit check programming, visit scheduling
- **Data Collection** - Electronic data capture, eSource integration, direct data entry at sites
- **Data Management** - Query management, medical coding (MedDRA/WHODrug), data review workflows
- **Reporting & Analytics** - Real-time dashboards, enrollment tracking, data quality metrics
- **Integration** - CDISC standards (CDASH, SDTM), API connectivity, lab data import, IRT/RTSM integration
- **Randomization** - Built-in or integrated randomization capabilities
- **ePRO/eCOA** - Patient-facing data collection if applicable

### 5. Security Questionnaire Responses

Common security domains to address:

| Domain | Key Points |
|--------|-----------|
| Data Encryption | AES-256 at rest, TLS 1.2+ in transit |
| Access Control | RBAC, MFA, SSO (SAML 2.0) |
| Network Security | WAF, IDS/IPS, DDoS protection |
| Vulnerability Management | Quarterly penetration testing, continuous scanning |
| Incident Response | Documented IR plan, 24-hour notification |
| Business Continuity | RPO/RTO targets, geographic redundancy |
| Data Residency | Configurable by region (US, EU, APAC) |
| Subprocessors | List of subprocessors, due diligence process |
| Employee Security | Background checks, annual training, NDA |
| Change Management | SDLC with validation, change control board |

### 6. Implementation & Support

- **Implementation Timeline** - Typical timelines by study complexity (simple: 4-6 weeks, moderate: 6-10 weeks, complex: 10-16 weeks)
- **Methodology** - Phased approach (Discovery, Design, Build, UAT, Go-Live, Hypercare)
- **Team Structure** - Project manager, clinical data architect, validation lead, training specialist
- **Training Model** - Role-based training for sites, monitors, data managers, medical reviewers
- **Support Model** - Tiered support (L1/L2/L3), SLA response times, 24/7 availability for critical issues
- **Ongoing Services** - Amendment management, mid-study changes, database lock support

### 7. Pricing Section

Structure pricing clearly:

- Platform license (per-study or enterprise)
- Study build and configuration
- Validation and testing
- Training
- Ongoing support and maintenance
- Optional modules (ePRO, coding, analytics, integrations)

Note: Flag that specific pricing requires scoping based on study protocol.

## Therapeutic Area Customization

Tailor responses based on the prospect's therapeutic focus:

- **Oncology** - RECIST criteria support, tumor measurement forms, survival endpoints, complex visit windows, unblinding workflows
- **Rare Disease** - Small patient populations, adaptive designs, natural history study support, patient registries
- **CNS/Neurology** - Cognitive assessment scales (ADAS-Cog, MMSE), rater training integration, complex scoring algorithms
- **Cardiovascular** - MACE endpoint adjudication workflows, ECG data integration, long-term follow-up
- **Immunology** - Patient diaries, flare tracking, composite endpoints, PRO integration
- **Vaccines** - Large-scale enrollment, lot tracking, reactogenicity data, safety surveillance
- **Cell & Gene Therapy** - Long-term follow-up (15+ years), manufacturing data linkage, complex eligibility

## Quality Checks

Before finalizing any RFP response:

1. **Completeness** - Every question in the RFP has a direct answer
2. **Accuracy** - All claims about certifications and capabilities are current and verifiable
3. **Consistency** - No contradictions between sections
4. **Compliance Language** - Use precise regulatory terminology (e.g., "validated system" not "tested system")
5. **Prospect Alignment** - Response references their specific therapeutic area, study phase, and stated priorities
6. **Formatting** - Follows any page limits, font requirements, or format specifications in the RFP
7. **References** - Include relevant case studies or references matching the prospect's profile

## Output Format

Deliver the response as:

1. **Main Response Document** - Structured per RFP requirements with clear section numbering matching the RFP
2. **Compliance Matrix** - Spreadsheet mapping each RFP requirement to the response section and compliance status (Compliant/Partial/Roadmap)
3. **Attachments List** - Certifications, sample reports, architecture diagrams, or other supporting documents to include

## Notes

- Never fabricate compliance certifications or capability claims
- Mark any response requiring verification from internal teams with `[VERIFY]`
- If a capability is on the product roadmap but not yet available, clearly state "Planned for [timeframe]" rather than claiming current availability
- Always have Legal review responses related to liability, indemnification, or data breach terms before submission
