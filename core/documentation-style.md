# Documentation Style

## Purpose
How documentation and code comments should be written across projects.

## Use When
- Always included as a base file

## Rules
- Docs should be practical, not fluffy.
- Prefer examples over abstract explanations.
- Add comments only when logic is non-obvious — a hidden constraint, subtle invariant, or workaround.
- Never write comments that describe what the code does (well-named code already does that).
- Keep README files useful for setup and onboarding.
- No multi-paragraph docstrings or multi-line comment blocks.

## Patterns
- README structure: what it is → how to run it → how to use it.
- Inline comment format: one short line explaining the WHY.
- When a workaround is needed, comment with the reason (e.g., a specific library bug or edge case).

## Anti-Patterns
- Comments that restate the function name (`// gets the user`).
- Fluffy intros in README files (`This is an amazing project that...`).
- Outdated comments that no longer match the code.
- Commented-out code left in the codebase.

## Related Files
- core/ai-coding-rules.md
- core/naming-conventions.md
