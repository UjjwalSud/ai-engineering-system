# Anti-Patterns

Do not do these. They fight the existing codebase.

---

- Do not move logic into controllers. Controllers only inject services and delegate.
- Do not create repositories unless explicitly asked.
- Do not add MediatR by default.
- Do not rename entities to singular. Project uses plural (`FormStructures`, `FormPages`).
- Do not create unused abstractions.
- Do not change route versioning (versioned vs version-neutral) without confirmation.
- Do not add separate folders if only one file exists and the project does not do that.
- Do not reference Infrastructure from Application or Domain.
- Do not put business logic in controllers.
- Do not add DbContext to the Application layer.
- Do not introduce domain events unless explicitly requested.
- Do not add CQRS unless explicitly requested.
- Do not put test/debug classes in production (e.g. `Test : INotificationMessage`).
- Do not change audit or soft-delete behavior without confirmation.
- Do not add separate folders for a single file when the project does not follow that pattern.
