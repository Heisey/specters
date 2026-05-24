# Express + Sequelize

## Purpose
Defines how Express services should integrate with Sequelize while following the shared backend, database, routing, response, and error handling patterns.

## Use When
- The backend uses Express.
- Sequelize is selected as the ORM.
- The service exposes relational database APIs.
- Express and Sequelize need to work together within the standard backend architecture.

## Rules
- Follow the shared rules defined in:
  - `backend/node/express/express.md`
  - `backend/node/express/routing.md`
  - `backend/node/express/middleware.md`
  - `backend/api-response-pattern.md`
  - `backend/request-error-pattern.md`
  - `database/database.md`
  - `database/migrations.md`
  - `database/transactions.md`
  - `backend/node/sequelize/sequelize.md`
- Routes must remain thin and declarative.
- Controllers must handle HTTP concerns only.
- Services must contain business logic.
- Repositories must contain Sequelize queries and persistence logic.
- Controllers must not access Sequelize models directly.
- Services must not access Sequelize models directly.
- Controllers and services must use repositories instead of direct model access.
- Sequelize models must live inside `data/models/`.
- Sequelize repositories must live inside `data/repositories/`.
- Database connections must live inside `data/db/`.
- Use centralized error middleware for all API errors.
- Use standardized API response helpers for successful responses.
- Use migrations for all schema changes.
- Use transactions for workflows that require consistency across multiple writes.
- Validate request input before business logic executes.
- Keep Sequelize-specific implementation details out of shared packages unless intentionally shared.

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
        sequelize.ts

      models/
        user.model.ts
        record.model.ts

      repositories/
        user.repository.ts
        record.repository.ts

    middleware/
    responses/
    errors/
    types/
```

## Request Flow Pattern

Use this flow:

```txt
route -> middleware -> controller -> service -> repository -> sequelize model -> database
```

Avoid:

```txt
route -> sequelize model
controller -> sequelize model
service -> sequelize model
```

## Patterns

### Route Pattern

Routes should:
- define HTTP methods
- define paths
- apply middleware
- connect controllers

Example:

```ts
router.get('/users/detail/:id', requireAuth, getUserDetail);
```

### Controller Pattern

Controllers should:
- read request input
- call service logic
- return standardized responses
- throw structured errors

Controllers should not:
- contain database queries
- contain Sequelize logic
- contain business workflows

Example:

```ts
const user = await userService.getUserById(id);

return new ServerResponse(user).send(res);
```

### Service Pattern

Services should:
- contain business logic
- orchestrate workflows
- manage transactions when needed
- coordinate repositories

Services should not:
- depend on Express request/response objects
- directly query Sequelize models

Example:

```ts
await sequelize.transaction(async (transaction) => {
  await userRepository.createUser(data, { transaction });
  await profileRepository.createProfile(profile, { transaction });
});
```

### Repository Pattern

Repositories should:
- contain Sequelize queries
- interact with models
- support transaction-aware operations
- isolate persistence logic

Example:

```ts
export async function findUserById(id: string) {
  return UserModel.findByPk(id);
}
```

### Model Pattern

Models should:
- define fields
- define associations
- define indexes
- represent table structure

Models should not:
- contain business workflows
- contain request handling logic

### Error Pattern

Controllers and middleware should throw structured application errors.

Error middleware should:
- map errors to API responses
- return consistent error shapes
- avoid leaking internal details

### Response Pattern

Controllers should use response helpers:

```ts
return new ServerResponse(records).send(res);
```

Avoid manually building response objects in controllers.

## Validation Pattern

Validate request input before business logic runs.

Validation may occur in:
- middleware
- controller boundaries
- validation helpers

Validation should:
- reject invalid input early
- throw structured validation errors
- avoid duplicated validation logic

## Transaction Pattern

Use transactions for:
- multi-step writes
- account creation workflows
- financial or inventory operations
- workflows requiring atomicity

The service layer should own transaction boundaries.

Repositories should participate in transactions when needed.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project uses a different ORM.
- The project uses raw SQL.
- A framework enforces a different architecture.
- A legacy structure already exists.
- A prototype intentionally uses a simplified structure.

## Anti-Patterns
- Direct Sequelize model access inside controllers.
- Direct Sequelize model access inside services.
- Business logic inside routes.
- Business logic inside models.
- Database queries spread across controllers and middleware.
- Skipping repositories.
- Formatting API responses manually in every controller.
- Catching and formatting errors inside controllers.
- Skipping migrations.
- Running later migrations after a failed migration.
- Performing multi-step writes without transactions.
- Mixing Express-specific logic into repositories or models.

## Related Files
- backend/node/express/express.md
- backend/node/express/routing.md
- backend/node/express/middleware.md
- backend/node/express/error-middleware.md
- backend/node/express/response-helper.md
- backend/api-response-pattern.md
- backend/request-error-pattern.md
- database/database.md
- database/migrations.md
- database/transactions.md
- backend/node/sequelize/sequelize.md
