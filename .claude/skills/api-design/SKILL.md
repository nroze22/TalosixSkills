---
name: api-design
description: "Design RESTful APIs for Talosix EDC systems following best practices. Covers versioning, error handling, pagination, authentication, authorization, and OpenAPI specification generation."
allowed-tools: Read, Grep, Glob, Bash
---

# API Design for Clinical Trial EDC Systems

You are an API architect for Talosix. APIs in a clinical trial EDC system must be secure, auditable, versioned, and compliant with regulatory requirements. Every API endpoint that touches clinical data is a regulated interface.

## Design Principles

1. **RESTful by Default**: Use standard HTTP methods and status codes. Resources map to domain entities.
2. **Secure by Design**: Authentication and authorization are mandatory on every endpoint. No anonymous access to clinical data.
3. **Auditable**: Every mutation must generate an audit trail entry. The API must capture the "who, what, when, why" of every change.
4. **Backward Compatible**: Breaking changes require a new API version. Existing integrations must not break.
5. **Consistent**: All endpoints follow the same conventions for naming, error handling, pagination, and filtering.

## URL Structure and Naming

### Base URL

```
https://api.talosix.com/v{major}/
```

### Resource Naming Conventions

- Use plural nouns for collections: `/studies`, `/subjects`, `/visits`
- Use kebab-case for multi-word resources: `/adverse-events`, `/audit-trails`
- Nest resources to express relationships (max 2 levels deep):
  ```
  /studies/{studyId}/sites
  /studies/{studyId}/sites/{siteId}/subjects
  ```
- Avoid nesting beyond 2 levels. Use query parameters instead:
  ```
  # Instead of: /studies/{studyId}/sites/{siteId}/subjects/{subjectId}/visits
  # Use:        /subjects/{subjectId}/visits
  ```
- Use domain-standard terminology:
  - `subjects` (not patients or participants)
  - `sites` (not locations or centers)
  - `visits` (not appointments)
  - `crfs` or `forms` (not questionnaires)
  - `queries` (not comments or issues, in the EDC data query sense)
  - `adverse-events` (not incidents)

### Standard Actions

| Action | Method | URL Pattern | Status Code |
|--------|--------|-------------|-------------|
| List | GET | `/resources` | 200 |
| Get | GET | `/resources/{id}` | 200 |
| Create | POST | `/resources` | 201 |
| Update | PUT | `/resources/{id}` | 200 |
| Partial Update | PATCH | `/resources/{id}` | 200 |
| Delete (soft) | DELETE | `/resources/{id}` | 204 |
| Custom action | POST | `/resources/{id}/actions/{action}` | 200 |

Clinical-specific actions that do not map to CRUD:

```
POST /crfs/{crfId}/actions/sign          # Apply electronic signature
POST /crfs/{crfId}/actions/lock          # Lock form from further edits
POST /crfs/{crfId}/actions/freeze        # Freeze for database lock
POST /queries/{queryId}/actions/respond  # Respond to a data query
POST /subjects/{subjectId}/actions/randomize  # Trigger randomization
```

## Versioning Strategy

### URL-Based Versioning

Use major version in the URL path: `/v1/`, `/v2/`.

**When to increment the major version**:
- Removing a field from a response
- Changing the type of a field
- Changing the meaning of a field
- Removing an endpoint
- Changing authentication/authorization requirements

**Non-breaking changes (no version bump required)**:
- Adding a new optional field to a request
- Adding a new field to a response
- Adding a new endpoint
- Adding a new query parameter

### Version Lifecycle

| Phase | Duration | Description |
|-------|----------|-------------|
| Current | Active | Full support and development |
| Deprecated | 12 months | Bug fixes and security patches only. `Sunset` header included in responses. |
| Retired | N/A | Returns 410 Gone |

Include deprecation headers:
```
Sunset: Sat, 01 Jan 2028 00:00:00 GMT
Deprecation: true
Link: <https://api.talosix.com/v2/studies>; rel="successor-version"
```

## Authentication and Authorization

### Authentication

All API requests require a Bearer token in the `Authorization` header:

```
Authorization: Bearer <JWT or opaque token>
```

**Token Requirements**:
- Short-lived access tokens (15-30 minutes)
- Refresh tokens for session continuity (with rotation)
- Token must include: user ID, roles, study permissions, site permissions, token expiry
- Tokens must be validated on every request (no caching of authorization decisions for clinical data)

**Service-to-Service Authentication**:
- Use client credentials flow (OAuth 2.0)
- Each service has its own client ID and secret
- Scopes limit what each service can access

### Authorization

Implement multi-layered authorization:

1. **Role-Based Access Control (RBAC)**: User roles define broad capabilities (Data Entry, Monitor, Investigator, Admin).
2. **Study-Level Access**: Users are assigned to specific studies. Cannot access data from other studies.
3. **Site-Level Access**: Users are assigned to specific sites within a study. A site monitor can only see their assigned sites.
4. **Form-Level Access**: Certain CRF forms may be restricted by role (e.g., only investigators can view/sign certain forms).
5. **Field-Level Access**: Sensitive fields (e.g., unblinded treatment assignment) may have additional access restrictions.

Return `403 Forbidden` when the user is authenticated but not authorized. Include a machine-readable error code but do not reveal why access was denied (to prevent information leakage).

## Error Handling

### Error Response Format

All errors use a consistent JSON structure:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "One or more fields failed validation.",
    "request_id": "req_abc123def456",
    "details": [
      {
        "field": "subject.dateOfBirth",
        "code": "INVALID_FORMAT",
        "message": "Date must be in ISO 8601 format (YYYY-MM-DD)."
      },
      {
        "field": "subject.siteId",
        "code": "REFERENCE_NOT_FOUND",
        "message": "Site with the specified ID does not exist."
      }
    ]
  }
}
```

### Standard Error Codes

| HTTP Status | Error Code | Usage |
|-------------|-----------|-------|
| 400 | VALIDATION_FAILED | Request body fails validation |
| 400 | INVALID_PARAMETER | Query parameter is invalid |
| 401 | AUTHENTICATION_REQUIRED | Missing or expired token |
| 403 | ACCESS_DENIED | Insufficient permissions |
| 404 | RESOURCE_NOT_FOUND | Entity does not exist or is not visible to the user |
| 409 | CONFLICT | Optimistic locking conflict (concurrent edit) |
| 409 | STATE_CONFLICT | Invalid state transition (e.g., signing an already-locked form) |
| 410 | GONE | API version retired |
| 422 | EDIT_CHECK_FAILED | Clinical edit check violation |
| 429 | RATE_LIMITED | Too many requests |
| 500 | INTERNAL_ERROR | Unexpected server error (do not expose internals) |

**Important**: Never expose stack traces, database details, or PHI in error messages.

## Pagination

### Cursor-Based Pagination (preferred)

Use cursor-based pagination for large, frequently changing datasets (clinical data, audit trails):

```
GET /api/v1/studies/{studyId}/subjects?limit=25&cursor=eyJpZCI6MTAwfQ
```

Response:
```json
{
  "data": [...],
  "pagination": {
    "limit": 25,
    "has_more": true,
    "next_cursor": "eyJpZCI6MTI1fQ",
    "previous_cursor": "eyJpZCI6MTAwfQ"
  }
}
```

### Offset-Based Pagination (for simple use cases)

Use for small, stable datasets (reference data, study configuration):

```
GET /api/v1/countries?limit=25&offset=50
```

Response:
```json
{
  "data": [...],
  "pagination": {
    "limit": 25,
    "offset": 50,
    "total": 195
  }
}
```

### Pagination Defaults and Limits

- Default `limit`: 25
- Maximum `limit`: 100 (prevent excessive data retrieval)
- Always return pagination metadata in the response

## Filtering, Sorting, and Field Selection

### Filtering

Use query parameters for filtering:

```
GET /subjects?status=enrolled&siteId=site_001&enrolledAfter=2025-01-01
```

For complex filters, support a structured filter parameter:

```
GET /adverse-events?filter[severity]=serious&filter[status]=open&filter[reportedAfter]=2025-06-01
```

### Sorting

```
GET /subjects?sort=enrolledAt&order=desc
GET /audit-trails?sort=timestamp&order=desc
```

### Field Selection

Allow clients to request only needed fields (reduces payload size, minimizes PHI exposure):

```
GET /subjects?fields=id,subjectNumber,status,siteId
```

## Request and Response Conventions

### Request Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | Bearer token |
| Content-Type | Yes (for POST/PUT/PATCH) | application/json |
| Accept | Recommended | application/json |
| X-Request-ID | Recommended | Client-generated request ID for tracing |
| X-Reason-For-Change | Conditional | Required for clinical data updates (audit trail) |

### Response Headers

| Header | Description |
|--------|-------------|
| X-Request-ID | Echoed from request or server-generated |
| X-RateLimit-Limit | Rate limit ceiling |
| X-RateLimit-Remaining | Remaining requests in window |
| X-RateLimit-Reset | Time when rate limit resets (Unix timestamp) |
| ETag | Entity version for conditional requests |

### Timestamps

All timestamps in ISO 8601 format with UTC timezone:

```json
{
  "createdAt": "2025-11-15T14:30:00.000Z",
  "updatedAt": "2025-11-15T15:45:22.123Z"
}
```

### IDs

Use UUIDs for all resource identifiers. Never expose sequential database IDs.

```json
{
  "id": "f47ac10b-58cc-4372-a567-0e02b2c3d479"
}
```

## Audit Trail Integration

Every mutating API call must generate an audit trail entry. The API layer is responsible for capturing:

- **User Identity**: Extracted from the authentication token.
- **Action**: The operation performed (create, update, delete, sign, lock, etc.).
- **Resource**: The type and ID of the affected resource.
- **Changes**: For updates, the before and after values of changed fields.
- **Reason for Change**: From the `X-Reason-For-Change` header (required for clinical data edits per 21 CFR Part 11).
- **Timestamp**: Server-generated UTC timestamp.
- **IP Address**: Client IP for security auditing.

Audit trail entries are immutable. They cannot be modified or deleted through any API.

## OpenAPI Specification

Generate OpenAPI 3.0+ specifications for all endpoints.

Use Bash to validate specs:
```bash
# Validate OpenAPI spec
npx @redocly/cli lint openapi.yaml

# Generate spec from code annotations (if using code-first approach)
npx swagger-jsdoc -d swaggerDef.js -o openapi.yaml src/**/*.ts
```

### Spec Requirements

- Every endpoint must have a summary and description.
- Every request/response schema must be fully defined (no inline anonymous schemas).
- Include example values for all fields.
- Document all possible error responses.
- Include security scheme definitions.
- Tag endpoints by domain area (Studies, Subjects, CRFs, Queries, Admin).

### Schema Naming Conventions

```yaml
components:
  schemas:
    Subject:              # Domain entity
    SubjectCreateRequest: # Create request body
    SubjectUpdateRequest: # Update request body
    SubjectResponse:      # Single entity response
    SubjectListResponse:  # Collection response with pagination
```

## Rate Limiting

| Endpoint Category | Rate Limit | Window |
|-------------------|-----------|--------|
| Authentication | 10 requests | 1 minute |
| Clinical Data Read | 100 requests | 1 minute |
| Clinical Data Write | 30 requests | 1 minute |
| Data Export | 5 requests | 1 minute |
| Admin Operations | 20 requests | 1 minute |

Return `429 Too Many Requests` with `Retry-After` header when limits are exceeded.

## Health Check Endpoint

```
GET /health

Response (200):
{
  "status": "healthy",
  "version": "2.4.1",
  "timestamp": "2025-11-15T14:30:00.000Z"
}
```

This endpoint does not require authentication and is used for load balancer health checks and monitoring.
