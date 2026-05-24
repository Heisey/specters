# React Query + Zustand

## Purpose
Rules for separating server state and client state.

## Use When
- Any React project that fetches data from an API
- Included alongside vite-spa.md or nextjs-ssr.md

## Rules
- React Query owns all server state (fetched data, loading, error).
- Zustand owns client/UI state (modals, sidebar open, selected tab, filters).
- Do not store fetched server data in Zustand unless there is a clear reason.
- Query keys must be consistent and predictable — use arrays with entity and id.
- Mutations should invalidate or optimistically update relevant queries after success.
- Use `enabled` to defer queries that depend on other data.

## Patterns
- Query key convention: `['users']`, `['users', userId]`, `['users', userId, 'posts']`.
- Co-locate query definitions with the feature they belong to.
- Zustand stores are flat — avoid deeply nested state.
- Use selectors in Zustand to avoid unnecessary re-renders.

## Anti-Patterns
- Caching API data in Zustand and manually keeping it in sync with React Query.
- Using React Query for purely local UI state.
- String-only query keys that are hard to invalidate precisely.
- Global loading state in Zustand when React Query already tracks it.

## Related Files
- frontend/vite-spa.md
- frontend/nextjs-ssr.md
- decisions/state-management.md
