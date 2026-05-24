# Middleware

## Purpose
Defines how middleware should be used in Express services.

## Use When
- The backend uses Express.
- Request processing needs reusable behavior before or after controllers.
- Cross-cutting concerns need to stay out of routes and controllers.

## Rules
- Use middleware for request pipeline concerns only.
- Keep middleware small, focused, and reusable.
- Middleware must not contain core business logic.
- Middleware must not directly perform database persistence unless it is part of a clearly defined infrastructure concern.
- Middleware should either:
  - attach context to the request
  - validate or transform request input
  - enforce access rules
  - pass control to the next handler
  - forward errors to centralized error middleware
- Use centralized error middleware for error response formatting.
- Do not format normal API responses inside middleware.
- Do not duplicate middleware logic across route files.
- Keep middleware order explicit in `app.ts`.

## Common Middleware Types

- auth middleware
- validation middleware
- logging middleware
- request parsing middleware
- rate limiting middleware
- permission middleware
- error middleware

## Patterns

### App-level middleware

Use in `app.ts` for global behavior.

```ts
app.use(express.json());
app.use(requestLogger);
```

### Route-level middleware

Use in route files for endpoint-specific behavior.

```ts
router.post('/records/create', requireAuth, validateCreateRecord, createRecord);
```

### Request context middleware

Use middleware to attach safe request context.

```ts
req.context = {
  requestId,
  user,
};
```

### Error forwarding

Async middleware must forward errors to the centralized error handler.

```ts
try {
  await nextStep();
} catch (error) {
  next(error);
}
```

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework wrapper provides its own middleware structure.
- A specific hosting platform requires a different request pipeline.
- A quick prototype intentionally uses simpler inline logic.

## Anti-Patterns
- Putting business logic inside middleware.
- Performing route-specific database queries inside generic middleware.
- Formatting success responses inside middleware.
- Swallowing errors instead of forwarding them.
- Registering middleware in unclear or hidden places.
- Creating giant middleware files that handle unrelated concerns.
- Duplicating auth, validation, or logging logic across routes.

## Related Files
- backend/node/express/express.md
- backend/node/express/routing.md
- backend/node/express/error-middleware.md
- backend/node/express/auth-middleware.md
- backend/request-error-pattern.md
