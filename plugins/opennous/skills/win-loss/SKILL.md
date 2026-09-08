---
name: win-loss
description: Analyzes closed-won and closed-lost deals from the Nous graph to surface the patterns that predict wins and losses — the signals, objections, and competitors that recur on each side — and what to do about them. Use when the user asks about win/loss, why deals are won or lost, what closed deals have in common, which objections keep killing deals, or to review closed business.
---

# Win/loss review

Find the repeatable difference between deals that close and deals that die, and turn it into changes the team can make. Read the closed deals; don't theorize beyond what the records show.

## Tools
- `query` — pull the two cohorts: accounts at stage `closed_won` and `closed_lost` (use `scope.property: "stage"` with `return: "entities"`). Also `facts: true` to search what recurs across them (e.g. objections, competitors).
- `get_account` — read a specific closed deal's objections, competitors, and timeline.

## Workflow
1. Pull the won cohort and the lost cohort via `query`.
2. For each side, gather the recurring facts — objections raised, competitors in play (and stance), buying signals present, ICP tier, and how long the cycle ran. Use `facts: true` searches to find what repeats across accounts, and `get_account` on a few to confirm.
3. Contrast the sides: what shows up on losses but rarely on wins (the killers), and what shows up on wins but rarely on losses (the predictors).
4. Translate each pattern into an action — a product gap to close, an objection to pre-empt, a competitor to out-position, an ICP signal to weight higher.

## Output
```
# Win/loss review — <n won · n lost>

**Why we win**
- <pattern> — appears in <n>/<won> wins (e.g. "champion + economic buyer both engaged")

**Why we lose**
- <pattern> — appears in <n>/<lost> losses (e.g. "hard pricing objection left open")
- Top recurring objection: <objection>  ·  Top competitor on losses: <competitor>

**Do about it**
1. <action tied to a loss pattern>
2. <action tied to a win pattern>
```

## Rules
- Ground every pattern in a count from the cohorts ("6 of 9 losses"), not a hunch.
- Separate a fired competitor (`past_failure` stance — an opening) from a live one — they mean opposite things.
- If there aren't enough closed deals yet to see a pattern, say so and report what you can.
