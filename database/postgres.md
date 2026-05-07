# Postgres

## Purpose
Guidance for projects using PostgreSQL as the primary database.

## Use When
- Building a SaaS app or any production relational data store
- Default choice for new projects unless there is a reason to use MySQL or MongoDB

## Rules
- Use Postgres for production SaaS apps — it is the default.
- Always use migrations — never modify the database schema directly.
- Add indexes for frequently queried columns and foreign keys.
- Use consistent, snake_case naming for tables and columns.
- Use UUIDs or serial integers for primary keys — be consistent within the project.
- Use `NOT NULL` constraints where a value is always required.

## Patterns
- Migration tool: Sequelize migrations or a standalone tool like `db-migrate`.
- Keep migrations additive when possible — avoid destructive column drops in the same migration that adds new columns.
- Name foreign key columns as `entity_id` (`user_id`, `org_id`).

## Anti-Patterns
- Skipping migrations and modifying schema manually.
- Missing indexes on foreign keys.
- Inconsistent naming (mixed camelCase and snake_case in the same schema).
- Storing JSON blobs where relational columns would serve better.

## Related Files
- decisions/database-choice.md
- backend/express-sequelize.md
