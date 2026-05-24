# Material UI (MUI)

## Purpose
Rules for projects using Material UI as the component library.

## Use When
- Building dashboards, admin tools, or internal apps
- Speed and consistency are prioritized over highly custom design

## Rules
- Use MUI components for speed and consistency.
- Use theme tokens (`theme.palette`, `theme.spacing`) instead of hardcoded values.
- Avoid over-customizing MUI components unless the design clearly requires it.
- Extend the theme for project-specific colors, typography, and spacing.
- Use `sx` prop for one-off overrides; use `styled()` for reusable custom variants.

## Patterns
- Define the theme in a single `theme.ts` file.
- Use `ThemeProvider` at the app root.
- Use MUI's `Grid` or `Stack` for layout.

## Anti-Patterns
- Hardcoding colors or spacing values instead of using theme tokens.
- Creating custom components that duplicate existing MUI components.
- Overriding MUI styles with CSS specificity hacks.

## Related Files
- frontend/vite-spa.md
- frontend/styled-components.md
