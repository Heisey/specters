# SaaS Application

## Purpose
Profile and rules for standard SaaS application projects.

## Use When
- Building a multi-user web application with accounts, a dashboard, and recurring value

## Rules
- Always include auth from day one — every SaaS app needs it.
- Build a simple onboarding flow early — first-run experience matters.
- Use a relational database by default (Postgres preferred).
- Design for multi-tenancy early even if v1 is single-tenant.
- Keep the billing integration simple in v1 (Stripe is the default).

## Patterns
- Core areas: auth, dashboard, settings, billing.
- Use soft deletes for user-facing data (add `deletedAt` column).
- Track created/updated timestamps on all key records.
- Use role-based access control only when multiple user roles are a real requirement.

## Anti-Patterns
- Skipping auth to ship faster — add it from the start.
- Building billing before there are users.
- Over-engineering multi-tenancy before you have more than one tenant.

## Related Files
- core/ai-coding-rules.md
- backend/auth-patterns.md
- database/postgres.md
- decisions/architecture-choice.md
