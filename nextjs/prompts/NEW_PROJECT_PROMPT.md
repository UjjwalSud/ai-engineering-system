# NEW_PROJECT_PROMPT

Use this prompt when creating a new Next.js App Router project in this system.

## Objective

Generate a production-ready, SEO-first baseline with mandatory scaffold files and reusable architecture before feature implementation.

## Instructions to AI

1. Plan first using `docs/MODULE_OUTPUT_FORMAT.md` style sections (summary, files, config, SEO, boundaries, verification).
2. Respect App Router (`src/app`) and Server Components by default.
3. Create the required startup scaffold before any page-specific feature work:
   - `src/config/site.ts`
   - `src/config/navigation.ts`
   - `src/config/footer.ts`
   - `src/lib/paths.ts`
   - `src/lib/metadata.ts`
   - `src/lib/schema.ts`
4. Add base route helpers in `paths.ts` at minimum for:
   - `home`
   - `about`
   - `contact`
   - `article(slug)` (helper if article routes are planned)
5. Ensure header/footer consume `navigation.ts` and `footer.ts`, never hardcoded arrays in components.
6. Implement SEO baseline:
   - Metadata defaults in `metadata.ts`
   - Canonical URL strategy
   - OG/Twitter defaults
   - robots/sitemap strategy
7. Add environment baseline:
   - `.env.example`
   - documented variables with server-only vs `NEXT_PUBLIC_*` separation
8. Reuse shared sections/components and avoid one-off page blobs.
9. Avoid hardcoded route strings and brand constants in UI files.
10. Enforce styling standards: Tailwind-first, avoid inline CSS, reuse shared style patterns/tokens.
11. Keep styling aligned with design system consistency (spacing, typography, colors, component states).

## Required output format

Return:

- Planned file tree (new/updated)
- Implementation notes by area (config, routes, SEO, UI, env)
- Exact files created and updated
- Key assumptions
- Verification checklist (lint/build/manual smoke)
