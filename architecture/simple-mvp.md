# Simple MVP

## Purpose
Guidance for small or early-stage projects that need to ship quickly while still following the standard repo structure and development environment.

## Use When
- The project is a prototype, proof of concept, or early v1.
- Speed matters, but the project should still remain organized and AI-friendly.
- The project may grow later, but full backend domain separation is not needed yet.

## Rules
- Use the standard monorepo structure even for simple projects.
- Use Docker for the local development environment so the project can run consistently across machines.
- Use `apps/` for user-facing applications.
- Use `backend/` when the project needs a backend server.
- Use `packages/` only when code is actually shared.
- Use `config/` for shared project configuration.
- Use `docs/` for human-facing documentation.
- Use `specs/` for AI-facing project instructions and project context.
- Use `infrastructure/` for Docker, local services, queues, gateways, and deployment support.
- Use `tools/` only when repo automation, generators, setup utilities, or internal CLIs are needed.
- Keep the backend simple unless the project clearly needs separate backend modules.
- Avoid microservices in v1 unless there is a strong project-specific reason.
- Avoid unnecessary infrastructure such as queues, caches, workers, or gateways until they are needed.
- Do not confuse simple with unstructured.
- Optimize for shipping, learning, and validating the idea.

## Patterns
- Simple frontend-only project:
  - `apps/web`
  - `config/`
  - `docs/`
  - `specs/`
  - `infrastructure/docker`

- Simple full-stack project:
  - `apps/web`
  - `backend/api`
  - `config/`
  - `docs/`
  - `specs/`
  - `infrastructure/docker`

- Add `packages/` when shared types, utilities, UI components, or API clients are needed.
- Add backend modules only when responsibilities are clear.
- Add queues, workers, gateways, or supporting services only when the project actually needs them.
- Docker should define the development environment first. Production containerization can be added later when needed.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project intentionally avoids Docker.
- The project is a quick throwaway experiment.
- The project must start with a modular backend.
- The project requires true microservices from the beginning.
- A specific hosting platform or framework requires a different structure.

## Anti-Patterns
- Treating an MVP as an excuse for messy folder structure.
- Skipping `specs/` because the project is small.
- Building microservices before the product has users or clear domain boundaries.
- Adding queues, workers, caches, or gateways before there is a real need.
- Creating shared packages before code is actually shared.
- Over-engineering the data model for features that may never ship.
- Complex CI/CD before simple local Docker development works.
- Storing important project decisions only in chat history instead of `specs/`.

## Related Files
- architecture/monorepo.md
- architecture/modular-backend.md
- architecture/microservices.md