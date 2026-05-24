# API Routing Pattern

## Purpose
Defines the standard approach for naming, organizing, and structuring API endpoints across backend services.

## Use When
- Any backend service exposes HTTP API routes.
- API endpoints need to remain predictable as the project grows.
- Multiple resources, domains, or feature areas need clear route organization.
- Frontend clients, backend developers, and AI agents need to understand where API functionality belongs.

## Rules
- Treat API routes like hierarchical namespaces.
- Group related functionality under the resource or domain it belongs to.
- Avoid flat endpoint names that combine multiple concepts into one route.
- Prefer predictable, readable, resource-based paths.
- Use explicit subpaths when a resource has multiple concerns.
- Endpoint structure should make the API discoverable without requiring constant documentation lookup.
- Route names should help organize backend controllers, services, permissions, and documentation.
- Do not create panic-named endpoints to avoid collisions.
- Keep endpoint naming consistent across the project.
- API route structure should be documented in `specs/` when it becomes part of the project architecture.

## Core Philosophy

A good API should behave like a filesystem.

Routes should feel like organized folders, not random files scattered across a desktop.

Bad:

```txt
/userwithposts
/useranalytics
/postcomments
/postlikes
```

Good:

```txt
/user/details
/user/posts
/user/analytics

/post/comments
/post/likes
```

The goal is predictability. Developers should be able to guess where an endpoint belongs before opening the documentation.

## Patterns

### Resource Namespace Pattern

Use a primary resource as the namespace.

```txt
/user/details
/user/posts
/user/analytics
```

### Related Resource Pattern

Group related functionality under the resource it belongs to.

```txt
/product/details
/product/reviews
/product/inventory
```

### Subresource Pattern

Use nested paths when one resource clearly belongs to another.

```txt
/team/members
/team/members/active
/team/members/inactive
```

### Domain Group Pattern

Use a domain namespace when the feature area owns multiple related actions.

```txt
/auth/login
/auth/logout
/auth/session
```

### Explicit Concern Pattern

Use explicit subpaths when a root resource would otherwise become ambiguous.

Instead of:

```txt
/project
```

Prefer:

```txt
/project/details
/project/tasks
/project/analytics
/project/settings
```

## Naming Guidelines

- Use clear resource or domain names.
- Use consistent singular or plural conventions per project.
- Use subpaths to represent concerns, features, or related collections.
- Use verbs only when the endpoint represents an action that is not naturally modeled as a resource.
- Prefer readable paths over strict REST purity when clarity is improved.
- Keep future growth in mind when naming routes.

## Backend Organization Alignment

Route structure should usually influence backend folder organization.

Example routes:

```txt
/project/details
/project/tasks
/project/analytics
```

May map to:

```txt
controllers/project/
  details.controller.ts
  tasks.controller.ts
  analytics.controller.ts
```

Or:

```txt
services/project/
  details.service.ts
  tasks.service.ts
  analytics.service.ts
```

The goal is compositional architecture instead of accidental accumulation.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- A third-party API contract requires a different route shape.
- A framework or platform enforces a different route convention.
- A public API must remain backward compatible with existing routes.
- A strict REST design is required for the project.
- A route is intentionally designed for webhooks, callbacks, health checks, or system-level behavior.

## Anti-Patterns
- Flat endpoint growth with no namespace structure.
- Combining multiple concepts into one route name.
- Creating endpoints such as `/userwithposts`, `/postcomments`, or `/useranalytics`.
- Naming routes like functions instead of API paths, such as `/getUserPosts`.
- Creating long compound routes such as `/userwithpostsandanalytics`.
- Mixing unrelated domains in the same route namespace.
- Letting route names drift away from controller and service organization.
- Allowing endpoint naming to be decided only at implementation time.
- Sacrificing clarity for strict REST purity when the result becomes ambiguous.

## Related Files
- backend/api-response-pattern.md
- backend/request-error-pattern.md
- backend/node/express/routing.md
