# Modular Backend

## Purpose
Guidance for projects that start with multiple backend services or modules while avoiding the full complexity of true microservices.

## Use When
- The project needs clear backend separation from the start.
- The project has common backend domains such as auth, records, billing, files, notifications, or AI processing.
- Services should be organized independently but do not yet require full microservice complexity.
- The project may later evolve into true microservices.

## Rules
- Use `backend/` for backend servers, modules, workers, and business-logic services.
- Start most projects with a modular backend instead of true microservices.
- Keep each backend module focused on one clear responsibility.
- Use separate folders for major backend domains.
- Keep shared code in `packages/`, not duplicated across backend modules.
- Backend modules must not import from `apps/`.
- Apps must communicate with backend modules through APIs, not direct imports.
- Store backend architecture decisions, service boundaries, and module responsibilities in `specs/`.
- Use `infrastructure/` for supporting systems such as Docker, queues, gateways, and reverse proxies.
- Only move to true microservices when independent deployment, scaling, or data ownership is required.

## Patterns
- Default backend structure:
  - `backend/auth`
  - `backend/records`

- Common optional backend modules:
  - `backend/files`
  - `backend/billing`
  - `backend/notifications`
  - `backend/ai`
  - `backend/workers`

- Shared backend code:
  - `packages/types`
  - `packages/utils`
  - `packages/api-client`
  - `packages/contracts`

- Each backend module should define:
  - its responsibility
  - its public API
  - its data ownership
  - its dependencies
  - its boundaries

- Backend modules may share the same deployment early on, but should remain organized so they can be separated later if needed.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project is small enough to use a single backend server.
- The project has no backend.
- The project requires true microservices from the start.
- The project is a quick prototype where structure would slow development too much.

## Anti-Patterns
- Calling a modular backend “microservices” when services are not independently deployed or isolated.
- Creating too many backend modules before responsibilities are clear.
- Backend modules importing directly from frontend app internals.
- Apps bypassing APIs and importing backend logic directly.
- Duplicating shared backend utilities instead of moving them into `packages/`.
- Mixing infrastructure concerns into backend module folders.
- Storing backend decisions only in chat history instead of `specs/`.

## Related Files
- architecture/monorepo.md
- architecture/microservices.md
- architecture/simple-mvp.md
- decisions/architecture-choice.md