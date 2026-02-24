---
name: playwright-test-generation
description: "Generate Playwright automated test scripts for the Talosix EDC platform. Uses Page Object Model, data-testid selectors, and covers clinical trial UI patterns including forms, validation, and e-signatures."
allowed-tools: Read, Grep, Glob, Bash
---

# Playwright Test Generation

## Purpose

Generate production-quality Playwright automated test scripts for the Talosix EDC platform. Tests follow the Page Object Model pattern, use `data-testid` selectors, and cover both happy path and error scenarios for clinical trial workflows.

## Context: Talosix EDC UI Patterns

The Talosix EDC frontend presents clinical trial-specific UI patterns that require specialized testing approaches:

- **CRF Forms:** Multi-section forms with conditional fields, repeating groups, edit checks firing on blur/save.
- **Validation Messages:** Inline field errors, form-level warnings, and query-generating violations.
- **E-Signature Dialogs:** Modal requiring username + password + meaning selection, per 21 CFR Part 11.
- **Audit Trail Viewer:** Expandable history panel showing change history for each field.
- **Query Panels:** Side panels for viewing, responding to, and managing data queries.
- **Role-Based UI:** Features and actions shown/hidden based on user role and study permissions.
- **Data Tables:** Paginated, sortable, filterable tables for subjects, visits, queries, etc.
- **Wizards:** Multi-step configuration wizards for study build.

## Workflow

1. **Analyze the feature:** Read the source code, component files, and/or Jira story to understand the UI behavior.
2. **Identify existing page objects:** Search the test directory for reusable page objects and utilities.
3. **Design test structure:** Plan the test file with describe blocks for happy path and error scenarios.
4. **Generate page objects** (if needed) following the existing POM pattern.
5. **Write test cases** with proper assertions, waits, and cleanup.
6. **Add test data fixtures** for deterministic execution.

## Project Structure Convention

```
tests/
  e2e/
    pages/                    # Page Object Models
      login.page.ts
      dashboard.page.ts
      crf-form.page.ts
      query-inbox.page.ts
      subject-list.page.ts
      ...
    fixtures/                 # Test data and setup utilities
      test-data.ts
      auth.setup.ts
      study.setup.ts
    specs/                    # Test specifications
      auth/
        login.spec.ts
        session.spec.ts
      crf/
        data-entry.spec.ts
        edit-checks.spec.ts
      queries/
        query-workflow.spec.ts
      ...
    utils/                    # Shared helpers
      api-helpers.ts
      date-helpers.ts
      wait-helpers.ts
```

Before generating, use Glob and Grep to discover the actual project structure and conventions.

## Page Object Model Pattern

Every page object must follow this structure:

```typescript
import { type Page, type Locator, expect } from '@playwright/test';

export class CrfFormPage {
  readonly page: Page;

  // Locators -- always use data-testid
  readonly formTitle: Locator;
  readonly saveButton: Locator;
  readonly submitButton: Locator;
  readonly fieldError: (fieldName: string) => Locator;
  readonly formField: (fieldName: string) => Locator;

  constructor(page: Page) {
    this.page = page;
    this.formTitle = page.getByTestId('crf-form-title');
    this.saveButton = page.getByTestId('crf-save-btn');
    this.submitButton = page.getByTestId('crf-submit-btn');
    this.fieldError = (fieldName: string) =>
      page.getByTestId(`field-error-${fieldName}`);
    this.formField = (fieldName: string) =>
      page.getByTestId(`field-${fieldName}`);
  }

  async navigate(subjectId: string, visitId: string, formId: string) {
    await this.page.goto(
      `/subjects/${subjectId}/visits/${visitId}/forms/${formId}`
    );
    await this.formTitle.waitFor({ state: 'visible' });
  }

  async fillField(fieldName: string, value: string) {
    const field = this.formField(fieldName);
    await field.clear();
    await field.fill(value);
    // Trigger blur to fire edit checks
    await field.blur();
  }

  async save() {
    await this.saveButton.click();
    await this.page.getByTestId('save-confirmation').waitFor({
      state: 'visible',
    });
  }

  async expectFieldError(fieldName: string, message: string) {
    await expect(this.fieldError(fieldName)).toHaveText(message);
  }

  async expectNoFieldError(fieldName: string) {
    await expect(this.fieldError(fieldName)).not.toBeVisible();
  }
}
```

### Page Object Rules

- All selectors use `data-testid` attributes. Never use CSS classes, tag names, or XPath for primary selectors.
- Use `getByTestId()`, `getByRole()`, `getByLabel()`, and `getByText()` in order of preference.
- Encapsulate navigation, actions, and assertions as page object methods.
- Page objects do not contain test logic (no `test()` blocks).
- Return `this` from action methods to allow chaining where appropriate.
- Include `waitFor` calls inside page object methods so tests do not need manual waits.

## Test File Pattern

```typescript
import { test, expect } from '@playwright/test';
import { CrfFormPage } from '../pages/crf-form.page';
import { LoginPage } from '../pages/login.page';
import { testData } from '../fixtures/test-data';

test.describe('CRF Data Entry - Lab Values', () => {
  let crfForm: CrfFormPage;

  test.beforeEach(async ({ page }) => {
    // Authentication via stored state or API setup
    const loginPage = new LoginPage(page);
    await loginPage.loginAs(testData.users.siteCoordinator);

    crfForm = new CrfFormPage(page);
    await crfForm.navigate(
      testData.subjects.subject001,
      testData.visits.visit2,
      testData.forms.labResults
    );
  });

  test.describe('Happy Path', () => {
    test('should save valid lab values', async () => {
      await crfForm.fillField('hemoglobin', '14.5');
      await crfForm.fillField('wbc', '7.2');
      await crfForm.save();

      await crfForm.expectNoFieldError('hemoglobin');
      await crfForm.expectNoFieldError('wbc');
    });

    test('should display saved values on form reopen', async ({ page }) => {
      await crfForm.fillField('hemoglobin', '14.5');
      await crfForm.save();

      // Reopen
      await crfForm.navigate(
        testData.subjects.subject001,
        testData.visits.visit2,
        testData.forms.labResults
      );
      await expect(crfForm.formField('hemoglobin')).toHaveValue('14.5');
    });
  });

  test.describe('Error Scenarios', () => {
    test('should show validation error for out-of-range value', async () => {
      await crfForm.fillField('hemoglobin', '99.9');
      await crfForm.expectFieldError(
        'hemoglobin',
        'Value outside expected range (12.0 - 17.5 g/dL)'
      );
    });

    test('should prevent save with required field empty', async () => {
      await crfForm.fillField('hemoglobin', '');
      await crfForm.save();
      await crfForm.expectFieldError('hemoglobin', 'This field is required');
    });

    test('should reject non-numeric input', async () => {
      await crfForm.fillField('hemoglobin', 'abc');
      await crfForm.expectFieldError(
        'hemoglobin',
        'Please enter a valid number'
      );
    });
  });
});
```

## Clinical Trial UI Testing Patterns

### E-Signature Testing

```typescript
async signRecord(username: string, password: string, meaning: string) {
  await this.page.getByTestId('sign-btn').click();
  await this.page.getByTestId('esig-dialog').waitFor({ state: 'visible' });
  await this.page.getByTestId('esig-username').fill(username);
  await this.page.getByTestId('esig-password').fill(password);
  await this.page.getByTestId('esig-meaning').selectOption(meaning);
  await this.page.getByTestId('esig-confirm').click();
  await this.page.getByTestId('esig-dialog').waitFor({ state: 'hidden' });
}
```

Test scenarios for e-signatures:
- Successful signature with valid credentials.
- Rejected signature with wrong password.
- Signature invalidated after record modification.
- Signature meaning recorded in audit trail.
- Concurrent signature attempt by another user.

### Audit Trail Verification

```typescript
async verifyAuditEntry(fieldName: string, expectedValues: {
  user: string;
  action: string;
  oldValue?: string;
  newValue: string;
}) {
  await this.page.getByTestId(`audit-toggle-${fieldName}`).click();
  const auditPanel = this.page.getByTestId(`audit-panel-${fieldName}`);
  await auditPanel.waitFor({ state: 'visible' });

  const latestEntry = auditPanel.getByTestId('audit-entry').first();
  await expect(latestEntry.getByTestId('audit-user')).toContainText(expectedValues.user);
  await expect(latestEntry.getByTestId('audit-action')).toContainText(expectedValues.action);
  await expect(latestEntry.getByTestId('audit-new-value')).toContainText(expectedValues.newValue);

  if (expectedValues.oldValue) {
    await expect(latestEntry.getByTestId('audit-old-value')).toContainText(expectedValues.oldValue);
  }
}
```

### Query Workflow Testing

```typescript
async openQuery(fieldName: string, queryText: string) {
  await this.page.getByTestId(`query-icon-${fieldName}`).click();
  await this.page.getByTestId('query-panel').waitFor({ state: 'visible' });
  await this.page.getByTestId('query-text-input').fill(queryText);
  await this.page.getByTestId('query-submit-btn').click();
  await expect(this.page.getByTestId('query-status')).toHaveText('Open');
}
```

### Data Table Testing

```typescript
async verifyTableRow(rowIndex: number, expectedData: Record<string, string>) {
  const row = this.page.getByTestId('data-table-row').nth(rowIndex);
  for (const [column, value] of Object.entries(expectedData)) {
    await expect(row.getByTestId(`cell-${column}`)).toHaveText(value);
  }
}

async sortByColumn(columnName: string) {
  await this.page.getByTestId(`column-header-${columnName}`).click();
}

async filterTable(filterField: string, filterValue: string) {
  await this.page.getByTestId(`filter-${filterField}`).fill(filterValue);
  await this.page.getByTestId('apply-filter-btn').click();
  // Wait for table to reload
  await this.page.getByTestId('table-loading').waitFor({ state: 'hidden' });
}
```

### Role-Based Access Testing

```typescript
test.describe('Role-Based Access', () => {
  test('site user cannot access data manager functions', async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.loginAs(testData.users.siteCoordinator);

    // Verify restricted nav items are not visible
    await expect(page.getByTestId('nav-db-lock')).not.toBeVisible();
    await expect(page.getByTestId('nav-data-export')).not.toBeVisible();

    // Verify direct URL access is blocked
    await page.goto('/admin/database-lock');
    await expect(page.getByTestId('access-denied')).toBeVisible();
  });
});
```

## Test Data Management

```typescript
// fixtures/test-data.ts
export const testData = {
  users: {
    siteCoordinator: { username: 'site_coord_01', password: 'TestPass123!', role: 'Site Coordinator' },
    cra: { username: 'cra_01', password: 'TestPass123!', role: 'CRA' },
    dataManager: { username: 'dm_01', password: 'TestPass123!', role: 'Data Manager' },
  },
  subjects: {
    subject001: 'SUBJ-001',
    subject002: 'SUBJ-002',
  },
  visits: {
    screening: 'SCR',
    visit1: 'V1',
    visit2: 'V2',
  },
  forms: {
    demographics: 'DM',
    labResults: 'LB',
    adverseEvents: 'AE',
  },
} as const;
```

- Use deterministic test data, never random values.
- Clean up test data in `afterEach` or use isolated test contexts.
- Use API calls in `beforeAll`/`beforeEach` for fast setup instead of UI-driven setup.
- Store authentication state to avoid repeated login flows.

## Selector Strategy

Priority order for selectors:

1. `data-testid` -- primary strategy, most stable.
2. `getByRole()` -- for accessible elements (buttons, links, headings).
3. `getByLabel()` -- for form fields with proper labels.
4. `getByText()` -- for static text content, use sparingly.

Never use:
- CSS class selectors (brittle, change with styling).
- XPath (hard to maintain).
- nth-child or positional selectors without testid context.

When generating tests, if a `data-testid` is not found in the source code, note it as a required addition and use the expected testid name following the convention: `[component]-[element]-[qualifier]`.

## Guidelines

- Use `async/await` consistently. Never use `.then()` chains.
- Prefer `expect` auto-retrying assertions over manual `waitFor` + assert.
- Set reasonable timeouts for clinical operations that may be slow (e.g., form save, data export).
- Group related tests in `describe` blocks.
- Each test must be independent. Use `beforeEach` for setup, not test order dependency.
- Include `test.describe.configure({ mode: 'serial' })` only when tests genuinely depend on shared state (rare).
- Add JSDoc comments to page object methods describing the clinical workflow context.
- Tag tests with annotations: `test('name', { tag: ['@smoke', '@regression', '@regulatory'] }, ...)`.
