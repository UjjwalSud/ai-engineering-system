# Add Endpoint Prompt

Use this template when adding a new endpoint to an existing controller/service. Replace the placeholders and paste into chat.

---

Add a new endpoint to the [ModuleName] module. Follow the architecture in `.cursor/docs/01_SYSTEM_ARCHITECTURE.md`.

**Module:** [e.g. FormDesigner]

**Endpoint:** [e.g. GET /api/FormDesigner/export-form/{id}]

**Purpose:** [e.g. Export form structure as JSON]

**Request:** [e.g. id from route]

**Response:** [e.g. JSON string or file]

**Permissions:** [e.g. SystemAction.Export, SystemResource.ManageForm]

---

**Expected output:**
- Service interface method
- Service implementation
- Controller action

**Constraints:** Follow existing controller patterns. Use MustHavePermission and OpenApiOperation. Keep controller thin—delegate to service.
