# Auth Patterns

## Purpose
Defines general authentication rules for projects that require users, sessions, accounts, or protected resources.

## Use When
- Any project requires user authentication.
- Users need to sign in, sign out, or access protected data.
- Backend services need to identify the current user.
- Frontend apps need a predictable auth flow.

## Rules
- Keep auth as simple as the project allows.
- Do not build complex roles, permissions, or RBAC in v1 unless required from day one.
- Use secure session-based auth or token-based auth based on the selected project stack.
- Store sensitive tokens securely.
- Do not store sensitive tokens in `localStorage`.
- Use `httpOnly`, `secure`, and `sameSite` cookies when storing auth credentials in browser cookies.
- Hash passwords before storing them.
- Do not implement custom password hashing.
- Use a proven password hashing library or framework-supported hashing mechanism.
- Protect authenticated routes with a shared auth guard, middleware, hook, or framework equivalent.
- Auth failures must use the standard request error pattern.
- Auth response shapes must follow the standard API response pattern.
- Shared auth types should live in `packages/types` or another clearly named shared package.

## Patterns

### Session-based auth

Use when:
- The project is web-first.
- The frontend and backend are tightly paired.
- SSR or server-rendered flows are important.
- Simpler credential management is preferred.

General rules:
- Store session identifiers in secure cookies.
- Use a server-side session store when sessions need to survive restarts or scale across instances.
- Configure cookies with:
  - `httpOnly`
  - `secure`
  - `sameSite`

### Token-based auth

Use when:
- The project has SPAs, mobile apps, or external API clients.
- Multiple clients need to authenticate against the same backend.
- Stateless access tokens are useful.

General rules:
- Use short-lived access tokens.
- Use refresh tokens only when the project needs long-lived sessions.
- Store refresh tokens securely.
- Support refresh token revocation when needed.
- Avoid storing sensitive tokens in browser-accessible storage.

### Auth guard pattern

Every protected route should pass through one shared auth layer.

The auth layer should:
- verify the user or token
- attach the authenticated user/context to the request
- reject unauthenticated access with an auth error
- avoid duplicating auth checks in every handler

### Authorization pattern

Start simple:
- authenticated user
- resource owner checks
- basic admin checks only when required

Add advanced authorization only when the product needs it:
- roles
- permissions
- policies
- organizations/teams
- multi-tenant access control

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project uses a third-party auth provider.
- The project has no password-based accounts.
- The project requires enterprise auth such as SSO, SAML, or OAuth/OIDC from day one.
- The project requires complex permissions or multi-tenant access from day one.
- A platform or framework requires a different auth model.

## Anti-Patterns
- Storing sensitive tokens in `localStorage`.
- Rolling your own password hashing.
- Building full RBAC before the product needs it.
- Duplicating auth checks across handlers instead of using a shared auth layer.
- Returning inconsistent auth errors.
- Mixing session and token approaches without a clear reason.
- Skipping HTTPS in deployed environments.
- Hiding auth rules only in code instead of documenting them in `specs/`.

## Related Files
- backend/api-response-pattern.md
- backend/request-error-pattern.md
- backend/api-routing-pattern.md
- errors/error-code-pattern.md