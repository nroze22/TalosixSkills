---
name: figma-component-generator
description: "Generates detailed, Figma-ready component specifications from requirements. Produces comprehensive specs covering all states, variants, design token mappings, interaction definitions, responsive behavior, and accessibility requirements. Specializes in clinical trial EDC components such as CRF fields, visit schedule cells, query badges, and audit trail entries."
argument-hint: "[component-name]"
---

# Figma Component Generator

## Purpose

Produce a complete component specification document that a designer can implement directly in Figma without ambiguity. Every state, variant, token, interaction, and edge case is defined up front, eliminating back-and-forth during design implementation.

## Inputs

- **Component name**: The name of the component to specify.
- **Purpose**: What the component does and where it is used (provided by user or inferred).
- **Variants needed**: Optional list of variants; if not provided, infer from the component type.

## Workflow

### Step 1: Gather Context

1. Search the existing codebase for any prior implementation or reference to this component.
2. Search for design system documentation or token files.
3. If the component relates to a Jira story, use `mcp__claude_ai_Atlassian__getJiraIssue` to pull requirements.
4. If design system docs are in Confluence, use `mcp__claude_ai_Atlassian__getConfluencePage` to retrieve them.
5. Ask the user for any additional context not found.

### Step 2: Define the Component Anatomy

Break the component into its structural parts:

```
Component: CRF Text Field
Anatomy:
  - Container (outer wrapper)
    - Label Row
      - Label text
      - Required indicator (asterisk)
      - Help icon (tooltip trigger)
    - Input Container
      - Input field
      - Unit label (optional, e.g., "mmHg")
      - Clear button (optional)
    - Feedback Row
      - Help text OR Error message OR Warning message
      - Character count (optional)
    - Meta Row (optional)
      - Query indicator icon
      - SDV indicator icon
      - Audit trail link
```

Include a text-based layout diagram:

```
+------------------------------------------+
| Label Text *                         [?] |
| +--------------------------------------+ |
| | Input value                    | unit | |
| +--------------------------------------+ |
| Help text or error message        0/200  |
| [Q] [SDV] [Audit]                        |
+------------------------------------------+
```

### Step 3: Define All States

For each component, define every possible visual state:

**Core States:**

| State | Description | Visual Treatment |
|-------|-------------|-----------------|
| Default | Empty, ready for input | Gray border, placeholder text |
| Focused | User has clicked into the field | Primary blue border (2px), subtle blue glow |
| Filled | Contains user-entered value | Dark text, gray border |
| Hover | Mouse cursor over the field | Slightly darker border |
| Disabled | Cannot be interacted with | Gray background, 50% opacity, no cursor |
| Read-Only | Displays value but cannot be edited | No border, value displayed as text, gray background |
| Error | Validation has failed | Red border, red error icon, red error text below |
| Warning | Soft check triggered | Amber border, amber warning icon, amber text below |
| Loading | Value being loaded or validated | Skeleton pulse animation or spinner in field |
| Success | Value accepted/verified | Green checkmark icon, brief green border flash |

**Clinical Trial Specific States:**

| State | Description | Visual Treatment |
|-------|-------------|-----------------|
| Queried | Open query on this field | Red query badge icon to the left of the field |
| Query Answered | Query has been responded to | Yellow query badge icon |
| Query Closed | Query resolved | Green query badge icon (fades after 5s) |
| SDV Verified | Source data verified by monitor | Blue checkmark SDV icon |
| SDV Unverified | Not yet source data verified | Gray circle SDV icon |
| Locked | Form is signed/locked | Read-only appearance + lock icon |
| Frozen | Data is frozen for analysis | Read-only appearance + snowflake icon |

### Step 4: Define All Variants

**Size Variants:**

| Size | Field Height | Font Size | Label Size | Padding |
|------|-------------|-----------|------------|---------|
| Small (sm) | 32px | 14px | 12px | 6px 8px |
| Medium (md) | 40px | 14px | 14px | 8px 12px |
| Large (lg) | 48px | 16px | 14px | 12px 16px |

Default: Medium (md).

**Type Variants (for CRF fields):**

| Type | Input Element | Additional Elements |
|------|--------------|-------------------|
| Text | `<input type="text">` | Character count (optional) |
| Numeric | `<input type="text" inputmode="numeric">` | Unit label, range hint |
| Date | Date picker input | Calendar icon, format hint (DD-MMM-YYYY) |
| Dropdown | Custom select | Chevron icon, option list overlay |
| Checkbox | Checkbox input | Checkmark icon within box |
| Radio | Radio group | Filled circle indicator |
| Textarea | `<textarea>` | Character count, resize handle |
| File Upload | Drop zone / file input | Upload icon, file list, progress bar |

**Theme Variants (if applicable):**

| Theme | Use Case | Background | Border |
|-------|----------|-----------|--------|
| Default | Standard data entry | White | Gray-200 |
| Highlighted | Flagged for review | Yellow-50 | Yellow-300 |
| Verified | SDV verified fields | Blue-50 | Blue-200 |

### Step 5: Map Design Tokens

Provide exact token references for every visual property:

```yaml
tokens:
  container:
    background:
      default: color.neutral.white        # #FFFFFF
      disabled: color.neutral.100         # F3F4F6
      readOnly: color.neutral.50          # F9FAFB
      error: color.red.50                 # FEF2F2
      warning: color.amber.50            # FFFBEB
    border:
      default: color.neutral.200          # E5E7EB
      hover: color.neutral.300            # D1D5DB
      focused: color.primary.500          # 2563EB
      error: color.red.500               # EF4444
      warning: color.amber.500           # F59E0B
      disabled: color.neutral.200         # E5E7EB
    borderWidth:
      default: border.width.1             # 1px
      focused: border.width.2             # 2px
    borderRadius: border.radius.md        # 6px
    shadow:
      focused: shadow.ring.primary        # 0 0 0 3px rgba(37,99,235,0.1)
      error: shadow.ring.error            # 0 0 0 3px rgba(239,68,68,0.1)

  label:
    color:
      default: color.neutral.700          # 374151
      disabled: color.neutral.400         # 9CA3AF
      error: color.red.700               # B91C1C
    fontSize: font.size.sm                # 14px
    fontWeight: font.weight.medium        # 500
    lineHeight: font.lineHeight.5         # 20px
    marginBottom: spacing.1               # 4px

  requiredIndicator:
    color: color.red.500                  # EF4444
    fontSize: font.size.sm                # 14px
    marginLeft: spacing.0.5               # 2px

  input:
    color:
      default: color.neutral.900          # 111827
      placeholder: color.neutral.400      # 9CA3AF
      disabled: color.neutral.500         # 6B7280
    fontSize:
      sm: font.size.sm                    # 14px
      md: font.size.sm                    # 14px
      lg: font.size.base                  # 16px
    fontWeight: font.weight.normal        # 400
    padding:
      sm: spacing.1.5 spacing.2           # 6px 8px
      md: spacing.2 spacing.3             # 8px 12px
      lg: spacing.3 spacing.4             # 12px 16px

  helpText:
    color: color.neutral.500              # 6B7280
    fontSize: font.size.xs                # 12px
    marginTop: spacing.1                  # 4px

  errorText:
    color: color.red.600                  # DC2626
    fontSize: font.size.xs                # 12px
    fontWeight: font.weight.medium        # 500
    marginTop: spacing.1                  # 4px
    icon: icon.exclamation-circle         # 16px, color.red.500

  warningText:
    color: color.amber.600               # D97706
    fontSize: font.size.xs                # 12px
    fontWeight: font.weight.medium        # 500
    marginTop: spacing.1                  # 4px
    icon: icon.exclamation-triangle       # 16px, color.amber.500
```

### Step 6: Define Interaction Specifications

**Transitions and Animations:**

| Interaction | Property | Duration | Easing | Details |
|-------------|----------|----------|--------|---------|
| Focus in | border-color, box-shadow | 150ms | ease-in-out | Border transitions to primary, ring appears |
| Focus out | border-color, box-shadow | 150ms | ease-in-out | Border returns to default, ring fades |
| Hover in | border-color | 100ms | ease-in | Border slightly darkens |
| Hover out | border-color | 100ms | ease-out | Border returns |
| Error appear | opacity, transform | 200ms | ease-out | Error text fades in and slides down 4px |
| Error dismiss | opacity, transform | 150ms | ease-in | Error text fades out and slides up |
| Loading pulse | opacity | 1500ms | ease-in-out | Skeleton shimmer animation, infinite |
| Success flash | border-color | 300ms | ease-out | Green border flash, then return to default |

**Keyboard Interactions:**

| Key | Action |
|-----|--------|
| Tab | Move focus to next field |
| Shift+Tab | Move focus to previous field |
| Enter | Submit form (if last field) or move to next field |
| Escape | Clear field / close dropdown / cancel edit |
| Arrow Up/Down | Navigate dropdown options |
| Space | Toggle checkbox / select radio |

**Mouse Interactions:**

| Action | Result |
|--------|--------|
| Click field | Focus the input |
| Click label | Focus the associated input |
| Click help icon | Show tooltip popover |
| Click clear button | Clear field value, maintain focus |
| Click query icon | Open query detail panel |
| Click SDV icon | Toggle SDV status (if authorized) |
| Click audit link | Open audit trail sidebar |

### Step 7: Define Responsive Behavior

| Breakpoint | Behavior |
|------------|----------|
| Mobile (< 640px) | Full width, label above input, meta row wraps |
| Tablet (640-1023px) | Full width or half width in 2-column form grid |
| Desktop (1024-1279px) | Follows form grid (1, 2, or 3 columns as configured) |
| Wide (>= 1280px) | Same as desktop, max field width 600px |

**Label positioning:**
- Mobile/Tablet: Always above the input (stacked).
- Desktop/Wide: Above by default; optionally left-aligned (label width 200px) for dense forms.

### Step 8: Define Accessibility Requirements

```yaml
accessibility:
  role: "textbox" (implicit from <input>)
  aria:
    - aria-label: Set if no visible label (rare)
    - aria-labelledby: Points to label element ID
    - aria-describedby: Points to help text or error message ID
    - aria-required: "true" if field is required
    - aria-invalid: "true" when validation error is present
    - aria-errormessage: Points to error text element ID
    - aria-disabled: "true" when disabled
    - aria-readonly: "true" when read-only
  focus:
    - Must have visible focus indicator (ring)
    - Focus indicator must have 3:1 contrast against background
    - Tab order follows visual layout order
  colorContrast:
    - Label text: 4.5:1 minimum against background
    - Input text: 4.5:1 minimum against field background
    - Placeholder text: 4.5:1 minimum (treat as informative content)
    - Error text: 4.5:1 minimum against background
    - Error state must use icon in addition to color
  touchTarget:
    - Minimum 44x44px clickable area
    - Help icon and meta icons must meet minimum size
```

### Step 9: Generate the Specification Document

Output the complete specification as a structured document:

```markdown
# Component Specification: [Component Name]

## Overview
[One paragraph describing purpose and usage context]

## Anatomy
[Structural breakdown with layout diagram]

## States
[Table of all states with visual descriptions]

## Variants
[Tables for size, type, and theme variants]

## Design Tokens
[Complete token mapping in YAML]

## Interactions
[Animation, keyboard, and mouse interaction tables]

## Responsive Behavior
[Breakpoint behavior table]

## Accessibility
[ARIA attributes, focus management, contrast requirements]

## Usage Guidelines
- DO: [Correct usage examples]
- DON'T: [Anti-pattern examples]

## Clinical Trial Context
[EDC-specific usage notes and regulatory considerations]
```

## Clinical Trial EDC Component Templates

When one of these components is requested, use the detailed template as a starting point:

### CRF Field Types
- **Text Field**: Free text entry with optional character limit.
- **Numeric Field**: Number entry with unit label and range validation display.
- **Date Field**: Date picker with DD-MMM-YYYY format enforcement.
- **Dropdown/Select**: Single or multi-select from coded list.
- **Checkbox Group**: Multiple selection from a set of options.
- **Radio Group**: Single selection from mutually exclusive options.
- **Textarea**: Multi-line text for narrative fields (AE descriptions, medical history).
- **File Upload**: Document attachment with type/size restrictions.
- **Lab Value Field**: Numeric with normal range display and out-of-range flagging.

### Visit Schedule Cell
- States: Scheduled, In Progress, Complete, Overdue, Missed, Not Required.
- Click behavior: Navigate to CRF list for that subject-visit.
- Hover: Show visit window dates and completion percentage.
- Badge: Number of incomplete CRFs.

### Query Badge
- States: Open (red), Answered (amber), Closed (green).
- Size variants: Inline (next to field), Count (on tab/section header).
- Click behavior: Open query detail panel.
- Tooltip: Query text preview on hover.

### Signature Button
- States: Ready to Sign, Signing (loading), Signed, Signature Broken.
- Click behavior: Open e-signature modal.
- Display: Signer name, date/time, meaning statement after signing.

### Audit Trail Entry
- Layout: Timestamp | User | Action | Field | Old Value | New Value | Reason.
- Grouping: By date, by field, by user.
- Read-only presentation (no interactive elements).

### Status Indicator
- Types: Subject status, CRF status, Visit status, Query status, Study status.
- Visual: Colored dot + text label.
- Consistent color mapping across the application.

## Quality Checklist

- [ ] Every state is defined with specific visual properties.
- [ ] Every variant combination is specified.
- [ ] All design tokens reference the token system (not raw values).
- [ ] Interaction specifications include timing and easing.
- [ ] Responsive behavior is defined for all breakpoints.
- [ ] Accessibility requirements are complete and specific.
- [ ] Usage guidelines include do/don't examples.
- [ ] Clinical trial regulatory context is addressed.
- [ ] The spec is detailed enough for a designer to implement without questions.
