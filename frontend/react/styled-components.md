# Styled Components

## Purpose
Rules for projects using styled-components for custom UI styling.

## Use When
- Building a product with a polished custom design system
- Design requires full control over styling
- MUI is not being used

## Rules
- Use styled-components for custom polished UI.
- Keep styled blocks close to the component they style unless they are reused globally.
- Avoid huge style files — split into logical modules.
- Use theme props for colors, spacing, and typography instead of hardcoded values.
- Use `ThemeProvider` at the app root.

## Patterns
- Co-locate small styled components with their parent component file.
- Extract shared styled primitives into a `styles/` or `ui/` folder.
- Use `css` helper for conditional styles.

## Anti-Patterns
- One giant `global.css` with all styles.
- Duplicating styled components across files instead of extracting shared ones.
- Hardcoding values instead of using theme variables.

## Related Files
- frontend/mui.md
- frontend/vite-spa.md
