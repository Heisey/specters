# Response Helper

## Purpose
Defines how standardized API responses should be implemented in Express services.

## Use When
- The backend uses Express.
- Controllers need consistent response formatting.
- The project uses the shared API response pattern.

## Rules
- Use a shared response helper, utility, or class for all successful API responses.
- Do not manually construct response objects inside controllers.
- Response helpers must follow the structure defined in `backend/api-response-pattern.md`.
- Response helpers should support:
  - records
  - metadata
  - status codes
- Controllers should only provide data and metadata to the helper.
- Response formatting must remain centralized and consistent across the service.
- Shared response types should come from `packages/types` or another clearly named shared package.
- Response helpers must not contain business logic.
- Response helpers must not perform database operations.

## Patterns

### Response Helper Structure

A response helper may be implemented as:
- a class
- a utility function
- a response builder
- a framework adapter

The implementation style may vary per project.

### Class-Based Pattern

```ts
return new ServerResponse(records).send(res);
```

### Created Response Pattern

```ts
return new ServerResponse(record, 201).send(res);
```

### Paginated Response Pattern

```ts
return new ServerResponse(records, 200, {
  total,
  page,
  pageSize,
}).send(res);
```

### Utility Function Pattern

```ts
return sendResponse(res, {
  records,
  meta,
});
```

### Metadata Pattern

Collection responses should include metadata:

```ts
meta: {
  count,
  total,
  page,
  pageSize,
}
```

## Response Helper Responsibilities

Response helpers should:
- standardize API responses
- reduce repeated response formatting
- centralize metadata handling
- simplify controller logic
- enforce consistent response structure

Response helpers should not:
- contain business logic
- perform validation
- access databases
- handle authentication
- handle application errors

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework enforces a different response system.
- A third-party API requires a different response shape.
- A streaming response or file response should bypass the standard wrapper.
- A lightweight prototype intentionally skips response helpers.

## Anti-Patterns
- Building response objects manually inside every controller.
- Returning inconsistent response structures.
- Mixing helper-based and manual response formatting.
- Embedding business logic inside response helpers.
- Performing database queries inside response helpers.
- Returning raw arrays or objects directly from controllers.

## Related Files
- backend/api-response-pattern.md
- backend/request-error-pattern.md
- backend/node/express/express.md
