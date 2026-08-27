# Legacy Modernization — Analysis First

A reusable methodology for reverse-engineering a legacy ASP.NET MVC application **before** modernizing it (for example to a modern .NET API + React frontend, often keeping the same database).

This is the **discovery / reverse-engineering phase**, not a migration implementation guide.

---

## The problem

Legacy modernization projects often fail because teams start rewriting before they understand:

- existing business rules
- database behaviour
- hidden JavaScript / AJAX contracts
- authentication and session assumptions
- cross-controller dependencies
- background processes and workers
- shared services and helpers
- third-party integrations
- legacy compatibility behaviour (old hashes, dormant paths, orphan config)
- configuration and environment settings
- runtime / platform behaviour (middleware, filters, startup pipeline)

When that happens, “modern” code ships without the quirks that kept the old system working — and production breaks in subtle ways.

---

## Role of AI coding tools

In this phase, treat AI assistants (Cursor, agents, etc.) primarily as:

> **codebase analysts and reverse-engineering assistants**

not as immediate code generators for the new stack.

Use them to:

- inventory controllers and actions
- trace managers → data access → SQL/EF
- document UI, JS, and business rules
- map entities and relationships
- inventory settings (with secrets redacted)
- find platform/background behaviour
- audit gaps and verify documentation completeness

Only after the blueprint is trustworthy should migration planning begin.

---

## Core philosophy

```text
Existing behaviour is the specification.
Modern architecture is the future implementation.
```

```text
Understand first.
Document second.
Migrate later.
```

That order reduces the risk of silently dropping hidden behaviour during modernization.

---

## High-level process

| Phase | Focus |
| ----- | ----- |
| **1 — Controller / Action Discovery** | Inventory every routable MVC action; record counts |
| **2 — Action-Level Reverse Engineering** | Seven-file analysis per action; deep dependency tracing |
| **3 — Documentation Navigation** | `README.md` + `INDEX.md` with relative links |
| **4 — Entity / Database Relationships** | One file per persistent entity; cross-link to actions |
| **5 — Application Configuration / Settings** | Logical settings groups; redact secrets |
| **6 — Background / Platform Behaviour** | Hosted services, middleware, auth, shared helpers — **only if found** |
| **7 — Coverage Gap Audit** | What would still be hard to understand? |
| **8 — Verification Index** | Index `Needs DB/Runtime Verification` themes |
| **9 — Final Coverage / Link Audit** | Counts, consistency, unbroken Markdown links |
| **10 — Migration Readiness Decision** | Ready vs not ready — evidence-based |

Detailed steps live in [legacy-system-analysis-plan.md](./legacy-system-analysis-plan.md).  
Folder layout lives in [documentation-structure.md](./documentation-structure.md).

---

## Scope boundary (this phase)

This process does **not** initially:

- redesign the database
- create React components
- create new APIs
- create new EF Core entities for the modern app
- refactor legacy code
- redesign authentication
- introduce Clean Architecture in the new app
- change business rules
- “fix” legacy bugs
- perform migration implementation

The goal is:

> **Create an accurate functional and technical blueprint of the current system.**

---

## Path variables

Use these placeholders when applying the methodology to a real project:

| Variable | Meaning |
| -------- | ------- |
| `<legacy-repo>` | Existing legacy application (read-only during analysis) |
| `<modernized-repo>` | Repo that will hold the modernized app (and often the analysis docs) |
| `<analysis-root>` | Where reverse-engineering Markdown is stored |
| `<legacy-source-root>` | Folder inside `<legacy-repo>` that contains the MVC web project / Controllers |

Example analysis root:

```text
<modernized-repo>/.cursor/legacy-system-analysis/
```

(or any agreed documentation location — the structure matters more than the path)

---

## Evidence-first rule

Do **not** create documentation categories merely because they sound useful.

For every proposed document ask:

1. Does this functionality actually exist in source?
2. Is it materially important to understanding the app?
3. Is it already documented elsewhere adequately?
4. Would a canonical document add genuine understanding?

If no — do not create the file.

---

## Related docs in this folder

| File | Purpose |
| ---- | ------- |
| [legacy-system-analysis-plan.md](./legacy-system-analysis-plan.md) | Full reusable plan (agent-ready) |
| [documentation-structure.md](./documentation-structure.md) | Visual documentation architecture |

---

## After this phase

When the readiness decision is **Ready for migration planning**, you can move on to architecture and implementation using your stack-specific systems (for example `dotnet/` and `nextjs/` in this repository). That work is **out of scope** for this guide.
