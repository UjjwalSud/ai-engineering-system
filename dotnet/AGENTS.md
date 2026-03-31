# AGENTS.md

## Project Identity

This is a Clean Architecture .NET backend with:

- Domain
- Application
- Infrastructure
- Host
- MSSQL/PostgreSQL migrators

---

## Golden Rules

- Follow existing project conventions over generic textbook patterns
- Use FormDesigner as the main reference module (see `.cursor/docs/REFERENCE_MODULES.md`)
- Do not introduce MediatR, CQRS, repositories, or domain events unless explicitly requested
- Keep controllers thin
- Keep business logic in Infrastructure services, matching the current architecture
- Application must not reference Infrastructure
- Domain must not reference Application or Infrastructure
- Preserve plural entity naming used in the project

---

## Layer Responsibilities

Domain:
- Entities
- Enums
- Value objects

Application:
- DTOs
- Service interfaces
- Permissions

Infrastructure:
- Service implementations
- EF Core queries
- External integrations

Host:
- Controllers
- API configuration
- Middleware

---

## Service Pattern

Services follow this pattern:

Application:
`I<Entity>Service`

Infrastructure:
`<Entity>Service`

Services contain business logic and interact with DbContext.

Controllers only call services.

---

## Before Generating Code

Always first identify:

1. Module name
2. DbContext
3. Entities
4. DTOs
5. Permissions
6. Controller style
7. Migration impact

For new modules:

Return **Module Plan first** (see `.cursor/docs/MODULE_OUTPUT_FORMAT.md`).

Only generate code after the plan.

---

## Output Expectations

When creating a module, generate:

- Entity classes
- DTOs
- Service interfaces
- Service implementations
- DbContext updates
- Controller
- Permissions
- Migration command suggestion

---

## When Editing Existing Code

- Prefer minimal, surgical changes
- Preserve existing naming, folder structure, and response patterns
- Do not refactor unrelated files
- Do not rewrite working code only to make it "cleaner"
- Follow patterns from nearby modules

---

## Ask Before Changing

Ask before:

- Renaming existing entities
- Switching to singular naming
- Introducing repositories
- Changing versioned vs version-neutral controller patterns
- Changing audit or soft-delete behavior

---

## When Uncertain

If the correct approach is unclear:

1. Search for the closest existing module
2. Follow existing code patterns
3. Ask for clarification before introducing a new architectural pattern

---

## Cursor Rules

Rules inside `.cursor/rules/` are always enforced.

Important rules include:

- controller-thin-rule.mdc
- dto-naming.mdc
- persistence-boundary-rule.mdc
- general-clean-architecture.mdc

---

## Key Docs

- `.cursor/docs/01_SYSTEM_ARCHITECTURE.md` — source of truth
- `.cursor/docs/02_ARCHITECTURE_DECISIONS.md` — approved/deferred improvements
- `.cursor/docs/REFERENCE_MODULES.md` — which module to copy
- `.cursor/docs/ANTI_PATTERNS.md` — what must not be done
- `.cursor/docs/MODULE_OUTPUT_FORMAT.md` — plan before code

---

## Prompt Templates

- `.cursor/prompts/NEW_MODULE_PROMPT.md` — new module template
- `.cursor/prompts/ADD_ENTITY_PROMPT.md` — add entity
- `.cursor/prompts/ADD_ENDPOINT_PROMPT.md` — add API endpoint
- `.cursor/prompts/REFACTOR_SERVICE_PROMPT.md` — service refactor