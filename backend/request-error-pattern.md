# Request Error Pattern

## Purpose
Defines the standard error handling model for backend services to ensure consistent, predictable API error responses across all languages and frameworks.

## Use When
- Any backend API needs structured error handling.
- Clients depend on consistent error codes and response shapes.
- Centralized error handling is required for maintainability and consistency.

## Rules
- Use a structured application error type for all expected errors.
- Backend handlers must throw or return structured errors instead of formatting responses directly.
- Error responses must be formatted in a centralized error handler (middleware, interceptor, or equivalent).
- Do not manually construct error responses in handlers.

- Every error must include:
  - `code` — a custom, application-level error code used to identify the problem.
  - `message` — a human-readable message.

- `code` must represent a stable, domain-level error (not tied to HTTP).
- `code` is used by:
  - frontend applications
  - logging and debugging
  - analytics and error tracking

- HTTP status codes must be used for protocol-level meaning only.
- Do not rely on HTTP status codes alone to identify errors.

- Error responses must follow the standard response shape defined in `api-response-pattern.md`.

- Unexpected errors must return:
  - `code: INTERNAL`
  - a generic message with no internal details

- Do not leak stack traces, database errors, or internal implementation details to clients.

- Use consistent HTTP status codes for each error type.
- Shared error types, enums, or schemas must live in `packages/types` or a shared package.
- Error handling must be consistent across all services in the project.

## Standard Error Shape

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message"
  }
}
```

## Error Codes (Suggested Defaults)

The following are commonly used application-level error codes:

```txt
VALIDATION_ERROR  — Invalid input from the client
UNAUTHORIZED      — Not authenticated
FORBIDDEN         — Authenticated but not permitted
NOT_FOUND         — Resource does not exist
CONFLICT          — State conflict (duplicate, stale data)
INTERNAL          — Unexpected server error
```

These codes are provided as a starting point and may be extended or customized per project.

### Guidelines

Error codes should be:
- stable over time
- consistent across services
- independent of HTTP status codes
- Error codes should represent domain-level meaning, not transport-level behavior.
- Prefer descriptive, explicit codes when needed (e.g., USER_NOT_FOUND, AUTH_INVALID_TOKEN).

### Notes

Projects may define their own error code system in a separate file.
More advanced or domain-specific error code structures should be documented outside this file.

## Status Code Mapping

HTTP status codes (protocol-level):

VALIDATION_ERROR → 400
UNAUTHORIZED     → 401
FORBIDDEN        → 403
NOT_FOUND        → 404
CONFLICT         → 409
INTERNAL         → 500

## Patterns

### Structured error type (generic)

- Define a reusable error type (class, struct, or schema depending on language).
- The error type must include:
  - code (application-level)
  - message
  - status code (HTTP)

### Handler usage

- Throw or return a structured error when:
  - validation fails
  - a resource is missing
  - access is denied
  - a conflict occurs

### Centralized error handler

- All errors must pass through a single error handling layer.
- The handler should:
  - detect structured errors
  - use the provided status code
  - return the application-level error code
  - format the response using the standard error shape
  - handle unexpected errors safely

### Implementation examples

- Node/Express:
  - Use error middleware.
- Python/FastAPI:
  - Use exception handlers.
- Other frameworks:
  - Use the framework’s centralized error handling mechanism.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A third-party API requires a different error format.
- A framework enforces a specific error handling model.
- A public API must remain backward compatible with an existing error format.
- Special endpoints (webhooks, streaming, file handling, health checks) require different behavior.

## Anti-Patterns
- Using HTTP status codes as the only way to identify errors.
- Using unstable or inconsistent error codes across services.
- Throwing raw errors instead of structured application errors.
- Returning error responses directly from handlers.
- Mixing error response formats across endpoints.
- Leaking stack traces or internal error details to clients.
- Using inconsistent HTTP status codes.
- Embedding business logic inside the error handling layer.
- Handling errors inside success response wrappers.

## Related Files
- backend/api-response-pattern.md
- backend/node/express.md
- errors/error-code-pattern.md
