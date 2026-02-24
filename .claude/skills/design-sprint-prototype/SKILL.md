---
name: Design Sprint Prototype
description: >
  Rapid prototyping workflow for design sprints inspired by the thoughtbot methodology.
  Gathers context from Confluence PRDs and user research, defines prototype scope,
  generates functional HTML/Tailwind wireframes with visual design, populates realistic
  clinical trial sample data, and wires up clickable navigation. Outputs a complete
  prototype ready for usability testing with clinical site users, CRAs, and data managers.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - Edit
argument-hint: "[feature-name-or-prd-link]"
---

# Design Sprint Prototype

## Purpose

Build a high-fidelity, clickable prototype in a single focused session for use in design sprint testing. The prototype must be realistic enough that clinical site staff, CRAs, and data managers can interact with it naturally and provide meaningful usability feedback. Speed is prioritized over perfection -- the goal is to test ideas, not ship production code.

## Design Sprint Context

This skill follows a compressed version of the design sprint methodology:

1. **Understand** - Gather all context about the problem and users.
2. **Scope** - Define exactly what to prototype and what to fake.
3. **Build** - Generate the functional prototype rapidly.
4. **Polish** - Apply visual design and realistic data.
5. **Connect** - Wire everything together into a seamless experience.
6. **Prepare** - Create testing materials for usability sessions.

## Workflow

### Step 1: Gather Context

**From Confluence (PRD, research, specs):**

1. Use `mcp__claude_ai_Atlassian__searchConfluenceUsingCql` to find relevant pages:
   ```
   text ~ "feature name" AND space = "PRODUCT" ORDER BY lastmodified DESC
   ```
2. Use `mcp__claude_ai_Atlassian__getConfluencePage` to retrieve:
   - Product Requirements Document.
   - User research findings or persona documents.
   - Competitive analysis or benchmarking pages.
   - Design system documentation.
3. Use `mcp__claude_ai_Atlassian__getConfluencePageDescendants` if content spans sub-pages.

**From Jira (stories, epics):**

1. Use `mcp__claude_ai_Atlassian__searchJiraIssuesUsingJql` to find related stories:
   ```
   project = TAL AND text ~ "feature name" ORDER BY rank ASC
   ```
2. Pull acceptance criteria and user stories from linked issues.

**From the user directly:**

1. Accept verbal or written description of the feature.
2. Accept sketches, screenshots, or reference URLs.
3. Ask clarifying questions about target users and key scenarios.

**Synthesize context into a brief:**

```markdown
## Prototype Brief

**Feature**: [Name]
**Target Users**: [Roles - e.g., Site Coordinator, CRA, Data Manager]
**Problem Statement**: [What problem are we solving]
**Key Scenarios**: [3-5 specific tasks users will attempt]
**Success Criteria**: [How we'll know the prototype works]
**Constraints**: [Technical, regulatory, timeline constraints]
```

Present the brief to the user for confirmation before proceeding.

### Step 2: Define Scope

Determine what to build and what to simulate:

**Prototype scope matrix:**

| Screen/Feature | Build Level | Notes |
|---------------|-------------|-------|
| Main workflow screens | Full fidelity | All interactions work |
| Secondary screens | Medium fidelity | Key elements work, some static |
| Settings/admin screens | Low fidelity / skip | Static or placeholder |
| Error states | Selected critical ones | Most impactful error paths |
| Edge cases | Skip | Out of scope for sprint test |

**What to build:**
- Every screen in the primary user flow being tested.
- All navigation between those screens.
- Form interactions (input, select, submit) on key screens.
- State changes (loading, success, error) for primary actions.
- Realistic data in all visible tables and lists.

**What to fake:**
- Backend persistence (use local state or static data).
- Authentication (assume logged in).
- Long-running processes (simulate with timeouts).
- Secondary workflows not being tested.
- Deep links to other parts of the application.

Present the scope to the user for approval.

### Step 3: Generate Rapid Wireframes

Build the prototype as a single-page application using HTML + Tailwind CSS + vanilla JavaScript (for maximum portability) or React if the user prefers.

**File structure:**

```
sprint-prototype/
  index.html              # Entry point with routing
  css/
    tailwind.css           # Tailwind via CDN or compiled
    tokens.css             # Talosix design tokens
    animations.css         # Transition and animation styles
  js/
    app.js                 # Navigation and state management
    data.js                # Sample data
    interactions.js        # UI interaction handlers
  screens/                 # Each screen as a module/partial
    dashboard.js
    subject-list.js
    crf-entry.js
    query-list.js
    ...
```

**For React variant:**

```
sprint-prototype/
  package.json
  vite.config.ts
  index.html
  src/
    main.tsx
    App.tsx
    router.tsx
    screens/
      Dashboard.tsx
      SubjectList.tsx
      CrfEntry.tsx
      ...
    components/
      Shell.tsx             # App shell with sidebar and header
      DataTable.tsx
      FormField.tsx
      StatusBadge.tsx
      Modal.tsx
      ...
    data/
      sampleData.ts
    hooks/
      usePrototypeState.ts  # Simple state management for prototype
```

**Speed techniques:**

- Use Tailwind CDN for instant setup (no build step for HTML version).
- Use Vite for React version (fast startup).
- Inline SVG icons from Heroicons or Lucide.
- Use CSS transitions instead of a JS animation library.
- Copy patterns from existing components in the codebase when available.

### Step 4: Apply Visual Design

Apply the Talosix design system tokens for a polished look:

**Color palette:**

```css
:root {
  /* Primary */
  --primary-50: #EFF6FF;
  --primary-100: #DBEAFE;
  --primary-500: #2563EB;
  --primary-600: #1D4ED8;
  --primary-700: #1E40AF;

  /* Neutral */
  --neutral-50: #F9FAFB;
  --neutral-100: #F3F4F6;
  --neutral-200: #E5E7EB;
  --neutral-300: #D1D5DB;
  --neutral-500: #6B7280;
  --neutral-700: #374151;
  --neutral-900: #111827;

  /* Status */
  --success-500: #16A34A;
  --success-50: #F0FDF4;
  --warning-500: #D97706;
  --warning-50: #FFFBEB;
  --error-500: #DC2626;
  --error-50: #FEF2F2;
  --info-500: #2563EB;
  --info-50: #EFF6FF;
}
```

**Layout patterns:**

- **App shell**: Fixed sidebar (256px) + top header (64px) + scrollable content area.
- **Sidebar**: Logo at top, navigation groups with icons, user menu at bottom.
- **Header**: Breadcrumbs, page title, action buttons (right-aligned).
- **Content**: Max width 1280px, centered, padding 24px.
- **Cards**: White background, 1px border gray-200, rounded-lg, shadow-sm, padding 24px.
- **Tables**: Full width within cards, alternating row backgrounds, sticky header.

**Typography:**

```css
/* Headings */
h1 { font-size: 1.5rem; font-weight: 700; color: var(--neutral-900); }
h2 { font-size: 1.25rem; font-weight: 600; color: var(--neutral-900); }
h3 { font-size: 1.125rem; font-weight: 600; color: var(--neutral-700); }

/* Body */
body { font-size: 0.875rem; color: var(--neutral-700); line-height: 1.5; }

/* Small/Caption */
.text-caption { font-size: 0.75rem; color: var(--neutral-500); }
```

### Step 5: Add Realistic Sample Data

Generate contextually accurate clinical trial data. This is critical -- unrealistic data breaks the illusion during testing.

**Study-level data:**

```javascript
const study = {
  id: 'TAL-2025-003',
  name: 'A Randomized, Double-Blind, Placebo-Controlled Phase III Study of Talixemab in Patients with Moderate-to-Severe Rheumatoid Arthritis',
  shortName: 'TALIXA-RA',
  phase: 'Phase III',
  therapeuticArea: 'Rheumatology',
  sponsor: 'Talosix Therapeutics',
  status: 'Enrolling',
  sites: 42,
  targetEnrollment: 450,
  currentEnrollment: 312,
};
```

**Site data:**

```javascript
const sites = [
  { id: 'SITE-1001', name: 'Memorial Research Center', pi: 'Dr. Sarah Chen', city: 'Boston, MA', status: 'Active', enrolled: 28, target: 35 },
  { id: 'SITE-1002', name: 'University Clinical Trials Unit', pi: 'Dr. James Morrison', city: 'Chicago, IL', status: 'Active', enrolled: 22, target: 30 },
  { id: 'SITE-1003', name: 'Pacific Northwest Research Institute', pi: 'Dr. Maria Santos', city: 'Seattle, WA', status: 'Active', enrolled: 15, target: 25 },
  // ... generate 10-15 sites
];
```

**Subject data:**

```javascript
const subjects = [
  { id: 'TAL-1001-0001', initials: 'J.S.', site: 'SITE-1001', status: 'Active', cohort: 'Treatment', enrollDate: '2025-03-15', lastVisit: 'Week 12', nextVisit: 'Week 16' },
  { id: 'TAL-1001-0002', initials: 'M.K.', site: 'SITE-1001', status: 'Active', cohort: 'Placebo', enrollDate: '2025-03-22', lastVisit: 'Week 8', nextVisit: 'Week 12' },
  { id: 'TAL-1001-0003', initials: 'R.P.', site: 'SITE-1001', status: 'Withdrawn', cohort: 'Treatment', enrollDate: '2025-04-01', lastVisit: 'Week 4', nextVisit: null, withdrawReason: 'Consent withdrawn' },
  // ... generate 20-30 subjects
];
```

**Visit schedule:**

```javascript
const visits = [
  { name: 'Screening', window: 'Day -28 to Day -1', crfs: ['Informed Consent', 'Demographics', 'Medical History', 'Inclusion/Exclusion', 'Vital Signs', 'Labs'] },
  { name: 'Baseline (Day 1)', window: 'Day 1', crfs: ['Vital Signs', 'Physical Exam', 'DAS28 Assessment', 'Patient Questionnaires', 'Study Drug Dispensing'] },
  { name: 'Week 2', window: 'Day 14 +/- 3', crfs: ['Vital Signs', 'Adverse Events', 'Concomitant Medications'] },
  { name: 'Week 4', window: 'Day 28 +/- 3', crfs: ['Vital Signs', 'Labs', 'Adverse Events', 'Concomitant Medications', 'DAS28 Assessment'] },
  { name: 'Week 8', window: 'Day 56 +/- 5', crfs: ['Vital Signs', 'Labs', 'Adverse Events', 'Concomitant Medications', 'DAS28 Assessment', 'Patient Questionnaires'] },
  { name: 'Week 12', window: 'Day 84 +/- 5', crfs: ['Vital Signs', 'Labs', 'Physical Exam', 'Adverse Events', 'Concomitant Medications', 'DAS28 Assessment', 'Patient Questionnaires'] },
  // ... continue through end of study
];
```

**Query data:**

```javascript
const queries = [
  { id: 'Q-00142', subject: 'TAL-1001-0001', visit: 'Week 4', crf: 'Vital Signs', field: 'Systolic BP', status: 'Open', daysOpen: 5, text: 'Systolic BP value of 245 mmHg appears to be out of expected range. Please verify or correct.', createdBy: 'System (Edit Check)', createdDate: '2025-07-10' },
  { id: 'Q-00138', subject: 'TAL-1002-0003', visit: 'Baseline', crf: 'Medical History', field: 'Onset Date', status: 'Answered', daysOpen: 12, text: 'Onset date is after the screening date. Please clarify the medical history event timeline.', createdBy: 'Jennifer Walsh (CRA)', createdDate: '2025-07-03', response: 'Date has been corrected. Onset was 2024-06-15, not 2025-06-15. Typographical error.' },
  // ... generate 10-15 queries
];
```

**Adverse event data:**

```javascript
const adverseEvents = [
  { id: 'AE-001', subject: 'TAL-1001-0001', term: 'Headache', severity: 'Mild', serious: false, related: 'Possibly', onsetDate: '2025-04-02', status: 'Ongoing' },
  { id: 'AE-002', subject: 'TAL-1001-0001', term: 'Nausea', severity: 'Moderate', serious: false, related: 'Probably', onsetDate: '2025-04-15', resolvedDate: '2025-04-18', status: 'Resolved' },
  { id: 'AE-003', subject: 'TAL-1002-0005', term: 'Pneumonia', severity: 'Severe', serious: true, related: 'Unlikely', onsetDate: '2025-06-20', status: 'Ongoing', saeReported: true },
  // ... generate 10-15 adverse events
];
```

### Step 6: Create Clickable Navigation

Wire all screens together into a seamless experience:

**Navigation structure:**

```javascript
const routes = {
  '/': 'Dashboard',
  '/subjects': 'Subject Listing',
  '/subjects/:id': 'Subject Detail',
  '/subjects/:id/visits/:visitId': 'Visit CRF List',
  '/subjects/:id/visits/:visitId/crfs/:crfId': 'CRF Data Entry',
  '/queries': 'Query Management',
  '/queries/:id': 'Query Detail',
  '/reports': 'Reports Dashboard',
  '/admin/sites': 'Site Management',
  '/admin/users': 'User Management',
};
```

**Navigation behaviors:**

- Sidebar links navigate between top-level screens.
- Table row clicks navigate to detail screens.
- Breadcrumbs allow backward navigation at every level.
- Browser back/forward buttons work (use hash or History API routing).
- Modal dialogs overlay the current screen and close to return.
- Form submissions show success feedback and navigate to the next logical screen.
- "Cancel" buttons return to the previous screen without changes.

**State management for prototype:**

```javascript
// Simple in-memory state for prototype interactions
const prototypeState = {
  currentUser: { name: 'Jennifer Walsh', role: 'CRA', site: null },
  selectedSubject: null,
  formDrafts: {},        // In-progress form data
  queryResponses: {},    // Draft query responses
  notifications: [],     // Triggered notifications
};
```

**Interactive elements that must work:**

- Form inputs accept text and show placeholder data.
- Dropdowns open and allow selection.
- Tables sort when column headers are clicked.
- Filter controls filter the visible data.
- Status badges show correct colors.
- Action buttons trigger appropriate feedback (toast, modal, navigation).
- E-signature dialog accepts input and shows signed state.
- Query response textarea accepts text and shows submitted state.

### Step 7: Output the Prototype

1. Write all files to a `sprint-prototype/` directory (or user-specified path).
2. Include setup instructions:

**For HTML version:**
```
Open sprint-prototype/index.html in your browser.
No build step required.
```

**For React version:**
```
cd sprint-prototype
npm install
npm run dev
Open http://localhost:5173 in your browser.
```

3. Create a `PROTOTYPE-GUIDE.md` with:
   - List of all screens and their URLs/routes.
   - Which interactions are functional vs simulated.
   - Known limitations and workarounds.
   - How to reset the prototype state (refresh the page).

### Step 8: Create Usability Testing Materials

Generate materials for the testing sessions:

**Test Script (`TEST-SCRIPT.md`):**

```markdown
# Usability Test Script: [Feature Name]

## Setup
- Duration: 30-45 minutes per session
- Participants: [Target user roles]
- Environment: [Browser, screen share tool]
- Prototype URL: [Local or deployed URL]

## Introduction (2 min)
"Thank you for joining. We are testing a new feature for [description].
There are no right or wrong answers. We want to understand how you would
naturally use this. Please think aloud as you work through the tasks."

## Warm-up (3 min)
- "Tell me about your current role and how you interact with [system area]."
- "How often do you [relevant action]?"

## Task 1: [Primary task] (8-10 min)
**Scenario**: "You have just received a notification that [context].
You need to [specific goal]."

**Observation points**:
- Does the user find the starting point?
- What navigation path do they take?
- Do they understand the form labels?
- How do they react to validation messages?
- Do they complete the task without help?

**Follow-up questions**:
- "What did you expect to happen when you clicked [element]?"
- "Was anything confusing or unexpected?"
- "How does this compare to your current process?"

## Task 2: [Secondary task] (8-10 min)
...

## Task 3: [Error recovery task] (5-8 min)
...

## Debrief (5 min)
- "Overall, how would you rate this experience on a scale of 1-5?"
- "What worked well?"
- "What would you change?"
- "Is there anything missing that you would need?"

## Observer Notes Template
| Participant | Task | Completed? | Time | Errors | Comments |
|-------------|------|-----------|------|--------|----------|
| P1 | Task 1 | Yes/No | Xm Xs | N | ... |
```

**Observation Worksheet (`OBSERVATION-SHEET.md`):**
- Pre-filled grid for each task and participant.
- Checkboxes for common usability issues.
- Space for verbatim quotes.
- Rating scales for task difficulty.

## Speed Guidelines

This is a sprint prototype. Prioritize:

1. **Coverage over polish** -- Get all screens in place before perfecting any one screen.
2. **Real data over lorem ipsum** -- Clinical users will reject unrealistic data immediately.
3. **Working navigation over animations** -- Users must be able to complete tasks end-to-end.
4. **Key interactions over edge cases** -- Build the happy path first, then add critical error states.
5. **Consistent patterns over unique layouts** -- Reuse the same card, table, and form patterns across screens.

Target timeline: A complete prototype should be achievable in 2-4 hours.

## Quality Checklist

- [ ] All screens in the test script are built and navigable.
- [ ] Navigation between all connected screens works.
- [ ] Sample data is realistic and internally consistent.
- [ ] Form inputs accept user input on key screens.
- [ ] Status indicators use consistent color coding.
- [ ] The app shell (sidebar, header, breadcrumbs) is present on all screens.
- [ ] The prototype works in Chrome and Safari.
- [ ] The test script covers all scenarios to be validated.
- [ ] PROTOTYPE-GUIDE.md documents all limitations.
- [ ] No placeholder text (lorem ipsum, "TODO", "TBD") is visible in the prototype.
