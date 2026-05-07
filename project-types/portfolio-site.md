# Portfolio Site

## Purpose
Profile and rules for personal portfolio or showcase websites.

## Use When
- Building a personal portfolio, creative showcase, or resume site

## Rules
- SEO matters — use SSR or static generation.
- Performance matters — optimize images, minimize JS.
- Keep it simple — no backend needed unless there is a contact form or CMS.
- Use a headless CMS or markdown files for content if updates are frequent.

## Patterns
- Next.js with static generation or ISR for most pages.
- Markdown or MDX for blog posts and project write-ups.
- Deploy on Vercel or Netlify.

## Anti-Patterns
- Overbuilding the stack for what is essentially a static site.
- Client-side rendering for public content that should be indexed.
- Adding a database before it is needed.

## Related Files
- frontend/nextjs-ssr.md
- decisions/rendering-strategy.md
