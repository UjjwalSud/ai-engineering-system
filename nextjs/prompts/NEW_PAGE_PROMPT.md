# NEW_PAGE_PROMPT

Use this prompt when adding a new page route to an existing project.

## Objective

Add one new page with consistent architecture, centralized routing/config, and SEO completeness.

## Instructions to AI

1. Plan first (do not jump straight to code).
2. Confirm required scaffold exists (`site.ts`, `uiText`, `paths.ts`, `metadata.ts`, `schema.ts`). If missing, create it first.
3. Add/update route definitions in `src/lib/paths.ts`; do not hardcode path strings.
4. Update `site.navigation` / `site.footer` if this page should appear there; labels from `UiText`.
5. Build the page under App Router; follow page-composition (stubs vs inline legal vs extract sections).
6. Reuse existing sections/UI patterns wherever possible; use shared `EmptyState` for empty/no-results UI.
7. Implement page metadata via `buildPageMetadata` (or shell helper for stubs).
8. Add `{pageName}PageSchema()` on the page when structured data is appropriate; shared helpers only in `schema.ts`.
9. Keep client components minimal and justified.
10. Use Tailwind utilities and shared class patterns; avoid inline CSS except narrow runtime-only cases.
11. Keep page styling aligned with existing UI primitives, section patterns, and design tokens/theme usage.

## Guardrails

- No inline nav/footer arrays in page/layout components.
- No duplicate metadata logic if helper exists.
- No broad refactors unrelated to this page.
- No one-off inline visual styling when reusable classes/components already exist.

## Required output format

Return:

- Plan summary
- Files changed and why
- Route/config updates
- SEO changes for the page
- Reused components vs newly created components
