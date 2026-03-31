# REFACTOR_PAGE_PROMPT

Use this prompt when refactoring an existing page to align with system standards.

## Objective

Refactor one page (and minimal related files) for architecture consistency, reuse, and SEO quality without unnecessary churn.

## Instructions to AI

1. Plan first with a before/after structure.
2. Keep changes surgical; do not refactor unrelated routes/features.
3. Move hardcoded routes to `src/lib/paths.ts` and replace inline strings.
4. Move inline menu/footer data to config if present.
5. Extract reusable sections/components from oversized `page.tsx` files where needed.
6. Ensure metadata and canonical usage go through shared helpers.
7. Keep Server Components default and isolate required client interactivity to leaf components.
8. Preserve existing behavior unless explicitly changing UX/content.
9. Remove ad hoc inline CSS where possible; migrate to Tailwind utilities/shared variants.
10. Keep styling aligned with shared primitives, section patterns, and project design tokens.

## Refactor checklist

- [ ] Route constants/handlers centralized
- [ ] Config-first wiring restored
- [ ] SEO metadata complete and specific
- [ ] Heading and semantic structure corrected
- [ ] Reuse improved (less one-off JSX)
- [ ] Diff remains scoped and reviewable
- [ ] Styling normalized (Tailwind-first, minimal/no inline CSS)

## Required output format

Return:

- Before/after architecture notes
- Files changed with rationale
- SEO and route/config improvements made
- Any assumptions or deferred follow-ups
