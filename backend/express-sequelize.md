# Express + Sequelize

## Purpose
Rules for REST API projects using Express and Sequelize.

## Use When
- Building a Node.js REST API
- Using a relational database with Sequelize as the ORM

## Rules
- Use controllers for request handling — one controller per resource.
- Use services for business logic when logic grows beyond simple CRUD.
- Use Sequelize models for persistence — define associations in the model files.
- Keep routes thin — routes only call controllers.
- Use centralized error handling middleware — controllers throw, middleware formats.
- Validate request input at the controller boundary.

## Patterns
- File structure: `routes/` → `controllers/` → `services/` → `models/`.
- Register all routes in a single `routes/index.ts` file.
- Use async/await throughout — wrap in try/catch or use an async handler wrapper.
- Return consistent response shapes (see api-response-pattern.md).

## Anti-Patterns
- Business logic in route handlers.
- Direct database calls in controllers (use services for anything non-trivial).
- Inconsistent response shapes across endpoints.
- Missing centralized error middleware.

## Related Files
- backend/api-response-pattern.md
- backend/request-error-pattern.md
- backend/auth-patterns.md
