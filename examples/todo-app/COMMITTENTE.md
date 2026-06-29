# 🧭 Acme Notes — Committente cockpit

> Filled from `templates/COMMITTENTE.md.template`. (Example instance.)

## 1. The model
You decide & trigger; the git files are the bus. One **manager** + two lanes (**execution**, **validation**).

| Session | What it does | Launch | Open in |
|---|---|---|---|
| Manager | decides, plans, commits | `/lancia-org` | Opus |
| Execution | writes code, runs micros | `/lancia-operativa <work-order>` | Sonnet/Opus |
| Validation | verifies correctness | `/lancia-validation` | Opus |

> ⚠️ Open every session from `C:\Users\me\Desktop\Projects\acme-notes`.

## 2. The cycle
Come to the manager → it writes a work-order → you launch the session → bring the output back → it triages & commits.

## 3. File map
`ActualStatus.md` (state) · `Sprint.md` (work) · `validation/{CHARTER,STATUS,INTERFACE}.md` · `CLAUDE.md` (rules).

## 4. What to do NOW (updated 2026-03-10)
1. Validate the search feature (`/lancia-validation` — runs open item #M1).
2. Then start S1.B (tag filtering) — `/lancia-operativa S1.B`.

## 5. Reminders
- A dead session loses nothing — the files remain.
- Only the manager commits.
