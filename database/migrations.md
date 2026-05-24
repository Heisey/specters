# Migrations

## Purpose
Defines general rules for managing database schema changes safely and consistently.

## Use When
- A project uses a relational database.
- Database schema changes need to be tracked over time.
- Multiple developers, environments, or deployments need consistent database structure.
- A backend service owns database tables or schemas.

## Rules
- Use migrations for all relational database schema changes.
- Do not change production database schemas manually.
- Every schema change must be represented by a migration file.
- Migration files must be committed to version control.
- Migrations must run in a predictable order.
- Migration names must clearly describe the schema change.
- Migrations should be small, focused, and easy to review.
- Do not combine unrelated schema changes in one migration.
- Avoid destructive migrations unless they are planned and documented.
- Data migrations must be handled carefully and documented when needed.
- Migrations must be tested locally before being applied to shared environments.
- Migration tooling should be selected based on the chosen backend stack and database.
- Each database must have its own migration tracking table.
- The migration tracking table must record whether each migration was applied successfully.
- If a migration fails, the migration runner must stop and must not continue to the next migration.
- Failed migrations must be investigated and resolved before later migrations are applied.
- Important migration decisions must be documented in `specs/`.

## Migration Tracking Table

Each database should maintain a migration tracking table.

The migration tracking table should record:
- migration name
- migration version or order
- applied status
- applied timestamp
- failure status or error details when supported
- checksum or hash when supported by the migration tool

Example table name:

```txt
schema_migrations
```

Example fields:

```txt
id
migration_name
migration_order
applied_successfully
applied_at
failed_at
error_message
checksum
```

The exact table shape may vary by migration tool, but the behavior must remain consistent.

## Failure Behavior

Migration execution must be fail-fast.

If a migration fails:
- stop migration execution immediately
- do not apply the next migration
- record the failed state when supported
- surface the error clearly
- require manual review or a documented recovery step before continuing

A database must not continue applying later migrations after a failed migration.

## Patterns

### Migration Naming

Use clear names that explain the change.

Good:

```txt
create_users_table
add_email_to_users
create_records_table
add_status_to_records
```

Avoid:

```txt
update_db
changes
fix_schema
migration_1
```

### Small Migration Pattern

Prefer focused migrations.

Good:

```txt
create_users_table
add_user_profile_fields
create_user_sessions_table
```

Avoid:

```txt
create_everything
```

### Schema Migration Pattern

Use schema migrations for:
- creating tables
- changing columns
- adding indexes
- creating constraints
- changing relationships

### Data Migration Pattern

Use data migrations for:
- backfilling new fields
- normalizing existing data
- moving data between tables
- updating records after schema changes

Data migrations should be:
- documented
- tested
- reversible when possible
- safe for existing production data

### Rollback Pattern

Migrations should include rollback behavior when supported by the migration tool.

Rollbacks should be used carefully when:
- data may be lost
- columns are dropped
- values are transformed
- production data has already changed

### Deployment Pattern

Before deployment:
- apply migrations in a staging or local environment
- verify the application still works
- confirm migration order
- confirm rollback or recovery plan when needed
- confirm failed migrations stop the migration process

## Destructive Changes

Destructive changes include:
- dropping tables
- dropping columns
- renaming columns
- changing column types
- removing indexes or constraints
- deleting production data

Destructive changes should:
- be avoided when possible
- be planned in multiple steps when needed
- be documented in `specs/`
- include a recovery strategy

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project does not use a relational database.
- The project uses a database service that manages schema changes differently.
- The project is a quick throwaway prototype.
- A legacy project already has an established migration strategy.
- A framework requires a specific migration workflow.

## Anti-Patterns
- Manually changing production database schemas.
- Making schema changes without migration files.
- Combining unrelated schema changes into one migration.
- Using vague migration names.
- Running untested migrations against shared or production environments.
- Continuing to run later migrations after a migration has failed.
- Not recording migration success or failure state.
- Sharing one migration tracking table across unrelated databases.
- Dropping columns or tables without a documented plan.
- Treating migration rollback as guaranteed data recovery.
- Storing migration decisions only in chat history instead of `specs/`.

## Related Files
- database/database.md
- database/postgres/postgres.md
- database/mysql/mysql.md
- database/transactions.md
