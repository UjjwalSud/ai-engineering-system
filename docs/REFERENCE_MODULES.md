# Reference Modules

Use these modules as the pattern to copy when creating new code. Do not copy from generic examples—copy from this project.

---

## FormDesigner

**Use for:** Standard CRUD modules with entity + DTO + service + controller patterns.

- `src/Core/Domain/FormDesigner/` — entities
- `src/Core/Application/FormDesigner/` — interfaces, Request/Response DTOs
- `src/Infrastructure/Orbit/FormDesigner/` — service implementations
- `src/Host/Controllers/FormDesigner/` — controllers

**Pattern:** Entity hierarchy (FormStructures → FormPages → FormPageTabs → FormPageFields), full CRUD, search with pagination, partial controller classes.

---

## LookUp

**Use for:** Small lookup/list management modules.

- Simpler structure than FormDesigner
- LookUpCodes, LookUpCodeValues pattern

---

## Appointment

**Use for:** Relational modules with multiple entities and complex workflows.

- TempAppointments, TempAppointmentParticipants, TempAppointmentAvailabilityWindow
- More relational logic

---

## Nexus Modules

**Use for:** Tenant, identity, subscription, localization concerns.

- `src/Infrastructure/Nexus/` — Identity, MultiTenant, Localization, Subscription, Lookup
- Uses `NexusDbContext` (not `ApplicationDbContext`)
- DbModels in Infrastructure (e.g. `ApplicationUser`, `Tenants`)

**Do not use:** For standard application-domain CRUD. Use Orbit modules (FormDesigner, LookUp, etc.) instead.
