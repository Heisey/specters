# MySQL

## Purpose
Defines MySQL-specific guidance, patterns, and rules for backend services using MySQL as the primary relational database.

## Use When
- The project already uses MySQL infrastructure.
- The system integrates with an existing MySQL database.
- MySQL is selected in `specs/PROJECT_CONTEXT.md`.
- The project requires a relational database but PostgreSQL is not the preferred choice.

## Rules
- Follow the shared rules defined in:
  - `database/database.md`
  - `database/migrations.md`
  - `database/transactions.md`
- Use migrations for all schema changes.
- Do not modify production schemas manually.
- Use `InnoDB` as the database engine for transaction support and relational integrity.
- Use `utf8mb4` character encoding for full Unicode support.
- Use consistent naming conventions across the schema.
- Prefer `snake_case` for:
  - table names
  - column names
  - index names
  - constraint names
- Use explicit foreign key relationships where relational integrity matters.
- Use indexes intentionally for:
  - foreign keys
  - frequently queried columns
  - sorting and filtering columns
  - unique constraints
- Use a consistent primary key strategy across the project.
- Use transactions for multi-step writes that must remain consistent.
- Be intentional with JSON column usage.
- Avoid using JSON columns as a replacement for proper relational schema design.
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
bigint
int
```

Avoid mixing unrelated ID styles without clear reason.

### Foreign Key Pattern

Use explicit foreign key naming:

```txt
user_id
organization_id
project_id
```

### Character Encoding Pattern

Use:

```txt
utf8mb4
```

This supports:
- full Unicode
- emoji
- multilingual content

Avoid mixing character sets or collations across tables unless intentionally required.

### Engine Pattern

Use:

```txt
InnoDB
```

for:
- transactions
- foreign keys
- row-level locking
- relational consistency

Avoid:

```txt
MyISAM
```

because it lacks proper transaction support.

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
- inventory or financial operations
- multi-table writes
- consistency-sensitive workflows

## JSON Usage Pattern

Use JSON columns only when:
- the structure is intentionally flexible
- the data is semi-structured
- the schema varies significantly

Avoid:
- storing relational data inside JSON blobs
- bypassing proper schema design for convenience
- placing important searchable fields only inside JSON

Define clear expected document shapes when JSON is used.

## Performance Guidelines

Monitor:
- slow queries
- missing indexes
- full table scans
- N+1 query patterns
- oversized JSON payloads

Prefer:
- indexes
- pagination
- query limits
- selective column loading

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project intentionally uses another relational database.
- A legacy schema already exists.
- The project intentionally uses different naming conventions.
- The project requires highly flexible document-style storage.
- A managed database platform enforces different conventions.

## Anti-Patterns
- Modifying schemas manually in production.
- Skipping migrations.
- Using MyISAM for transactional workloads.
- Mixing character sets or collations across tables.
- Missing indexes on foreign keys or frequently queried fields.
- Mixing naming conventions across the schema.
- Mixing unrelated primary key strategies.
- Using JSON columns as a catch-all storage mechanism.
- Querying the database directly from controllers or routes.
- Accessing ORM models directly inside services.
- Treating MySQL like a document database without clear reason.

## Related Files
- database/database.md
- database/migrations.md
- database/transactions.md
- database/postgres/postgres.md
- database/mongodb/mongodb.md
- backend/node/express/express.md
