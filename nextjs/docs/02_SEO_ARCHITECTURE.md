# SEO Architecture — Next.js (App Router)

This document defines practical SEO policies for public-facing Next.js applications generated with this system.

---

## Metadata policy

- Every public route must provide metadata via static `metadata` export or `generateMetadata`.
- Centralize defaults/builders in `src/lib/metadata.ts`.
- Avoid page-by-page copy-paste metadata logic.
- Metadata should reflect real page intent, not placeholder text.

---

## Title and description rules

- Each indexable page must have a specific, human-readable `<title>`.
- Each indexable page must include a concise meta description (typically one to two sentences).
- Do not reuse identical titles/descriptions across many pages unless intentionally canonicalized.
- Prefer deterministic title patterns, e.g. `"Page Name | Brand"` or `"Topic - Brand"`.

---

## Canonical URL policy

- Use one canonical host/protocol for all public pages.
- Build canonical URLs from centralized site config (`src/config/site.ts`) and metadata helpers.
- Dynamic pages must emit canonical URLs for the resolved slug/path.
- If duplicate route shapes exist (query variants, tracking params), canonicalize to the primary URL.

---

## Robots policy

- Public content pages should default to indexable (`index,follow`) unless there is a clear reason not to index.
- Non-public pages (preview, admin, internal tools, thank-you pages as needed) should set `noindex,nofollow`.
- Do not rely on robots.txt alone to prevent indexing of sensitive pages.

---

## Sitemap policy

- Maintain a sitemap strategy aligned with App Router route ownership.
- Include only canonical, indexable URLs.
- Exclude non-public, duplicate, or blocked URLs.
- Keep dynamic content inclusion deterministic (published-only, stable slugs).

---

## OpenGraph and Twitter policy

- Set OG/Twitter metadata for all high-value public pages (homepage, service pages, articles, major landing pages).
- Use page-specific title/description where appropriate.
- Provide stable, absolute social image URLs.
- Keep OG/Twitter defaults centralized in `src/lib/metadata.ts` with optional per-page overrides.

---

## JSON-LD / structured data policy

- Build schema payloads from `src/lib/schema.ts` helpers.
- Use appropriate schema types by page intent (e.g. `Organization`, `WebSite`, `Article`, `BreadcrumbList`, `FAQPage` when valid).
- Ensure structured data matches visible content (no misleading or hidden claims).
- Prefer one coherent schema strategy per page over many fragmented scripts.

---

## Internal linking expectations

- Ensure key pages are reachable through header/footer/navigation and contextual links.
- Use path helpers from `src/lib/paths.ts`; avoid hardcoded URL strings.
- Add contextual links between related articles, services, and supporting pages.
- Keep anchor text descriptive and content-relevant.

---

## Indexability guidance

- Validate no accidental `noindex` on primary marketing/content pages.
- Avoid rendering important content only behind client-only interactions.
- Ensure core page content is server-rendered or otherwise indexable in rendered HTML.
- Keep heading structure semantic (one logical `h1`, then ordered subsections).

---

## SEO expectations for content pages

For article/blog/service pages, baseline expectations:

- Unique title + meta description.
- Clear main heading and semantic section hierarchy.
- Canonical URL and social metadata present.
- Structured data where applicable.
- Internal links to related pages/categories.
- No thin placeholder body content on published pages.

---

## Related docs

- [01_SYSTEM_ARCHITECTURE.md](./01_SYSTEM_ARCHITECTURE.md)
- [03_CONTENT_MODEL.md](./03_CONTENT_MODEL.md)
- [04_UI_SYSTEM.md](./04_UI_SYSTEM.md)
- [MODULE_OUTPUT_FORMAT.md](./MODULE_OUTPUT_FORMAT.md)
