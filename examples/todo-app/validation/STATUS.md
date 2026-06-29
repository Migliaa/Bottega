# Validation — STATUS (Acme Notes)

> The register (facts). The channel is `INTERFACE.md`.

| Feature | Correctness | Edge cases | Notes |
|---|---|---|---|
| Auth (S0) | ✅ | ✅ | session expiry + wrong-password paths covered |
| Notes CRUD (S0) | ✅ | ✅ | empty-title rejected; long-body ok |
| Search (S1.A) | ⏳ in progress | — | validating now (item #M1) |
| Tag filtering (S1.B) | — | — | not built yet |
