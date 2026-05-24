# Error Middleware

## Purpose
Defines how centralized error handling should be implemented in Express services.

## Use When
- The backend uses Express.
- The project uses centralized request error handling.
- Controllers and middleware need consistent error formatting.

## Rules
- Use centralized error middleware for all API error responses.
- Controllers and middleware must forward errors to the centralized error middleware.
- Do not manually format error responses inside controllers.
- Error middleware must follow the structure defined in `backend/request-error-pattern.md`.
- Error middleware must handle:
  - structured application errors
  - unexpected runtime errors
- Unexpected errors must return:
  - a safe generic message
  - no internal implementation details
- Error middleware must not contain business logic.
- Error middleware must not perform database operations.
- Error middleware must be registered after routes and other middleware in `app.ts`.
- Shared error types should come from `packages/types` or another clearly named shared package.

## Patterns

### Middleware Registration

Register error middleware after all routes:

```ts
app.use(errorMiddleware);
```

### Structured Error Handling

Structured application errors should:
- provide a status code
- provide an application-level error code
- provide a human-readable message

### Example Error Middleware

```ts
export function errorMiddleware(err, req, res, next) {
  if (err instanceof RequestError) {
    return res.status(err.statusCode).json({
      success: false,
      error: {
        code: err.code,
        message: err.message,
      },
    });
  }

  return res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL',
      message: 'Something went wrong.',
    },
  });
}
```

### Async Error Forwarding

Async controllers and middleware should forward errors:

```ts
try {
  await performAction();
} catch (error) {
  next(error);
}
```

Or use an async wrapper helper.

### Logging Pattern

Unexpected errors may be logged before returning the response.

Logging should:
- avoid leaking secrets
- avoid exposing sensitive data
- include enough information for debugging

## Responsibilities

Error middleware should:
- standardize error responses
- centralize error formatting
- map application errors to HTTP responses
- safely handle unexpected runtime failures

Error middleware should not:
- contain business logic
- perform validation
- authenticate users
- query databases
- mutate application state

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework provides a different centralized error system.
- A third-party API contract requires a different error format.
- The service is not HTTP-based.
- A lightweight prototype intentionally skips centralized error middleware.

## Anti-Patterns
- Formatting error responses inside controllers.
- Catching errors and hiding them silently.
- Returning stack traces to clients.
- Mixing multiple error response formats.
- Embedding business logic inside error middleware.
- Performing database queries inside error middleware.
- Registering error middleware before routes.

## Related Files
- backend/request-error-pattern.md
- backend/api-response-pattern.md
- backend/node/express/express.md
- backend/node/express/middleware.md
