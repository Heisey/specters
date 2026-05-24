# Auth Middleware

## Purpose
Defines how authentication and authorization middleware should be implemented in Express services.

## Use When
- The backend uses Express.
- Routes require authenticated access.
- User identity or permissions must be verified before reaching controllers.
- The project uses the shared auth patterns and request error patterns.

## Rules
- Use shared auth middleware for protected routes.
- Auth middleware must remain focused on authentication and authorization concerns only.
- Auth middleware must not contain business logic.
- Auth middleware must not manually format API responses.
- Auth middleware must forward errors to centralized error middleware.
- Auth middleware should attach authenticated user context to the request.
- Auth middleware must use the shared request error pattern for auth failures.
- Auth middleware should remain reusable across routes and services.
- Route files should apply auth middleware instead of duplicating auth checks inside controllers.
- Shared auth types should come from `packages/types` or another clearly named shared package.

## Patterns

### Protected Route Pattern

```ts
router.get('/users/detail/:id', requireAuth, getUserDetail);
```

### Role or Permission Pattern

```ts
router.post(
  '/admin/users/create',
  requireAuth,
  requirePermission('ADMIN'),
  createAdminUser
);
```

### Auth Context Pattern

Auth middleware should attach safe auth context to the request.

```ts
req.auth = {
  userId,
  roles,
  permissions,
};
```

### Token Verification Pattern

Auth middleware may:
- verify session state
- verify access tokens
- verify API keys
- verify permissions or roles

The implementation depends on the selected auth system.

### Unauthorized Pattern

Authentication failures should throw or forward structured errors.

```ts
throw new RequestError(
  'UNAUTHORIZED',
  'Authentication required.',
  401
);
```

### Forbidden Pattern

Authorization failures should throw or forward structured errors.

```ts
throw new RequestError(
  'FORBIDDEN',
  'You do not have permission to access this resource.',
  403
);
```

## Responsibilities

Auth middleware should:
- verify identity
- verify access permissions
- attach auth context to the request
- block unauthorized access
- forward auth failures to centralized error middleware

Auth middleware should not:
- contain business logic
- query unrelated domain data
- manually format API responses
- perform response formatting
- duplicate authorization logic across routes

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project uses a third-party auth provider.
- The framework provides its own auth pipeline.
- The service uses API keys, machine auth, or custom gateway auth.
- The project intentionally handles auth differently for specific routes or services.

## Anti-Patterns
- Duplicating auth checks inside controllers.
- Performing business logic inside auth middleware.
- Manually formatting auth error responses.
- Attaching unsafe or sensitive data directly to the request object.
- Mixing authentication and unrelated validation logic in the same middleware.
- Creating route-specific auth middleware that cannot be reused.
- Bypassing centralized error middleware for auth failures.

## Related Files
- backend/auth-patterns.md
- backend/request-error-pattern.md
- backend/node/express/express.md
- backend/node/express/middleware.md
- backend/node/express/error-middleware.md
