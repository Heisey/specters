# Microservices

## Purpose
Guidance for projects using a true microservices architecture where backend logic is split into independently deployable services with isolated data and clear domain boundaries.

## Use When
- The system has outgrown a modular backend structure.
- Domain boundaries are clearly defined and stable.
- Independent deployment of services is required.
- Different parts of the system require independent scaling.
- Teams or workflows benefit from isolating services.

## Rules
- Do not use microservices as the default starting architecture.
- Start with a modular backend before introducing microservices.
- Each service must own its own domain logic and data store.
- Services must not share databases or directly access another service’s data.
- Services must communicate through defined interfaces only:
  - HTTP APIs (REST or RPC)
  - Asynchronous messaging (events, queues)
- Define API contracts or message schemas before splitting services.
- Each service must be independently deployable.
- Each service must expose a health check endpoint.
- Use `backend/` to organize services in a monorepo when applicable.
- Use `infrastructure/` for supporting systems such as API gateways, queues, service discovery, and observability.
- Shared contracts must live in `packages/` and be versioned when used across services.
- Store all service boundaries, communication patterns, and architecture decisions in `specs/`.

## Patterns
- Monorepo structure:
  - `backend/service-records`
  - `backend/service-auth`
  - `backend/service-billing`

- Shared contracts:
  - `packages/types`
  - `packages/contracts`

- Communication:
  - Synchronous: HTTP APIs over internal networking
  - Asynchronous: Kafka, RabbitMQ, or similar systems defined in `infrastructure/`

- Routing:
  - Use an API gateway or reverse proxy for external access
  - Internal service communication should not rely on public endpoints

- Observability:
  - Centralized logging, monitoring, and tracing defined in `infrastructure/`

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project is an MVP or early-stage product where a modular backend is more appropriate.
- The system does not yet have clear domain boundaries.
- Operational complexity outweighs the benefits of service separation.
- A hybrid approach is required for gradual migration from a modular backend.

## Anti-Patterns
- Using microservices as the default starting architecture.
- Splitting services before domain boundaries are clearly defined.
- Services sharing a database or tightly coupling data access.
- Creating overly small services that increase operational overhead.
- Lack of versioning for inter-service APIs or message contracts.
- Treating microservices as a folder structure instead of a true architectural boundary.
- Storing service communication rules only in code instead of `specs/`.

## Related Files
- architecture/monorepo.md
- architecture/modular-backend.md
- decisions/architecture-choice.md