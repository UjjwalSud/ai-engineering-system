# Anti-Patterns

This list complements the Cursor rules under [`rules/`](../rules/). Each item maps to **severity** and the **rule file** that encodes the expectation.

**Severity**

- **Block** — do not ship; violates project standards.
- **Discourage** — avoid unless there is a documented exception.

---

## Routing, config, and duplication

| Anti-pattern | Severity | Prefer | Rule |
|--------------|----------|--------|------|
| Hardcoding path strings in components (`href="/about"`) | Block | Import from `lib/paths.ts` or use typed helpers | [`routes-and-menus-rule.mdc`](../rules/routes-and-menus-rule.mdc), [`no-hardcoding-rule.mdc`](../rules/no-hardcoding-rule.mdc) |
| Duplicating nav or footer items in multiple files | Block | Single definitions in `config/navigation.ts` and `config/footer.ts` | [`config-first.mdc`](../rules/config-first.mdc) |
| Copy-pasting site name, base URL, or brand strings | Block | `config/site.ts` and shared metadata helpers | [`config-first.mdc`](../rules/config-first.mdc) |
| Scattering feature flags or copy in random components | Discourage | Central config or content modules | [`config-first.mdc`](../rules/config-first.mdc) |

---

## SEO and markup

| Anti-pattern | Severity | Prefer | Rule |
|--------------|----------|--------|------|
| Missing or generic `<title>` / meta description on public pages | Block | `generateMetadata` or exports with real copy | [`seo-first.mdc`](../rules/seo-first.mdc) |
| Multiple `<h1>` or skipped heading levels for styling | Block | One `<h1>`, then `h2` → `h3` in order | [`seo-first.mdc`](../rules/seo-first.mdc) |
| Non-semantic div soup for navigations and articles | Discourage | `nav`, `main`, `article`, `section`, `header`, `footer` where appropriate | [`seo-first.mdc`](../rules/seo-first.mdc) |
| Duplicating metadata logic in every page | Block | Shared helpers in `lib/metadata.ts` | [`seo-first.mdc`](../rules/seo-first.mdc) |

---

## Components and UI

| Anti-pattern | Severity | Prefer | Rule |
|--------------|----------|--------|------|
| One giant `page.tsx` with hundreds of lines of JSX | Block | Thin page + `sections/` (and optional `views/`) | [`ui-consistency-rule.mdc`](../rules/ui-consistency-rule.mdc) |
| New Hero/CTA/Feature block per page instead of reusing sections | Discourage | Parameterized section components | [`ui-consistency-rule.mdc`](../rules/ui-consistency-rule.mdc) |
| Random spacing, typography, or color per page | Discourage | Shared tokens, utilities, and UI primitives | [`ui-consistency-rule.mdc`](../rules/ui-consistency-rule.mdc) |

---

## Server vs client

| Anti-pattern | Severity | Prefer | Rule |
|--------------|----------|--------|------|
| `"use client"` on a whole page or layout “just in case” | Block | Server by default; client only where needed | [`server-component-first.mdc`](../rules/server-component-first.mdc) |
| Importing heavy client-only libraries in server components without splitting | Discourage | Dynamic import or a small client leaf component | [`server-component-first.mdc`](../rules/server-component-first.mdc) |

---

## APIs, env, and dependencies

| Anti-pattern | Severity | Prefer | Rule |
|--------------|----------|--------|------|
| Trusting JSON body shape without validation | Block | Zod (or equivalent) in `route.ts` | [`api-validation-and-errors.mdc`](../rules/api-validation-and-errors.mdc) |
| Different error JSON shapes per endpoint | Discourage | Shared error helper and status conventions | [`api-validation-and-errors.mdc`](../rules/api-validation-and-errors.mdc) |
| `process.env` secret keys in client components | Block | Server-only usage; `NEXT_PUBLIC_*` only for truly public values | [`env-and-secrets.mdc`](../rules/env-and-secrets.mdc) |
| New npm dependency for a one-line utility | Discourage | Inline or `lib/utils.ts` | AGENTS.md golden rules |

---

## Change hygiene

| Anti-pattern | Severity | Prefer | Rule |
|--------------|----------|--------|------|
| Refactoring unrelated modules while fixing a bug | Discourage | Minimal, surgical diffs | [`minimal-surgical-changes.mdc`](../rules/minimal-surgical-changes.mdc) |
| Deep relative imports across features (`../../../`) | Discourage | `@/` (or project) aliases | [`imports-path-alias.mdc`](../rules/imports-path-alias.mdc) |

---

## Accessibility (if `accessibility-baseline.mdc` is enabled)

| Anti-pattern | Severity | Prefer |
|--------------|----------|--------|
| Icon-only buttons without accessible names | Block | `aria-label` or visible text |
| Click handlers on non-interactive elements without keyboard support | Discourage | `button` / `Link` / proper roles |

---

## Quick “do not” list (strict)

Do **not**:

- Hardcode routes inside components.
- Duplicate metadata logic across pages without shared helpers.
- Create large monolithic page components.
- Use client components without a concrete need.
- Introduce random UI patterns that break visual consistency.
- Add dependencies without clear need.
- Scatter config across many ad-hoc files.
- Ignore SEO structure and heading rules.

For the positive workflow (plan before code), see [MODULE_OUTPUT_FORMAT.md](./MODULE_OUTPUT_FORMAT.md).
