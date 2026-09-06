---
name: forecast
description: Rolls up the pipeline into a commit / best-case / at-risk forecast, narrated from stage, deal health, and recorded signals — with the specific risk on each committed number and the one move to protect it. Use when the user asks for their forecast, what will close, commit vs best-case, whether they'll hit the number, or a quarter/month roll-up. Judges deals on evidence, not hope.
---

# Forecast

Not a sum of open deals — a judged roll-up. Every number carries its risk, and a deal with no next step booked is not a commit.

## Tools
- `mcp__nous__query` — open deals with `stage`, `deal.value`, close date, health band, and recent signals; scope to the period the user names (this quarter/month).
- `mcp__nous__score` — the ICP fit + intent to weight a deal's likelihood when health alone is ambiguous.

## Workflow
1. **Set the period** from the request (default: current quarter). Pull open deals via `query`.
2. **Categorize each deal:**
   - **Commit** — late stage, healthy, a next step booked, no unaddressed hard objection.
   - **Best-case** — real upside but a live risk (stalled stage, open objection, gone quiet).
   - **Pipeline / at-risk** — early, or health/signals contradict the stage.
3. **Sum each band.** For every committed deal, name the one risk that could move it out.
4. **The move that protects the number** — the single highest-leverage action across the committed + best-case set.

## Output
```
# Forecast — <period>
**Commit:** $<X>  ·  **Best-case:** $<Y>  ·  **At-risk:** $<Z>

**Commit (with the risk on each)**
- <account> — $<v>, <stage> → risk: <what could move it>

**Best-case (upside if the risk clears)**
- <account> — $<v> → <the blocker> → <what unlocks it>

**Protect the number**
→ <the one action to take this week, and which deal it saves>
```

## Rules
- **Evidence, not hope** — categorize on health + stage + recorded signals; a deal whose signals contradict its stage is not commit, whatever the rep feels.
- **No next step booked → not commit.** Say so.
- **Every committed number carries its risk** — a commit list without risks is a wish list.
- Ground each deal's placement in its record; don't inflate value or invent a close date.
