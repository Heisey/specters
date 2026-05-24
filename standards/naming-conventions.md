# Naming Conventions

## Purpose
Standard naming preferences across all projects.

## Use When
- Always included as a base file

## Rules
- Use clear, descriptive names.
- Prefer domain-specific names: `UserRecord`, `BuilderPageModel`, `RequestError`.
- Avoid vague names like `data`, `item`, `thing` unless they are local and obvious.
- React components: PascalCase (`UserCard`, `DashboardPage`).
- Functions and variables: camelCase (`getUserById`, `isLoading`).
- Constants that are truly constant: UPPER_SNAKE_CASE (`MAX_RETRY_COUNT`).
- Files: kebab-case unless the project already uses a different convention (`user-card.tsx`).
- TypeScript types and interfaces: PascalCase (`UserRecord`, `ApiResponse`).
- Boolean variables: prefix with `is`, `has`, or `can` (`isLoading`, `hasError`, `canEdit`).

## Patterns
- Name things by what they are, not what they do: `UserList` not `RenderUsers`.
- Event handlers: prefix with `handle` (`handleSubmit`, `handleClose`).
- Async functions: name clearly without redundant `async` suffix.

## Anti-Patterns
- Single-letter variables outside of short loop indices.
- Abbreviations that are not universally understood (`usrRcd` instead of `userRecord`).
- Names that describe implementation instead of intent.

## Related Files
- core/ai-coding-rules.md
- core/documentation-style.md
