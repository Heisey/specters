# Frontend Architecture

## Purpose
Defines how frontend applications should be structured and organized for scalability, maintainability, SSR support, and AI-assisted development.

## Use When
- A project contains one or more frontend applications.
- Frontend apps need predictable structure and boundaries.
- Teams or AI agents need to quickly understand where frontend logic belongs.
- Frontend concerns need separation between routing, UI, state, API access, and rendering.

## Rules
- Frontend apps must have a predictable folder structure.
- Keep routing, components, state, API access, styling, forms, and validation separated by responsibility.
- Use clear boundaries between:
  - UI rendering
  - business workflows
  - server state
  - local state
  - API communication
- App-specific code should remain inside the owning app.
- Shared frontend code should move into `packages/` only when reused.
- Avoid giant mixed-purpose folders.
- Avoid unclear folder names such as:
  - misc
  - helpers
  - common
  - temp
- Use architecture that supports:
  - SSR
  - hydration
  - SEO
  - reusable components
  - AI-assisted development
- Frontend structure should make it easy to locate:
  - routes
  - layouts
  - pages
  - components
  - stores
  - hooks
  - styles
  - API calls
- Providers should live under `components/providers/`, not as a separate root-level folder.
- Important frontend architecture decisions must be documented in `specs/`.

## Standard Frontend Structure

```txt
apps/<app-name>/
  src/

    pages/
    layouts/
    routes/

    components/
      base/
      custom/
      providers/
      templates/
      hoc/

    features/

    stores/
    hooks/

    api/

    styles/
    assets/

    forms/
    validation/

    utils/
    types/
```

The exact structure may vary per framework, but responsibilities should remain consistent.

## Patterns

### pages/

Use `pages/` for:
- route-level UI
- screen-level composition
- page-specific orchestration

Pages should:
- compose components
- call hooks or stores
- avoid large reusable UI definitions

### layouts/

Use `layouts/` for:
- shared page structure
- navigation shells
- app wrappers
- SSR layout composition

Examples:
- dashboard layout
- auth layout
- public site layout

### routes/

Use `routes/` for:
- route definitions
- route configuration
- route registration
- route grouping

Routing strategy should follow:
- `frontend/routing.md`
- `frontend/ssr.md`
- `frontend/hydration.md`

### components/

Use `components/` as the main UI and composition folder.

Components should be organized by responsibility:

```txt
components/
  base/
  custom/
  providers/
  templates/
  hoc/
```

### components/base/

Use `components/base/` for foundational reusable UI components.

Examples:
- Button
- Input
- Select
- Modal
- Card
- Container

Base components should:
- be reusable
- be predictable
- avoid business logic
- avoid page-specific behavior

### components/custom/

Use `components/custom/` for app-specific composed components.

Examples:
- UserProfileCard
- DashboardStatsPanel
- BillingSummary
- ProjectSwitcher

Custom components may combine base components, hooks, and app-specific UI behavior.

### components/providers/

Use `components/providers/` for provider components and app-level wrappers.

Examples:
- AuthProvider
- QueryProvider
- ThemeProvider
- AppProviders
- HydrationProvider

Providers should:
- wrap application or layout trees
- keep provider setup centralized
- avoid unrelated UI rendering
- avoid business workflows when possible

### components/templates/

Use `components/templates/` for larger reusable page or section compositions.

Examples:
- DashboardTemplate
- AuthPageTemplate
- MarketingPageTemplate
- SettingsPageTemplate

Templates should:
- define reusable structure
- compose base and custom components
- avoid owning unrelated business logic

### components/hoc/

Use `components/hoc/` for higher-order components and wrapper utilities.

Examples:
- withAuth
- withLayout
- withPermission

HOCs should:
- remain focused
- avoid hidden side effects
- not replace clear routing or provider patterns when those are more appropriate

### features/

Use `features/` for:
- feature-scoped frontend logic
- grouped feature behavior
- complex workflows

Examples:
- auth feature
- checkout feature
- profile feature

### stores/

Use `stores/` for:
- global UI state
- feature state
- app-level frontend state

Do not place server data fetching directly inside UI state stores unless intentionally designed.

### hooks/

Use `hooks/` for:
- reusable frontend logic
- frontend orchestration helpers
- UI behavior composition

Hooks should:
- remain focused
- avoid giant all-purpose hooks
- avoid hidden side effects when possible

### api/

Use `api/` for:
- API clients
- API request functions
- API adapters
- server communication

Frontend apps should not scatter fetch logic across random components.

### styles/

Use `styles/` for:
- global styles
- theme configuration
- styling system setup

Prefer component-scoped styling whenever possible.

### forms/

Use `forms/` for:
- reusable form logic
- form components
- form orchestration

### validation/

Use `validation/` for:
- validation schemas
- frontend validation helpers
- shared validation rules

### utils/

Use `utils/` carefully.

Only place code here that:
- does not belong to another clear responsibility
- remains small and reusable
- is not business logic

Avoid turning `utils/` into a dumping ground.

## Shared Package Pattern

Move frontend code into `packages/` only when:
- reused across multiple apps
- intentionally shared
- stable enough to centralize

Good shared package candidates:

```txt
packages/
  ui/
  types/
  api-client/
  validation/
```

Avoid premature sharing.

## AI-Friendly Structure Pattern

Frontend structure should help AI agents safely:
- locate features
- modify pages
- trace API calls
- update components
- understand routing
- identify state ownership
- find providers and app wrappers
- distinguish base UI from custom UI

Avoid:
- giant files
- unclear folder names
- mixed responsibilities
- deeply hidden logic

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework enforces a different structure.
- The project intentionally uses feature-first architecture.
- The project is a small prototype.
- A mobile or desktop framework uses different conventions.
- A legacy structure already exists.

## Anti-Patterns
- Giant `components/` folders with no organization.
- Placing providers in a disconnected root-level folder when the project standard expects `components/providers/`.
- Mixing routing, API calls, and UI in the same files.
- Putting feature-specific logic into shared packages too early.
- Creating unclear folders such as `misc/` or `helpers/`.
- Scattering API calls across random components.
- Hiding state ownership across multiple unrelated stores.
- Duplicating reusable frontend logic across apps.
- Treating `utils/` as a dumping ground.
- Storing architecture decisions only in chat history instead of `specs/`.

## Related Files
- frontend/frontend.md
- frontend/routing.md
- frontend/state-management.md
- frontend/api-client.md
- frontend/component-architecture.md
- frontend/styling.md
- frontend/ssr.md
- frontend/hydration.md
- architecture/monorepo.md
