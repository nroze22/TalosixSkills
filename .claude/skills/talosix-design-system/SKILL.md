---
name: talosix-design-system
description: Talosix design language, component patterns, UX guidelines, design tokens, and Stitch wireframing specifications for the clinical trial EDC platform. Load when generating UI designs, wireframes, user flows, frontend code, or reviewing designs for Talosix. Covers eCRF patterns, accessibility (WCAG 2.1 AA), 21 CFR Part 11 compliance, and Figma/Stitch consistency rules.
user-invocable: false
---

# Talosix Design System

This skill defines the complete design language for the Talosix clinical trial EDC platform. It is loaded automatically whenever generating UI designs, wireframes, user flows, or frontend code.

## Design Principles

### Clinical Trial UX Philosophy
- **Accuracy Over Speed**: Users enter regulated data; design for precision, not rapid clicking
- **Audit Trail Transparency**: Every action should feel traceable and documented
- **Error Prevention**: Prevent mistakes through smart defaults, constraints, and validation
- **Context Persistence**: Users work across long sessions; never lose their place or data
- **Role-Aware Interfaces**: CRAs, Site Coordinators, Data Managers, and Medical Monitors have different needs

### Regulatory Compliance (21 CFR Part 11)
- All data entry must support electronic signatures
- Audit trails must be visible and accessible
- Session timeouts must warn users before logout
- Form changes must show clear before/after states
- Reason for change must be capturable for any edit

## Design Tokens

### Color Palette

```css
/* Primary Brand */
--talosix-primary-900: #1a365d;      /* Deep navy - headers, primary actions */
--talosix-primary-700: #2c5282;      /* Navy - active states */
--talosix-primary-500: #3182ce;      /* Blue - links, interactive elements */
--talosix-primary-100: #ebf8ff;      /* Light blue - selected backgrounds */

/* Secondary */
--talosix-secondary-700: #553c9a;    /* Purple - secondary actions */
--talosix-secondary-500: #805ad5;    /* Light purple - accents */

/* Semantic Colors */
--talosix-success-700: #276749;      /* Dark green - confirmed states */
--talosix-success-500: #38a169;      /* Green - success, complete, verified */
--talosix-success-100: #f0fff4;      /* Light green - success backgrounds */

--talosix-warning-700: #c05621;      /* Dark orange - attention needed */
--talosix-warning-500: #dd6b20;      /* Orange - warnings, pending review */
--talosix-warning-100: #fffaf0;      /* Light orange - warning backgrounds */

--talosix-error-700: #c53030;        /* Dark red - critical errors */
--talosix-error-500: #e53e3e;        /* Red - errors, required, overdue */
--talosix-error-100: #fff5f5;        /* Light red - error backgrounds */

--talosix-query-open: #e53e3e;       /* Red - open queries */
--talosix-query-answered: #dd6b20;   /* Orange - answered, awaiting review */
--talosix-query-closed: #38a169;     /* Green - closed queries */

/* Neutral Palette */
--talosix-gray-900: #1a202c;         /* Near black - primary text */
--talosix-gray-700: #4a5568;         /* Dark gray - secondary text */
--talosix-gray-500: #718096;         /* Gray - placeholder text, disabled */
--talosix-gray-300: #cbd5e0;         /* Light gray - borders */
--talosix-gray-100: #f7fafc;         /* Off white - backgrounds */
--talosix-white: #ffffff;            /* White - cards, inputs */

/* Data Status Colors */
--talosix-status-blank: #e2e8f0;     /* Light gray - no data entered */
--talosix-status-incomplete: #fbd38d; /* Yellow - partial data */
--talosix-status-complete: #9ae6b4;  /* Light green - complete */
--talosix-status-verified: #38a169;  /* Green - SDV complete */
--talosix-status-locked: #a0aec0;    /* Gray - locked/frozen */
```

### Typography

```css
--talosix-font-primary: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
--talosix-font-mono: 'JetBrains Mono', 'Fira Code', monospace;

--talosix-text-xs: 0.75rem;    /* 12px - labels, badges */
--talosix-text-sm: 0.875rem;   /* 14px - secondary text, table cells */
--talosix-text-base: 1rem;     /* 16px - body text, form inputs */
--talosix-text-lg: 1.125rem;   /* 18px - section headers */
--talosix-text-xl: 1.25rem;    /* 20px - page headers */
--talosix-text-2xl: 1.5rem;    /* 24px - major headers */

--talosix-font-normal: 400;
--talosix-font-medium: 500;
--talosix-font-semibold: 600;
--talosix-font-bold: 700;

--talosix-leading-tight: 1.25;
--talosix-leading-normal: 1.5;
--talosix-leading-relaxed: 1.75;
```

### Spacing System

```css
/* Base unit: 4px */
--talosix-space-1: 0.25rem;   /* 4px */
--talosix-space-2: 0.5rem;    /* 8px */
--talosix-space-3: 0.75rem;   /* 12px */
--talosix-space-4: 1rem;      /* 16px */
--talosix-space-5: 1.25rem;   /* 20px */
--talosix-space-6: 1.5rem;    /* 24px */
--talosix-space-8: 2rem;      /* 32px */
--talosix-space-10: 2.5rem;   /* 40px */
--talosix-space-12: 3rem;     /* 48px */
--talosix-space-16: 4rem;     /* 64px */

/* Component-Specific */
--talosix-form-field-gap: var(--talosix-space-4);
--talosix-section-gap: var(--talosix-space-8);
--talosix-card-padding: var(--talosix-space-6);
--talosix-table-cell-padding: var(--talosix-space-3);
```

### Border & Shadow

```css
--talosix-radius-sm: 0.25rem;  /* 4px - buttons, inputs */
--talosix-radius-md: 0.375rem; /* 6px - cards */
--talosix-radius-lg: 0.5rem;   /* 8px - modals */
--talosix-radius-full: 9999px; /* Pills, avatars */

--talosix-border-width: 1px;
--talosix-border-color: var(--talosix-gray-300);
--talosix-border-focus: var(--talosix-primary-500);

--talosix-shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
--talosix-shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
--talosix-shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
--talosix-shadow-focus: 0 0 0 3px rgba(49, 130, 206, 0.4);
```

## Component Patterns

### Form Fields (eCRF Data Entry)

Every field must support: label, input control, help text, validation messages, query indicator, audit trail access, edit reason capture.

```
+-------------------------------------------------------------+
| [Label]* ----------------------------------------- [?] [Query] |
| +----------------------------------------------------------+ |
| | [Input Field]                                            | |
| +----------------------------------------------------------+ |
| Help text appears here in gray                               |
| Warning/error appears here in red                            |
+-------------------------------------------------------------+
```

**Field States:**
- **Blank**: Light gray background, no data
- **In Progress**: White background, partial entry
- **Complete**: Light green left border
- **Has Query**: Red dot indicator, field highlighted
- **Locked**: Gray background, edit disabled
- **Verified**: Green checkmark, SDV complete

### Data Tables

Tables display subject lists, visit schedules, and query logs.

Requirements: sortable columns, filterable, selectable rows (checkbox column), inline status indicators, action column (rightmost), sticky header on scroll, pagination or infinite scroll.

```
+--------------------------------------------------------------------------+
| [x] Subject ID | Site     | Visit   | Status     | Queries | Actions    |
+--------------------------------------------------------------------------+
| [x] 001-0001   | Site 101 | Visit 3 | * Complete | 2 open  | ... v      |
| [x] 001-0002   | Site 101 | Visit 2 | o Partial  | -       | ... v      |
| [x] 001-0003   | Site 102 | Screen  | ! Overdue  | 1 open  | ... v      |
+--------------------------------------------------------------------------+
| Showing 1-25 of 342                              | < 1 2 3 ... 14 >     |
```

### Navigation Patterns

**Global Navigation (Left Sidebar):**
```
+------------------+
| [Talosix Logo]   |
+------------------+
| Dashboard        |
| Subjects         |
| Forms            |
| Queries          |
| Reports          |
| Study Setup      |
+------------------+
| Study: ABC-123 v |
| Site: 101 v      |
+------------------+
| [User Avatar]    |
| Settings | Logout|
+------------------+
```

**Breadcrumb**: `Study ABC-123 > Site 101 > Subject 001-0001 > Visit 2 > Demographics`

**Tabs**: `[Overview] [Forms] [Queries] [Documents] [Audit Trail]`

### Modals & Dialogs

**Confirmation Dialog (Destructive Actions):**
```
+--------------------------------------------+
| Warning: Confirm Action              [X]   |
+--------------------------------------------+
| Are you sure you want to [action]?         |
| This will affect [X] records.              |
|                                            |
| Reason for change: *                       |
| +----------------------------------------+ |
| |                                        | |
| +----------------------------------------+ |
|            [Cancel]  [Confirm Action]      |
+--------------------------------------------+
```

**Electronic Signature Modal:**
```
+--------------------------------------------+
| Electronic Signature Required        [X]   |
+--------------------------------------------+
| You are signing: [Action Description]      |
| Meaning: I confirm that I have reviewed    |
| this data and it is accurate.              |
|                                            |
| Username: [_________________________]      |
| Password: [_________________________]      |
|              [Cancel]  [Sign & Submit]     |
+--------------------------------------------+
```

### Query Management

```
+-------------------------------------------------------------+
| OPEN QUERY                                    Query #1234    |
+-------------------------------------------------------------+
| Form: Demographics  |  Field: Date of Birth                 |
| Subject: 001-0001   |  Visit: Screening                     |
+-------------------------------------------------------------+
| Data Manager (Jan 15, 2025 10:32 AM)                        |
| Please verify the date of birth. The value entered           |
| (01/01/1800) appears to be invalid.                          |
+-------------------------------------------------------------+
| Site Coordinator (Jan 15, 2025 2:15 PM)                     |
| Corrected. The correct DOB is 01/01/1980. Source             |
| document attached.                                           |
+-------------------------------------------------------------+
| Response: [____________________________________]             |
| [Attach File]       [Cancel] [Answer Query] [Close Query]   |
+-------------------------------------------------------------+
```

### Status Badges

- `*` Complete (green solid)
- `o` Incomplete (gray hollow)
- `~` In Progress (half-filled)
- `!` Warning (orange triangle)
- `x` Error (red X)
- `v` Verified (green checkmark)
- `#` Locked (lock icon, gray)

## Page Templates

### Dashboard
```
+--------------------------------------------------------------------------+
| Dashboard                                        Study: ABC-123 v        |
+--------------------------------------------------------------------------+
| +----------------+ +----------------+ +----------------+ +--------------+ |
| | Total Subjects | | Open Queries   | | Overdue Visits | | SDV Rate     | |
| |     247        | |     18         | |      5         | |   78%        | |
| | +12 this week  | | -3 from last   | | Action needed  | | Target: 85%  | |
| +----------------+ +----------------+ +----------------+ +--------------+ |
|                                                                          |
| +-------------------------------------+ +------------------------------+ |
| | Enrollment Trend                    | | My Tasks                     | |
| | [Line Chart: Target vs Actual]      | | [ ] Review 3 answered queries| |
| |                                     | | [ ] Complete SDV for Site 102| |
| |                                     | | [ ] Sign off Visit 4 forms   | |
| +-------------------------------------+ +------------------------------+ |
|                                                                          |
| +----------------------------------------------------------------------+ |
| | Recent Activity                                                      | |
| | Subject 001-0045 enrolled at Site 103 - 2 hours ago                  | |
| | Query #1234 closed by J. Smith - 3 hours ago                         | |
| | Visit 3 completed for Subject 001-0032 - 5 hours ago                 | |
| +----------------------------------------------------------------------+ |
+--------------------------------------------------------------------------+
```

### eCRF Form Entry
```
+--------------------------------------------------------------------------+
| < Back to Subject    Subject 001-0001 | Visit 2 | Demographics           |
+--------------------------------------------------------------------------+
| [Form Info] [Data Entry] [Queries (2)] [Audit Trail]                     |
+--------------------------------------------------------------------------+
|  Demographics Form                                 Status: ~ Partial     |
|  ----------------------------------------------------------------        |
|  IDENTIFICATION                                                          |
|  +------------------------------------------------------------------+   |
|  | Subject Initials *                                          [?]  |   |
|  | [JDS_______________________________________________]              |   |
|  | Enter first, middle, last initials                               |   |
|  +------------------------------------------------------------------+   |
|                                                                          |
|  +------------------------------------------------------------------+   |
|  | Date of Birth *                                    [?] [QUERY]   |   |
|  | [01/01/1980_______________________________________] [calendar]    |   |
|  | ! Query open: Please verify date of birth                        |   |
|  +------------------------------------------------------------------+   |
|                                                                          |
|  +------------------------------------------------------------------+   |
|  | Sex *                                                       [?]  |   |
|  | ( ) Male   ( ) Female   ( ) Other   ( ) Unknown                  |   |
|  +------------------------------------------------------------------+   |
|                                                                          |
|                              [Save Draft]  [Mark Complete & Sign]        |
+--------------------------------------------------------------------------+
```

## Responsive Breakpoints

```css
--talosix-breakpoint-sm: 640px;   /* Small tablets */
--talosix-breakpoint-md: 768px;   /* Tablets - minimum supported */
--talosix-breakpoint-lg: 1024px;  /* Laptops */
--talosix-breakpoint-xl: 1280px;  /* Desktops - optimal */
--talosix-breakpoint-2xl: 1536px; /* Large screens */
```

Clinical users primarily use tablets and desktops. Minimum supported: 768px (iPad portrait). Optimal: 1280px+.

## Accessibility (WCAG 2.1 AA)

- Color contrast: minimum 4.5:1 for text, 3:1 for large text
- Focus indicators visible on all interactive elements (minimum 2px outline)
- All form fields have programmatically associated labels
- Error messages announced to screen readers
- Keyboard navigation for all functionality; no keyboard traps
- Touch targets minimum 44x44px for tablet use
- High contrast mode available
- Form fields readable at 200% zoom
- Session timeout warnings with extension option
- No seizure-inducing animations

```css
--talosix-transition-fast: 150ms ease-out;
--talosix-transition-normal: 250ms ease-out;
--talosix-transition-slow: 350ms ease-out;

@media (prefers-reduced-motion: reduce) {
  * { transition: none !important; animation: none !important; }
}
```

## Icon System

Use Lucide icons (https://lucide.dev):
- Add: Plus | Edit: Pencil | Delete: Trash-2 | Save: Save
- Search: Search | Filter: Filter | Settings: Settings | User: User
- Calendar: Calendar | File: FileText | Query: MessageCircle
- Warning: AlertTriangle | Error: AlertCircle | Success: CheckCircle
- Lock: Lock | Signature: PenTool | Audit: History

## Stitch Wireframe Specifications

### Mandatory Global Layout for Stitch

Every Stitch prompt MUST include this identical structure:

**Left Sidebar (200px, background #2c5282):**
- Talosix logo (white)
- Nav items (white text, hover #3182ce): Dashboard, Subjects, Forms, Queries, Reports, Study Setup
- Active section: background #1a365d
- Bottom: Study selector, Site selector, User avatar + Settings/Logout

**Top Header (64px, white, border-bottom 1px #cbd5e0):**
- Left: Breadcrumb (gray #4a5568)
- Right: User profile avatar + name

**Main Content (background #f7fafc, padding 32px):**
- Page title: 24px bold, #1a202c
- Content starts 16px below title

### Tab Consistency Rules

- Active tab: Navy underline #2c5282, bold text
- Inactive: Gray #718096, normal weight
- 16px spacing, 48px height, 16px font
- CRITICAL: First screen establishes tab order; all subsequent screens use identical order
- Never reorder, add, or remove tabs between screens of the same feature

### Exact Color Codes for All Stitch Prompts

```
Navy Blue (sidebar, buttons): #2c5282
Darker Navy (headers): #1a365d
Light Blue (links): #3182ce
Pale Blue (backgrounds): #ebf8ff
Success Green: #38a169
Warning Orange: #dd6b20
Error Red: #e53e3e
Near Black (text): #1a202c
Dark Gray (secondary): #4a5568
Mid Gray (borders): #cbd5e0
Light Gray (backgrounds): #f7fafc
White: #ffffff
```

### Stitch Prompt Template

```
Desktop web application screen for Talosix clinical trial EDC platform.
SCREEN: [Name]
Design: Navy blue primary (#2c5282), white backgrounds, clinical EDC aesthetic, 2560px width.

LEFT SIDEBAR (200px, #2c5282): [Copy nav spec exactly]
TOP HEADER (64px, white): [Copy header spec exactly]
MAIN CONTENT (#f7fafc, 32px padding): [Screen-specific content]
[IF TABS]: List all tabs in canonical order, mark active tab.

Professional clinical software aesthetic. Navy blue (#2c5282) for all primary actions.
```

### Consistency Checklist

Before every Stitch prompt verify: sidebar is #2c5282, nav order is canonical (Dashboard > Subjects > Forms > Queries > Reports > Study Setup), header has breadcrumb left / profile right, tabs follow canonical order, all hex codes match spec, page title is 24px bold #1a202c, background is #f7fafc, status colors are standard.

## Implementation Notes

### When Generating Wireframes
- Use Stitch for wireframe generation; maximize wireframes per call
- Chain additional Stitch calls for complex features
- Include global navigation, form field states, breadcrumbs, status indicators, action buttons (bottom-right for forms)

### When Generating User Flows
- Start with user role and goal
- Include decision points for validation
- Show error recovery paths
- Include regulatory touchpoints (signatures, audit)
- End with clear success state

### When Generating Code
- Use the design tokens defined above
- Follow component patterns exactly
- Include all field states
- Ensure form fields support reason-for-change
- Build in audit trail hooks
- Use `data-testid` attributes for QA

## File Structure

```
/wireframes/[feature-name]/
  user-flow.mermaid
  wireframe-desktop.html
  wireframe-tablet.html
  states.md
  consistency-checklist.md

/components/[component-name]/
  component.tsx
  component.stories.tsx
  component.test.tsx
```
