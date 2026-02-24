---
name: Error Handling Strategy
description: Design error handling patterns for Talosix EDC systems with structured logging for audit trails, user-friendly error messages, recovery patterns, and clinical trial data integrity protection.
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Error Handling Strategy for Talosix EDC

## Domain Context

In clinical trial EDC systems, error handling is directly tied to patient safety and data integrity. A poorly handled error can lead to silent data loss, corrupted audit trails, or misleading information presented to clinical site users. FDA 21 CFR Part 11 and ICH E6(R2) require that computerized systems have adequate controls to prevent unauthorized or accidental data modification. Error handling is a critical component of these controls.

## Error Classification

### Severity Levels

| Level | Definition | EDC Example | Response |
|-------|-----------|-------------|----------|
| Critical | Data integrity at risk, system unusable | Audit trail write failure, database corruption | Halt operation, alert immediately, do not proceed |
| High | Core functionality impaired | Form save failure, edit check engine error | Block the operation, notify user, log for investigation |
| Medium | Feature degraded but workaround exists | Report generation timeout, non-critical integration failure | Notify user, suggest workaround, log and queue for retry |
| Low | Minor issue, no data impact | UI rendering glitch, non-essential API warning | Log, display non-intrusive notification |

### Error Categories

#### Data Integrity Errors (CRITICAL -- never swallow)
- Audit trail write failures.
- Transaction commit failures during data save.
- Checksum or hash mismatches on stored data.
- Electronic signature verification failures.
- Concurrent modification conflicts.

#### Validation Errors (Expected, user-facing)
- Edit check failures on form data.
- Required field violations.
- Data type or range violations.
- Cross-form consistency check failures.

#### Integration Errors (External system failures)
- CTMS, IVRS, lab system connectivity failures.
- SSO/identity provider timeouts.
- Notification service (email, SMS) failures.

#### Infrastructure Errors (System-level)
- Database connection pool exhaustion.
- Memory or disk space limits.
- Network timeouts between services.
- Cache unavailability.

## Error Handling Patterns

### Pattern 1: Fail-Safe for Data Integrity

When an operation involves clinical data, the system must fail safely -- meaning no partial writes, no silent data loss, and no corrupted state.

```
BEGIN TRANSACTION
  Write form data to data table
  Write audit trail entry to audit table
  IF audit trail write fails:
    ROLLBACK entire transaction
    Return error to user: "Data was NOT saved"
    Log critical error with full context
    Alert operations team
  END IF
COMMIT TRANSACTION
```

**Principle**: Never save clinical data without its corresponding audit trail entry. If the audit trail cannot be written, the data save must be rolled back. Data without an audit trail is a regulatory violation.

### Pattern 2: Structured Error Response

All API error responses should follow a consistent structure.

```json
{
  "error": {
    "code": "FORM_SAVE_CONFLICT",
    "message": "This form was modified by another user. Please refresh and re-enter your changes.",
    "details": {
      "field": null,
      "conflictingUser": "jsmith",
      "conflictTimestamp": "2025-01-15T14:30:00Z"
    },
    "traceId": "abc-123-def-456",
    "timestamp": "2025-01-15T14:30:05Z"
  }
}
```

**Fields**:
- `code`: Machine-readable error identifier for client-side handling.
- `message`: Human-readable message safe to display to clinical site users.
- `details`: Contextual information relevant to the error type.
- `traceId`: Correlation ID for tracing through logs and services.
- `timestamp`: Server-side timestamp of the error.

### Pattern 3: Validation Error Aggregation

For form submissions, collect all validation errors rather than failing on the first one.

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "Please correct the following errors before saving.",
    "validationErrors": [
      {
        "field": "WEIGHT",
        "code": "RANGE_CHECK",
        "message": "Weight must be between 20 and 300 kg.",
        "editCheckId": "EC-001",
        "severity": "hard"
      },
      {
        "field": "VISIT_DATE",
        "code": "CHRONOLOGICAL_CHECK",
        "message": "Visit date cannot be before the informed consent date.",
        "editCheckId": "EC-042",
        "severity": "hard"
      }
    ]
  }
}
```

**Severity types**:
- `hard`: Blocks save. Data cannot be submitted until resolved.
- `soft`: Generates a query. Data can be saved but is flagged for review.

### Pattern 4: Retry with Backoff

For transient failures (network timeouts, temporary service unavailability), implement retry with exponential backoff.

```
attempt = 0
maxAttempts = 3
baseDelay = 1000ms

WHILE attempt < maxAttempts:
  TRY:
    result = performOperation()
    RETURN result
  CATCH TransientError:
    attempt += 1
    IF attempt == maxAttempts:
      LOG error with all attempt details
      RAISE PermanentFailureError
    delay = baseDelay * (2 ^ attempt) + randomJitter()
    WAIT delay
```

**Constraints for EDC**:
- Never retry operations that modify data without idempotency guarantees.
- Always retry with a unique request ID to detect duplicate processing.
- Log each retry attempt for the audit trail.
- Set a maximum total retry duration (e.g., 30 seconds) to avoid blocking the user.

### Pattern 5: Circuit Breaker

For external service integrations that may be down for extended periods.

```
States: CLOSED (normal) -> OPEN (failing) -> HALF_OPEN (testing recovery)

CLOSED: Forward requests normally. Track failure rate.
  IF failure rate > threshold (e.g., 50% in 60 seconds):
    Transition to OPEN

OPEN: Immediately return fallback response. Do not call external service.
  AFTER timeout (e.g., 30 seconds):
    Transition to HALF_OPEN

HALF_OPEN: Allow one test request through.
  IF success: Transition to CLOSED
  IF failure: Transition to OPEN
```

**EDC application**: Use for CTMS integration, email notification service, lab data import. Provide graceful degradation -- the EDC system continues to function for data entry even if external integrations are temporarily unavailable.

### Pattern 6: Dead Letter Queue

For asynchronous operations that fail after all retries.

- Failed messages are moved to a dead letter queue for manual investigation.
- Each dead letter entry includes: original message, error details, retry history, timestamp.
- Alert operations team when dead letter queue depth exceeds threshold.
- EDC-critical: Any message involving data modifications must be resolved, not silently discarded.

## Structured Logging for Audit Trails

### Log Entry Structure

```json
{
  "timestamp": "2025-01-15T14:30:05.123Z",
  "level": "ERROR",
  "service": "form-service",
  "traceId": "abc-123-def-456",
  "spanId": "ghi-789",
  "userId": "user-42",
  "studyId": "STUDY-001",
  "siteId": "SITE-101",
  "action": "FORM_SAVE",
  "errorCode": "DB_WRITE_FAILURE",
  "message": "Failed to save form data: connection timeout",
  "context": {
    "formId": "FORM-AE-001",
    "subjectId": "SUB-1042",
    "visitId": "VISIT-03",
    "attemptNumber": 3
  },
  "stack": "full stack trace here"
}
```

### Logging Requirements
- **Always log**: traceId, userId, studyId, action, timestamp.
- **Never log**: Patient PII (names, dates of birth, medical record numbers) in application logs. Use subject identifiers only.
- **Correlation**: Use consistent trace IDs across services for a single user action.
- **Immutability**: Application logs should be shipped to an append-only log store.
- **Retention**: Logs for validated systems must be retained per regulatory requirements.

### Log Levels for EDC

| Level | Use | Example |
|-------|-----|---------|
| CRITICAL | Data integrity risk, system halt required | Audit trail write failure |
| ERROR | Operation failed, user action needed | Form save failed after retries |
| WARN | Unexpected condition, system recovered | Retry succeeded on second attempt |
| INFO | Normal operations, significant events | User logged in, form saved, data exported |
| DEBUG | Detailed diagnostic information | Query execution plan, request/response payloads |

## User-Friendly Error Messages

### Principles
- Tell the user what happened in plain language.
- Tell the user what they can do about it.
- Never expose internal system details (stack traces, SQL errors, internal IPs).
- Never blame the user.
- Provide a reference code they can share with support.

### Message Templates

| Scenario | Bad Message | Good Message |
|----------|------------|-------------|
| Database timeout | "Error: ETIMEDOUT 10.0.1.5:5432" | "We could not save your data due to a temporary issue. Please try again. If this continues, contact support with reference ID: ABC123." |
| Concurrent edit | "Error 409: Conflict" | "This form was updated by another user while you were editing. Please refresh to see the latest data, then re-enter your changes." |
| Permission denied | "403 Forbidden" | "You do not have permission to perform this action. Please contact your study administrator if you believe this is incorrect." |
| Validation failure | "Invalid input" | "The value entered for Weight (450 kg) is outside the expected range (20-300 kg). Please verify and correct." |

### Localization Considerations
- Clinical trials are global. Error messages should support internationalization.
- Use message codes that map to locale-specific text.
- Maintain consistent medical/clinical terminology per locale.

## Data Integrity Protection

### Defensive Coding Practices
- Validate all inputs at the API boundary, not just in the UI.
- Use database constraints (NOT NULL, CHECK, FOREIGN KEY) as a safety net.
- Implement optimistic locking on clinical data records to detect concurrent modifications.
- Hash or checksum critical data at rest to detect tampering.

### Transaction Boundaries
- Group related writes (data + audit trail) in a single database transaction.
- Use savepoints for complex multi-step operations so partial rollback is possible.
- Never auto-commit individual statements when multiple related writes are required.

### Idempotency
- Design write operations to be safely retryable.
- Use idempotency keys for API requests that modify data.
- Detect and reject duplicate form submissions at the server side.

### Data Loss Prevention
- Buffer unsaved form data on the client side (localStorage or similar).
- Warn users before navigating away from unsaved changes.
- Implement auto-save with conflict detection for long-running data entry sessions.

## Error Monitoring and Alerting

### Alert Thresholds
- **Immediate alert (page on-call)**: Any critical-level error, audit trail failure, data integrity check failure.
- **Urgent alert (within 15 minutes)**: Error rate exceeds 5% of requests, all retries exhausted for data operations.
- **Standard alert (next business day)**: Elevated warning rate, dead letter queue growth, non-critical integration failures.

### Error Dashboards
- Real-time error rate by service and endpoint.
- Top errors by frequency and impact.
- Error trends over time (detect gradual degradation).
- Per-study and per-site error rates (detect site-specific issues).

## When Applying This Skill

1. Read the existing error handling code to understand current patterns.
2. Identify gaps against the patterns described above, prioritizing data integrity.
3. Check that audit trail writes are never separated from data writes.
4. Verify that error messages shown to users are appropriate and helpful.
5. Ensure structured logging includes the required contextual fields.
6. Recommend specific improvements with code examples.
