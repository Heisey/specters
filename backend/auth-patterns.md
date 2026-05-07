# Auth Patterns

## Purpose
Authentication guidance for projects at different stages.

## Use When
- Any project that requires user authentication

## Rules
- Keep auth simple for MVPs — do not overbuild roles and permissions early.
- Use secure session handling or token-based auth depending on project needs.
- Store tokens securely — httpOnly cookies for web apps, never localStorage for sensitive tokens.
- Hash passwords with bcrypt before storing.
- Do not build complex permission systems in v1 unless the product requires it from day one.

## Patterns

Session-based (simpler, good for SSR):
- Use `express-session` with a secure store (Redis or DB-backed).
- Set `httpOnly`, `secure`, and `sameSite` on session cookies.

Token-based (good for SPAs and APIs):
- Use JWT signed with a secret or RS256.
- Short-lived access tokens + refresh token rotation.
- Store refresh tokens server-side for revocation support.

Auth middleware:
```ts
function requireAuth(req, res, next) {
  if (!req.user) throw new RequestError('UNAUTHORIZED', 'Authentication required.', 401);
  next();
}
```

## Anti-Patterns
- Storing JWT in localStorage (XSS risk).
- Rolling your own password hashing.
- Building a full RBAC system before the product needs it.
- Skipping HTTPS in any deployed environment.

## Related Files
- backend/express-sequelize.md
- backend/request-error-pattern.md
