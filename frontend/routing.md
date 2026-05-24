# Routing

## Purpose
Defines general frontend routing rules for user-facing applications, regardless of framework or router library.

## Use When
- A frontend app has multiple pages, views, screens, or layouts.
- Routes need predictable organization.
- URL structure affects navigation, state, SEO, SSR, or hydration.
- Framework-specific routing files need a shared foundation.

## Rules
- Use clear, predictable route structure.
- Keep route definitions organized in `routes/` when the framework allows it.
- Use `pages/` for route-level UI.
- Use `layouts/` for shared route shells and page structure.
- Routes should map clearly to pages, layouts, and user journeys.
- Use URL parameters for shareable page-level state when appropriate.
- Do not hide important navigation state only in memory when it should be linkable.
- Do not put business logic directly inside route definitions.
- Do not place API request logic directly inside route definitions unless the framework requires a loader pattern.
- Keep protected routes explicit and easy to find.
- Document important route structure decisions in `specs/`.
- Routing strategy must align with the selected rendering strategy:
  - SPA
  - SSR
  - SSG
  - hybrid rendering

## Standard Structure

```txt
apps/<app-name>/
  src/
    routes/
    pages/
    layouts/
    components/
      providers/
```

## Patterns

### Route Definition Pattern

Use `routes/` for route configuration when supported.

```txt
routes/
  public.routes.ts
  auth.routes.ts
  dashboard.routes.ts
```

Route files should:
- define paths
- connect paths to pages
- apply layout wrappers when needed
- apply route guards when needed

### Page Pattern

Use `pages/` for route-level screens.

```txt
pages/
  HomePage.tsx
  LoginPage.tsx
  DashboardPage.tsx
  SettingsPage.tsx
```

Pages should:
- compose layouts and components
- connect to state or data hooks
- avoid containing large reusable UI definitions

### Layout Pattern

Use `layouts/` for shared route structure.

```txt
layouts/
  PublicLayout.tsx
  AuthLayout.tsx
  DashboardLayout.tsx
```

Layouts may handle:
- navigation shells
- sidebars
- headers
- footers
- shared page structure

### Protected Route Pattern

Protected routes should use a clear auth guard, middleware, loader, or wrapper depending on the framework.

Examples:

```txt
/dashboard
/settings
/account
```

Protected route behavior should:
- check authentication status
- redirect unauthenticated users
- avoid duplicating auth checks on every page

### URL State Pattern

Use URL state for values that should be:
- shareable
- bookmarkable
- reload-safe
- visible across navigation

Examples:
- filters
- tabs
- search queries
- pagination
- selected records when appropriate

Example:

```txt
/projects?page=2&status=active
```

Avoid storing important shareable state only in local component state.

### Route Naming Pattern

Use route paths that are clear and user-focused.

Good:

```txt
/dashboard
/projects
/projects/detail/:id
/settings/profile
/settings/security
```

Avoid:

```txt
/page1
/projectstuff
/userthing
/temp-dashboard
```

### SSR Routing Pattern

When SSR is selected:
- routes must be renderable on the server
- route data requirements must be clear
- route-level redirects should work server-side when needed
- hydration behavior must be considered

### SPA Routing Pattern

When SPA is selected:
- client-side navigation should be predictable
- refresh behavior must be supported by hosting configuration
- protected routes must handle loading and redirect states cleanly

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework enforces file-based routing.
- A mobile or desktop app uses navigation instead of URL routing.
- A small prototype intentionally uses inline route definitions.
- A legacy routing structure already exists.
- The app is a single-page landing page with no meaningful internal routes.

## Anti-Patterns
- Hiding route definitions across unrelated files.
- Putting business logic inside route definitions.
- Mixing page-level UI and reusable components without separation.
- Storing shareable navigation state only in memory.
- Creating unclear paths such as `/stuff`, `/misc`, or `/temp`.
- Duplicating auth route guard logic across many pages.
- Ignoring SSR or hydration behavior when the app requires SSR.
- Letting route structure drift away from page and layout organization.
- Storing route decisions only in chat history instead of `specs/`.

## Related Files
- frontend/frontend.md
- frontend/frontend-architecture.md
- frontend/state-management.md
- frontend/api-client.md
- frontend/authentication.md
- frontend/seo.md
- frontend/ssr.md
- frontend/hydration.md
