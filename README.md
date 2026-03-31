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

Each system contains:

- `AGENTS.md` → Core instructions for that stack
- `docs/` → Architecture, decisions, and reference patterns
- `prompts/` → Reusable prompts for common development tasks
- `rules/` → Guidelines to enforce consistency and best practices

---

## How I Use This

Instead of writing everything from scratch each time, I:

1. Choose the relevant system (`dotnet` or `nextjs`)
2. Start with a clear plan based on that system’s architecture
3. Use prompts to generate consistent outputs
4. Follow rules to maintain structure and boundaries
5. Refer to existing patterns for alignment

---

## Why This Exists

Over time, I found that:

- Ad-hoc development leads to inconsistency
- Repeated patterns can be standardized
- Different stacks require different architectural rules
- A structured system improves speed, quality, and predictability

This repository is my attempt to keep development consistent across projects and technologies.
