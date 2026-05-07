# Request Error Pattern

## Purpose
Standard error handling model for consistent, predictable API errors.

## Use When
- Any Express-based API in this playbook

## Rules
- Use a `RequestError` class for all expected application errors.
- Controllers throw `RequestError` — middleware catches and formats the response.
- Use standard error codes for all error types.
- Unexpected errors (uncaught exceptions) return `INTERNAL` with no leak of stack traces.

## Patterns

Error categories:
```
VALIDATION_ERROR  — Invalid input from the client
UNAUTHORIZED      — Not authenticated
FORBIDDEN         — Authenticated but not permitted
NOT_FOUND         — Resource does not exist
CONFLICT          — State conflict (duplicate, stale data)
INTERNAL          — Unexpected server error
```

`RequestError` class:
```ts
class RequestError extends Error {
  constructor(
    public code: ErrorCode,
    public message: string,
    public statusCode: number
  ) {
    super(message);
  }
}
```

Usage in a controller:
```ts
if (!user) throw new RequestError('NOT_FOUND', 'User not found.', 404);
```

Error middleware:
```ts
app.use((err, req, res, next) => {
  if (err instanceof RequestError) {
    return res.status(err.statusCode).json({
      success: false,
      error: { code: err.code, message: err.message }
    });
  }
  res.status(500).json({ success: false, error: { code: 'INTERNAL', message: 'Something went wrong.' } });
});
```

## Anti-Patterns
- Throwing raw `Error` objects with no structured code.
- Formatting error responses inside controllers instead of middleware.
- Leaking stack traces or internal error details to clients.
- Using HTTP status codes inconsistently.

## Related Files
- backend/express-sequelize.md
- backend/api-response-pattern.md
