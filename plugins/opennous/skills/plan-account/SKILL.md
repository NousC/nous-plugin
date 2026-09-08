---
name: plan-account
description: Builds a strategic account plan for one company or person from the Nous graph — who they are, the buying committee, open objections and competitors, deal health, ICP fit, and the recommended next moves. Use when the user asks to plan, brief, strategize, or prep an account or meeting, or asks what the plan is for an account or to get up to speed on one.
---

# Account plan

Turn one account's record into a plan the user can act on. You read the graph; you do not guess — if the record is thin, say so rather than inventing.

## Tools
- `get_account` — the full record: facts, Intel, buying committee, timeline, ICP.
- `get_context` — pass `intent: "account_review"` (or `meeting_prep`) for a task-shaped read when the account is large.
- `score` — the live ICP fit + intent if the record doesn't already carry it.

## Workflow
1. Resolve the account: call `get_account` with the email, domain, or entity id the user gave. If a name is ambiguous, ask which one from the candidates it returns.
2. Read the record for: who they are and their role; the buying committee and who's engaged; open objections and their hardness; competitors in play and stance; deal health and stage; ICP fit and intent; the most recent meaningful activity.
3. Identify the gaps that matter — a missing economic buyer, an unaddressed hard objection, a live competitor, a stalled stage, no next step booked.
4. Decide the next moves: the single most important action, then 2-3 supporting ones, each tied to a fact in the record (not generic advice).

## Output
Lead with a two-line situation summary, then use this shape (adapt to what the record holds):

```
# <Account> — account plan
**Health:** <band> · **ICP:** <score>/100 <tier> · **Stage:** <stage>

**Where it stands**
<2-3 sentences: the real state of the relationship and deal>

**Buying committee**
- <name> — <role>, <engaged / not yet> <champion/blocker if known>

**Open risks**
- <objection or competitor> — <why it matters, hard/soft>

**Next moves**
1. <the one thing to do next, and why — grounded in a fact>
2. <supporting move>
```

## Rules
- If ICP fit, committee, or health is missing, name it as a gap to fill, not a number to fabricate.
- Keep it to what changes the next action — this is a plan, not a data dump.
