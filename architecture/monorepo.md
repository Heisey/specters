# Monorepo

## Purpose
Guidance for projects using a monorepo structure with multiple applications, backend services, shared packages, and project-level tooling.

## Use When
- Multiple user-facing apps, backend services, or shared packages belong to the same product.
- Apps and backend services need to share types, utilities, UI components, or API clients.
- The project benefits from a single place to manage dependencies, tooling, specs, and documentation.
- AI agents need a predictable repo structure to understand where code, config, docs, and specs belong.

## Rules
- Use `apps/` for user-facing applications only, such as web, mobile, admin, dashboards, or portals.
- Use `backend/` for API servers, workers, background jobs, and business-logic services.
- Use `infrastructure/` for Docker, queues, gateways, deployment config, and supporting services.
- Use `packages/` for shared code such as types, utilities, UI components, API clients, and reusable helpers.
- Use `config/` for shared project configuration such as TypeScript, linting, formatting, and environment templates.
- Use `tools/` for repo automation, generators, setup utilities, and internal CLIs.
- Use `docs/` for human-readable documentation.
- Use `specs/` for AI-facing project instructions, architecture decisions, and implementation rules.

- Keep app boundaries clear. Apps must not directly import another app’s internal files.
- Shared logic must be placed in `packages/`, not duplicated across apps or backend services.
- Avoid circular dependencies between packages.
- Use path aliases consistently across apps, backend services, and packages.
- Use workspace tooling when the project includes multiple apps, backend services, or shared packages.
- Keep folder names predictable, responsibility-based, and free of ambiguity.

## Patterns
- Root structure:
  - `apps/`
  - `backend/`
  - `infrastructure/`
  - `packages/`
  - `config/`
  - `tools/`
  - `docs/`
  - `specs/`

- Example apps:
  - `apps/web`
  - `apps/mobile`
  - `apps/admin`

- Example backend services:
  - `backend/api`
  - `backend/auth`
  - `backend/workers`

- Example shared packages:
  - `packages/types`
  - `packages/utils`
  - `packages/ui`
  - `packages/api-client`

- Shared TypeScript types must live in `packages/types` or another clearly defined shared package.
- Reusable frontend components must live in `packages/ui` only when they are shared across multiple apps.
- Project-specific components must remain within the app that owns them.

## Exceptions
Override these rules in `PROJECT_CONTEXT.md` when:
- The project is a simple single-app repo that does not require a monorepo structure.
- The project does not include backend services.
- The project does not require shared packages.
- A different folder structure is required for a specific platform or constraint.
- A lightweight or experimental project needs reduced structure for speed.

## Anti-Patterns
- One app importing directly from another app’s source.
- Backend services importing directly from app internals.
- Circular dependencies between packages.
- Duplicating shared logic across apps or backend services instead of moving it into `packages/`.
- Mixing package manager conventions, such as pnpm and npm workspaces, in the same repo.
- Putting everything into one large package and calling it a monorepo.
- Using vague folders like `misc/`, `common/`, or `project/` as dumping grounds.
- Storing important architectural decisions only in chat history instead of `specs/`.

## Related Files
- architecture/simple-mvp.md
- decisions/architecture-choice.md