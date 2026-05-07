# MySQL

## Purpose
Guidance for projects using MySQL as the primary database.

## Use When
- The project or existing system already uses MySQL
- Migrating or integrating with a MySQL-based system

## Rules
- Use MySQL when the project or existing infrastructure prefers it.
- Always use migrations.
- Be explicit with JSON column usage — define a clear document shape, do not use it as a catch-all.
- Use `InnoDB` engine (default in modern MySQL) for transaction support.
- Use consistent snake_case naming for tables and columns.

## Patterns
- Migration tool: Sequelize migrations.
- Use `utf8mb4` character set for full Unicode support (including emoji).
- Define foreign key constraints explicitly.

## Anti-Patterns
- Skipping migrations.
- Using JSON columns as a substitute for proper schema design.
- Mixing character sets across tables.
- Using `MyISAM` engine (no transactions).

## Related Files
- decisions/database-choice.md
- backend/express-sequelize.md
