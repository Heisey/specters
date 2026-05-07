# State Management

## Purpose
How to choose the right state management approach for each type of state.

## Default
Use **React Query + Zustand** for most projects.

## Decision Rules

| State Type | Tool |
|---|---|
| Server data (fetched from API) | React Query |
| Global UI state (modals, sidebar, user preferences) | Zustand |
| Component-only state (form inputs, toggles) | React `useState` / `useReducer` |
| Complex isolated component tree state | React Context |

## Rules
- Do not mix concerns — if data comes from the server, React Query owns it.
- Do not reach for Context for global app state — Zustand is simpler and avoids re-render issues.
- Component state is fine for most UI state — only lift to Zustand when multiple components need it.

## Related Files
- frontend/react-query-zustand.md
- frontend/vite-spa.md
