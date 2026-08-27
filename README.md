# AI Engineering System

This repository contains my working setup for building applications using a structured, repeatable approach.

It combines:

- Clean Architecture principles
- Reusable prompts
- Defined rules for consistency
- Documented workflows

---

## Structure

This repository is organized by **technology stack**, where each stack has its own isolated AI system.

### Available Systems

- `dotnet/` → Clean Architecture backend system (.NET)
- `nextjs/` → SEO-first frontend system (Next.js)
- `legacy-modernization/` → Reverse-engineer legacy ASP.NET MVC before modernization
- `project-document-analysis/` → Turn project briefs into a consistent understanding report

Stack systems (`dotnet/`, `nextjs/`) typically contain:

- `AGENTS.md` → Core instructions for that stack
- `docs/` → Architecture, decisions, and reference patterns
- `prompts/` → Reusable prompts for common development tasks
- `rules/` → Guidelines to enforce consistency and best practices

The legacy modernization guide focuses on **discovery and documentation** first (see [legacy-modernization/README.md](./legacy-modernization/README.md)).

For new or unclear briefs, start with [project-document-analysis/README.md](./project-document-analysis/README.md).

---

## How I Use This

Instead of writing everything from scratch each time, I:

1. For brownfield work: run [legacy-modernization](./legacy-modernization/README.md) until the legacy blueprint is migration-ready
2. Choose the relevant build system (`dotnet` or `nextjs`)
3. Start with a clear plan based on that system’s architecture
4. Use prompts to generate consistent outputs
5. Follow rules to maintain structure and boundaries
6. Refer to existing patterns for alignment

---

## Why This Exists

Over time, I found that:

- Ad-hoc development leads to inconsistency
- Repeated patterns can be standardized
- Different stacks require different architectural rules
- A structured system improves speed, quality, and predictability

This repository is my attempt to keep development consistent across projects and technologies.
