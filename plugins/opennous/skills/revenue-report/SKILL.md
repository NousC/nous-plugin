---
name: revenue-report
description: The first-look retrospective on the whole revenue motion — what happened over the window, what was striking, who won and lost and why, what nobody was tracking, how the pipeline actually behaves, and what the market said. Use after a backfill completes, when the user asks for "the revenue report", "how did the last six months go", "what does my pipeline look like overall", or when onboarding reaches its payoff. Produces ONE document. For the deep read on closed deals alone, use `win-loss`; for today's worklist use `focus`.
---

# Revenue report

The first time this operator sees their whole revenue motion in one place. Six months that lived
scattered across a CRM, a notetaker and an inbox, now one view they have never had.

The job is the **"oh — I could never see this before"** moment. Revelation first; the actions fall
out of it.

## Principles

- **Revelation, not inventory.** Never open with "I imported 412 activities." Open with the
  picture of their revenue. Counts are a footnote.
- **Retrospective.** A review of what HAPPENED — wins, losses, stalls, what was striking — not a
  to-do list.
- **Every claim sourced.** Each fact carries its `[n]` marker, so they can open the call or the
  email it came from. A number they can't trace is a number they have to take on faith.
- **Honest about coverage.** One seat's backfill is one seat's slice. Say what's missing and what
  connecting it would reveal. Never imply the picture is complete when it isn't.
- **Never fabricate a funnel.** No CRM stages and no Stripe means no tracked outcomes — say so
  plainly and make it a finding, don't infer revenue that nobody recorded.

## Tools

`query` for the cohorts and the patterns · `get_account` for the accounts you name ·
`insights` for what the market said · `score` for fit · `whoami` for whose slice this is.

## The sections

Write the ones the record supports, in this order. Skip any that would be empty; three sections
of substance beat eight with filler.

**1. The picture.** A few sentences that land the "oh". The state of their revenue over the
window as a picture, not metrics — what a leader needs to grasp in ten seconds.

**2. What stood out.** The arc of the period, and above all the *surprising*: a deal that went
dark, a segment that over-indexed, an account nobody owned. Name the notable, never the routine.

**3. Won, lost, stalled.** The outcomes and why. Won — and the signals that led there. Lost — and
the reason (objection, competitor, silence). Stalled — live-looking deals gone quiet, with how
long and the likely cause. Then the pattern: what predicts a win here versus a loss. Keep this to
the shape of it; the full analysis is its own document (`win-loss`), and this section should say
so rather than duplicating it. **With no tracked outcomes, say exactly that** — "you have zero
recorded closes; connect a CRM or Stripe to see wins and losses and to grade the ICP against real
revenue" — and note the ICP stays a hypothesis until deals close.

**4. What you weren't tracking.** The highest-value section, and usually the reason they keep the
report. What was INVISIBLE: relationships that live in conversations but were never in the CRM,
promised follow-ups that never happened, accounts that went dark after real interest. This is
where the leader says "oh no". Name them, with the date and the quote.

**5. How the pipeline behaves.** The patterns in how they actually run revenue: where deals
stall, follow-up cadence, which sources convert, the objections that keep coming back, the shape
of the motion. Behaviour they can't see from inside any one tool.

**6. What the market told you.** From `insights` — product, positioning, market and buyer themes
with the people who said them. Frame as decisions to make: a positioning that isn't landing, a
competitor being displaced, an unmet ask, a pricing signal.

**7. Coverage.** Whose view this is, honestly. Team-shared sources (CRM, Stripe, outbound) carry
the whole team's accounts; personal sources (notetaker, email) carry only this seat's
conversations. Name what's hiding — an account owned by a teammate who hasn't onboarded is a
shell without its story — and what connecting the rest would add.

## Output

Hand it over as ONE document, titled for the window it covers ("Revenue report — last 6 months").
Then say one line in the chat: the single finding you'd lead with if you had ten seconds, and
what you'd do about it.

## Rules

- **Named accounts and real numbers, or nothing.** "Several deals stalled" is not a finding;
  "Bidpath, Acme and Windseeker have sat in evaluation since July" is.
- **Their words, not yours.** Where a buyer said the thing, quote them.
- **Say what's thin.** A section built on four data points should admit it. The report's value is
  that it's trustworthy, and the fastest way to lose that is confident emptiness.
