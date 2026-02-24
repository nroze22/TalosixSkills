---
name: Figma to Code
description: >
  Converts Figma designs into production-ready React/TypeScript components with
  Tailwind CSS. Uses the Figma MCP server to read design data, extracts component
  structure and design tokens, generates accessible components with proper typing,
  and outputs Storybook stories. Follows Talosix EDC component conventions.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - Edit
argument-hint: "[figma-url]"
---

# Figma to Code

## Purpose

Bridge the design-to-development handoff by converting Figma designs into production-quality React/TypeScript code. This eliminates manual translation of visual specs, reduces implementation drift from design intent, and accelerates delivery of pixel-accurate UI.

## Prerequisites

- Figma MCP server configured and accessible.
- The Figma file URL or node ID for the target design.
- Knowledge of the target codebase structure (scan existing components first).

## Workflow

### Step 1: Read the Figma Design

1. Parse the Figma URL to extract the file key and node ID.
   - URL format: `https://www.figma.com/design/{fileKey}/{fileName}?node-id={nodeId}`
2. Use the Figma MCP server to fetch the design data:
   - File structure and page hierarchy.
   - Component instances and their properties.
   - Auto-layout properties (direction, spacing, padding, alignment).
   - Fill and stroke styles (colors, gradients, borders).
   - Text styles (font family, size, weight, line height, letter spacing).
   - Effect styles (shadows, blurs).
   - Constraints and responsive behavior.
3. If multiple frames or components are selected, process each one.

### Step 2: Analyze Existing Codebase Patterns

Before generating code, scan the target codebase for conventions:

```
Search for:
- Existing component directory structure
- Import patterns (absolute vs relative paths)
- Naming conventions (PascalCase components, camelCase props)
- State management patterns (useState, useReducer, context, stores)
- Form handling libraries (react-hook-form, formik, or custom)
- Testing patterns (testing-library, enzyme)
- Storybook configuration and story format
- Tailwind configuration (custom theme values, plugins)
- Existing design token files
```

Adapt all generated code to match the codebase conventions found.

### Step 3: Extract Design Tokens

Map Figma styles to design tokens:

**Colors:**
```typescript
// Extract from Figma fills
const colors = {
  primary: { value: '#2563EB', figmaStyle: 'Primary/Default' },
  primaryHover: { value: '#1D4ED8', figmaStyle: 'Primary/Hover' },
  // ... map all colors used in the design
};
```

**Typography:**
```typescript
const typography = {
  headingLg: {
    fontFamily: 'Inter',
    fontSize: '1.25rem',
    fontWeight: 600,
    lineHeight: 1.4,
    letterSpacing: '-0.01em',
  },
  // ... map all text styles
};
```

**Spacing:**
- Extract padding and gap values from auto-layout frames.
- Map to Tailwind spacing scale (p-2, p-4, gap-3, etc.).

**Shadows:**
- Map Figma drop shadows to Tailwind shadow utilities or custom CSS.

**Border Radius:**
- Map corner radius values to Tailwind rounded utilities.

If tokens differ from the existing Tailwind config, flag discrepancies in the output.

### Step 4: Determine Component Architecture

Analyze the design to identify:

1. **Atomic components**: Buttons, inputs, badges, icons.
2. **Molecule components**: Form fields (label + input + error), table rows, card headers.
3. **Organism components**: Complete forms, data tables, navigation bars, modal dialogs.
4. **Template layouts**: Page-level layouts combining organisms.

For each component, determine:
- Props interface (what varies between instances).
- State requirements (controlled vs uncontrolled, internal state).
- Event handlers needed (onClick, onChange, onSubmit).
- Children/slot patterns (where content is projected in).

### Step 5: Generate React Components

For each identified component, generate:

**Component file (`ComponentName.tsx`):**

```typescript
import React from 'react';
import { cn } from '@/lib/utils'; // or project's classname utility

// -- Types --
export interface ComponentNameProps {
  /** Description of the prop */
  variant?: 'default' | 'primary' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  children: React.ReactNode;
  className?: string;
  'data-testid'?: string;
}

// -- Component --
export const ComponentName: React.FC<ComponentNameProps> = ({
  variant = 'default',
  size = 'md',
  disabled = false,
  children,
  className,
  'data-testid': testId,
}) => {
  return (
    <div
      className={cn(
        'base-classes-here',
        variantStyles[variant],
        sizeStyles[size],
        disabled && 'opacity-50 cursor-not-allowed',
        className,
      )}
      data-testid={testId}
      role="appropriate-role"
      aria-disabled={disabled}
    >
      {children}
    </div>
  );
};

ComponentName.displayName = 'ComponentName';
```

**Conventions to follow:**

- Always include `data-testid` prop for QA automation.
- Always include `className` prop for style overrides.
- Use `cn()` or `clsx()` for conditional class merging (match project pattern).
- Export named components (not default exports) unless project convention differs.
- Include JSDoc comments on all props.
- Use semantic HTML elements (`<button>`, `<nav>`, `<main>`, `<table>`).
- Include `displayName` for React DevTools.

### Step 6: Apply Accessibility Attributes

For every component, include appropriate accessibility:

**Interactive Elements:**
- `role` attribute when semantic HTML is insufficient.
- `aria-label` or `aria-labelledby` for elements without visible text.
- `aria-describedby` for supplementary descriptions.
- `aria-expanded`, `aria-haspopup` for menus and dropdowns.
- `aria-selected`, `aria-checked` for selection controls.
- `tabIndex` for custom interactive elements.
- Keyboard event handlers (`onKeyDown`) for Enter and Space activation.

**Form Components:**
- `<label>` elements with `htmlFor` linking to inputs.
- `aria-required` for required fields.
- `aria-invalid` and `aria-errormessage` for validation errors.
- `aria-describedby` linking to help text.
- Proper `type` attributes on inputs.
- `autocomplete` attributes where applicable.

**Data Tables:**
- `<caption>` for table description.
- `scope="col"` and `scope="row"` on header cells.
- `aria-sort` on sortable column headers.
- `aria-label` on action buttons within cells.

**Navigation:**
- `<nav>` element with `aria-label`.
- `aria-current="page"` on active navigation item.
- Skip navigation link at the top of the page.

**Modals/Dialogs:**
- `role="dialog"` with `aria-modal="true"`.
- `aria-labelledby` pointing to dialog title.
- Focus trap within the dialog.
- Return focus to trigger element on close.

### Step 7: Handle Talosix EDC-Specific Patterns

**Form Components with Validation:**
```typescript
// CRF field wrapper with query and SDV indicators
interface CrfFieldProps {
  label: string;
  required?: boolean;
  queryStatus?: 'open' | 'answered' | 'closed' | null;
  sdvStatus?: 'verified' | 'unverified' | null;
  editCheckWarning?: string;
  auditTrailCount?: number;
  children: React.ReactNode;
}
```

**Data Tables with Clinical Context:**
- Subject listing tables with status badges.
- Adverse event tables with severity color coding.
- Visit schedule grids with cell-state styling.
- Query tables with age-based urgency indicators.

**Audit Trail UI:**
- Read-only chronological change display.
- Reason-for-change capture on edits.
- Timestamp formatting per site locale.

**E-Signature Patterns:**
- 21 CFR Part 11 compliant signature dialog.
- Meaning statement display.
- Credential re-entry form.
- Signature manifestation display.

### Step 8: Generate Storybook Stories

For each component, generate a story file:

```typescript
import type { Meta, StoryObj } from '@storybook/react';
import { ComponentName } from './ComponentName';

const meta: Meta<typeof ComponentName> = {
  title: 'Components/CategoryName/ComponentName',
  component: ComponentName,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['default', 'primary', 'danger'],
    },
    size: {
      control: 'select',
      options: ['sm', 'md', 'lg'],
    },
    disabled: { control: 'boolean' },
  },
};

export default meta;
type Story = StoryObj<typeof ComponentName>;

export const Default: Story = {
  args: {
    children: 'Component Content',
    variant: 'default',
    size: 'md',
  },
};

export const AllVariants: Story = {
  render: () => (
    <div className="flex flex-col gap-4">
      <ComponentName variant="default">Default</ComponentName>
      <ComponentName variant="primary">Primary</ComponentName>
      <ComponentName variant="danger">Danger</ComponentName>
    </div>
  ),
};

export const Disabled: Story = {
  args: {
    children: 'Disabled State',
    disabled: true,
  },
};
```

Include stories for:
- Default state.
- All variant combinations.
- All size combinations.
- Disabled state.
- Error state (for form components).
- Loading state (if applicable).
- With realistic clinical trial data.

### Step 9: Output and Verification

1. Write all component files to the appropriate directory in the codebase.
2. Write all story files alongside their components.
3. Run linting if available (`npm run lint` or `npx eslint`).
4. Run TypeScript compilation check (`npx tsc --noEmit`).
5. Report any design-to-code discrepancies:
   - Colors not in the Tailwind config.
   - Fonts not loaded in the project.
   - Spacing values that do not map to the Tailwind scale.
   - Interactions described in Figma prototyping that need additional implementation.

## Quality Checklist

- [ ] All components have TypeScript interfaces with JSDoc comments.
- [ ] All interactive elements have accessibility attributes.
- [ ] All components include `data-testid` prop.
- [ ] All components include `className` prop for overrides.
- [ ] Tailwind classes match Figma design values within 1px tolerance.
- [ ] Storybook stories cover all states and variants.
- [ ] No hardcoded strings (use props or constants).
- [ ] Responsive behavior matches Figma breakpoints.
- [ ] Color contrast meets WCAG 2.1 AA (4.5:1 for text, 3:1 for large text).
- [ ] Keyboard navigation works for all interactive elements.
- [ ] Code matches existing codebase conventions.
