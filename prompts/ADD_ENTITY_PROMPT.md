# Add Entity to Existing Module Prompt

Use this template when adding an entity to an existing module. Replace the placeholders and paste into chat.

---

Add a new entity to the existing [ModuleName] module. Follow the architecture in `.cursor/docs/01_SYSTEM_ARCHITECTURE.md` and `.cursor/docs/REFERENCE_MODULES.md`.

**Module:** [e.g. FormDesigner]

**New entity:** [e.g. FormPageFieldOptions]

**Fields:** [Field1], [Field2], [Field3], ...

**Relationships:** [e.g. Belongs to FormPageFields]

**Enums (if any):** [e.g. OptionType: Single, Multiple]

**Permissions:** [Add to existing or new]

**DbContext:** ApplicationDbContext (or NexusDbContext)

---

**Expected output:**
- Entity class
- DTOs (Create, Update, Search, Response)
- Service interface methods
- Service implementation
- DbSet in DbContext
- Controller endpoints (if new controller needed)
- Permissions in SystemPermissions
- Migration command

**Constraints:** Follow existing patterns in the module. Use plural entity naming. Add CancellationToken to async methods.
