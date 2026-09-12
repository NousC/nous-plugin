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

## What this report is for, and what it is not

**Cause, not behaviour.** `revenue-report` owns what winning deals DID — follow-up timing,
stakeholder counts, stage velocity. This owns WHY the decision went the way it did, in the words of
the people who decided it.

⛔ **Do not put a won-versus-lost behaviour table in this report.** It is the revenue report's
section 05, and the same chart in two reports is padding in one of them — a reader who notices
stops trusting both. Reference it in a line and stay on cause.

## Say where the causes come from

There is **no CRM loss-reason field** in this product, which means the usual move — take the rep's
dropdown and correct it against the evidence — is unavailable. Every cause here is derived from
what was recorded while the deal was live: an objection still open when it closed, a competitor
named on a call, a budget or timing constraint stated out loud, a stretch of silence.

Say that in the report. Nobody's dropdown habits are in the data, which means nobody's dropdown
habits are in the finding, and that is the reason a reader should trust this taxonomy over the one
their CRM prints.

The price is an **unclassified bucket**, and it is a feature. A deal that closed with no objection,
no competitor and no stated constraint anywhere on the record is a deal you cannot explain.
**Report that count rather than distributing those deals across the causes you can see.** A
taxonomy with no residual is a taxonomy that guessed.

## Enough deals, or do not write it

A taxonomy over sixty losses is a finding. Over nine it is a story about nine deals wearing the
clothes of a pattern. **Below roughly 20 closed deals in the window, do not write this report** —
say so in one line and point at the win/loss snapshot inside `revenue-report`, which is the honest
read at that volume.

## The sections

Seven, in this order.

**01 — Executive readout.** The counts and the money: *42 won, 66 did not close, $3.35M won,
$4.35M that did not.* Then the single largest **avoidable** pattern with the pipeline it cost —
"no-decision, $1.45M" is a number a leader can act on this quarter, where "lost to competition" is
usually not. Then the spine of the whole report as a table:

| Finding | The evidence behind it | What it implies |
|---|---|---|
| Indecision costs more than competition | $1.45M no-decision against $0.91M competitive | Work urgency, not differentiation |

Everything below is that table in detail.

**02 — Why deals were lost.** The taxonomy, derived from evidence, with its share of the cohort and
whether the team controls it:

| Cause | Deals | Share | Controllable | What was on the record |
|---|---|---|---|---|
| An objection was raised and never resolved | 24 | 36% | Yes | *"We still need to understand how this handles our SOC 2 obligations"* — never answered |
| The conversation stopped before the decision | 21 | 32% | Yes | 23 days median silence before close |
| No decision-maker was ever identified | 19 | 29% | Yes | Nothing on record |
| A competitor was in the deal | 14 | 21% | Partly | *"We are already some way down the road with Meridian"* |
| A budget or commercial constraint was stated | 9 | 14% | No | *"Nothing new gets signed until the new fiscal year"* |

Every row carries a real line somebody said, or it is a label — and a label is what this report
exists to replace.

Three things to say alongside it. The causes **overlap and never sum**: one deal can carry an open
objection, a silence and no named buyer, so a total above the cohort size is the tell that they are
not exclusive. "No decision-maker was ever identified" is the **absence of a record** — it says
nobody wrote down who signs, which is a gap in what was captured before it is a reason a buyer
walked, and it cannot carry a quote. And the **unclassified count** goes here, in the open.

**03 — The objections that decided it.** The most usable section in the report, because a rep can
act on an answer and can only nod at a taxonomy:

| Objection | Accounts | What they said | The answer that worked |
|---|---|---|---|
| Security and compliance review | 18 | *"We cannot start without a completed SOC 2 questionnaire and a DPA"* | Send both unprompted after the first technical call, and name a date for sign-off in the same message |

Keep the answer in the words that were used rather than summarising it into advice. Where an
objection was handled in the deals that closed and left open in the ones that did not, say so — that
contrast is the finding.

**04 — Who we lose to.** Only where a competitor actually resolved to a name:

| Competitor | Deals | Value | What they were said to do better |
|---|---|---|---|

⛔ **Never infer a vendor name from a sentence.** These claims are prose about a competing tool, not
a vendor field, and guessing produces "we lose to Grant" — a contact's first name printed as a
competitor, in the one section a CRO will quote back at their team. Where no name resolved, write
one line saying a competitor was present in that many deals and that none resolved to a named
vendor, and quote the lines.

**05 — A deal we won, traced.** One real, named account as a dated timeline:

| Date | What happened | Why it mattered |
|---|---|---|
| 14 May | Discovery call, two stakeholders attended | Two before anything was shown; most losses never got past one |

Close with how many other wins carried the same shape. One deal is vivid and can be dismissed as
cherry-picked; the count is what makes it defensible.

**06 — A deal nobody decided, traced.** The same shape, plus where it could have been caught. Give
it the same room as 05: it is the most expensive loss mode in most books and it looks healthy in a
CRM for weeks. Name the earliest point at which the outcome was already visible.

**07 — Inspection questions.** The report turned into a management routine:

| Question | When a manager asks it | What to do when the answer is missing |
|---|---|---|
| Who signs this, and have they been on a call? | Before the deal enters proposal | Do not send the proposal; get the introduction first |

Five to eight rows, each traceable to a cause in section 02. Questions a manager asks in a pipeline
review, answerable from the record in under a minute — never policies.

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
