# Transactions

## Purpose
Defines general rules for using database transactions to keep data consistent when multiple operations must succeed or fail together.

## Use When
- A backend operation performs multiple related database writes.
- A workflow must be atomic.
- Partial data changes would leave the system in an invalid state.
- A service needs consistency across related records.

## Rules
- Use transactions when multiple database operations must succeed or fail as one unit.
- Do not use transactions for simple single-record reads or writes unless the database or framework requires it.
- Keep transactions as small and short-lived as possible.
- Do not perform slow external work inside a transaction.
- Do not call third-party APIs inside a database transaction.
- Do not wait on user input inside a transaction.
- Transaction logic must live in the service or data layer, not in controllers or routes.
- Repositories must support transaction-aware operations when needed.
- If any operation inside a transaction fails, the entire transaction must roll back.
- Failed transactions must surface a structured error through the request error pattern.
- Important transaction decisions must be documented in `specs/` when they affect system behavior.

## Core Principle

A transaction is for protecting consistency.

If a workflow would become invalid when only half of it succeeds, it should use a transaction.

Example:

```txt
create user
create user profile
create default settings
```

These should either all succeed or all fail.

## Patterns

### Service-Owned Transaction Pattern

Use the service layer to decide when a transaction is needed.

```txt
controller -> service -> transaction -> repositories -> database
```

The service owns the workflow.  
Repositories perform the data operations.

### Repository Participation Pattern

Repositories should accept a transaction context when needed.

```ts
await userRepository.create(userData, { transaction });
await profileRepository.create(profileData, { transaction });
```

Repositories should not start unrelated transactions unless they own the full workflow.

### Multi-Step Write Pattern

Use transactions for operations such as:
- creating multiple related records
- updating several records as one workflow
- moving ownership from one entity to another
- creating audit records with primary records
- updating balances, counts, or inventory
- replacing related records in bulk

### Rollback Pattern

If any step fails:
- roll back all database changes from the transaction
- do not continue the workflow
- return or throw a structured application error
- log enough context for debugging without leaking sensitive data

### Boundary Pattern

Define transaction boundaries clearly.

Good:

```txt
start transaction
  create user
  create profile
  create default settings
commit transaction
```

Avoid:

```txt
start transaction
  create user
  call external billing API
  send email
  wait for external response
commit transaction
```

External side effects should happen outside the transaction or through an outbox/event pattern when needed.

## Common Transaction Use Cases

Use transactions for:
- account creation involving multiple tables
- order creation and inventory updates
- payment records and ledger updates
- permission or role updates across related tables
- bulk replacements of child records
- audit-log creation tied to a primary write
- state transitions that update multiple records

## Avoid Transactions For

Avoid transactions for:
- simple reads
- single independent writes
- long-running workflows
- external API calls
- email sending
- file uploads
- background jobs that can be retried independently

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The selected database does not support transactions.
- The selected database has limited transaction support.
- A framework or ORM manages transactions differently.
- A workflow intentionally uses eventual consistency instead of transactions.
- A distributed system requires sagas, outbox, or event-driven consistency patterns instead.

## Anti-Patterns
- Performing multiple dependent writes without a transaction.
- Starting transactions in controllers or route files.
- Keeping transactions open longer than necessary.
- Calling external APIs inside a transaction.
- Sending emails inside a transaction.
- Mixing transaction-aware and non-transaction-aware repository calls in the same workflow.
- Swallowing transaction errors and continuing execution.
- Assuming transactions replace proper validation or business rules.
- Storing transaction decisions only in chat history instead of `specs/`.

## Related Files
- database/database.md
- database/migrations.md
- database/postgres/postgres.md
- database/mysql/mysql.md
- backend/request-error-pattern.md
- backend/node/express/express.md
