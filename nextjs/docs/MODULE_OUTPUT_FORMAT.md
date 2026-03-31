# Module / Feature Output Format

In this stack, a **module** means a **feature slice**: a coherent set of routes, UI, config, and SEO that ships together (e.g. “Pricing page”, “Contact + API”, “Blog listing”).

**Do not write application code until this plan is filled out** (or explicitly waived by the user for tiny edits).

---

## 1. Summary

| Field | Value |
|--------|--------|
| Feature name | |
| Owner / context | (optional) ticket, issue, or goal |
| Routes added or changed | (paths as defined in `lib/paths.ts`) |

---

## 2. Files and folders

### 2.1 Tree (planned)

Document new or touched paths, for example:

```txt
src/app/(marketing)/pricing/page.tsx
src/components/sections/PricingCards.tsx
src/config/navigation.ts   # update
src/lib/paths.ts           # update
```

### 2.2 Reuse vs create

| Component / area | Reuse existing | Create new | Notes |
|------------------|----------------|------------|--------|
| Layout / header / footer | | | |
| Sections | | | |
| UI primitives | | | |

---

## 3. Config and single source of truth

Check all that apply and describe the change:

- [ ] `src/lib/paths.ts` — new path constants
- [ ] `src/config/navigation.ts` — new nav items or order
- [ ] `src/config/footer.ts` — new footer links
- [ ] `src/config/site.ts` — site-wide values
- [ ] `src/lib/metadata.ts` — new helpers or defaults

**Rule:** no hardcoded routes or nav in JSX — see [`config-first.mdc`](../rules/config-first.mdc).

---

## 4. SEO

For each **public** route:

| Route | `<title>` approach | Meta description | OG / Twitter | JSON-LD |
|-------|-------------------|------------------|--------------|---------|
| | `generateMetadata` or static | One–two sentences | yes/no | yes/no |

Confirm:

- [ ] Exactly one logical `<h1>` per page
- [ ] Heading hierarchy is valid
- [ ] Internal links updated where relevant

---

## 5. Server vs client boundary

List components that **must** be Client Components and why:

| Component | Reason (interaction / browser API / library) |
|-----------|-----------------------------------------------|
| | |

Everything else should default to **Server Components** ([`server-component-first.mdc`](../rules/server-component-first.mdc)).

---

## 6. API routes (if any)

| Method | Path | Validation | Success shape | Error shape |
|--------|------|------------|---------------|-------------|
| | | Zod schema | | |

- [ ] No secrets in client bundles
- [ ] Rate limiting / abuse considerations for public forms

---

## 7. Verification checklist

Before considering the work complete:

| Step | Command / action |
|------|-------------------|
| Lint | `npm run lint` (or project equivalent) |
| Build | `npm run build` |
| Manual | Key pages render, nav/footer links work, metadata in devtools |

Add project-specific checks (e.g. E2E) if the repo has them.

---

## 8. Output summary for the user / reviewer

At the end of implementation, provide:

1. **Files created**
2. **Files updated**
3. **Config / SEO changes**
4. **Reused components** vs **new components**
5. **Assumptions** (anything not specified in the brief)

---

## Relationship to AGENTS.md

This document expands the short **“Before Generating Code”** list in [`AGENTS.md`](../AGENTS.md). Use it for every non-trivial feature.

---

## Worked example — Add an About page

Use this as a concrete reference for how a completed plan should look.

### 1. Summary

| Field | Value |
|--------|--------|
| Feature name | About page |
| Owner / context | Marketing site baseline |
| Routes added or changed | `paths.about` |

### 2. Files and folders

#### 2.1 Tree (planned)

```txt
src/app/(marketing)/about/page.tsx                # new
src/components/sections/PageIntro.tsx             # reuse (existing)
src/components/sections/StatsSection.tsx          # reuse (existing)
src/lib/paths.ts                                  # update
src/config/navigation.ts                          # update
src/config/footer.ts                              # update (optional, if footer should link About)
src/lib/metadata.ts                               # update (helper usage or defaults)
```

#### 2.2 Reuse vs create

| Component / area | Reuse existing | Create new | Notes |
|------------------|----------------|------------|--------|
| Layout / header / footer | Yes | No | Existing shell consumes config |
| Sections | Yes | No | Reuse intro + stats + CTA sections |
| UI primitives | Yes | No | Existing button/card primitives |

### 3. Config and single source of truth

- [x] `src/lib/paths.ts` — add `about`
- [x] `src/config/navigation.ts` — add About nav item
- [x] `src/config/footer.ts` — add About link if required
- [ ] `src/config/site.ts` — no change
- [x] `src/lib/metadata.ts` — generate page metadata via helper

### 4. SEO

| Route | `<title>` approach | Meta description | OG / Twitter | JSON-LD |
|-------|-------------------|------------------|--------------|---------|
| `/about` | `generateMetadata` helper | Company overview + trust summary | yes | optional Organization/Breadcrumb |

Confirm:

- [x] Exactly one logical `<h1>` per page
- [x] Heading hierarchy is valid
- [x] Internal links updated where relevant

### 5. Server vs client boundary

| Component | Reason (interaction / browser API / library) |
|-----------|-----------------------------------------------|
| `about/page.tsx` and sections | None; keep as Server Components |

### 6. API routes (if any)

No API route changes required.

### 7. Verification checklist

| Step | Command / action |
|------|-------------------|
| Lint | `npm run lint` |
| Build | `npm run build` |
| Manual | `/about` renders, nav/footer links resolve, metadata visible |

### 8. Output summary for reviewer

1. **Files created**: `src/app/(marketing)/about/page.tsx`
2. **Files updated**: `src/lib/paths.ts`, `src/config/navigation.ts`, optional `src/config/footer.ts`, `src/lib/metadata.ts`
3. **Config / SEO changes**: centralized path/nav update + About metadata
4. **Reuse vs new**: reused existing sections/components; no unnecessary UI variants
5. **Assumptions**: About should be public and indexable
