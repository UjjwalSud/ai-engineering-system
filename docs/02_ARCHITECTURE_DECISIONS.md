# Architecture Decisions

This document tracks architectural decisions, current deviations from ideal, and what to apply (or not) when generating or modifying code. It is **not law**—it is an improvement backlog with explicit adoption status.

---

## 1. Current Deviations

What exists today that differs from ideal:

| Aspect | Current State |
|--------|---------------|
| **Repository Pattern** | Not used. Services inject `DbContext` directly. `IRepository<>` exists but is commented out in `Persistence/Startup.cs`. |
| **Domain Events** | Commented out in `BaseEntity` and `SaveChangesAsync`. No event-driven behavior. |
| **CQRS/MediatR** | MediatR is commented out. Using service layer only. |
| **Validation** | FluentValidation schema processor exists for OpenAPI, but `ValidationException` handling is commented out in `ExceptionMiddleware`. |
| **CancellationToken** | Many service methods do not accept `CancellationToken`. |
| **Auditing Logic** | `BaseDbContext` and `NexusBaseDbContext` both contain nearly identical audit logic (~100 lines each). |
| **Test Code** | `UserService.CreateUpdate.cs` contains `Test : INotificationMessage` and broadcasts it from `UpdateAsync`. |

---

## 2. Approved Future Improvements

**Apply to new code now.** When creating new modules or services:

- Add `CancellationToken cancellationToken = default` to all async service methods and pass it through to EF calls.
- Enable FluentValidation exception handling when validators exist (uncomment `ValidationException` handling in `ExceptionMiddleware`, add validators for new Request DTOs).

---

## 3. Deferred / Do Not Introduce Yet

**Do not auto-apply.** Only introduce when explicitly requested or when module complexity justifies it:

- **MediatR/CQRS** — Unless module complexity justifies it.
- **Repository abstraction** — Unless explicitly requested. Keep using `DbContext` directly.
- **Domain events** — Unless explicitly requested.
- **Singular entity naming** — Project uses plural (`FormStructures`, `FormPages`). Do not rename unless asked.
- **Extract shared auditing logic** — Backlog; do not refactor unless requested.
- **Host → Migrator coupling** — Low priority; do not change unless deploying with PostgreSQL.

---

## 4. Architecture Assessment (Reference)

### What's Done Well

| Aspect | Assessment |
|--------|------------|
| Layer Separation | Domain → Application → Infrastructure → Host. Dependencies flow inward correctly. |
| Domain Purity | Domain has no dependencies on Application or Infrastructure (only Shared). |
| Application Independence | Application does not reference Infrastructure. |
| Service Abstraction | Application defines interfaces; Infrastructure implements them. |
| Convention-Based DI | `ITransientService`/`IScopedService` marker interfaces with assembly scanning. |
| Multi-Tenancy | Tenant isolation via `ISoftDelete`, `IAuditableEntity`, and query filters. |
| Auditing | Audit trail for create/update/delete with separate `AuditingDbContext`. |
| Authorization | Permission-based (`MustHavePermission`, `SystemPermissions`) with role hierarchy. |
| Multiple DbContexts | Sensible split: `ApplicationDbContext`, `NexusDbContext`, `AuditingDbContext`. |
| Database Flexibility | Support for both MSSQL and PostgreSQL via Migrators. |

### Improvement Details (Backlog)

- **Repository**: Define `IRepository<T>` in Application, implement in Infrastructure. Improves testability. Do not introduce unless requested.
- **FluentValidation**: Register validators, handle `ValidationException` in `ExceptionMiddleware`. Apply for new modules when validators exist.
- **Duplicate auditing**: Extract to shared `AuditHandler`. Backlog only.
- **Host → Migrator**: Consider conditional reference for PostgreSQL. Low priority.
- **Test code**: Remove `Test` class and its usage from `UserService`. One-time cleanup.

---

## 5. Summary

- **Adopt for new code**: CancellationToken, FluentValidation (when validators exist).
- **Do not auto-introduce**: Repositories, MediatR, CQRS, domain events, singular naming.
- **Backlog**: Extract auditing logic, fix Host→Migrator coupling, remove test code.
