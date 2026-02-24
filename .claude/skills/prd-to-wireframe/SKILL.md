---
name: PRD to Wireframe Prototype
description: >
  Converts Product Requirements Documents from Confluence into interactive
  HTML/React wireframe prototypes with clinical trial EDC patterns. Pulls PRDs
  via Atlassian MCP, extracts user stories and acceptance criteria, then generates
  a fully navigable prototype with Tailwind CSS styling ready for local preview
  or Figma Code-to-Canvas export.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - Edit
argument-hint: "[confluence-page-url-or-prd-title]"
---

# PRD to Wireframe Prototype

## Purpose

Transform a Product Requirements Document into a working, clickable wireframe prototype that stakeholders can interact with before any production development begins. This bridges the gap between written requirements and visual design, catching misalignment early.

## Workflow

### Step 1: Acquire the PRD

**Option A - Confluence via Atlassian MCP:**

1. Use `mcp__claude_ai_Atlassian__searchConfluenceUsingCql` to locate the PRD page by title or space key.
2. Use `mcp__claude_ai_Atlassian__getConfluencePage` to retrieve the full page content.
3. If the PRD spans child pages, use `mcp__claude_ai_Atlassian__getConfluencePageDescendants` to pull all sub-pages.

**Option B - Pasted or local PRD:**

1. Accept PRD text pasted directly by the user.
2. Or read a local file path provided by the user.

### Step 2: Extract Structured Requirements

Parse the PRD and extract:

- **User Stories**: Actor, action, outcome triples.
- **Acceptance Criteria**: Testable conditions per story.
- **User Flows**: Sequential steps a user takes through the system.
- **Screen Inventory**: Every distinct screen or view mentioned or implied.
- **Data Entities**: Objects displayed or manipulated (subjects, visits, CRFs, queries).
- **Roles/Permissions**: Which user roles see which screens and actions.
- **Business Rules**: Validation rules, edit checks, conditional logic.

Create a structured summary and present it to the user for confirmation before proceeding.

### Step 3: Define the Screen Map

From the extracted requirements, produce a screen map:

```
screens:
  - id: dashboard
    title: Study Dashboard
    description: Overview of study progress
    navigates_to: [subject-list, visit-schedule, query-list]
  - id: subject-list
    title: Subject Listing
    description: Paginated table of enrolled subjects
    navigates_to: [subject-detail, enroll-subject]
  ...
```

Present the screen map to the user for review. Adjust as needed.

### Step 4: Generate the Prototype

Generate an HTML + Tailwind CSS prototype (or React/TypeScript if the user prefers).

**File structure:**

```
prototype/
  index.html          # Entry point with navigation
  screens/
    dashboard.html
    subject-list.html
    ...
  styles/
    tokens.css        # Talosix design tokens
  scripts/
    navigation.js     # Client-side routing between screens
  data/
    sample-data.json  # Realistic clinical trial sample data
```

**For React variant:**

```
prototype/
  package.json
  src/
    App.tsx
    screens/
      Dashboard.tsx
      SubjectList.tsx
      ...
    components/
      Layout.tsx
      Sidebar.tsx
      ...
    data/
      sampleData.ts
    styles/
      tokens.css
```

### Step 5: Wire Up Navigation

- Every link and button that navigates between screens must be functional.
- Use hash-based routing for plain HTML or React Router for React.
- Breadcrumbs should reflect the current location.
- Back buttons should work correctly.
- Tab navigation between related views should function.

### Step 6: Apply Clinical Trial EDC Patterns

Use these established patterns for clinical trial EDC interfaces:

**eCRF Form Layouts:**
- Vertical single-column layout for data entry forms.
- Field label above input, help text below.
- Required field indicators (red asterisk).
- Source Data Verification (SDV) indicators per field.
- Query flag icons next to fields with open queries.
- Edit check warnings inline below fields.
- Form-level status bar: Draft | Complete | Signed | Locked.

**Visit Schedule Grids:**
- Matrix layout: subjects as rows, visits as columns.
- Cell states: Scheduled (gray), In Progress (blue), Complete (green), Overdue (red), Missed (strikethrough).
- Click cell to navigate to CRF list for that subject-visit.
- Collapsible visit groups (Screening, Treatment, Follow-up).

**Subject Listing Tables:**
- Sortable columns: Subject ID, Site, Status, Last Visit, Next Visit.
- Filter bar with dropdowns for site, status, cohort.
- Inline status badges with color coding.
- Row click navigates to subject detail.
- Bulk action toolbar for selected subjects.

**Query Management Panels:**
- Query list with status filters: Open, Answered, Closed.
- Query detail: field reference, query text, response thread.
- Query creation dialog with field context.
- Age indicators (days open) with escalation coloring.

**Audit Trail Views:**
- Chronological list of changes.
- Each entry: timestamp, user, field, old value, new value, reason.
- Filter by date range, user, field, action type.
- Read-only, non-editable presentation.

**E-Signature Dialogs:**
- Modal overlay with meaning statement.
- Username and password re-entry fields.
- "I confirm that..." attestation text.
- Regulatory reference (21 CFR Part 11).
- Cancel and Sign buttons with appropriate emphasis.

### Step 7: Include Realistic Sample Data

Generate contextually appropriate sample data:

- Study names: "TAL-2024-001: Phase III Oncology Study"
- Site IDs: "SITE-1001 - Memorial Research Center"
- Subject numbers: "TAL-001-0001" through "TAL-001-0025"
- Visit names: "Screening", "Baseline", "Week 2", "Week 4", etc.
- CRF names: "Demographics", "Medical History", "Vital Signs", "Adverse Events"
- Query text: "Please verify the systolic blood pressure value of 245 mmHg"
- Timestamps in ISO 8601 format with timezone.

### Step 8: Output and Handoff

1. Write all prototype files to a `prototype/` directory (or user-specified path).
2. If HTML: provide instructions to open `index.html` in a browser.
3. If React: include a `package.json` and instructions to run `npm install && npm start`.
4. Generate a `DESIGNER-NOTES.md` file with:
   - Screens that need visual design refinement.
   - Placeholder areas where illustrations or icons are needed.
   - Interaction details that need animation/transition design.
   - Areas where the PRD was ambiguous and assumptions were made.
   - Responsive behavior recommendations.
   - Suggestions for Figma component creation via Code-to-Canvas.

## Design Tokens (Talosix Defaults)

Use these defaults unless the user specifies otherwise:

```css
:root {
  --color-primary: #2563EB;
  --color-primary-hover: #1D4ED8;
  --color-success: #16A34A;
  --color-warning: #D97706;
  --color-error: #DC2626;
  --color-neutral-50: #F9FAFB;
  --color-neutral-100: #F3F4F6;
  --color-neutral-200: #E5E7EB;
  --color-neutral-700: #374151;
  --color-neutral-900: #111827;
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
  --font-family: 'Inter', sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --border-radius: 6px;
}
```

## Quality Checklist

Before delivering the prototype, verify:

- [ ] Every screen from the PRD is represented.
- [ ] All navigation paths work (no dead-end screens).
- [ ] Form fields have appropriate input types and placeholder text.
- [ ] Tables have sortable column headers.
- [ ] Status indicators use consistent color coding.
- [ ] Modal dialogs can be opened and closed.
- [ ] Breadcrumbs reflect current navigation path.
- [ ] Sample data is realistic and contextually appropriate.
- [ ] No lorem ipsum text remains (use domain-specific placeholder text).
- [ ] DESIGNER-NOTES.md is comprehensive.
