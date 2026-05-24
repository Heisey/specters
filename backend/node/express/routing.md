# Routing

## Purpose
Defines how HTTP routes should be structured and organized inside Express services.

This file covers Express-specific route implementation. General API route naming and namespace rules are defined in `backend/api-routing-pattern.md`.

## Use When
- The backend uses Express for handling HTTP requests.
- Express route files need to be organized in a scalable and maintainable way.
- Routes need to connect HTTP methods and paths to controllers.
- Generic API routing rules need Express-specific implementation guidance.

## Rules
- Follow the generic route naming rules defined in `backend/api-routing-pattern.md`.
- Use route modules to group related endpoints.
- Each route file must represent a single domain, resource, or route namespace.
- Routes must only define:
  - HTTP method
  - path
  - middleware chain
  - controller handler
- Do not include business logic inside route files.
- Do not include database logic inside route files.
- Do not manually format responses in route files.
- Route files must remain thin and declarative.
- Routes must call controllers, not services or repositories directly.
- Use middleware in routes only for request pipeline concerns such as auth, validation, rate limiting, or permissions.
- Use versioned base paths when the API needs versioning, such as `/api/v1`.
- Keep route registration centralized in the Express app setup or a dedicated route registration file.
- Prefer explicit concern-based routes such as `/users/detail/:id` over ambiguous root resource routes such as `/users/:id`.

## Patterns

### Route File Structure

```txt
routes/
  users.routes.ts
  auth.routes.ts
  records.routes.ts
```

### Basic Route Definition

Use concern-based paths under the resource namespace.

```ts
router.get('/users/detail/:id', getUserDetail);
router.post('/users/detail/create', createUser);
```

### Route With Middleware

```ts
router.get('/users/detail/:id', requireAuth, getUserDetail);
```

### Route Registration

Routes should be registered in a central location:

```ts
app.use('/api/v1', userRoutes);
app.use('/api/v1', authRoutes);
app.use('/api/v1', recordRoutes);
```

### Namespace-Based Routing

Group routes by domain or namespace:

```txt
routes/
  users.routes.ts
  auth.routes.ts
  projects.routes.ts
```

Example:

```ts
router.get('/projects/detail/:id', getProjectDetail);
router.get('/projects/tasks/:id', getProjectTasks);
router.get('/projects/analytics/:id', getProjectAnalytics);
```

### Nested Routes

Use nested paths only when one resource clearly belongs to another.

```ts
router.get('/teams/members/:teamId', getTeamMembers);
router.get('/teams/members/active/:teamId', getActiveTeamMembers);
```

### Controller Mapping

Routes should point to controllers that match the route namespace and concern.

```txt
routes/
  projects.routes.ts

controllers/
  projects/
    detail.controller.ts
    tasks.controller.ts
    analytics.controller.ts
```

## Route Naming Notes

- Use the resource or domain as the first namespace.
- Use a clear concern after the resource namespace.
- Use route params after the concern when fetching a specific record.
- Prefer explicit routes that explain what the endpoint returns.
- Avoid ambiguous root routes when the resource has multiple possible concerns.

Good:

```txt
/users/detail/:id
/users/posts/:id
/users/analytics/:id
/projects/detail/:id
/projects/tasks/:id
/projects/settings/:id
```

Avoid:

```txt
/users/:id
/projects/:id
/userwithposts
/projectanalytics
```

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A framework or hosting platform enforces a different routing structure.
- The project is small enough to intentionally use a single route file.
- A third-party or public API contract requires different route patterns.
- A route is for a webhook, callback, health check, or system endpoint that does not fit normal resource routing.
- The project explicitly chooses strict REST resource routing in `PROJECT_CONTEXT.md`.

## Anti-Patterns
- Putting all routes in one large file.
- Adding business logic inside route definitions.
- Adding database queries inside route definitions.
- Calling services or repositories directly from routes.
- Mixing unrelated domains in a single route file.
- Hardcoding response formatting inside routes.
- Creating deeply nested route structures without clear need.
- Duplicating route naming rules already defined in `backend/api-routing-pattern.md`.
- Letting Express route organization drift away from controller and service organization.
- Using ambiguous routes like `/users/:id` when the project standard expects concern-based routes such as `/users/detail/:id`.

## Related Files
- backend/api-routing-pattern.md
- backend/node/express/express.md
- backend/node/express/middleware.md
- backend/api-response-pattern.md
- backend/request-error-pattern.md
