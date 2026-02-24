---
name: design-qa
description: "Automated design QA that checks implementation code against Figma design specs. Uses the Figma MCP server to read design data, then compares spacing, colors, typography, and responsive behavior against the codebase. Includes WCAG 2.1 AA accessibility auditing and clinical trial EDC-specific UI checks for form clarity, error visibility, and regulatory compliance patterns."
allowed-tools: Read, Grep, Glob, Bash
argument-hint: "[figma-url] [implementation-path]"
---

# Design QA

## Purpose

Perform systematic quality assurance comparing implemented UI code against Figma design specifications. Catch visual regressions, accessibility violations, and clinical trial UX issues before they reach users. Produce a structured report that designers and developers can act on.

## Inputs

1. **Figma URL**: Link to the design file or specific frame/component.
2. **Implementation path**: Path to the implemented component, page, or feature in the codebase.

## Workflow

### Step 1: Read the Figma Design Spec

1. Parse the Figma URL to extract file key and node ID.
2. Use the Figma MCP server to retrieve:
   - Frame dimensions and layout properties.
   - Auto-layout: direction, spacing (gap), padding (top, right, bottom, left), alignment.
   - Fill colors (hex values with opacity).
   - Stroke: color, weight, dash pattern.
   - Text properties: font family, size, weight, line height, letter spacing, alignment.
   - Corner radius values.
   - Effects: drop shadows (x, y, blur, spread, color), blurs.
   - Component variants and their property values.
   - Constraints for responsive behavior.
3. Organize extracted values into a structured spec document for comparison.

### Step 2: Read the Implementation

1. Read all relevant source files at the implementation path:
   - React/TSX component files.
   - CSS/SCSS/Tailwind style files.
   - Tailwind configuration for custom theme values.
   - Global stylesheet for base styles.
2. Extract implemented values:
   - Tailwind classes and their resolved CSS values.
   - Inline styles.
   - CSS custom properties (variables) used.
   - Responsive breakpoint classes (sm:, md:, lg:, xl:).
   - Conditional styles (hover, focus, active, disabled states).

### Step 3: Visual Specification Comparison

Compare Figma spec against implementation for each property category:

**Spacing (padding, margin, gap):**

| Property | Figma Value | Implementation | Status |
|----------|-------------|----------------|--------|
| Padding top | 16px | `pt-4` (16px) | PASS |
| Padding right | 24px | `pr-4` (16px) | FAIL - Expected 24px (pr-6) |
| Gap between items | 12px | `gap-3` (12px) | PASS |

Tolerance: exact match required for spacing (0px tolerance).

**Colors:**

| Element | Figma Color | Implementation | Status |
|---------|------------|----------------|--------|
| Background | #F9FAFB | `bg-gray-50` (#F9FAFB) | PASS |
| Text | #111827 | `text-gray-900` (#111827) | PASS |
| Border | #E5E7EB | `border-gray-300` (#D1D5DB) | FAIL - Wrong gray |

Tolerance: exact hex match required.

**Typography:**

| Property | Figma Value | Implementation | Status |
|----------|-------------|----------------|--------|
| Font family | Inter | `font-sans` (Inter) | PASS |
| Font size | 14px | `text-sm` (14px) | PASS |
| Font weight | 600 | `font-semibold` (600) | PASS |
| Line height | 20px | `leading-5` (20px) | PASS |
| Letter spacing | -0.01em | not set | FAIL - Missing |

**Border Radius:**

| Element | Figma Value | Implementation | Status |
|---------|-------------|----------------|--------|
| Card | 8px | `rounded-lg` (8px) | PASS |
| Button | 6px | `rounded-md` (6px) | PASS |
| Input | 6px | `rounded` (4px) | FAIL - Expected 6px |

**Shadows:**

| Element | Figma Shadow | Implementation | Status |
|---------|-------------|----------------|--------|
| Card | 0 1px 3px rgba(0,0,0,0.1) | `shadow-sm` | PASS (approximate) |
| Modal | 0 25px 50px rgba(0,0,0,0.25) | `shadow-2xl` | PASS (approximate) |

Shadow tolerance: approximate match acceptable if the visual weight is equivalent.

**Dimensions:**

| Element | Figma Value | Implementation | Status |
|---------|-------------|----------------|--------|
| Button height | 40px | `h-10` (40px) | PASS |
| Icon size | 20px | `w-5 h-5` (20px) | PASS |
| Max width | 1280px | `max-w-7xl` (1280px) | PASS |

### Step 4: Responsive Behavior Check

Compare responsive behavior at each breakpoint:

**Breakpoints to check:**
- Mobile: 375px (Figma mobile frame)
- Tablet: 768px (md breakpoint)
- Desktop: 1024px (lg breakpoint)
- Wide: 1280px (xl breakpoint)

For each breakpoint, verify:
- Layout direction changes (flex-col to flex-row).
- Visibility changes (hidden/shown elements).
- Spacing adjustments.
- Font size changes.
- Column count changes in grids.
- Navigation pattern changes (hamburger vs full nav).

### Step 5: Accessibility Audit

Perform WCAG 2.1 AA compliance checks on the implementation code:

**Color Contrast (WCAG 1.4.3 / 1.4.6):**
- Normal text (< 18px or < 14px bold): minimum 4.5:1 contrast ratio.
- Large text (>= 18px or >= 14px bold): minimum 3:1 contrast ratio.
- UI components and graphical objects: minimum 3:1 against adjacent colors.
- Check all foreground/background color combinations in the code.
- Calculate contrast ratios using relative luminance formula:
  ```
  L = 0.2126 * R + 0.7152 * G + 0.0722 * B
  Ratio = (L1 + 0.05) / (L2 + 0.05) where L1 > L2
  ```

**Keyboard Navigation (WCAG 2.1.1 / 2.1.2):**
- All interactive elements must be reachable via Tab key.
- Custom components must have `tabIndex` and keyboard event handlers.
- Focus must be visible (check for `focus:` or `focus-visible:` classes).
- No keyboard traps (except intentional modal focus traps with Escape exit).
- Tab order follows logical reading order (check for `tabIndex` values > 0).

**Screen Reader Compatibility (WCAG 1.1.1 / 4.1.2):**
- Images have `alt` attributes (or `aria-hidden="true"` for decorative).
- Form inputs have associated `<label>` elements or `aria-label`.
- Buttons have accessible text (visible text, `aria-label`, or `aria-labelledby`).
- ARIA roles are correctly applied.
- Live regions (`aria-live`) for dynamic content updates.
- Page landmark regions (`<main>`, `<nav>`, `<aside>`, `<header>`, `<footer>`).

**Touch Targets (WCAG 2.5.5):**
- Interactive elements must be at least 44x44px.
- Check button and link dimensions.
- Check spacing between adjacent touch targets.
- Inline links in text blocks are exempt.

**Focus Management (WCAG 2.4.3 / 2.4.7):**
- Focus indicators must be visible (not `outline-none` without replacement).
- Focus must be managed in modals (trap and restore).
- Focus must move to new content when dynamically loaded.

**Error Identification (WCAG 3.3.1 / 3.3.2):**
- Form errors must be identified in text (not just color).
- Required fields must be indicated.
- Error messages must describe the error and suggest correction.
- Labels and instructions are provided for form inputs.

### Step 6: Clinical Trial EDC-Specific Checks

**Form Field Clarity:**
- [ ] Field labels are descriptive and unambiguous.
- [ ] Required field indicators are present and consistent (asterisk + legend).
- [ ] Help text or tooltips are available for clinical-specific fields.
- [ ] Units of measure are displayed next to numeric fields.
- [ ] Date format is explicitly shown (DD-MMM-YYYY per ICH E6).
- [ ] Field-level audit trail indicator is present where required.

**Error Message Visibility:**
- [ ] Validation errors appear immediately adjacent to the field.
- [ ] Error text uses color AND icon (not color alone for accessibility).
- [ ] Error summary appears at the top of the form for screen readers.
- [ ] Soft-check warnings are visually distinct from hard-check errors.
- [ ] Error count badge is displayed on the form tab/section.

**Action Confirmation Dialogs:**
- [ ] Destructive actions (delete, unblind, lock) require explicit confirmation.
- [ ] Confirmation dialog clearly states the action and its consequences.
- [ ] "Cancel" is the default/primary action; "Confirm" is secondary.
- [ ] Keyboard shortcut does not bypass confirmation.

**Data Validation Feedback:**
- [ ] Real-time validation provides immediate feedback as user types.
- [ ] Cross-field validation runs on form submission.
- [ ] Validation rules are visible to the user (e.g., "Must be between 60 and 200").
- [ ] Out-of-range values show the acceptable range in the error message.

**Read-Only State Indicators:**
- [ ] Locked/signed forms are visually distinct from editable forms.
- [ ] Read-only fields have a different background or border treatment.
- [ ] "Locked" / "Signed" status banner is prominent.
- [ ] Edit buttons are hidden or disabled in read-only state.
- [ ] Reason for read-only state is communicated (e.g., "Signed by Dr. Smith").

**Query Indicators:**
- [ ] Open query icon is visible next to the affected field.
- [ ] Query count badge is displayed at the form level.
- [ ] Query status colors are consistent (Open=red, Answered=yellow, Closed=green).
- [ ] Clicking the query icon navigates to or opens the query detail.

### Step 7: Generate the QA Report

Produce a structured report:

```markdown
# Design QA Report

**Design**: [Figma URL]
**Implementation**: [File path(s)]
**Date**: [Current date]
**Overall Status**: [PASS / FAIL - N issues found]

## Summary

| Category | Checks | Pass | Fail | Warnings |
|----------|--------|------|------|----------|
| Spacing | 12 | 10 | 2 | 0 |
| Colors | 8 | 7 | 1 | 0 |
| Typography | 10 | 9 | 0 | 1 |
| Borders/Radius | 5 | 5 | 0 | 0 |
| Shadows | 3 | 3 | 0 | 0 |
| Responsive | 8 | 6 | 2 | 0 |
| Accessibility | 15 | 12 | 3 | 0 |
| EDC-Specific | 10 | 8 | 1 | 1 |
| **Total** | **71** | **60** | **9** | **2** |

## Failures (Must Fix)

### 1. Padding mismatch on card component
- **Property**: padding-right
- **Expected (Figma)**: 24px
- **Actual (Code)**: 16px (`pr-4`)
- **Fix**: Change `pr-4` to `pr-6` on line 23 of CardComponent.tsx
- **Severity**: Medium

### 2. Missing ARIA label on icon button
- **Element**: Close button in modal header
- **Issue**: `<button>` contains only an SVG icon with no accessible text
- **Fix**: Add `aria-label="Close dialog"` to the button element
- **Severity**: High (accessibility)

...

## Warnings (Should Fix)

### 1. Letter spacing not set on heading
- **Property**: letter-spacing
- **Expected (Figma)**: -0.01em
- **Actual (Code)**: normal (default)
- **Impact**: Minor visual difference
- **Fix**: Add `tracking-tight` class

...

## Passes (Verified)

[Collapsed list of all passing checks]
```

## Severity Levels

- **Critical**: Blocks usability or regulatory compliance (missing e-signature, broken form validation).
- **High**: Accessibility violations, significant visual deviations.
- **Medium**: Spacing/color mismatches that affect visual quality.
- **Low**: Minor differences unlikely to be noticed by users.

## Quality Checklist

- [ ] All Figma properties have been compared against implementation.
- [ ] All accessibility checks have been performed.
- [ ] All EDC-specific checks have been evaluated.
- [ ] Responsive behavior has been checked at all breakpoints.
- [ ] Each failure includes a specific, actionable fix.
- [ ] Severity levels are assigned to all issues.
- [ ] Report is formatted for easy scanning and triage.
