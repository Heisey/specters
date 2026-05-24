# Sequelize

## Purpose
Defines general rules and patterns for using Sequelize as the ORM in Node.js backend services.

## Use When
- The backend uses Node.js.
- Sequelize is selected as the ORM.
- A service needs relational database models, repositories, migrations, or transactions.
- Sequelize needs to implement the shared database rules.

## Rules
- Use Sequelize only when it is selected in `specs/PROJECT_CONTEXT.md`.
- Follow the shared database rules defined in:
  - `database/database.md`
  - `database/migrations.md`
  - `database/transactions.md`
- Keep Sequelize-specific code inside the backend service data layer.
- Store database connection setup in `data/db/`.
- Store Sequelize models in `data/models/`.
- Store query and persistence logic in `data/repositories/`.
- Do not import Sequelize models directly into controllers.
- Do not import Sequelize models directly into services.
- Services must call repositories instead of using Sequelize models directly.
- Models should describe table structure and relationships, not business logic.
- Repositories should contain Sequelize queries.
- Use migrations for all relational schema changes.
- Use transactions for multi-step writes that must succeed or fail together.
- Keep model names, table names, and column names consistent with the selected database conventions.
- Shared record and input types should live in `packages/types` or another clearly named shared package when needed across apps or services.

## Standard Data Structure

```txt
backend/<service-name>/
  src/
    data/
      db/
        sequelize.ts
      models/
        user.model.ts
        record.model.ts
      repositories/
        user.repository.ts
        record.repository.ts
```

## Patterns

### Database Connection Pattern

Use `data/db/` for Sequelize setup.

```txt
data/db/sequelize.ts
```

This file should:
- initialize Sequelize
- load database configuration
- configure connection pooling
- export the Sequelize instance
- avoid defining business logic

### Model Pattern

Use `data/models/` for Sequelize model definitions.

Models should:
- define fields
- define table names
- define indexes where appropriate
- define associations where appropriate
- avoid business logic

Example:

```ts
export class UserModel extends Model {}
```

### Repository Pattern

Use `data/repositories/` for Sequelize queries.

Repositories should:
- call Sequelize models
- handle data fetching and persistence
- accept transaction context when needed
- return records or domain-friendly data to services

Example:

```ts
export async function findUserById(id: string) {
  return UserModel.findByPk(id);
}
```

Services should call repositories:

```ts
const user = await userRepository.findUserById(id);
```

Services should not call models directly:

```ts
// Avoid
const user = await UserModel.findByPk(id);
```

### Transaction Pattern

Repositories should support transaction-aware operations.

```ts
await userRepository.createUser(data, { transaction });
await profileRepository.createProfile(data, { transaction });
```

The service should own the workflow and transaction boundary when multiple repository calls must succeed together.

### Migration Pattern

Use Sequelize migrations or a selected migration tool for schema changes.

Migrations should:
- be committed to version control
- run in order
- be tracked by a migration table
- stop execution if a migration fails
- follow the rules in `database/migrations.md`

### Type Pattern

Separate:
- input types
- record types
- hydrated/expanded record types when needed

Example:

```ts
interface UserInput {}
interface UserRecord extends UserInput {
  id: string;
  createdAt: Date;
  updatedAt: Date;
}
```

## Naming Guidelines

- Use project database naming conventions for tables and columns.
- Prefer `snake_case` table and column names for SQL schemas unless overridden.
- Use clear model file names such as:
  - `user.model.ts`
  - `record.model.ts`
  - `organization.model.ts`
- Use clear repository file names such as:
  - `user.repository.ts`
  - `record.repository.ts`
  - `organization.repository.ts`

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project uses a different ORM.
- The project uses raw SQL instead of Sequelize.
- A legacy Sequelize structure already exists.
- A framework requires a different model or repository layout.
- A prototype intentionally uses a flatter data layer.

## Anti-Patterns
- Importing Sequelize models directly into controllers.
- Importing Sequelize models directly into services.
- Placing business logic inside Sequelize models.
- Skipping repositories and spreading queries across the codebase.
- Defining database connections in controllers, services, or routes.
- Modifying schemas manually instead of using migrations.
- Running later migrations after a migration has failed.
- Starting transactions in controllers or route files.
- Mixing Sequelize-specific code into shared packages.
- Storing Sequelize decisions only in chat history instead of `specs/`.

## Related Files
- database/database.md
- database/migrations.md
- database/transactions.md
- database/postgres/postgres.md
- database/mysql/mysql.md
- backend/node/express/express.md
- backend/node/sequelize/express-sequelize.md
