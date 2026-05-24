# Marketplace

## Purpose
Profile and rules for two-sided marketplace projects.

## Use When
- Building a platform that connects two user types (buyers/sellers, clients/providers, etc.)

## Rules
- Model both sides of the marketplace explicitly from day one (e.g., `Buyer`, `Seller`).
- Payments and payout flows are core — integrate Stripe Connect early.
- Trust and safety features matter — plan for reviews, disputes, and moderation.
- Keep search and discovery simple in v1 — basic filters before advanced ranking.
- Design transaction records carefully — they are the core of the business.

## Patterns
- `Transaction` or `Order` model is the central domain entity.
- Use state machines for transaction lifecycle (pending → confirmed → completed → disputed).
- Separate buyer and seller dashboards if their workflows differ significantly.

## Anti-Patterns
- Building advanced recommendation systems before there is enough data.
- Skipping payout logic and planning to add it later.
- Conflating buyer and seller models into one messy `User` model.
- Over-engineering search in v1.

## Related Files
- core/ai-coding-rules.md
- backend/auth-patterns.md
- database/postgres/postgres.md
- architecture/simple-mvp.md
