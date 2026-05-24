# State Management

## Purpose
Defines how frontend state should be structured, separated, and managed across frontend applications.

## Use When
- A frontend app manages UI state, server state, forms, authentication state, or feature state.
- Multiple state management tools exist in the project.
- State ownership needs clear boundaries.
- Frontend architecture needs predictable state flow.

## Rules
- Separate state by responsibility.
- Do not use one global state solution for every type of state.
- Server state and UI state should remain separate.
- Page-scoped state should remain separate from global app state when possible.
- Local component state should stay local unless it truly needs sharing.
- Shared state tools must be intentionally selected in `specs/PROJECT_CONTEXT.md`.
- State ownership should be obvious and easy to locate.
- State should be organized so AI agents can safely identify:
  - where data comes from
  - who owns the data
  - who updates the data
  - who consumes the data
- Avoid unnecessary global state.
- Avoid duplicating the same state across multiple stores.
- Use URL state for shareable navigation-related state when appropriate.
- Avoid mixing server fetching logic into unrelated UI state stores.
- Important state architecture decisions must be documented in `specs/`.

## Core State Separation Principle

Different kinds of state have different responsibilities.

Do not treat all state the same.

## State Categories

### Server State

Server state is:
- API data
- async backend data
- cached query data
- paginated data
- synchronized remote state

Examples:
- user profile data
- dashboard API data
- project lists
- search results

Server state should usually:
- come from API clients
- support caching
- support invalidation
- support refetching
- support loading/error states

### Global UI State

Global UI state is:
- frontend-only shared UI behavior
- application-wide interface state

Examples:
- sidebar open/closed
- theme
- modal visibility
- notifications
- UI preferences

Global UI state should:
- remain lightweight
- avoid storing server records unnecessarily
- avoid duplicating API state

### Page-Scoped State

Page-scoped state belongs to:
- one page
- one route section
- one workflow

Examples:
- selected dashboard filters
- wizard progress
- local search state
- temporary editing state

Page-scoped state should:
- stay close to the owning page
- avoid unnecessary globalization
- move into URL state when sharing/bookmarking matters

### Local Component State

Local component state belongs only to the component that owns it.

Examples:
- dropdown open state
- tab selection
- input focus
- animation state

Prefer local component state whenever broader sharing is unnecessary.

## Standard Structure

```txt
src/
  stores/
  hooks/
  api/
  pages/
  components/
```

## Patterns

### Separation Pattern

Use different tools or layers for different state concerns.

Example separation:

```txt
server state -> query/cache layer
global UI state -> UI store
page state -> page-level store or hooks
local state -> component state
URL state -> router/query params
```

### Server State Pattern

Server state should:
- live near API/query layers
- support caching
- support invalidation
- support background refetching

Avoid storing server state permanently inside UI stores unless intentionally required.

### UI Store Pattern

Global UI stores should contain:
- UI preferences
- UI visibility state
- app-level UI coordination

Avoid:
- giant catch-all stores
- storing unrelated concerns together
- duplicating server data

### Page Store Pattern

Page stores should:
- remain scoped to the route or workflow
- avoid leaking temporary state globally
- be easy to remove or reset

### URL State Pattern

Use URL state for:
- filters
- sorting
- tabs
- pagination
- searchable views
- navigation-related state

Example:

```txt
/projects?page=2&status=active
```

### Hook Composition Pattern

Use hooks to compose state behavior.

Examples:
- useAuth
- useProjectFilters
- useModalState
- useDashboardData

Hooks should:
- remain focused
- avoid giant all-purpose hooks
- avoid hidden side effects when possible

## AI-Friendly State Pattern

State architecture should make it obvious:
- which layer owns the state
- whether the state is server or UI state
- how state changes propagate
- where mutations occur
- where API synchronization occurs

Avoid hidden state mutations and unclear ownership.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework enforces a specific state architecture.
- The project intentionally centralizes all state.
- The app is small enough to use minimal state structure.
- A mobile or desktop platform requires different state patterns.
- A legacy architecture already exists.

## Anti-Patterns
- One giant global store for everything.
- Mixing server state and UI state together.
- Duplicating API data across multiple stores.
- Storing temporary page state globally without reason.
- Hiding important state inside random utility files.
- Creating giant hooks that manage unrelated concerns.
- Using global state when local component state is enough.
- Treating URL state and app state as unrelated systems.
- Spreading fetch logic across random components.
- Storing state architecture decisions only in chat history instead of `specs/`.

## Related Files
- frontend/frontend.md
- frontend/frontend-architecture.md
- frontend/routing.md
- frontend/api-client.md
- frontend/forms.md
- frontend/authentication.md
- frontend/ssr.md
- frontend/hydration.md
