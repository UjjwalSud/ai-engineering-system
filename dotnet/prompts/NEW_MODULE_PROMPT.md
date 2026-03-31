# New Module Creation Prompt

Use this template when asking the AI to create a new module. Replace the placeholders and paste into chat.

---

Create a new module following the architecture in `.cursor/docs/01_SYSTEM_ARCHITECTURE.md`, the decisions in `.cursor/docs/02_ARCHITECTURE_DECISIONS.md`, the checklist in `.cursor/rules/new-module-creation.mdc`, and the reference module in `.cursor/docs/REFERENCE_MODULES.md`.

**Before generating code:** Return the Module Plan first (see `.cursor/docs/MODULE_OUTPUT_FORMAT.md`). Then generate code only after the plan is confirmed.

**Module name:** [e.g. OrderManagement]

**Entities and fields:**
- [EntityName1]: [Field1], [Field2], [Field3], ...
- [EntityName2]: [Field1], [Field2], ...

**Relationships:** [e.g. Order has many OrderItems]

**Enums (if any):** [e.g. OrderStatus: Pending, Confirmed, Shipped]

**Permissions:** [e.g. ManageOrders - View, Create, Update, Delete for Admin]

**DbContext:** ApplicationDbContext (or NexusDbContext for identity/tenant-related)

**Reference:** Use FormDesigner module as the pattern (see `.cursor/docs/REFERENCE_MODULES.md`).

---

**Expected output:**
1. Folder structure
2. Domain entities
3. DTOs
4. Service interfaces
5. Service implementations
6. DbContext changes
7. Controller code
8. Permissions to add
9. Migration command
10. Any assumptions

**Important constraints:**
- Follow existing plural entity naming
- Use FormDesigner as reference
- Do not invent new architectural patterns
- Use CancellationToken in async methods
- Do not place business logic in controllers
- Match existing success/error handling style
- See `.cursor/docs/ANTI_PATTERNS.md` for what not to do
