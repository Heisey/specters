# Express

## Purpose
Defines the standard structure and rules for Node.js backend services that use Express.

## Use When
- The selected backend framework is Express.
- A backend service needs HTTP routes, middleware, controllers, or API endpoints.
- Generic backend patterns need Express-specific implementation guidance.

## Rules
- Use Express only when it is selected in `PROJECT_CONTEXT.md`.
- Keep Express-specific implementation inside the backend service that owns it.
- Use route modules to define HTTP paths.
- Use controllers or handlers to process requests.
- Use service modules for business logic.
- Do not put business logic directly inside route definitions.
- Do not manually format responses in every controller.
- Use the standard API response pattern for successful responses.
- Use the standard request error pattern for errors.
- Use centralized error middleware for formatting error responses.
- Use middleware for cross-cutting request concerns such as auth, validation, logging, parsing, and error handling.
- Use an async handler wrapper or equivalent pattern so async errors reach the centralized error middleware.
- Shared types must come from `packages/types` or another clearly named shared package.
- Express-specific types and helpers should stay inside the Express backend service unless intentionally shared.
- Database access must be isolated in the `data/` layer.
- Services must not directly interact with database clients or ORMs.
- Models must not be imported directly into services or controllers.
- All database access must go through repositories or the data layer.

## Standard Service Structure

```txt
backend/<service-name>/
  src/
    app.ts
    server.ts
    routes/
    controllers/
    services/
    data/
      db/
      models/
      repositories/
    middleware/
    responses/
    errors/
    types/
```

## Patterns

### app.ts

Use `app.ts` to:
- create the Express app
- register global middleware
- register routes
- register error middleware

### server.ts

Use `server.ts` to:
- load runtime configuration
- start the HTTP server
- listen on the configured port

### routes/

Use `routes/` for route definitions only.

Routes should:
- define paths
- define HTTP methods
- connect routes to controllers
- avoid business logic

### controllers/

Use `controllers/` for HTTP request handling.

Controllers should:
- read request input
- call service logic
- return standardized responses
- throw structured errors when needed

### services/

Use `services/` for business logic.

Services should:
- contain application behavior
- avoid direct dependency on Express request/response objects
- be reusable outside HTTP handlers when possible

### data/

Use `data/` for database access and persistence logic.

#### db/
- database connection setup
- client initialization
- pooling configuration

#### models/
- ORM models or schema definitions
- represent database structure
- contain no business logic

#### repositories/
- data access layer
- contain all queries and persistence logic
- expose methods for services to use
- act as the only interface between services and the database

Services must call repositories instead of accessing models or database clients directly.

### middleware/

Use `middleware/` for reusable request pipeline logic.

Middleware may handle:
- authentication
- validation
- logging
- request parsing
- error handling

### responses/

Use `responses/` for Express-specific response helpers.

### errors/

Use `errors/` for Express-specific error helpers or adapters.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project uses a different Node backend framework.
- The service is not HTTP-based.
- A hosting platform or framework requires a different structure.
- A quick prototype intentionally uses a flatter structure.

## Anti-Patterns
- Putting all routes in one large file.
- Putting business logic directly in route files.
- Putting business logic directly in controllers.
- Accessing the database directly inside controllers or services.
- Importing models directly into services or controllers.
- Manually formatting response objects in every controller.
- Catching and formatting errors inside every controller.
- Importing frontend app code into backend services.
- Creating Express-specific dependencies inside shared packages.
- Skipping centralized middleware for errors, auth, or validation.

## Related Files
- backend/api-response-pattern.md
- backend/request-error-pattern.md
- backend/api-routing-pattern.md
- backend/auth-patterns.md
- database/database.md
- backend/node/express/routing.md
- backend/node/express/middleware.md
- backend/node/express/response-helper.md
- backend/node/express/error-middleware.md
- backend/node/express/auth-middleware.md
