# Microservices

## Purpose
Guidance for projects using a microservices architecture.

## Use When
- Services have clear, independent responsibilities and teams
- Different services need to scale independently
- The product has proven domain boundaries worth splitting

## Rules
- Use microservices only when services have clear independent responsibilities.
- Each service owns its own domain logic and data store.
- Avoid premature service splitting — monolith first.
- Define API contracts (REST or message schemas) clearly before splitting.
- Services communicate via HTTP APIs or async messaging — not direct DB sharing.

## Patterns
- Each service has its own repo or package in a monorepo.
- Define shared types in a contract package consumed by both sides.
- Use health check endpoints on every service.
- Use an API gateway or reverse proxy for external routing.

## Anti-Patterns
- Splitting services before domain boundaries are clear.
- Services sharing a database.
- Microservices that are too small to justify the operational overhead.
- No versioning on inter-service APIs.

## Related Files
- architecture/monorepo.md
- decisions/architecture-choice.md
