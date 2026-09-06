---
name: pipeline-review
description: Reviews the whole account portfolio from the Nous graph — stage distribution, deal health, ICP fit, what's slipping, competitive risk, and the deals worth re-engaging — and writes a pipeline briefing grounded in real numbers and named accounts. Use when the user asks to review the pipeline, analyze the funnel, find at-risk or re-engage deals, spot where deals are stalling, or asks "how's my pipeline / what should I work on".
---

# Pipeline review

Turn the portfolio into a briefing the user can act on in five minutes: the shape of the funnel, what's healthy, what's slipping, and who to touch next — always with numbers and names.

## Tools
- `mcp__nous__pipeline` — the whole portfolio in one call: every in-touch account with health band, ICP fit, stage, days-quiet, open flags, open-objection count, and live competitors, plus stage counts. Start here; do not rebuild the funnel from raw `query` calls.
- `mcp__nous__get_account` — drill into a specific account the review surfaces.

## Workflow
1. Call `mcp__nous__pipeline` once. Reason over the returned rows directly.
2. Read the funnel shape from the stage counts — name the biggest stage-to-stage drop.
3. Classify accounts from the same data:
   - **Re-engage** — an open flag, or high ICP fit + low health + high days-quiet.
   - **Competitive risk** — a non-empty competitors list.
   - **Slipping** — at-risk / needs-attention with rising days-quiet in a late stage.
   - **Healthy movers** — high health advancing through stages.
4. Pick the few accounts in each bucket that actually matter (not the whole list) and, if useful, `get_account` one to add a concrete reason.

## Output
```
# Pipeline review — <date>
**Funnel:** <stage: n> · <stage: n> · …   (biggest drop: <stage → stage>)
**Health mix:** <n healthy · n at-risk · n needs-attention>

**Re-engage now**
- <account> — ICP <score>, <days> quiet, <flag/reason>

**Competitive risk**
- <account> — <competitor> (<stance>)

**Slipping**
- <account> — <stage>, <days> quiet, <why>
```

## Rules
- Every line names a real account and a real number from the data — no vague "several deals".
- Frame re-engagement as the next send/touch, never "un-pause this".
- If the portfolio is small or empty, say so plainly and suggest running backfill or ingest first.
