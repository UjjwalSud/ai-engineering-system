# Legacy Analysis — Documentation Structure

Visual reference for the reverse-engineering documentation tree produced by [legacy-system-analysis-plan.md](./legacy-system-analysis-plan.md).

All paths are relative to `<analysis-root>`.

---

## Target tree

```text
legacy-system-analysis/          # typical name for <analysis-root>
│
├── README.md                    # methodology, counts, status, model
├── INDEX.md                     # human navigation
├── coverage-gaps.md             # gap audit + final coverage status
├── verification-required.md     # OPTIONAL — only if unresolved themes exist
│
├── <ControllerName>/            # one folder per *Controller.cs
│   ├── README.md
│   ├── <Action>-GET/
│   │   ├── summary.md
│   │   ├── controller-flow.md
│   │   ├── database.md
│   │   ├── ui.md
│   │   ├── javascript.md
│   │   ├── business-rules.md
│   │   └── dependencies.md
│   └── <Action>-POST/
│       └── (same seven files)
│
├── system-relationships/
│   ├── README.md
│   └── entities/
│       ├── <EntityA>.md
│       └── <EntityB>.md
│
├── application-settings/
│   ├── README.md
│   └── settings/
│       ├── DatabaseSettings.md
│       ├── AuthenticationSettings.md
│       └── <LogicalGroup>.md
│
├── background/                  # CONDITIONAL — only if workers/jobs exist
│   ├── README.md
│   └── <ServiceOrJob>.md
│
└── platform/                    # CONDITIONAL — only if cross-cutting runtime docs needed
    ├── README.md
    ├── startup-pipeline.md
    ├── authorization.md
    └── <Topic>.md
```

---

## Conditional structures

Create these **only** when evidence justifies them:

| Path | Create when |
| ---- | ----------- |
| `background/` | Hosted services, WebJobs, queue workers, schedulers exist |
| `platform/` | Shared runtime behaviour is hard to reconstruct from actions alone |
| `verification-required.md` | Unresolved DB / runtime / env / external verification themes exist |
| `workflows/` | Multi-area E2E flow cannot be understood from actions + platform |
| `integrations/` | Major third parties need a canonical index beyond settings/actions |
| `ui-map/` | Screen inventory would add info not already in INDEX + `ui.md` |

If uncertain, prefer updating `coverage-gaps.md` over inventing folders.

---

## Dual navigation model

### Behaviour view

```text
Controller
  → Action
  → summary / flow / database / ui / javascript / rules / dependencies
```

### Data view

```text
Entity
  → Table / PK / FK
  → Related entities
  → Controllers / Actions
  → Read / Write behaviour
```

---

## Cross-link examples

### Action → Entity → Action

```text
OrderController/Create-POST/database.md
  → ../../system-relationships/entities/Order.md
  → ../entities/Account.md
  → ../../AccountController/ViewDetail-GET/summary.md
```

### README → INDEX → Controller → Action

```text
README.md
  → INDEX.md
  → UserController/README.md
  → UserController/Index-GET/summary.md
```

### Settings → Consumers

```text
application-settings/settings/EmailSettings.md
  → ../../UserController/ResetPassword-POST/summary.md
  → ../../platform/email-service.md   (if platform doc exists)
```

### Gaps → Canonical docs

```text
coverage-gaps.md
  → ./platform/authorization.md
  → ./background/DocumentProcessor.md
```

---

## Root file responsibilities

| File | Role |
| ---- | ---- |
| `README.md` | Inventory, methodology, discovered counts, documentation model, links to major areas |
| `INDEX.md` | Expandable navigation for controllers/actions + entities + optional areas |
| `coverage-gaps.md` | Real gaps, confirmed absences, final coverage status, readiness notes |
| `verification-required.md` | Thematic index of unresolved verification items (links out) |

Advertise in `README.md` / `INDEX.md` **only** folders that actually exist.

---

## Fictional mini-example (structure only)

```text
legacy-system-analysis/
├── README.md
├── INDEX.md
├── coverage-gaps.md
├── verification-required.md
├── UserController/
│   ├── README.md
│   └── Index-GET/
│       └── summary.md ...
├── AccountController/
├── OrderController/
├── system-relationships/
│   └── entities/
│       ├── User.md
│       ├── Account.md
│       └── Order.md
├── application-settings/
│   └── settings/
│       ├── DatabaseSettings.md
│       └── EmailSettings.md
├── background/
│   └── OrderCleanupJob.md
└── platform/
    ├── authorization.md
    └── session-helper.md
```

Names above are generic placeholders — replace with names discovered in the target legacy app.

---

## Link integrity

Before declaring analysis complete:

1. Resolve all relative links under `<analysis-root>`
2. Fix casing / path mistakes
3. Ensure INDEX entries point at real files
4. Ensure gap “Covered” links point at real canonical docs

Do not claim completion with known broken navigation.
