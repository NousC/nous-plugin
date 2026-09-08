---
name: forecast
description: Rolls up the pipeline into a commit / best-case / at-risk forecast, narrated from stage, deal health, and recorded signals — with the specific risk on each committed number and the one move to protect it. Use when the user asks for their forecast, what will close, commit vs best-case, whether they'll hit the number, or a quarter/month roll-up. Judges deals on evidence, not hope.
---

# Forecast

Not a sum of open deals — a judged roll-up. Every number carries its risk, and a deal with no next step booked is not a commit.

## Tools
- `query` — open deals with `stage`, `deal.value`, close date, health band, and recent signals; scope to the period the user names (this quarter/month).
- `score` — the ICP fit + intent to weight a deal's likelihood when health alone is ambiguous.

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

## Artifact (Claude Code only)
After the text roll-up, render it as a **branded HTML artifact** and publish it. Copy
**`../../references/artifact-template.html`** and follow **`../../references/artifact-design.md`**
exactly. The **shell and rules are shared** with every OpenNous artifact (masthead, `Generated for
{name}, {company}`, full-UUID account/person links, footnote source citations, sentence case, no em
dash or colon, numerals, depth over surface, footer). The **body is this skill's own**: the commit,
best-case, and at-risk bands, the specific risk on each committed number, and the one move to protect
the number. The natural visual here is a **commit vs best-case vs at-risk bar** of the three totals.
The text stays the answer. Not on Claude Code? Skip the artifact.

## Rules
- **Evidence, not hope** — categorize on health + stage + recorded signals; a deal whose signals contradict its stage is not commit, whatever the rep feels.
- **No next step booked → not commit.** Say so.
- **Every committed number carries its risk** — a commit list without risks is a wish list.
- Don't inflate a deal value or invent a close date.
