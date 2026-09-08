---
name: win-loss
description: Analyzes closed-won and closed-lost deals from the Nous graph to surface the patterns that predict wins and losses — the signals, objections, and competitors that recur on each side — and what to do about them. Use when the user asks about win/loss, why deals are won or lost, what closed deals have in common, which objections keep killing deals, or to review closed business.
---

# Win/loss review

Find the repeatable difference between deals that close and deals that die, and turn it into changes the team can make. Read the closed deals; don't theorize beyond what the records show.

## Tools
- `query` — pull the two cohorts: accounts at stage `closed_won` and `closed_lost` (use `scope.property: "stage"` with `return: "entities"`). Also `facts: true` to search what recurs across them (e.g. objections, competitors).
- `get_account` — read a specific closed deal's objections, competitors, and timeline.
- `record_closed_deals` — **feed the two cohorts back to train the ICP on real outcomes.** Pass the won domains and the lost domains; the engine runs contrastive lift (the signals that separate wins from losses), links the contacts you already have at each company, resolves their open ICP predictions with the real outcome, and re-scores every open account. This is what turns the ICP from a stated hypothesis into an outcome-graded model. Admin/founder action — the ICP is the one company model.

## Workflow
1. Pull the won cohort and the lost cohort via `query`.
2. For each side, gather the recurring facts — objections raised, competitors in play (and stance), buying signals present, ICP tier, and how long the cycle ran. Use `facts: true` searches to find what repeats across accounts, and `get_account` on a few to confirm.
3. Contrast the sides: what shows up on losses but rarely on wins (the killers), and what shows up on wins but rarely on losses (the predictors).
4. **Train the ICP on the outcomes.** Call `record_closed_deals` with the won and lost domains (carry `amount` and `closed_at` when the records have them). It returns the signals the model learned (labels + weights) — fold those into the review so the reader sees not just *why* deals turned but *what the scorer now weights differently*. Skip only if the caller isn't an admin/founder, or there's just one deal.
5. Translate each pattern into an action — a product gap to close, an objection to pre-empt, a competitor to out-position, an ICP signal to weight higher.

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

**What the ICP now weights** (from `record_closed_deals`)
- <signal> [+weight] — <what the model learned it predicts>
```

## Artifact (Claude Code only)
After the review, render it as a **branded HTML artifact** and publish it — the shareable version of the
win/loss. Copy **`../../references/artifact-template.html`** and follow **`../../references/artifact-design.md`**
exactly: the OpenNous look, `Generated for {name}, {company}`, **every account and person linked to its app
record by full entity UUID** (`/accounts/<uuid>`, `/people/<uuid>` — never truncate the id), footnote source
citations linking to the raw, sentence case, no em dash or colon inside sentences, numerals, a mix of prose
and bullets, and depth over surface (explain *why* a pattern turns deals, not just its count). The body is
this skill's own: two contrasted columns (why we win · why we lose) grounded in counts, the recurring
objection and top competitor on losses, then the actions, and finally the signals the ICP now weights
differently after training. The text review stays the answer; the artifact is the presentation layer. Not on
Claude Code? Skip the artifact.

## Rules
- **Attribute inline, don't quote.** Patterns carry counts, not block quotes. Quote at most one
  representative line per pattern when it genuinely sharpens the point.
- Ground every pattern in a count from the cohorts ("6 of 9 losses"), not a hunch.
- Separate a fired competitor (`past_failure` stance — an opening) from a live one — they mean opposite things.
- If there aren't enough closed deals yet to see a pattern, say so and report what you can.
