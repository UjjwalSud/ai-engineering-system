# NEW_PROJECT_PROMPT

Use this prompt when creating a new Next.js App Router project in this system.

## Objective

Generate a production-ready, SEO-first baseline with mandatory scaffold files and reusable architecture before feature implementation.

## Instructions to AI

1. Plan first using `docs/MODULE_OUTPUT_FORMAT.md` style sections (summary, files, config, SEO, boundaries, verification).
2. Respect App Router (`src/app`) and Server Components by default.
3. Create the required startup scaffold before any page-specific feature work:
   - `src/config/site.ts` (identity + `navigation` + `footer` + `socialLinks`)
   - `src/constants/uiText` (`UiText`)
   - `src/lib/paths.ts` (include `encodePathSlug` / `decodePathSlug` for dynamic slugs)
   - `src/lib/metadata.ts`
   - `src/lib/schema.ts`
4. Add base route helpers in `paths.ts` at minimum for:
   - `home`
   - `about`
   - `contact`
   - `article(slug)` (helper if article routes are planned)
5. Ensure header/footer consume `site.navigation` / `site.footer` with `UiText` labels — never hardcoded arrays or copy in components.
6. Implement SEO baseline:
   - `buildRootMetadata` / `buildPageMetadata` in `metadata.ts`
   - Canonical URL strategy
   - OG/Twitter defaults
   - Page-local `{pageName}PageSchema()`; shared helpers only in `schema.ts`
   - robots/sitemap strategy
7. Add environment baseline:
   - `.env.example`
   - documented variables with server-only vs `NEXT_PUBLIC_*` separation
8. Reuse shared sections/components and avoid one-off page blobs (see page-composition rules).
9. Avoid hardcoded route strings, brand constants, and UI copy in UI files.
10. Enforce styling standards: Tailwind-first, avoid inline CSS, reuse shared style patterns/tokens.
11. Keep styling aligned with design system consistency (spacing, typography, colors, component states).

## Required output format

Return:

- Planned file tree (new/updated)
- Implementation notes by area (config, routes, SEO, UI, env)
- Exact files created and updated
- Key assumptions
- Verification checklist (lint/build/manual smoke)
