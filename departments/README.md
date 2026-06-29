# Departments — ready-made recipes

Departments are **configured per project.** Don't enable all of them — enable the minimum, and add a lane only when it's genuinely overloaded. A manager + **execution** is enough to start.

Each recipe below describes an archetype: its mandate, what it owns, its boundaries, the gates it participates in, and how it uses the `INTERFACE.md` protocol. To instantiate one:

1. Copy the relevant recipe into `<dept>/CHARTER.md` (or generate it via `/bootstrap`).
2. Create `<dept>/STATUS.md` (the register) and `<dept>/INTERFACE.md` (from the template).
3. Add a `/lancia-<dept>` command (from `templates/commands/lancia-dept.md.template`).

| Archetype | "The …" | Persistent? | Enable when |
|---|---|---|---|
| [execution](execution.md) | implementer | no (disposable) | always — this is the doing lane |
| [validation](validation.md) | correctness guarantor | yes | output correctness matters and must be defensible |
| [research](research.md) | scout | one-off | you need deep, cited investigation before deciding |
| [publishing](publishing.md) | distributor | yes | you ship content/marketing to the outside world |
| [compliance](compliance.md) | risk guarantor | yes (internal-only) | the project has legal/regulatory/policy surface |

> **Naming:** keep the folder name matching the command (`validation/` ↔ `/lancia-validation`). The two signature lanes — manager and execution — are universal; the rest are optional.
