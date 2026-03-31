# Refactor Service Prompt

Use this template when refactoring a service. Replace the placeholders and paste into chat.

---

Refactor the [ServiceName] service. Follow the architecture in `.cursor/docs/01_SYSTEM_ARCHITECTURE.md` and `.cursor/docs/02_ARCHITECTURE_DECISIONS.md`.

**Service:** [e.g. FormStructureService]

**Refactor type:** [e.g. Add CancellationToken, Extract method, Split logic]

**Scope:** [e.g. All methods in this service only]

---

**Constraints:**
- Do not introduce repositories unless explicitly requested
- Do not add MediatR or CQRS
- Preserve existing behavior
- Add CancellationToken to async methods (approved improvement)
- Use existing error handling (NotFoundException, BadRequestException)
- See `.cursor/docs/ANTI_PATTERNS.md` for what not to do
