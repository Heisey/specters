# AI Tool

## Purpose
Profile and rules for projects that expose AI model capabilities to users.

## Use When
- Building a product powered by LLMs, embeddings, or AI APIs
- The core value is AI-generated output (text, images, code, etc.)

## Rules
- Treat AI API calls as external network calls — handle errors and rate limits explicitly.
- Stream responses when latency would otherwise make the UI feel slow.
- Log inputs and outputs during development for debugging — remove or gate in production.
- Design the UX around the latency of AI calls — use loading states and optimistic UI.
- Keep prompt logic in dedicated files, not scattered through controllers.

## Patterns
- Separate prompt construction from business logic.
- Use a thin wrapper around the AI SDK so it can be swapped or mocked in tests.
- Rate limit AI endpoints to protect cost and prevent abuse.
- Cache deterministic AI outputs when appropriate.

## Anti-Patterns
- Hardcoding prompts inline in route handlers.
- No error handling for AI API failures (quota exceeded, model errors).
- Streaming to the client without proper backpressure handling.
- Trusting AI output without any validation for safety-critical paths.

## Related Files
- core/ai-coding-rules.md
- backend/express-sequelize.md
- decisions/architecture-choice.md
