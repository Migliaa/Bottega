# 06 · Agentic governance — when (and how) to give an automation autonomy

Bottega itself is a multi-agent system, and the projects it runs often build *more* automations (a workflow, a sub-agent, a monitoring job, an in-product agent). This page is the checklist applied **before** building or granting autonomy to any of them. It's distilled from the public literature on agentic automation and from hard lessons; treat it as a standard, not a suggestion.

## The seven rules

1. **Start from the pain/volume, not the technology.** Automate a process that is **high-volume, repetitive, with data available, and a low/containable cost-of-error**. No volume yet (e.g. a pre-launch function with zero users)? **Don't automate yet** — record the trigger condition and move on. Premature automation is an anti-pattern.

2. **Autonomy level = criticality × reversibility, not model power.** High criticality + low reversibility → minimal autonomy + a strong human gate. Extreme criticality with no possible supervision → **don't automate.**

3. **Crawl → walk → run.** Start *assisted* (human decides) or *supervised* (human approves the critical points). Increase autonomy **only** when the prior level is *measurably* reliable (green evals, a quality baseline).

4. **Production-grade layer is mandatory before autonomy.** Minimal-scope, short-lived secrets in a vault (never in the prompt/code); a **per-agent identity** (attributable audit, granular revocation, least privilege on tools); prompts/policies/tools **versioned like code** with eval-gated promotion and a **pinned model**; a **kill-switch + rollback** ready; **human-in-the-loop gates implemented as deterministic code** in the workflow graph — **never** a decision left to the LLM (or it's bypassable via prompt injection); **retrieved data is untrusted** (prompt-injection defense); an audit trail of which version produced each decision.

5. **The graft is four interfaces.** Every automation plugs in through: a **trigger** (sync/event/cron); **tools** (least privilege); a **sink** (irreversible actions pass through the gate); and a **feedback/eval loop** (without measuring the outcome, the agent can't be governed). Host checklist before you start: data access verified, dedicated identity/permissions, a test environment, a human owner + runbook, observability on day one, rollback verified.

6. **The simplest architecture that solves it.** Single-agent with tool-use before multi-agent; add agents/autonomy only when the simpler level genuinely isn't enough.

7. **Learn from the failures.** In domains with liability (medical, legal, financial-advice positioning, etc.), a generative assistant **without grounding + gates + adversarial tests is a liability**, not a feature. Such automations are **gated by the compliance department before they exist.** Vendor ROI numbers are *interested claims* until you re-verify them at the source.

## Where this lives in a project

- The **rules** (this page) are constant across projects.
- The **project-specific decisions** — *what* you automate, at *which* stage, behind *which* gates — live in a project file (suggested name `AUTOMATION_PLAYBOOK.md` in the root), maintained by the manager.

## A one-line test before building any automation

> "Is this high-volume with a contained cost-of-error, do I have a deterministic human gate on the irreversible step, and can I measure whether it's working?" If any answer is no, you're not ready to give it autonomy yet — keep a human in the loop and write down the trigger to revisit.

Next: **[07 · Glossary](07-glossary.md)**.
