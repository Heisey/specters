# API Response Pattern

## Purpose
Defines the standard API response shape for successful responses across all backend services, regardless of language or framework.

## Use When
- Any backend API returns data to a client.
- Clients require predictable response shapes for state management, caching, or shared types.
- Consistency across services and projects is required.

## Rules
- Use a consistent response wrapper for all successful API responses.
- Do not return raw records, raw objects, or raw arrays directly from handlers.
- Use `records` as the response data field.
- Use `meta` for response metadata.
- Use `success: true` for all successful responses.
- Collections must include `meta.count`.
- Paginated collections must include `meta.total`, `meta.page`, and `meta.pageSize`.
- Single-record responses must include `meta.count: 1` when a record exists.
- Empty single-record responses should be handled as errors (e.g., `NOT_FOUND`) instead of returning `records: null`.
- Error responses must follow the standard error pattern defined in `request-error-pattern.md`.
- Shared response types must live in `packages/types` or a clearly defined shared package.
- Backend handlers must use a shared response helper, utility, or abstraction appropriate to the selected backend stack.
- Do not manually construct response objects in every handler.

## Standard Shape

```ts
{
  success: true;
  records: T;
  meta: {
    count: number;
    total?: number;
    page?: number;
    pageSize?: number;
  };
}
```

## Patterns

### Single record response

```json
{
  "success": true,
  "records": { "id": 1, "name": "Acme" },
  "meta": { "count": 1 }
}
```

### Collection response

```json
{
  "success": true,
  "records": [{ "id": 1 }, { "id": 2 }],
  "meta": { "count": 2 }
}
```

### Paginated collection response

```json
{
  "success": true,
  "records": [{ "id": 1 }, { "id": 2 }],
  "meta": {
    "count": 2,
    "total": 50,
    "page": 1,
    "pageSize": 20
  }
}
```

### Implementation pattern (generic)

- Use a shared response abstraction (class, helper, or function).
- The abstraction should:
  - accept records
  - accept optional metadata
  - handle status codes
  - return a properly formatted response

### Implementation examples

- Node/Express:
  - Use a `ServerResponse` class or helper.
- Python/FastAPI:
  - Use a response builder function or schema model.
- Other frameworks:
  - Implement a consistent wrapper aligned with this structure.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A third-party API contract requires a different response shape.
- A framework enforces a specific response format.
- A public API must remain backward compatible with an existing response shape.
- A streaming response, file download, webhook, or health check endpoint should not use the standard wrapper.

## Anti-Patterns
- Returning raw arrays at the top level.
- Returning raw objects without a wrapper.
- Mixing response field names such as `data`, `records`, `result`, or `items`.
- Manually rebuilding the same response shape in every handler.
- Returning `success: true` for failed operations.
- Embedding error data inside success responses.
- Omitting metadata from collection responses.
- Using inconsistent pagination field names across endpoints.

## Related Files
- backend/request-error-pattern.md
- backend/api-routing-pattern.md
- backend/node/express.md
