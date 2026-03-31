# UI System — Reusable Patterns

This document defines reusable UI composition patterns to prevent random, one-off page generation.

---

## UI system goals

- Keep page composition consistent across routes.
- Maximize section/component reuse.
- Keep spacing, typography, and interaction patterns predictable.

---

## Layout wrappers

- Use route/layout wrappers to enforce consistent container widths and vertical rhythm.
- Keep `app/**/page.tsx` thin: assemble reusable sections instead of large inline JSX blocks.
- Prefer shared layout primitives in `components/layout` and `components/ui`.

---

## Header and footer expectations

- Header navigation is rendered from `src/config/navigation.ts`.
- Footer links are rendered from `src/config/footer.ts`.
- Route strings should come from `src/lib/paths.ts`, not inline literals.
- Visual style may vary by project brand, but information architecture should stay centralized.

---

## Hero sections

- Use a reusable hero section pattern for top-of-page context.
- Typical structure: eyebrow (optional), H1, supporting paragraph, primary/secondary CTA.
- Keep CTAs linked via path helpers/config.

---

## Section headings

- Enforce one logical `h1` per page.
- Use `h2` for major sections and `h3` for nested groups.
- Avoid skipping heading levels solely for visual styling.

---

## Card patterns

- Use reusable card components for features, services, articles, and testimonials.
- Keep card anatomy consistent: title, summary, optional metadata, link/action.
- Avoid creating new card variants per page unless there is a real UX need.

---

## CTA sections

- Reuse a small set of CTA patterns (single CTA, dual CTA, inline CTA).
- Keep copy concise and action-oriented.
- Ensure CTA destinations map to `paths.ts` and are represented in navigation strategy where relevant.

---

## Stats sections

- Use a shared stat grid pattern for numeric proof points.
- Keep labels short and values scannable.
- Avoid decorative stats with no business context.

---

## FAQ sections

- Reuse one FAQ section component with consistent item structure.
- Answers should be concise and factual.
- Add `FAQPage` schema only when FAQ content is substantive and visible.

---

## Article cards and lists

- Use shared article cards for listings and related content modules.
- Keep metadata placement consistent (date/category/read-time as applicable).
- Use semantic list structures and descriptive links.

---

## Consistency expectations

Across pages, keep:

- Spacing scale consistent (no random section padding jumps).
- Typography scale and heading rhythm consistent.
- Button variants and link styles consistent.
- Surface/visual hierarchy consistent for equivalent section types.

When uncertain, reuse an existing section before creating a new one.

---

## Styling standards

- Tailwind CSS is the default styling system for layout, spacing, typography, colors, and states.
- Avoid inline CSS (`style={{}}`) unless a narrowly scoped runtime-only case requires it.
- Prefer shared theme/token usage (Tailwind config, CSS variables, common utility patterns) over one-off visual values.
- If a visual pattern repeats, extract/reuse a shared component or variant instead of duplicating class sets.
- Keep spacing, typography, color usage, and component states consistent across pages and route groups.

---

## Related docs

- [01_SYSTEM_ARCHITECTURE.md](./01_SYSTEM_ARCHITECTURE.md)
- [02_SEO_ARCHITECTURE.md](./02_SEO_ARCHITECTURE.md)
- [03_CONTENT_MODEL.md](./03_CONTENT_MODEL.md)
