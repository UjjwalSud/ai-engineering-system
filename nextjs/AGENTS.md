# AGENTS.md — Next.js AI Engineering System

## Purpose

This system defines how AI agents and contributors generate and evolve **Next.js (App Router)** applications in a structured, scalable, and SEO-first way.

Goals: predictable architecture, high SEO quality, reusable patterns, minimal rework.

---

## When this system is appropriate

Use this system for:

- Marketing websites
- SEO-first company websites
- Content-heavy sites (articles, services, landing pages)
- Next.js App Router frontend applications

Do not use this system **as-is** for:

- Dashboard-heavy internal apps with little SEO value
- Highly interactive SPA-style products unless adapted with a stronger client-state architecture

---

## Core stack

- Next.js (latest, App Router only)
- TypeScript — enable **`strict`** in `tsconfig.json` on new projects; add explicit validation where the project is not strict
- Tailwind CSS
- Server Components by default; Client Components only when required

---

## Golden rules

1. **SEO-first** for every public page
2. **Config-first** — no hardcoded navigation or routes
3. **Server Components** by default
4. **Reusable sections** over one-off page blobs
5. **Single source of truth** for paths, nav, footer, site settings, metadata defaults
6. **Consistent UI** patterns across pages
7. **Styling consistency** — Tailwind-first, reusable classes/components, avoid inline styles
8. **Minimal client-side JavaScript**
9. **No unnecessary dependencies**
10. **Clear separation of concerns**
11. **Every change** aligns with the architecture in the docs below

---

## Architecture at a glance

- **Routes** live under `src/app/` (with route groups as needed).
- **Composition** uses `src/components/` (`layout/`, `sections/`, `shared/`, `ui/`).
- **Config** is centralized: `src/config/*` and `src/lib/*` single-source modules.

Details, diagrams, and API/security notes: **[docs/01_SYSTEM_ARCHITECTURE.md](docs/01_SYSTEM_ARCHITECTURE.md)**.

---

## Key docs (read in this order for new work)

| Document | Purpose |
|----------|---------|
| [docs/01_SYSTEM_ARCHITECTURE.md](docs/01_SYSTEM_ARCHITECTURE.md) | Stack, folders, data flow, rendering, APIs |
| [docs/02_SEO_ARCHITECTURE.md](docs/02_SEO_ARCHITECTURE.md) | SEO policies: metadata, canonicals, robots, sitemap, OG/Twitter, JSON-LD |
| [docs/03_CONTENT_MODEL.md](docs/03_CONTENT_MODEL.md) | Supported page/content types and their structure expectations |
| [docs/04_UI_SYSTEM.md](docs/04_UI_SYSTEM.md) | Reusable layout and UI patterns for consistent generation |
| [docs/ANTI_PATTERNS.md](docs/ANTI_PATTERNS.md) | What not to do — with severity and rule links |
| [docs/MODULE_OUTPUT_FORMAT.md](docs/MODULE_OUTPUT_FORMAT.md) | **Plan template** before writing a feature |

---

## Cursor rules (`rules/`)

Rules in [`rules/`](./rules/) encode day-to-day constraints. They overlap on purpose (e.g. config + routes); treat them as **one policy**.
Visual consistency should come from reusable patterns, shared classes/components, and theme/token usage rather than ad hoc inline styling.

### Index

| Rule file | Purpose |
|-----------|---------|
| [config-first.mdc](rules/config-first.mdc) | Menus, footer, routes from config — not inline strings |
| [no-hardcoding-rule.mdc](rules/no-hardcoding-rule.mdc) | No hardcoded routes, nav, footer, metadata defaults, site settings |
| [routes-and-menus-rule.mdc](rules/routes-and-menus-rule.mdc) | Central paths; header/footer use config |
| [seo-first.mdc](rules/seo-first.mdc) | Metadata, one H1, heading hierarchy, semantics, indexability |
| [server-component-first.mdc](rules/server-component-first.mdc) | RSC default; `"use client"` only when needed |
| [ui-consistency-rule.mdc](rules/ui-consistency-rule.mdc) | Reuse sections; consistent layout and typography |
| [styling-standards.mdc](rules/styling-standards.mdc) | Tailwind-first styling; avoid inline CSS; reuse tokens/patterns |
| [api-validation-and-errors.mdc](rules/api-validation-and-errors.mdc) | Validate API inputs; consistent JSON errors |
| [minimal-surgical-changes.mdc](rules/minimal-surgical-changes.mdc) | Small, focused diffs; do not refactor unrelated code |
| [imports-path-alias.mdc](rules/imports-path-alias.mdc) | Prefer `@/` (or project alias) over deep relatives |
| [env-and-secrets.mdc](rules/env-and-secrets.mdc) | Server-only secrets; safe public env usage |
| [accessibility-baseline.mdc](rules/accessibility-baseline.mdc) | Names for controls, focus, basic a11y |

**Cursor:** If your workspace only loads rules from `.cursor/rules`, copy or symlink these files there, or add a project rule that points to `rules/`.

---

## Required startup scaffold (mandatory before feature work)

Before creating pages or features, initialize these files as single sources of truth:

- `src/config/site.ts`
- `src/config/navigation.ts`
- `src/config/footer.ts`
- `src/lib/paths.ts`
- `src/lib/metadata.ts`
- `src/lib/schema.ts`

No feature implementation should bypass this baseline. If one of these files is missing, create it first.

**Startup protocol (required):**

1. Create/confirm this scaffold.
2. Define base route entries in `src/lib/paths.ts` (`home`, `about`, `contact`, and dynamic helpers like `article(slug)` when needed).
3. Wire initial header/footer from config (`navigation.ts`, `footer.ts`) using those paths.
4. Set metadata and schema defaults in `src/lib/metadata.ts` and `src/lib/schema.ts`.
5. Only then begin feature-specific page/component work.

---

## Environment baseline

- Include `.env.example` in every generated project.
- Document each variable with purpose and safe example values.
- Keep secret variables server-only (no `NEXT_PUBLIC_` prefix).
- Use `NEXT_PUBLIC_*` only for values intentionally exposed to browsers.
- Avoid direct `process.env` access in scattered files; use focused server-side modules/helpers.

---

## Before generating code

AI must **not** jump straight to code for non-trivial work. Provide a plan that covers:

1. Architecture impact (see [MODULE_OUTPUT_FORMAT.md](docs/MODULE_OUTPUT_FORMAT.md))
2. Folder/file tree (new and changed)
3. Routes and `paths.ts` updates
4. Config updates (`navigation`, `footer`, `site` as needed)
5. SEO (titles, descriptions, OG/JSON-LD if applicable)
6. Which **existing** section/components to reuse vs create

Full checklist: **[docs/MODULE_OUTPUT_FORMAT.md](docs/MODULE_OUTPUT_FORMAT.md)**.

---

## After implementation (output expectations)

Summarize for the reviewer:

- Files created and updated
- Config and SEO changes
- Reused vs new components
- Assumptions

---

## Philosophy

This is not about generating isolated pages. It is about generating **consistent systems**, **scalable architecture**, and **production-ready code**. Every deliverable should feel like part of one well-designed application.
