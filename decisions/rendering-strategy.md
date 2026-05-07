# Rendering Strategy

## Purpose
How to choose between SPA and SSR rendering approaches.

## Options
- SPA → `frontend/vite-spa.md`
- SSR → `frontend/nextjs-ssr.md`

## Decision Rules

| Scenario | Choice |
|---|---|
| Public marketing pages needing SEO | SSR (Next.js) |
| Internal dashboard or admin tool | SPA (Vite) |
| Portfolio site with SEO needs | SSR or static generation |
| User-facing app with both public and private pages | SSR (Next.js App Router handles both) |
| Rapid MVP with no SEO requirement | SPA (Vite — simpler setup) |

## Default
When in doubt and SEO is not a concern: **SPA with Vite**.
When public-facing pages exist: **SSR with Next.js**.

## Related Files
- frontend/vite-spa.md
- frontend/nextjs-ssr.md
