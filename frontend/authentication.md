# Authentication

## Purpose
Defines frontend authentication architecture, patterns, and responsibilities for user-facing applications.

## Use When
- A frontend app supports user authentication.
- Routes require authenticated access.
- Authentication state must persist across navigation or sessions.
- Frontend apps communicate with protected backend APIs.

## Rules
- Keep authentication architecture centralized and predictable.
- Frontend authentication must align with backend authentication patterns.
- Do not scatter auth checks across unrelated components.
- Use centralized auth providers, hooks, or stores.
- Protected route behavior should be explicit and easy to locate.
- Authentication state should remain separate from unrelated UI state.
- Do not store sensitive authentication tokens insecurely.
- Prefer secure cookies or other secure backend-driven auth strategies when possible.
- Avoid storing sensitive tokens in localStorage unless intentionally required and documented.
- Frontend auth systems should support:
  - loading states
  - refresh handling
  - session persistence
  - logout flows
  - protected routes
- API clients should integrate cleanly with authentication handling.
- Important authentication decisions must be documented in `specs/`.

## Standard Structure

```txt
src/
  components/
    providers/

  hooks/
  stores/

  api/
    services/

  routes/
```

## Patterns

### Auth Provider Pattern

Use providers for app-level authentication context.

Examples:

```txt
components/providers/
  AuthProvider.tsx
```

Auth providers may handle:
- auth initialization
- current user loading
- auth refresh handling
- auth context exposure
- auth loading states

### Auth Hook Pattern

Use hooks for consuming auth state and behavior.

Examples:

```txt
hooks/
  useAuth.ts
```

Auth hooks may expose:
- current user
- auth status
- loading state
- login actions
- logout actions
- permission checks

### Protected Route Pattern

Protected routes should:
- verify authentication status
- handle loading states safely
- redirect unauthenticated users when needed
- avoid duplicated auth logic across pages

Examples:

```txt
/dashboard
/settings
/account
```

### Session Persistence Pattern

Authentication should survive:
- refreshes
- navigation
- hydration
- SSR transitions when applicable

Session persistence strategy depends on:
- cookies
- sessions
- tokens
- backend auth implementation

### API Client Integration Pattern

API clients may handle:
- auth headers
- credentials
- session cookies
- token refresh
- auth interceptors

Authentication behavior should remain centralized.

### Permission Pattern

Authorization checks should be:
- centralized
- reusable
- predictable

Examples:
- role checks
- permission checks
- feature access checks

Avoid hardcoding permissions directly across many components.

### Logout Pattern

Logout should:
- clear frontend auth state
- clear cached user data when needed
- invalidate backend sessions or tokens
- redirect safely when appropriate

## SSR Authentication Pattern

When SSR is used:
- authentication state must support server rendering
- protected SSR routes should handle redirects safely
- hydration must not create auth mismatches
- auth loading states should remain predictable

## AI-Friendly Authentication Pattern

Authentication architecture should make it easy for AI agents to:
- locate auth ownership
- locate auth state
- trace protected routes
- identify auth flows
- identify permission checks
- identify session persistence behavior

Avoid:
- hidden auth utilities
- duplicated permission checks
- auth logic scattered across components

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A third-party auth provider controls the frontend flow.
- A framework enforces a different auth structure.
- The app intentionally uses token-only auth storage.
- A mobile or desktop app requires platform-specific auth handling.
- A legacy auth architecture already exists.

## Anti-Patterns
- Scattering auth checks across random components.
- Storing sensitive auth tokens insecurely.
- Duplicating permission logic across pages.
- Mixing auth state with unrelated UI state.
- Embedding login/logout workflows directly into unrelated components.
- Hiding auth state ownership.
- Rebuilding auth request logic across multiple API files.
- Ignoring SSR hydration mismatches for auth-sensitive pages.
- Storing auth architecture decisions only in chat history instead of `specs/`.

## Related Files
- frontend/frontend.md
- frontend/frontend-architecture.md
- frontend/routing.md
- frontend/state-management.md
- frontend/api-client.md
- frontend/ssr.md
- frontend/hydration.md
- backend/auth-patterns.md
