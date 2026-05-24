# Postgres

## Purpose
Defines PostgreSQL-specific guidance, patterns, and rules for backend services using Postgres as the primary relational database.

## Use When
- A project requires a relational database.
- The project needs strong consistency, transactions, indexing, and relational querying.
- The backend service stores structured production data.
- PostgreSQL is selected in `specs/PROJECT_CONTEXT.md`.

## Rules
- Use PostgreSQL as the default relational database unless another database is intentionally selected.
- Follow the shared rules defined in:
  - `database/database.md`
  - `database/migrations.md`
  - `database/transactions.md`
- Use migrations for all schema changes.
- Do not modify production schemas manually.
- Use indexes intentionally for:
  - foreign keys
  - frequently queried columns
  - unique constraints
  - sorting or filtering columns
- Use consistent naming conventions across the schema.
- Prefer `snake_case` for:
  - table names
  - column names
  - index names
  - constraint names
- Use consistent primary key strategy across the project.
- Prefer UUIDs for distributed systems or public-facing identifiers.
- Use `NOT NULL` constraints when values are always required.
- Use explicit foreign key relationships where relational integrity matters.
- Use transactions for multi-step writes that must remain consistent.
- Store relational data relationally whenever possible.
- Avoid using JSON columns as a replacement for proper schema design unless flexibility is intentionally required.
- Database access must remain inside the backend service data layer.

## Patterns

### Standard Naming Pattern

Tables:

```txt
users
user_profiles
organization_members
```

Columns:

```txt
user_id
created_at
updated_at
organization_id
```

### Primary Key Pattern

Use one consistent primary key strategy across the project.

Examples:

```txt
uuid
ulid
bigserial
serial
```

Avoid mixing unrelated ID styles without clear reason.

### Foreign Key Pattern

Use explicit foreign key naming:

```txt
user_id
organization_id
project_id
```

### Indexing Pattern

Add indexes for:
- foreign keys
- lookup-heavy fields
- unique identifiers
- commonly filtered columns

Examples:

```txt
email
organization_id
created_at
status
```

### Migration Pattern

Use additive migrations whenever possible.

Good:

```txt
add_status_to_users
create_user_sessions_table
```

Avoid:
- destructive schema changes without planning
- large unrelated migrations
- manual production schema changes

### Transaction Pattern

Use transactions for:
- account creation workflows
- financial or inventory operations
- multi-table writes
- consistency-sensitive operations

## JSON Usage Pattern

Use JSON or JSONB only when:
- the structure is intentionally flexible
- the data is semi-structured
- relational modeling would create unnecessary complexity

Avoid:
- storing relational data as large JSON blobs
- bypassing relational design for convenience
- placing core searchable fields only inside JSON

## Performance Guidelines

Monitor:
- slow queries
- missing indexes
- unnecessary joins
- N+1 query patterns
- oversized JSON payloads

Prefer:
- explicit indexes
- pagination
- query limits
- selective column loading

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project intentionally uses a different relational database.
- A legacy schema already exists.
- The project intentionally uses camelCase naming.
- The project requires flexible document-style storage patterns.
- A managed platform enforces different database conventions.

## Anti-Patterns
- Modifying schemas manually in production.
- Skipping migrations.
- Missing indexes on foreign keys or frequently queried fields.
- Mixing naming conventions across the schema.
- Mixing multiple unrelated primary key styles.
- Storing relational data as unstructured JSON blobs.
- Querying the database directly from controllers or routes.
- Accessing ORM models directly inside services.
- Creating giant tables with unrelated responsibilities.
- Treating PostgreSQL like a document database without clear reason.

## Related Files
- database/database.md
- database/migrations.md
- database/transactions.md
- database/mysql/mysql.md
- database/mongodb/mongodb.md
- backend/node/express/express.md
