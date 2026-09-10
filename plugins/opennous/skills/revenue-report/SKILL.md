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

**1. The picture.** A few sentences that land the "oh". The state of their revenue over the
window as a picture, not metrics. What a leader needs to grasp in ten seconds.

**2. What stood out.** The arc of the period and, above all, the surprising: a deal that went
dark, a segment that over-indexed, an account nobody owned. Name the notable, never the routine.

**3. Revenue leakage — the number.** Lead with one figure: *potential revenue leakage identified:
$1.26M*. Then break it down by pattern, with the pipeline each accounts for:

| Leakage pattern | Pipeline affected |
|---|---|
| No engagement for >14 days | $420K |
| No economic buyer involved | $310K |
| No follow-up within 48h of a meeting | $185K |
| Technical objection left unresolved | $145K |
| Champion went quiet or left | $110K |
| Proposal sent with no next step booked | $90K |

Name the biggest accounts inside each pattern. This is the strongest section in the report: you
are not offering sales insights, you are saying *we reconstructed six months of your revenue
process and found where the money leaks*. **Without deal values, run the same patterns on deal
COUNT and say plainly that dollars need a CRM or Stripe.**

**4. What a win looks like here.** Build the profile from their own closed-won deals — days from
first meeting to close, stakeholders involved, when the economic buyer appears, meetings before
close, follow-up latency, whether the next meeting gets booked inside the previous one, when
pricing enters relative to the business case. Then hold the LIVE pipeline against it: *"Acme
resembles the shape of your won deals; Globex doesn't — single-threaded, 11 days quiet, and
pricing came up before the economic buyer did."* This is where a retrospective starts improving
today's pipeline.

**5. What your CRM missed.** Two columns, then your read. The CRM's stage, close date and next
step against what the activity actually says — last real meeting 19 days ago, the outbound
unanswered, a budget freeze mentioned on a call, the decision-maker who has never attended
anything, "probably revisit next quarter" said out loud. Then the honest assessment: *likely
slipped*. This is the section that proves why connecting everything around an account beats
trusting CRM hygiene.

**6. What you weren't tracking.** Relationships that live in conversations but were never in the
CRM, promised follow-ups that never happened, accounts that went dark after real interest. With
the date and the quote.

**7. What the market told you.** From `insights` — product, positioning, market and buyer themes
with the people who said them, framed as decisions to make.

**8. Coverage.** Whose view this is. Team-shared sources (CRM, Stripe, outbound) carry the whole
team; personal sources (notetaker, email) carry one seat's conversations. Name what's hiding and
what connecting the rest would add.

**9. The playbook — five to ten changes.** End with changes they can implement, each built on a
number from their own data. Not analytics: a playbook.

> **01 — Discovery follow-up.** Won deals get a follow-up 4.5× faster than stalled ones.
> *Change: follow up within 24 hours and book the next interaction before discovery ends.*
>
> **02 — Multithreading.** 79% of wins involved three or more stakeholders; 31% of losses did.
> *Change: introduce the economic buyer before the proposal.*

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
