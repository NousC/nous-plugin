---
name: win-loss
description: Why you win, why you lose, and what systematically changes the outcome — read from what customers actually said across calls and email, not the closed-lost dropdown. Splits losses into controllable, structural and no-decision, compares won against lost behaviour directly, and cuts it by segment, competitor, stage and source where the sample allows. Use when the user asks about win/loss, why deals are won or lost, what closed deals have in common, which objections keep killing deals, or to review closed business.
---

# Win/loss review

Find the repeatable difference between deals that close and deals that die, and turn it into changes the team can make. Read the closed deals; don't theorize beyond what the records show.


## Shape it for the reader

Check the role block in your context. Win/loss is one analysis but not one document: a CRO
reads it for what it says about the engine and the forecast; a VP Sales for what it says about
the team and where coaching changes the number; Product for the capability gaps behind the
losses, with the revenue attached. Honour the reader's `Leave out` line.

The findings themselves never move. Which one leads does.
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

## The sections

**Executive summary.** The counts and the money: *42 won, 67 lost, $2.8M won, $4.1M lost over the
window.* Then the primary win drivers and loss drivers in a line each, and — the sentence that
matters most — **the largest CONTROLLABLE loss driver, with the pipeline it cost.** "Stakeholder
coverage, ~$840K" is a number a leader can act on this quarter.

**Why we win.** Not "product quality". The patterns, grouped, each with its count:
- *Business pain* — a clear expensive problem, an urgent initiative, executive visibility.
- *People* — a strong champion, the economic buyer engaged early, several stakeholders.
- *Product* — integration requirements met, clear technical fit, fast time to value.
- *Process* — fast follow-up, real discovery, clear next steps, a business case written down.
- *Commercial* — ROI established before price came up, procurement engaged early.

**Why we lose — in three buckets, never one "reason" field.** This is the part a CRM's
closed-lost dropdown gets wrong, and you have what people actually said:
- **Controllable** — thin discovery, no champion, single-threaded, weak follow-up, no business
  case, pricing handled badly, the wrong stakeholder, a technical objection left open, a
  competitor out-positioning us, a bad handoff. These are the ones worth the report.
- **Structural** — budget cancelled, a restructure, the project killed, an acquisition, a
  regulatory block, a capability we genuinely don't have. Name them and move on; they aren't a
  coaching problem.
- **No decision** — its own category, and usually the biggest. Most "lost" B2B pipeline never
  chose a competitor; it chose nothing. *"41% of lost pipeline was no-decision. Those deals
  shared three things: weak urgency, no economic buyer, no quantified business impact."* That is
  worth ten times more than "lost reason: timing".

**Won versus lost.** The direct comparison, as a table, with the behaviours you can actually
observe:

| Behaviour | Won | Lost |
|---|---|---|
| Economic buyer engaged | 76% | 28% |
| 3+ stakeholders | 81% | 39% |
| Next step within 48h | 89% | 52% |
| Champion identified | 83% | 44% |
| Business impact quantified | 68% | 21% |
| >14 days inactivity | 12% | 57% |

Then name the widest gap as the strongest observed differentiator — and say **observed**, not
proven.

**The cuts.** Only where the sample supports them, and say the n: by segment, by use case, by
competitor, by rep (behaviours, never a leaderboard), by stage, by persona, by source, by deal
size, and over time — *are the loss reasons changing?*

**What the ICP now weights.** From `record_closed_deals`: the signals the model learned, so the
reader watches the ICP move from their stated theory to what actually closed revenue.

**Do about it.** Each pattern turned into one change, tied to its number.

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
- **Read what customers SAID, not the closed-lost field.** The dropdown says "price"; the call
  says they never found an internal sponsor. The whole reason this analysis is worth running on
  our data is that we have the conversation and the CRM only has the label.
- Ground every pattern in a count from the cohorts ("6 of 9 losses"), never a hunch.
- **"Associated with", never "caused by".** These are correlations in one team's history. The
  claim is that won deals looked like this — not that doing this wins deals.
- **Say the n, and say when it's too small.** A percentage over seven deals is a story about
  seven deals. Report it as such rather than dressing it as a rate.
- Separate a fired competitor (`past_failure` stance — an opening) from a live one; they mean
  opposite things.
- Never rank reps. Differences in behaviour are a coaching input; a leaderboard built from an
  agent's read of a CRM is how a team stops trusting the tool.
- If there aren't enough closed deals to see a pattern, say that plainly and report what you can.
