# SEO_AUDIT_PROMPT

Use this prompt to audit SEO quality and architecture consistency of a Next.js project.

## Objective

Identify SEO and indexability gaps with concrete fixes aligned to this system.

## Instructions to AI

1. Audit public routes first, then supporting SEO infrastructure.
2. Check each page for:
   - title/description quality
   - one logical H1 and heading hierarchy
   - canonical URL behavior
   - indexability directives
   - OG/Twitter coverage
   - JSON-LD usage where relevant
3. Check system-level architecture:
   - metadata helpers in `src/lib/metadata.ts`
   - schema helpers in `src/lib/schema.ts`
   - route centralization in `src/lib/paths.ts`
   - internal linking consistency via config/routes
4. Check operational SEO surfaces:
   - robots policy
   - sitemap policy
   - canonical host handling
5. Flag hardcoded routes and duplicated metadata logic.
6. Provide prioritized findings and focused remediation steps.

## Required output format

Return:

- Audit scope
- Findings by severity (Critical, High, Medium, Low)
- File-level remediation plan
- Suggested quick wins vs deeper follow-up work
