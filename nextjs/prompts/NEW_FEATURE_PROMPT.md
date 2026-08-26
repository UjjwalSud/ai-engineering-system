# NEW_FEATURE_PROMPT

Use this prompt for non-trivial feature work that touches multiple files/routes/components.

## Objective

Implement a feature slice with minimal, surgical changes while preserving SEO-first and config-first architecture.

## Instructions to AI

1. Start with a concrete plan using `docs/MODULE_OUTPUT_FORMAT.md`.
2. Describe architecture impact and list all planned file changes.
3. Respect App Router boundaries and Server Components by default.
4. Route strings must come from `src/lib/paths.ts` (include dynamic helpers where needed).
5. Header/footer/menu data must come from `src/config/site.ts` (`site.navigation` / `site.footer`); labels from `UiText`.
6. Reuse existing section/UI components before creating new variants (see page-composition).
7. Ensure metadata (`buildPageMetadata`), canonical URLs, social metadata, and page JSON-LD are addressed for affected public pages.
8. For forms/APIs, include validation, safe errors, and basic abuse controls.
9. Avoid unrelated refactors; keep scope focused.
10. Keep styling Tailwind-first; avoid inline CSS unless runtime-only constraints require it.
11. Reuse existing UI primitives/section patterns and keep styles consistent with project tokens/design system.

## Required output format

Return:

- Feature plan (with tree)
- Config/path updates
- SEO implications
- Server/client boundary decisions
- Files created and updated
- Assumptions and verification steps
