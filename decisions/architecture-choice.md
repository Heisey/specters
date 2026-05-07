# Architecture Choice

## Purpose
How to choose the right architecture pattern for a project.

## Default
**Simple MVP** — always start here unless there is a clear reason not to.

## Decision Rules

| Scenario | Choice |
|---|---|
| New product, small team, v1 | Simple MVP |
| Multiple apps or packages sharing code | Monorepo |
| Services with clear independent teams and scale requirements | Microservices |
| Extensibility is a primary product requirement | Plugin System |

## Rules
- Start with Simple MVP unless the product design clearly requires something else.
- Move to Monorepo when you have two or more apps/packages that share meaningful code.
- Move to Microservices only when service boundaries are proven and the team has the operational maturity to manage them.
- Plugin System is only needed when external extensibility is a core feature.

## Anti-Patterns
- Choosing microservices for a one-person project.
- Building a plugin system before the core product is stable.
- Starting with a monorepo when there is only one app.

## Related Files
- architecture/simple-mvp.md
- architecture/monorepo.md
- architecture/microservices.md
- architecture/plugin-system.md
