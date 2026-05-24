# MongoDB

## Purpose
Defines MongoDB-specific guidance, patterns, and rules for backend services using MongoDB as the primary document database.

## Use When
- The data model is document-heavy or naturally hierarchical.
- Flexible or evolving schemas are intentionally required.
- The project stores semi-structured or highly variable data.
- The system handles NLP outputs, AI-generated content, logs, events, or user-defined structures.
- MongoDB is selected in `specs/PROJECT_CONTEXT.md`.

## Rules
- Follow the shared rules defined in:
  - `database/database.md`
  - `database/transactions.md`
- Use MongoDB intentionally, not simply to avoid schema design.
- Define clear document shapes even when schemas are flexible.
- Use schema validation or model definitions when possible.
- Prefer explicit schema definitions through tools such as Mongoose.
- Database access must remain inside the backend service data layer.
- Use indexes intentionally for:
  - frequently queried fields
  - lookup fields
  - sorting fields
  - unique identifiers
- Avoid deeply nested document structures unless they are intentionally modeled that way.
- Avoid unbounded arrays that continuously grow.
- Use references when relationships become large, reusable, or frequently queried independently.
- Use transactions only when consistency requirements justify the added complexity.
- Keep document sizes reasonable and predictable.
- Design collections intentionally instead of treating MongoDB like a generic dump store.

## Patterns

### Collection Naming Pattern

Use consistent collection naming.

Examples:

```txt
users
user_profiles
job_documents
ai_outputs
```

### Schema Definition Pattern

Use explicit schemas or validation rules whenever possible.

Example concerns:
- required fields
- data types
- default values
- indexes
- validation rules

### Flexible Data Pattern

MongoDB works well for:
- AI or NLP outputs
- user-defined fields
- evolving metadata
- event storage
- logs
- configuration documents
- semi-structured content

### Embedding Pattern

Embed documents when:
- the child data belongs tightly to the parent
- the data is usually loaded together
- the embedded data remains reasonably small

Example:

```txt
user preferences
small settings objects
profile metadata
```

### Reference Pattern

Use references when:
- relationships become large
- documents are queried independently
- data is reused across collections
- arrays could grow indefinitely

Examples:

```txt
user_id
organization_id
project_id
```

### Read Performance Pattern

Use lightweight query patterns when full document behavior is unnecessary.

Example with Mongoose:

```ts
Model.find().lean();
```

### Transaction Pattern

Use transactions only when:
- multiple collections must remain consistent
- partial writes would create invalid state
- the workflow truly requires atomicity

Avoid unnecessary transactions for simple document operations.

## Document Design Guidelines

Prefer:
- focused collections
- predictable document shapes
- reasonable nesting depth
- intentional indexing
- bounded array sizes

Avoid:
- giant catch-all documents
- deeply recursive nesting
- storing unrelated concerns together
- unlimited embedded arrays
- treating collections like arbitrary JSON storage

## Performance Guidelines

Monitor:
- slow queries
- missing indexes
- oversized documents
- large array growth
- excessive document nesting
- high write amplification

Prefer:
- indexes
- pagination
- projection/selective field loading
- collection-specific query patterns
- clear document ownership

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project intentionally uses schema-less storage.
- A legacy collection structure already exists.
- The project intentionally uses embedded-first modeling.
- A framework or managed platform enforces different conventions.
- The project intentionally mixes relational and document databases.

## Anti-Patterns
- Using MongoDB only to avoid designing data structures.
- Treating MongoDB as a generic dump store.
- Deeply nested documents that are difficult to update safely.
- Embedding unbounded arrays.
- Skipping indexes on queried fields.
- Querying MongoDB directly from controllers or routes.
- Accessing models directly inside services.
- Mixing unrelated data concerns into giant documents.
- Recreating relational schemas poorly inside documents without clear reason.

## Related Files
- database/database.md
- database/transactions.md
- database/postgres/postgres.md
- database/mysql/mysql.md
- backend/node/express/express.md
