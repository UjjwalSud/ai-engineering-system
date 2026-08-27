# Legacy System Analysis Plan (Reusable)

Agent-ready methodology for reverse-engineering a legacy ASP.NET MVC application before modernization.

**Phase goal:** accurate blueprint of *current* behaviour.  
**Not in scope:** migration design, new APIs, React UI, DB redesign, refactoring legacy code.

---

## Path variables

| Variable | Meaning |
| -------- | ------- |
| `<legacy-repo>` | Existing legacy application repository |
| `<modernized-repo>` | Repository for the modernized application (and often analysis docs) |
| `<analysis-root>` | Root folder for reverse-engineering documentation |
| `<legacy-source-root>` | Legacy web project root (typically contains `Controllers/`) |

```text
<legacy-repo>/
  <legacy-source-root>/
    Controllers/
    ...

<modernized-repo>/
  <analysis-root>/
    README.md
    INDEX.md
    ...
```

Use **relative Markdown links** inside `<analysis-root>` so docs work on GitHub and in clones.  
Do **not** put absolute machine paths in the published analysis.

---

## Constraints

- **Read-only** on `<legacy-repo>` during analysis (unless the team explicitly allows doc-only tooling).
- Write analysis **only** under `<analysis-root>` (usually inside `<modernized-repo>`).
- **No migration implementation** in this phase.
- **Docs on demand:** create folders when analysis starts - do not bulk-scaffold empty templates.
- **Live database not required** for the initial pass (see Database connectivity rule).
- **Secrets:** never copy credentials/keys into docs - use `[REDACTED - SECRET]`.
- **Evidence-first:** do not invent entities, FKs, workflows, integrations, jobs, or platform docs.
- Discover and record actual counts (`<controller-count>`, `<action-count>`, `<entity-count>`, etc.). Do not treat any sample size as a requirement.

---

## Knowledge-base model

```text
1. Controller / Action Analysis
   → how application behaviour works

2. Entity / System Relationships
   → how persistent data is connected

3. Application Settings
   → which configuration controls behaviour and where it is used

4. Background Services (if any)
   → processes outside MVC request/response

5. Platform / Runtime Behaviour (if needed)
   → shared auth, session, middleware, helpers, etc.

6. Verification Required
   → indexed DB / runtime / env / external-service unknowns

7. Coverage Gap Audit
   → important behaviour not naturally covered by the above
```

---

# Phase 1 - Controller / Action Discovery

## 1.1 Inventory

1. Create `<analysis-root>/README.md` with methodology, scope, and a controller inventory table.
2. Discover controllers under `<legacy-source-root>/Controllers` (or equivalent).
3. Detect **routable actions**: public instance methods MVC can route - **not** “has a return type” alone.
4. Exclude: constructors, `[NonAction]`, lifecycle overrides (`OnActionExecuting`, etc.), obvious non-route helpers.
5. Record preliminary counts; mark status Incomplete until each controller is fully analysed.

## 1.2 File creation order

- Root `README.md` first.
- Controller `README.md` only when that controller’s analysis starts.
- Action folders only when that action is analysed (no empty scaffolds).

## 1.3 Analysis order

Prefer dependency / business order (auth & account early; large “home” or catch-all controllers last).  
Complete one controller fully before starting the next.

Base controllers: document via inheriting actions, or add a platform note later if shared lifecycle is material.

---

# Phase 2 - Action-Level Reverse Engineering

## 2.1 Folder pattern

```text
<analysis-root>/<ControllerName>/
  README.md
  <ActionName>-GET/
    summary.md
    controller-flow.md
    database.md
    ui.md
    javascript.md
    business-rules.md
    dependencies.md
  <ActionName>-POST/
    ...
```

### Rules

| Rule | Reason |
| ---- | ------ |
| Separate folders for GET vs POST (and other verbs) of the same action name | Different flows, validation, side effects |
| AJAX endpoints get their own action folders | They are real entry points |
| Private helpers do **not** get action folders | Document them inside calling actions |
| Keep controller folders 1:1 with `*Controller.cs` | Easier source cross-check |

## 2.2 Trace beyond the controller

For every action, follow:

```text
Controller
→ Service / Manager
→ Repository / Data Access
→ EF / LINQ / SQL / SPs
→ Database entities / tables
→ View / Partial
→ JavaScript
→ AJAX
→ Validation
→ Authorization
→ Session / Cache
→ Emails
→ Files / storage
→ External APIs
→ Background enqueue / jobs
→ Audit / logging
→ Result / redirect / JSON
```

The controller is the **entry point**, not the analysis boundary.

## 2.3 Per-file intent (minimum)

| File | Contents |
| ---- | -------- |
| `summary.md` | Purpose, HTTP verb, auth, main outcome |
| `controller-flow.md` | Step-by-step execution |
| `database.md` | Reads/writes, SQL/EF, entities; link to entity docs when they exist |
| `ui.md` | Views, forms, layout |
| `javascript.md` | Client scripts, AJAX contracts |
| `business-rules.md` | Rules with **Confirmed** / **Inferred** |
| `dependencies.md` | Services, config keys, external systems |

Prefer **File / Class / Method** references over fragile line numbers.

## 2.4 Database connectivity rule

> Live database connectivity is **not** required for the initial analysis.

Use source evidence:

- EF / ORM entities and attributes
- `DbContext` / ObjectContext `DbSet`s
- LINQ queries
- repositories
- SQL strings
- database projects / scripts
- stored-procedure call sites
- migrations / mappings
- configuration

If something cannot be confirmed without a real DB, mark:

```text
Needs DB Verification
```

and continue. Do not block the analysis.

## 2.5 Confidence / evidence model

| Label | Meaning |
| ----- | ------- |
| Confirmed - explicit mapping | Attribute / Fluent / clear mapping in code |
| Confirmed - navigation / FK relationship | Nav property and/or `[ForeignKey]` (or equivalent) |
| Confirmed - SQL / LINQ join | Join/filter evidence in queries |
| Inferred - application usage | Used together in app logic; not a proven physical FK |
| Needs DB Verification | Requires live schema / data confirmation |
| Needs Runtime Verification | Requires running app / environment confirmation |

**Critical:** application-level id matching must **not** automatically be described as a physical SQL foreign key.

---

# Phase 3 - Documentation Navigation

## 3.1 Root files

| File | Responsibility |
| ---- | -------------- |
| `README.md` | Methodology, scope, discovered counts, status, documentation model |
| `INDEX.md` | Human navigation: controllers/actions, entities, and any other existing areas |

## 3.2 Navigation goals

Someone opening `INDEX.md` should be able to browse by:

```text
Controller / Action
  OR
Entity / Table
```

and, if present, settings / background / platform / verification / coverage gaps.

## 3.3 Link rules

- Relative links only (GitHub-portable).
- Controller README links to each action’s `summary.md`.
- Action docs link back to controller README and `INDEX.md`.
- Do not rewrite analysis content just to add navigation - additive links only.

---

# Phase 4 - Entity / Database Relationships

Complement the vertical controller view with a horizontal data map.

## 4.1 Discover entities

Inventory persistent entities from:

- `DbSet<>` / context mappings
- entity classes / attributes / Fluent config
- existing action `database.md` / flow / rules
- legacy repo verification when ambiguous

**Do not** treat ViewModels / DTOs / search models as database entities unless they are truly mapped.

## 4.2 Structure

```text
<analysis-root>/system-relationships/
  README.md
  entities/
    <EntityName>.md
```

One persistent entity → one canonical Markdown file.

## 4.3 Recommended entity sections

1. Entity overview (type, source, table, PK, description)
2. Source mapping (class, context, configuration)
3. Database mapping (PK, important columns, FK properties, soft-delete/status fields)
4. Relationships (type, evidence, confidence; app relationship vs physical FK)
5. Related entities (relative links)
6. Controller / action usage (links to real `summary.md` / `database.md`)
7. Read behaviour
8. Write behaviour
9. Important business constraints (entity-level; link action rules)
10. Stored procedures / raw SQL usage
11. Needs DB Verification

## 4.4 Bidirectional navigation

```text
Action database.md
      ↓
Entity.md
      ↓
Related entities
      ↓
Other controller / actions
```

```text
Entity.md
   ↓
Controller / Action summary
   ↓
Detailed action analysis
```

Where an action’s `database.md` clearly uses a documented entity, add only a small related-entities section - do not rewrite existing DB analysis.

---

# Phase 5 - Application Configuration / Settings

## 5.1 Structure

```text
<analysis-root>/application-settings/
  README.md
  settings/
    <LogicalGroup>.md
```

Group related settings logically (not one file per key).

Example group names (use what the app actually has):

```text
DatabaseSettings.md
AuthenticationSettings.md
EmailSettings.md
StorageSettings.md
PaymentSettings.md
ExternalApiSettings.md
ApplicationBehaviourSettings.md
JobsAndBackgroundSettings.md
ObservabilitySettings.md
```

## 5.2 Discover settings

Scan the legacy repo for:

- application configuration files (`appsettings*.json`, `Web.config`, transforms, custom `.config`)
- environment variables
- connection strings
- configuration / options classes
- feature flags
- URLs and endpoints
- storage / file paths
- email, auth, payment, scheduler, integration settings
- static constants used as configuration
- JavaScript / Razor-exposed configuration

Explain in the settings README **how** the app loads configuration (classic `ConfigurationManager` vs ASP.NET Core `IConfiguration`, etc.).

## 5.3 Per-setting fields

```text
Key:
Source File:
Configuration Section:
Current Value:
Purpose:
Used By:
Environment Specific:
Sensitive:
Notes:
```

Trace `Used By` to classes / methods / controllers / actions. Link action analysis where helpful.

## 5.4 Secret handling (mandatory)

Never copy into documentation:

- passwords
- API secrets / access tokens / refresh tokens
- private keys / signing keys
- connection-string credentials
- SMTP passwords
- payment-provider secrets

Represent them as:

```text
[REDACTED - SECRET]
```

Non-secret behavioural values (timeouts, page sizes, public URLs, feature flags) may be documented with real values when useful.

This is a **behaviour map**, not a secret inventory.

---

# Phase 6 - Background / Platform Behaviour

Controller docs will not naturally capture everything.

## 6.1 Scan for real runtime behaviour

Inspect the full legacy solution for items such as:

- startup / hosting / DI
- middleware
- global filters
- authentication / authorization
- session behaviour
- caching
- background services / hosted services
- schedulers / recurring jobs
- WebJobs / workers / queues
- email processors
- PDF / document processing
- shared JavaScript protocols / magic strings
- shared data helpers / raw SQL helpers
- encryption / password compatibility
- integration lifecycle (including dormant / commented paths)

## 6.2 Conditional folders

Create only when justified:

```text
<analysis-root>/background/     # hosted services, workers, jobs
<analysis-root>/platform/       # cross-cutting runtime (auth, session, middleware, …)
```

Suggested background docs (examples only):

```text
background/
  README.md
  <HostedServiceName>.md
  <WorkerOrJobName>.md
```

Suggested platform docs (create only for real findings):

```text
platform/
  README.md
  startup-pipeline.md
  authorization.md
  session-helper.md
  ...
```

Document **live vs dormant** paths clearly (e.g. enqueue commented out but worker project still exists).

## 6.3 Evidence-first reminder

Do not create `workflows/`, `integrations/`, `ui-map/`, or similar unless:

- the behaviour spans multiple areas **and**
- isolated action docs are insufficient **and**
- a canonical doc would materially help

---

# Phase 7 - Coverage Gap Audit

Create:

```text
<analysis-root>/coverage-gaps.md
```

### Guiding question

> If an experienced developer only had `<analysis-root>` plus access to `<legacy-repo>`, what **materially important** existing behaviour would still be hard to understand?

### Per gap (only genuine gaps)

```text
Gap:
Category:
Found In:
What It Does:
Why Existing Analysis Does Not Fully Represent It:
Related Controllers / Entities / Settings:
Current Documentation Coverage: None / Partial / Covered
Recommended Documentation Location:
Verification Status: Confirmed / Inferred / Needs Runtime or DB Verification
```

Update gaps when canonical docs are later created (`Partial` → `Covered` with links).

Also record **confirmed absences** (e.g. “no Hangfire”, “no webhook handlers found”) so readers do not invent them.

Do **not** manufacture findings to fill categories.

---

# Phase 8 - Verification Index

If unresolved verification themes exist, create:

```text
<analysis-root>/verification-required.md
```

This is an **index**, not a rewrite of hundreds of action notes.

Index themes such as:

| Type | Examples |
| ---- | -------- |
| Needs DB Verification | Physical FKs, table/column names, SP behaviour |
| Needs Runtime Verification | Deployed workers, timezone schedules, live provider edge cases |
| Needs Environment Verification | Env-specific URLs, feature flags |
| Needs External-Service Verification | Third-party sandbox vs production behaviour |

Link to the detailed source docs. Prefer resolving themes when DB/runtime access becomes available.

A system can still be **ready for migration planning** while verification items remain clearly indexed.

---

# Phase 9 - Final Coverage / Link Audit

## 9.1 Consistency checks

| Check | Expectation |
| ----- | ----------- |
| Controllers | Folder count = discovered `*Controller` count |
| Actions | Action folders = discovered routable action count |
| Seven files | Each action folder has the seven analysis files |
| Entities | Entity files = mapped persistent entity count |
| Settings | Settings README/index matches settings files |
| Platform / background | `coverage-gaps` references resolve to real docs |
| Navigation | `INDEX.md` / `README.md` only advertise folders that exist |

Record discovered totals as you find them (do not hard-code sizes from another project).

## 9.2 Link integrity

Validate relative Markdown links across `<analysis-root>`:

- root README / INDEX
- controller READMEs
- action docs
- entity docs
- settings / platform / background / verification / coverage-gaps

Do not claim completion with known broken internal navigation.

## 9.3 Final coverage status

Append a summary to `coverage-gaps.md` (or root README), for example:

```text
Controller / Action Analysis: Complete | Incomplete
Entity Relationship Analysis: Complete | Incomplete
Application Settings: Complete | Incomplete
Background Services: Complete | Not Applicable | Incomplete
Platform Analysis: Complete | Not Applicable | Incomplete
Workflows / Integrations / UI Map: Not Required | Created | Incomplete
Verification Items: Indexed (N themes remain)
Link Audit: Complete | Failed
```

Use actual findings. Do not mark Complete unless verified.

---

# Phase 10 - Migration Readiness Decision

Produce an evidence-based conclusion:

```text
Ready for migration planning
```

or:

```text
Not yet ready - material gaps remain
```

Distinguish:

| Kind | Meaning |
| ---- | ------- |
| Documentation gap | Behaviour exists but is not adequately documented |
| DB verification gap | Documented from source; needs live schema confirmation |
| Runtime verification gap | Documented from source; needs running system confirmation |

**Ready** is allowed when docs cover material behaviour and remaining items are indexed verification themes - not when major undocumented runtime paths remain.

**Stop here.** Do not start migration design in the same pass unless the team explicitly begins a new phase.

---

# Quality rules (all phases)

- Self-contained action docs; label confidence honestly
- No invented SP / SQL / FK behaviour
- No migration suggestions inside analysis files - document legacy as-is
- Prefer File / Class / Method over line numbers
- Relative links only in `<analysis-root>`
- Secrets always `[REDACTED - SECRET]`
- Evidence-first: no empty category folders

---

# Suggested analysis root layout (end state)

See [documentation-structure.md](./documentation-structure.md) for the visual reference.

Minimum expected when Phases 1–5 are complete:

```text
<analysis-root>/
  README.md
  INDEX.md
  coverage-gaps.md
  <Controller>/...
  system-relationships/
  application-settings/
```

Conditional:

```text
background/
platform/
verification-required.md
```

---

# Using this plan with an AI agent

1. Fill in `<legacy-repo>`, `<modernized-repo>`, `<analysis-root>`, `<legacy-source-root>`.
2. Instruct the agent to follow phases in order.
3. Require evidence-first documentation and secret redaction.
4. Require a final link audit and readiness decision.
5. Forbid migration implementation until readiness is agreed.

This plan is intentionally detailed enough to paste into Cursor (or similar) as the methodology contract for the discovery phase.
