---
name: revenue-report
description: Reconstructs how revenue actually moved over the window and diagnoses where it leaks — pipeline created and converted, where deals die, quantified revenue leakage, what a winning deal looks like here, what the CRM missed, and a playbook of changes drawn from their own data. Use after a backfill completes, when the user asks for "the revenue report", "how did the last six months go", "where are we losing deals", or when onboarding reaches its payoff. Produces ONE document. For the deep read on closed deals alone, use `win-loss`; for today's worklist use `focus`.
---

# Revenue report

The first time this operator sees their whole revenue motion in one place: six months that lived
scattered across a CRM, a notetaker and an inbox, reconstructed into how revenue actually moved
— not what the CRM says today.

**Diagnostic, not dashboard.** This is the difference between a report someone reads once and one
they act on:

> ❌ Stage 3 → Stage 4 conversion: 34%
>
> ✅ $481K of pipeline stalled between discovery and evaluation. 67% of those had no documented
> next step within 48 hours of the discovery call. Deals that closed averaged 1.3 days from
> discovery to the next scheduled interaction; the ones that stalled averaged 5.8.
> **Change: no discovery advances without a calendared next step.**

Every number earns its place by implying an action.


## Who you are writing it for

**Read the role block in your context before you decide what this report leads with.** It
carries the reader's objective, what to lead with, and — the part that matters most — what to
leave out.

The same six months produce a different document for different seats, and this is not tone.
A CRO opens on pipeline coverage against the number and forecast risk; naming one deal as the
headline finding wastes their read. A RevOps lead opens on where the funnel leaks with counts,
and treats a named account as evidence of a pattern, never as the finding. An AE opens on the
deals they own and what changed on each; an aggregate conversion rate tells them nothing about
which call to make. An SDR wants the accounts worth the next hour and the angle — a strategy
essay is the wrong document entirely.

Two rules:

- **The `Leave out` line is binding.** It is what makes the report feel written for someone
  rather than addressed to them. Including it anyway because it seemed useful is the failure.
- **No role block, or several roles?** Write the whole-book read (the default below). With
  several, lead with the most senior and say which hat each section is under.

Everything below still holds. The role decides the ORDER and the EMPHASIS, never the honesty:
sourcing, the `[n]` markers, and saying plainly what the data cannot support are the same for
every seat.
## Tools

`query` for the cohorts, stages and timings · `pipeline` for the live funnel · `get_account` for
the accounts you name · `insights` for what the market said · `score` for fit · `whoami` for
whose slice this is.

## What to reconstruct

Pull what the record supports; skip what it doesn't. Never infer a funnel nobody recorded.

- **Pipeline created and converted** — opportunities and value created, by source, segment and
  owner. Then the funnel: lead → meeting → opportunity → qualified → won.
- **Where it dies** — the stage-to-stage drop that costs the most, in dollars, not percent.
- **Velocity** — median days between stages, time to close, and time sitting idle. Idle time is
  the one most teams have never measured.
- **Follow-up behaviour** — meeting → follow-up latency, unanswered threads, meetings with no
  next step booked.
- **Stakeholder coverage** — single- versus multi-threaded, and whether an economic buyer ever
  appeared.
- **Momentum** — activity accelerating or decaying before wins and losses.
- **Friction** — the objections, competitors, pricing moments and technical blockers that recur,
  and at which stage they land.
- **What converts** — which segments, use cases, sources and signals actually produce revenue
  rather than meetings.

## The sections

Eleven, in this order. The order is the argument: what happened, whether you can trust it, where
it leaks, what winning looks like, what is still savable, and what to change. Each one opens with
a **thesis** — one sentence stating what the section found, which a reader could disagree with —
and never a label. "Most value loss begins between discovery and evaluation" rather than "Pipeline
performance".

⛔ Never write that thesis as a negation followed by a correction. No "it is not X, it is Y", no
"the funnel is not broken at the bottom — the loss begins earlier". State the finding once,
positively, and stop. The construction sounds insightful and says half as much as the plain
sentence.

**A section with no number, no series and no quote is not a section.** Cut it. Padding a thin
window with prose is what makes a report read like an essay nobody finishes.

**01 — Executive summary.** The headline numbers first: pipeline created, closed won, created→won
rate, and the leakage figure. Then two sentences on what the window says. Then three to five
bullets of the biggest takeaways. Then the priorities, as a table:

| Priority | Why it matters | 30-day action |
|---|---|---|
| Multi-thread before proposal | 79% of wins had 3+ stakeholders; 38% of losses did | No proposal leaves without the economic buyer on a call |

**02 — Data coverage and method.** The section that makes the rest believable, so never cut it to
save room. How much was reconstructed and from where:

| Source | Backfilled | Coverage | Used for |
|---|---|---|---|
| CRM | 137 opportunities, 14 stages | Mar 1 – Aug 31 | Stage history, values, outcomes |
| Gmail | 8,462 threads | Mar 1 – Aug 31 | Follow-up timing, silence, stakeholder reach |

Then say plainly how to read the figures. An **observed fact** is present in a source record: a
stage change, an email timestamp, an attendee, a sentence in a transcript. A **derived metric** is
computed from observed facts: time to follow-up, stakeholder count, days idle, stage duration. An
**inferred pattern** is an association across the won and lost cohorts and is not proof of cause.
A **recommendation** is an operating change suggested by a repeated pattern plus current exposure.
Name every gap: a source that is not connected is a finding, not an omission.

**03 — Pipeline performance.** The funnel, read off the stage HISTORY rather than where records sit
today:

| Stage | Entered | Advanced | Conversion | Median days |
|---|---|---|---|---|

Report it in the team's OWN stage names, in the order their deals actually move through them —
never mapped onto a ladder of ours they do not use. Then created pipeline by outcome: won, lost,
no-decision, still open, with value and share. Close with the stage where the most value stops.

**04 — Revenue leakage.** Lead with one figure — *potential revenue leakage identified: $1.26M* —
then break it down by pattern with the pipeline each accounts for:

| Leakage pattern | Pipeline affected | Observed behaviour | Why it matters | Priority |
|---|---|---|---|---|
| No engagement for >14 days | $420K | No recorded touch either way for two weeks | Deals past 14 days quiet close at a third of the base rate | High |

⚠️ These figures **overlap and never sum to a total.** One deal can be quiet, single-threaded and
missing an economic buyer at once, so each row is exposure on its own. Say so in the section; a
reader who adds them up and gets more than the pipeline stops trusting the whole document.
**Without deal values, run the same patterns on deal COUNT and say plainly that dollars need a CRM
or Stripe connected.**

**05 — What winning deals did differently.** Built from their own closed-won deals, against the
ones that did not close:

| Signal | Won | Lost / no decision | Gap |
|---|---|---|---|
| Stakeholders engaged (median) | 4 | 2 | 2× |
| Hours to follow up after a meeting | 9.6 | 34.2 | 3.6× slower |

State the cohort sizes beside any percentage, and say **observed**, not proven — these are
associations, and multi-threaded deals tend to be the same deals with an engaged economic buyer.
Fold the segment or ICP cut in here as one extra table where the sample supports it, with its n.
It does not earn its own section.

**06 — Pipeline velocity.** Days per phase, won against not-won, and the longest silence while the
deal was live — the measure most teams have never taken. Then what the gap means for the forecast:
behavioural deterioration is usually visible in email and meeting activity two to three weeks
before the CRM stage moves, which is why slow deals get recognised as risky too late.

**07 — Current pipeline recovery.** The section that makes this report worth reading twice, so give
it room. Apply the historical patterns to the deals that are STILL OPEN:

| Account | Value | Days quiet | Patterns present | Intervention |
|---|---|---|---|---|

Name the accounts. ⛔ Never call the total "recoverable revenue" — it is pipeline where a specific,
observable failure mode was found and where something can still be done. Say which of them need a
manager rather than a rep, and which should be re-qualified out rather than nursed.

**08 — Win/loss snapshot.** A TEASE, not an analysis: the counts, the values, the no-decision pool,
and at most one table. Two or three sentences, then say the full read is the `win-loss` skill.
⛔ Do not analyse loss reasons here, do not name competitors, and do not quote buyers on why they
chose. That is the other report, and writing it twice makes one of them padding.

**09 — The next 30 days.** Five to ten operating changes, each built on a figure from this report:

| Operating change | Trigger | Owner | Success measure |
|---|---|---|---|
| Follow up within 24 hours | Any customer meeting ends | AE | Median hours to next touch under 12 by day 30 |

Then what the system should watch continuously: deal momentum, the stakeholder map, commitments
made on both sides, risk language, and whether observed activity supports the stage and close date.

**10 — Appendix: one account, reconstructed.** A single real account as a dated timeline — date ·
source · observed event · what it meant commercially — plus a short table of metric definitions.
This is the proof the reconstruction is real, so pick an account with a genuine trail.

**11 — How this report was generated.** The closing page. When it ran, over what window, from which
sources, how a figure was computed, and what it refreshes from. Section 02 says what the DATA
covers; this says what the RUN did, which is the question somebody asks three weeks later when a
number has moved. End with the limits, in plain words.

## Which seats this report is for

The founder, the CRO and the VP Sales — the seats that think in coverage, leakage and what to
change. The role block still decides ORDER and EMPHASIS within these eleven sections.

It is **not** the report for every seat. An SDR needs the accounts worth the next hour and the
angle, which is `triage-leads` or `focus`; an AE needs their own deals and what changed, which is
`review-pipeline`. Bending this document to fit those asks produces an approximation instead of an
answer.

## Output

Hand it over as ONE document, titled for the window it covers. Then one line in the chat: the
finding you'd lead with if you had ten seconds, and what you'd do about it.

## Rules

- **Named accounts and real numbers, or cut the line.** "Several deals stalled" is not a finding.
- **Say "associated with", never "caused".** You are reading correlation in one team's history.
  Deals with an economic buyer closed more often — that is what the record shows, and claiming
  causality from it is the fastest way to lose a numerate reader.
- **Admit thin samples.** A pattern drawn from four deals says so. Confident emptiness is the
  fastest way to lose trust in a report like this.
- **Their words, not yours.** Where a buyer said the thing, quote them, with the date.
- **No tracked outcomes means say so.** No CRM stages and no Stripe: make it a finding, note that
  the ICP stays a hypothesis until deals close, and never invent revenue nobody recorded.
