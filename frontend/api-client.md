# API Client

## Purpose
Defines how frontend applications should communicate with backend services through centralized API clients and request layers.

## Use When
- A frontend app communicates with backend APIs.
- API requests need consistent handling.
- Authentication, headers, interceptors, retries, or response parsing need shared behavior.
- Frontend apps require predictable API architecture.

## Rules
- Frontend applications must communicate with backend services through centralized API clients.
- Do not scatter raw fetch or HTTP requests across random components.
- API request logic should live inside `api/`.
- API clients should handle:
  - base URLs
  - authentication headers
  - credentials
  - interceptors
  - request configuration
  - response normalization
- Components should not contain large request-building logic.
- API clients must follow the shared backend response patterns.
- API clients should work cleanly with the selected server-state solution.
- Shared API utilities should move into `packages/` only when reused across apps.
- API clients should remain framework-agnostic when possible.
- Important API architecture decisions must be documented in `specs/`.

## Standard Structure

```txt
src/
  api/
    client/
    services/
    adapters/
    hooks/
```

The exact structure may vary by framework or query library.

## Patterns

### Client Pattern

Use `api/client/` for:
- HTTP client setup
- base request configuration
- interceptors
- auth handling
- request defaults

Examples:

```txt
api/client/
  axios.ts
  fetch-client.ts
```

Client setup may handle:
- base URL
- credentials
- auth tokens
- request headers
- response interceptors
- error normalization

### Service Pattern

Use `api/services/` for endpoint-specific request functions.

Examples:

```txt
api/services/
  auth.service.ts
  user.service.ts
  project.service.ts
```

Service files should:
- group related endpoints
- keep endpoint logic centralized
- avoid UI rendering concerns

### Adapter Pattern

Use adapters when backend responses require transformation before frontend usage.

Examples:
- date normalization
- response mapping
- legacy API compatibility
- response flattening

Avoid scattering response transformations across components.

### Query Integration Pattern

API clients should integrate cleanly with:
- server-state tools
- query libraries
- caching systems

Examples:
- query hooks
- mutation hooks
- invalidation helpers

Avoid embedding API request logic directly into UI components.

### Authentication Pattern

API clients may handle:
- auth headers
- cookies
- token refresh
- session credentials
- auth interceptors

Authentication handling should remain centralized.

### Error Handling Pattern

API clients should normalize backend errors into predictable frontend-friendly structures.

Frontend components should not need to parse many backend error formats.

### Typed Response Pattern

Use shared types whenever practical.

Examples:

```ts
ApiResponse<UserRecord>
ApiResponse<ProjectRecord[]>
```

Shared types may live in:

```txt
packages/types/
```

## Request Flow Pattern

Use this flow:

```txt
component -> hook/query -> api service -> api client -> backend
```

Avoid:

```txt
component -> raw fetch
component -> random axios call
component -> backend response parsing
```

## AI-Friendly API Pattern

API architecture should make it easy for AI agents to:
- locate endpoints
- trace requests
- identify mutations
- identify caching boundaries
- identify auth handling
- identify response types

Avoid:
- hidden request utilities
- duplicated fetch logic
- request logic mixed into unrelated UI files

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework enforces a different API architecture.
- The project intentionally uses direct fetch patterns.
- A small prototype intentionally avoids abstraction.
- A third-party SDK replaces direct HTTP requests.
- A legacy frontend architecture already exists.

## Anti-Patterns
- Raw fetch calls scattered across components.
- Duplicated API request logic.
- Manually rebuilding auth headers everywhere.
- Parsing backend responses differently in every component.
- Embedding request logic directly inside large UI components.
- Mixing UI rendering and API orchestration together.
- Hiding API requests inside unrelated utility files.
- Storing API architecture decisions only in chat history instead of `specs/`.

## Related Files
- frontend/frontend.md
- frontend/frontend-architecture.md
- frontend/state-management.md
- frontend/authentication.md
- frontend/forms.md
- frontend/validation.md
- backend/api-response-pattern.md
- backend/request-error-pattern.md
