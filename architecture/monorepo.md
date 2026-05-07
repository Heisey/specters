# Monorepo

## Purpose
Guidance for projects using a monorepo structure with multiple apps or packages.

## Use When
- Multiple apps or services share common code
- A shared component library or utility package is needed
- The team wants a single place to manage dependencies and tooling

## Rules
- Use shared libraries (`packages/`) for common code — types, utilities, UI components.
- Keep app boundaries clear — apps should not directly import each other's internals.
- Avoid circular dependencies between packages.
- Use path aliases consistently across apps.
- Use a workspace tool: pnpm workspaces, npm workspaces, or Turborepo.

## Patterns
- Structure: `apps/web`, `apps/api`, `packages/ui`, `packages/shared`.
- Shared TypeScript types live in `packages/shared` or `packages/types`.
- Use Turborepo for task orchestration (build, test, lint pipelines).

## Anti-Patterns
- One app importing directly from another app's source.
- Circular package dependencies.
- Mixing package manager conventions (e.g., mixing pnpm and npm workspaces).
- Putting everything in one giant package and calling it a monorepo.

## Related Files
- architecture/simple-mvp.md
- decisions/architecture-choice.md
