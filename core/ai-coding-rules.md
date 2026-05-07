# AI Coding Rules

## Purpose
General rules for AI coding agents working on any project in this playbook.

## Use When
- Starting any new project
- Always included as a base file

## Rules
- Follow existing project structure before adding new patterns.
- Prefer simple, readable code over clever code.
- Do not introduce new libraries unless requested or clearly justified.
- Keep files focused and small.
- Use TypeScript strictly — no implicit `any`.
- Explain major architecture changes before applying them.
- Avoid rewriting working systems unnecessarily.
- Respect project-specific overrides in `PROJECT_CONTEXT.md`.
- Do not add error handling for scenarios that cannot happen.
- Do not add features beyond what the task requires.
- Default to writing no comments — only add one when the WHY is non-obvious.

## Patterns
- Read `PROJECT_CONTEXT.md` before generating code.
- Follow the most project-specific rule when rules conflict.
- Ask for clarification when requirements are ambiguous.

## Anti-Patterns
- Adding complexity for hypothetical future requirements.
- Introducing abstractions before they are needed (three similar lines beats a premature abstraction).
- Rewriting or refactoring code that is not part of the current task.
- Guessing at intent — ask instead.

## Related Files
- core/naming-conventions.md
- core/documentation-style.md
- templates/PROJECT_CONTEXT.md
