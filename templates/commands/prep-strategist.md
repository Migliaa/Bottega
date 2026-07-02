---
description: (MANAGER) Prep a strategist run — rebuild the BRIEF + write your questions in strategy/ASK.md. Do NOT launch the strategist.
---
You are the **MANAGER**. Prepare a run of the **STRATEGIST** (the board layer). Two steps, then stop.

**1) Rebuild `strategy/BRIEF.md`** (it goes stale as the repo moves). Cheap way = a **fan-out of cheaper readers** (a `Workflow`), one per subsystem, each **COMPACT** (structure + source-of-truth files + ≤6 candidate issues with severity+`file:line`+why + strengths + strategy notes), **no code dumps**. Synthesize the digests into `strategy/BRIEF.md` (dense). Update `strategy/READING_LIST.md` if the source-of-truth files changed. **The BRIEF is the strategist's input — it does NOT read the repo.** *(No fan-out available? Read the key files yourself, concisely.)*

**2) Update `strategy/ASK.md → From Manager`**: your specific questions, doubts, imperfect architectures to review, complex plans to design. One line each, `[ ]`, id `#M<n>`.

Then **commit `strategy/`** and tell the committente: **"ready → launch `/lancia-strategist`"**. **Do NOT launch the strategist yourself** (the committente does, in a separate top-model session).
