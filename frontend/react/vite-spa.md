# Vite React SPA

## Purpose
Rules for projects using Vite with React and TypeScript as a single-page application.

## Use When
- Building an internal dashboard or admin tool
- SEO is not a primary concern
- Fast iteration is more important than SSR

## Rules
- Use Vite with React and TypeScript.
- Use React Router for client-side routing.
- Use React Query for all server state.
- Use Zustand for local/global UI state.
- Keep route-level page components thin — delegate to child components.
- Move reusable UI into `components/`.
- Use path aliases (`@/`) for clean imports.

## Patterns
- Pages live in `pages/` or `routes/` and compose components.
- Shared components live in `components/`.
- Hooks live in `hooks/`.
- API call logic lives in `api/` or alongside React Query definitions.

## Anti-Patterns
- Storing server data in Zustand (use React Query instead).
- Fat page components that mix data fetching and rendering logic.
- Skipping TypeScript types on props.

## Related Files
- frontend/react-query-zustand.md
- decisions/rendering-strategy.md
- decisions/state-management.md
