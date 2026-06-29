# 🔌 Interface — Manager ↔ Validation (Acme Notes)

> Async channel. Protocol: validation does open `From Manager` items at start; writes back + commits `validation/` at close. Manager reads this at every start and before gates.
> Full state: `validation/STATUS.md`. IDs: `#M<n>` (manager), `#V<n>` (validation).

## 📍 Digest — Validation
- 2026-03-10 — S0 fully validated. **Search (S1.A) under validation now** (#M1). Detail: `validation/STATUS.md`.

## 📥 From Manager → Validation
- [ ] `2026-03-10` **#M1** — Validate S1.A search: correctness on the seed set + edge cases (empty query, special characters, no-match). Confirm FTS triggers stay in sync after an edit.

## 📤 From Validation → Manager
- [x] `2026-03-09` **#V1** — S0 (auth + CRUD) validated, independently re-run. → STATUS green.
- [ ] `2026-03-10` **#V2** — Question: should search match note **titles** with higher rank than body? Affects the test I write for #M1. (Manager: decide ranking expectation.)
