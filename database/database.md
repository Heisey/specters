# Database

## Purpose
Defines general database rules and patterns that apply across backend services regardless of the selected database engine.

## Use When
- A project stores persistent data.
- A backend service needs database access.
- A project needs consistent rules for data ownership, schema design, migrations, transactions, and data access.
- Database-specific files need a shared foundation.

## Rules
- Choose the database intentionally based on project needs.
- Document the selected database in `specs/PROJECT_CONTEXT.md`.
- Keep database access isolated in the backend service `data/` layer.
- Controllers must not access the database directly.
- Services must not access database clients, ORM models, or raw queries directly.
- Services must call repositories or data-layer abstractions.
- Models, schemas, database clients, and repositories must live inside the backend service data layer.
- Shared database types should live in `packages/types` only when they are needed across apps or services.
- Do not place database-specific implementation details inside frontend apps.
- Do not share a database between true microservices unless explicitly overridden.
- Use migrations for relational databases.
- Use transactions when multiple database operations must succeed or fail together.
- Avoid over-modeling features before the product needs them.
- Store important database decisions in `specs/`.

## Standard Backend Data Structure

```txt
backend/<service-name>/
  src/
    data/
      db/
      models/
      repositories/
```

## Patterns

### db/

Use `data/db/` for:
- database connection setup
- database client initialization
- connection pooling
- environment-based database config

### models/

Use `data/models/` for:
- ORM models
- schema definitions
- database table/entity representations

Models should:
- describe database structure
- avoid business logic
- avoid direct Express or framework dependencies

### repositories/

Use `data/repositories/` for:
- queries
- persistence logic
- database reads and writes
- data access methods consumed by services

Repositories should be the primary interface between services and the database.

### Service to Data Flow

Use this flow:

```txt
controller -> service -> repository -> database
```

Avoid this:

```txt
controller -> database
service -> database client
service -> ORM model
```

### Database Selection

Use database-specific files for implementation guidance:

- `database/postgres/postgres.md`
- `database/mysql/mysql.md`
- `database/mongodb/mongodb.md`

### Migrations

Use `database/migrations.md` for rules about:
- schema changes
- migration naming
- migration safety
- rollback strategy

### Transactions

Use `database/transactions.md` for rules about:
- atomic operations
- consistency
- rollback behavior
- multi-step writes

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project is frontend-only and has no database.
- The project uses a hosted backend or third-party database service.
- A framework requires a different data-layer structure.
- A prototype intentionally uses a simpler structure.
- A true microservice requires isolated database ownership.
- A legacy project must preserve an existing database structure.

## Anti-Patterns
- Accessing the database directly inside controllers.
- Accessing ORM models directly inside services.
- Putting database logic in route files.
- Sharing database clients with frontend apps.
- Duplicating query logic across services instead of using repositories.
- Putting business logic inside models.
- Creating shared database tables between true microservices.
- Changing relational schemas manually without migrations.
- Using transactions inconsistently for multi-step writes.
- Storing database decisions only in chat history instead of `specs/`.

## Related Files
- database/postgres/postgres.md
- database/mysql/mysql.md
- database/mongodb/mongodb.md
- database/migrations.md
- database/transactions.md
- backend/node/express/express.md
- architecture/modular-backend.md
- architecture/microservices.md
