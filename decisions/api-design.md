# API Design

## Purpose
How to choose and structure the API layer.

## Default
**REST** for all MVPs and most projects.

## Rules
- REST is the default — familiar, simple, well-understood.
- Use consistent response wrappers on all endpoints (see `backend/api-response-pattern.md`).
- Use centralized error handling (see `backend/request-error-pattern.md`).
- Avoid GraphQL unless there is a strong reason (complex data graphs, mobile clients with bandwidth constraints, public APIs with many consumers).
- Version APIs when they are public-facing (`/v1/users`).

## Decision Rules

| Scenario | Choice |
|---|---|
| MVP or internal API | REST |
| Public API with many consumers | REST with versioning |
| Complex data graph with many related entities | Consider GraphQL |
| Real-time data | REST + WebSockets or SSE |

## Anti-Patterns
- Adopting GraphQL for a simple CRUD API.
- Inconsistent response shapes across endpoints.
- No error structure — raw HTTP status codes only.

## Related Files
- backend/api-response-pattern.md
- backend/request-error-pattern.md
- backend/express-sequelize.md
