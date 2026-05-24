# Frontend

## Purpose
Defines the general frontend rules and principles that apply across user-facing applications, regardless of framework or rendering strategy.

## Use When
- A project includes a user-facing application.
- A frontend app needs consistent structure, state management, API usage, routing, styling, accessibility, or rendering rules.
- Framework-specific frontend files need a shared foundation.

## Rules
- Use `apps/` for user-facing applications.
- Keep frontend applications focused on user interaction, presentation, routing, and client-side behavior.
- Do not place backend business logic inside frontend apps.
- Do not directly access databases from frontend apps.
- Use shared packages for reusable types, utilities, API clients, and UI components.
- Keep app-specific code inside the app that owns it.
- Move shared frontend code into `packages/` only when it is reused across multiple apps.
- Use predictable folder structure inside frontend apps.
- Keep routing, state management, API access, styling, forms, validation, and authentication organized by clear responsibility.
- Prefer explicit boundaries between UI, state, API calls, and business behavior.
- Store important frontend architecture decisions in `specs/`.
- Follow project-specific frontend selections defined in `specs/PROJECT_CONTEXT.md`.

## Core Principles

Frontend code should be:
- predictable
- component-driven
- easy to navigate
- easy for AI agents to modify safely
- separated by responsibility
- aligned with the selected rendering strategy

The frontend should not become a dumping ground for:
- backend logic
- database logic
- unrelated utilities
- duplicated shared code
- framework-specific hacks without documentation

## Standard Frontend Concerns

Frontend projects should define clear rules for:

- application structure
- routing
- state management
- API clients
- authentication
- forms
- validation
- styling
- component architecture
- accessibility
- SEO
- SSR
- hydration

Use the related frontend files for detailed guidance.

## Patterns

### App Boundary Pattern

Each frontend app owns its own app-specific implementation.

```txt
apps/
  web/
  admin/
  mobile/
```

Apps should not import directly from another app's internal files.

Shared code should move into:

```txt
packages/
  types/
  utils/
  ui/
  api-client/
```

### Responsibility Pattern

Separate frontend responsibilities clearly.

```txt
routing -> pages/layouts
server state -> API client + server-state tool
global UI state -> UI store
local component state -> component/context
forms -> form layer
validation -> validation layer
styling -> styling system
```

### Shared Package Pattern

Use shared packages only when code is reused.

Good shared package candidates:
- shared types
- API client utilities
- reusable UI components
- formatting helpers
- validation schemas used by multiple apps

Avoid moving code into shared packages too early.

### API Boundary Pattern

Frontend apps should communicate with backend services through API clients.

Avoid:
- hardcoded fetch calls scattered across components
- backend implementation knowledge inside UI components
- direct database access
- duplicated API response parsing

### Rendering Strategy Pattern

Frontend rendering strategy must be intentional.

Supported strategies may include:
- SPA
- SSR
- SSG
- hybrid rendering

Rendering strategy should be documented in `specs/PROJECT_CONTEXT.md`.

### AI-Friendly Frontend Pattern

Frontend apps should be organized so an AI agent can safely locate:
- routes
- pages
- components
- state
- API calls
- styles
- forms
- validation
- tests

Avoid hiding important behavior in unclear folders or large mixed-purpose files.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project has no frontend.
- A framework requires a different structure.
- A small prototype intentionally uses a simplified structure.
- A platform such as mobile, desktop, or embedded UI requires different conventions.
- A legacy frontend structure already exists.

## Anti-Patterns
- Putting backend business logic in frontend apps.
- Accessing databases directly from frontend code.
- Importing internals from one app into another app.
- Duplicating shared utilities across apps instead of moving them into `packages/`.
- Moving code into shared packages before it is actually shared.
- Mixing API calls directly into deeply nested UI components without a clear pattern.
- Using one global state tool for every kind of state.
- Ignoring accessibility until the end of development.
- Treating SSR, SEO, and hydration as afterthoughts when the project needs indexable pages.
- Storing frontend decisions only in chat history instead of `specs/`.

## Related Files
- frontend/frontend-architecture.md
- frontend/routing.md
- frontend/state-management.md
- frontend/api-client.md
- frontend/authentication.md
- frontend/forms.md
- frontend/validation.md
- frontend/styling.md
- frontend/component-architecture.md
- frontend/accessibility.md
- frontend/seo.md
- frontend/ssr.md
- frontend/hydration.md
- architecture/monorepo.md
