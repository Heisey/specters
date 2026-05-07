# Next.js SSR

## Purpose
Rules for projects using Next.js with server-side rendering.

## Use When
- Public-facing pages that need SEO
- Marketing pages, portfolios, or content-heavy sites
- Fast first render is a requirement

## Rules
- Use SSR when SEO, public pages, or fast first render matter.
- Keep server/client component boundaries explicit — mark with `"use client"` only when necessary.
- Avoid unnecessary client components — default to server components.
- Use Next.js App Router conventions for routing.
- Use `layout.tsx` for shared UI, `page.tsx` for route-specific content.
- Fetch data in server components unless interactivity requires client-side fetching.

## Patterns
- Server components fetch data directly — no React Query needed at the server layer.
- Use React Query on the client for interactive data fetching after initial load.
- Keep `"use client"` boundaries as low in the tree as possible.

## Anti-Patterns
- Making everything a client component by default.
- Mixing data fetching patterns inconsistently across routes.
- Using the Pages Router in new projects.

## Related Files
- frontend/react-query-zustand.md
- decisions/rendering-strategy.md
