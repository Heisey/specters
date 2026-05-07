# Database Choice

## Purpose
How to choose the right database for a project.

## Options
- Postgres → `database/postgres.md`
- MySQL → `database/mysql.md`
- MongoDB → `database/mongodb.md`

## Decision Rules

| Scenario | Choice |
|---|---|
| SaaS app or default relational app | Postgres |
| Existing MySQL system or strong team preference | MySQL |
| Document-heavy, flexible, or NLP-style data | MongoDB |
| Unsure — new greenfield project | Postgres |

## Default
**Postgres** unless there is a specific reason to use something else.

## Related Files
- database/postgres.md
- database/mysql.md
- database/mongodb.md
