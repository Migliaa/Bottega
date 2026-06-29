# Acme Notes — Current state

> Auto-loaded each session. Keep compact. Last updated: 2026-03-10
>
> 🔔 Manager startup step: pull & read `validation/INTERFACE.md` (digest + open `From Validation`), triage, answer in `From Manager`. Same before every gate.

## 📌 STATE — read this first
**Where we are:** Auth + notes CRUD shipped (S0, done). Search (S1.A) implemented, **awaiting validation** (item #M1 in validation's interface).
**→ NEXT:** validation signs off search → then S1.B (tag filtering).
**In flight:** none (no execution session open).
**Open fronts:** tag filtering (S1.B, micros not yet defined); full-text search ranking is naive (noted as tech-debt).

## Sprint order
S0 ✅ (auth + CRUD) · **S1** search & filtering (A done/validating, B next) · S2 sharing (later).

## In one line
S0 done & committed (`a1b2c3d`). S1.A (search) ready-to-test, validation pending. Static checks green.
