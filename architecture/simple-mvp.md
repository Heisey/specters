# Simple MVP

## Purpose
Architecture rules for small, fast-moving MVP projects.

## Use When
- Building a v1 or early product
- The team is small or the project is solo
- Speed of shipping matters more than scalability right now

## Rules
- Prefer simple architecture first — optimize later when the problem is proven.
- Avoid microservices in v1 unless there is a strong, specific reason.
- Keep deployment simple — a single server or simple hosting platform.
- Optimize for learning and shipping, not for hypothetical scale.
- Avoid premature abstraction — write straightforward code.

## Patterns
- Single repo with frontend and backend separated by folder.
- One deployment target to start (Railway, Render, Fly.io, Vercel + separate API).
- Monolithic backend until service boundaries become obvious.

## Anti-Patterns
- Building microservices before the product has users.
- Over-engineering the data model for features that may never ship.
- Complex CI/CD pipelines in v1 when a simple deploy script works.
- Adding infrastructure (queues, caches, workers) before they are needed.

## Related Files
- decisions/architecture-choice.md
- architecture/monorepo.md
