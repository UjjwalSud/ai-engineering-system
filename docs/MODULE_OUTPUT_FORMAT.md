# Module Output Format

Before generating code for a new module, return this plan first. Then generate code only after the plan is confirmed.

---

## Module Plan

- **Module:** [Module name]
- **DbContext:** [ApplicationDbContext or NexusDbContext]
- **Entities:** [List of entity names and key fields]
- **Enums:** [List of enums and values, or "None"]
- **Permissions:** [SystemResource and SystemAction permissions]

**Files to create:**
- [List paths]

**Files to update:**
- [List paths]

**Migration command:**
```
Add-Migration {Name} -Context {ContextName}
```

---

Then generate code only after the plan.
