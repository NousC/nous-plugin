---
name: brief
description: Briefs you on an account — shaped by YOUR job. An SDR gets why this account, who to contact and what to say; an AE gets the deal, the committee, the risks and what moves it; CS gets adoption, health, commitments and the renewal. Use when the user says "brief me", "prep me for my meeting with", "account brief for", "what do I need to know before I talk to", "who am I meeting with", or names an upcoming call. This is the working read on one account; for the full strategic plan use `plan-account`.
---

# Brief

One account, read for the job the operator actually does. An SDR and an AE looking at the same
company need different briefs — not the same brief at different lengths. The SDR is trying to
CREATE the opportunity, the AE to CLOSE it, CS to keep and grow it. Write the one they need.

## First: whose brief is this

Call `whoami`. It returns the operator's GTM role(s), and that decides the shape before you read
a thing about the account.

| Role | Shape |
|---|---|
| `sdr`, `bdr` | **Prospecting brief** — why this account, who to contact, what to say |
| `account_executive`, `sales` | **Deal brief** — where the deal stands, who decides, what could kill it |
| `customer_success` | **Customer brief** — value delivered, health, commitments, renewal |
| several roles | The one the ACCOUNT calls for (below) |
| `founder`, `revops`, nothing on file | Let the account decide (below) |

**When the role doesn't decide it, the account does.** A founder does all three jobs in a week,
so read where this account actually is: no meeting held yet → prospecting brief; live deal →
deal brief; closed-won or an existing customer → customer brief. Ask the operator only when the
record is genuinely ambiguous, and ask in one line ("Working this as an SDR or an AE?") — never
open with a question you could have answered from `whoami` or the stage.

## Tools
- `get_context` — `intent: "meeting_prep"` for a call, `"account_review"` otherwise. The headline
  tool: the task-shaped read, each fact carrying its speaker and verbatim quote.
- `get_account` — the full record when the brief needs a thread `get_context` didn't surface: a
  specific objection, a commitment made two calls ago, the buying committee.
- `score` — ICP fit and intent, where the brief turns on whether this account is worth the time.

## The three shapes

Cover what the record supports. **Skip any section with nothing real behind it** — an empty
heading is worse than a missing one, and "No data available" is not a section. They're in
priority order: with material for only three, write those three.

### Prospecting brief (SDR)

Answers: *why should I contact this account, who, and what do I say?*

- **The account** — what they do, size, market, and whatever about their setup matters to us.
- **Why this account** — ICP fit in a sentence, with the score. If they're a weak fit say so
  plainly and say what would change it; a brief that talks an SDR into a bad account costs them
  a week.
- **What's happening there** — the triggers: hiring, funding, a launch, a leadership change, a
  post, a site visit. This is the reason to reach out NOW rather than next quarter.
- **Who to contact** — the likely buyer, plus the champion or entry point if they differ. Name
  them, name the title, and say why that person rather than the obvious one.
- **What we know about them** — what they've said publicly or to us. Their words, not a summary.
- **The angle** — the strongest specific reason THIS account should talk to us, in a line the
  operator could nearly send as-is.
- **Where we've been** — every prior touch and what came back: replies, clicks, silence, a
  sequence that bounced. Never pitch into a thread that already went cold without naming it.
- **The move** — one action with a person attached: "message the VP Sales about X", not
  "consider outreach".

### Deal brief (AE)

Answers: *how do I move this toward a close, what's missing, and what kills it?*

- **The deal** — stage, value, expected close, what they're actually buying.
- **Health** — healthy, slipping or stalled, and why: days quiet, a passed commitment, a stage
  that hasn't moved. Be blunt. A green light on a dead deal is worse than no light.
- **The problem they're solving** — in their words, with the cost of leaving it alone.
- **What good looks like to them** — the outcome they said they want, not the feature they asked
  about.
- **The committee** — champion, economic buyer, blocker, and the ones we haven't met. For each:
  what they care about and where they stand. Name who is MISSING — single-threaded is a risk,
  not a gap.
- **What they've said** — the quotes that matter, dated. Objections, requirements, hesitation.
- **Risks** — pricing, timing, authority, a competitor, a technical requirement, an unanswered
  security question. Say which one you'd bet kills it.
- **Commitments** — what each side promised and whether it happened. An overdue promise of OURS
  is the first thing to fix.
- **Open questions** — what we'd still need to know to forecast this honestly.
- **The move** — the next step that advances the stage, and who it's on.

### Customer brief (CS)

Answers: *are they getting value, will they stay, will they grow?*

- **What they bought** — package, seats, term, renewal date.
- **Why they bought** — the original pain and the outcome they were promised. That's the bar.
- **What sales promised** — commitments made in the deal, especially any still outstanding. The
  handoff is where value quietly dies.
- **Where the rollout is** — onboarding, integrations, migration: done, in flight, or stuck.
- **Adoption** — who's actually using it, and the direction of travel. A flat line is a signal.
- **The people** — champion, admin, sponsor, and who's gone quiet. A champion leaving is the
  highest-signal event on an account; if it happened, lead with it.
- **How they feel** — sentiment from what they actually said, with the quote.
- **Open issues** — bugs, blockers, missing pieces, and how long they've been open.
- **Health and renewal** — the honest read, the date, and what has to be true to renew.
- **Expansion** — a new team, a new use case, rising usage. Only where the record shows it.
- **The move** — the one thing to do this week.

## Close the loop

The things you tell them to do are decisions. Write the recommendation down before it's acted on
(`decision.proposed` with a `decision_id`, a `rationale`, and the `evidence_ids` you reasoned
from), record what they decided, and stamp the same `decision_id` on the interaction when it
fires. Full mechanic: `../../references/decision-loop.md`.

Do not narrate this. It's bookkeeping, not part of the output.

## Rules

- **Show the evidence.** Every fact carries its speaker and verbatim quote from `get_context`.
  The quote IS the value — the operator walks in knowing the actual words, not a paraphrase of
  them. Never invent one to fill a gap.
- **A brief manages the account, it doesn't summarise it.** Every section should change what the
  operator does next. If a line wouldn't alter a single decision, cut it.
- **Say what's missing.** "No economic buyer identified", "no reply since Aug 10", "we never
  answered their security question". The gaps are the actionable part, and they're the thing a
  summary always leaves out.
- **Never pad.** Three sections of substance beat nine with filler. They're reading this between
  meetings.
