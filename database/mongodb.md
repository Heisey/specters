# MongoDB

## Purpose
Guidance for projects using MongoDB as the primary data store.

## Use When
- The data model is document-heavy or naturally hierarchical
- Flexible schemas are genuinely needed (NLP outputs, user-defined fields, etc.)
- NOT the default choice for standard relational data

## Rules
- Use MongoDB for document-heavy or flexible-schema data — not as a default relational replacement.
- Define clear document shapes even though the schema is flexible.
- Use Mongoose for schema enforcement and model definitions.
- Use indexes for frequently queried fields.
- Avoid deeply nested documents — flatten where possible.

## Patterns
- Define Mongoose schemas with explicit field types and required constraints.
- Use `lean()` for read-only queries that do not need Mongoose document methods.
- Reference related documents by ID when the relationship is significant.

## Anti-Patterns
- Using MongoDB simply to avoid designing a schema.
- Deeply nested documents that are hard to update atomically.
- Skipping indexes on queried fields.
- Embedding large arrays that grow unboundedly.

## Related Files
- decisions/database-choice.md
