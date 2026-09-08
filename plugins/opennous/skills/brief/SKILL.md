---
name: brief
description: Briefs you before a call or meeting — who you're meeting, where the relationship and deal stand, the open threads, and the one goal + next move for this specific conversation. Use when the user says "brief me", "prep me for my meeting with", "what do I need to know before I talk to", "who am I meeting with", or names an upcoming call. This is the fast pre-room read; for a full strategic plan use `plan-account`.
---

# Brief

Get the user walk-in ready in one read: the account's state and the single thing this conversation is for. Fast and specific — not the whole plan, just what changes how they show up.

## Tools
- `get_context` — pass `intent: "meeting_prep"` with the person/company. The headline tool: it returns the task-shaped read (who they are, deal state, what's open, the suggested next move).
- `get_account` — pull the full record only if the brief needs a thread `get_context` didn't surface (a specific past objection, a commitment made last call).

## Workflow
1. **Identify the meeting.** Use the person/company the user named; if they point at "my next meeting", ask who it's with (or read it from their own calendar tool) — never guess.
2. **Get the context.** `get_context` with `meeting_prep` intent. This is the spine of the brief.
3. **Pull the open threads:** the last meaningful touch, any commitment either side made, open objections, competitors in play, and whether a next step is already booked.
4. **Name the goal for THIS meeting** — advance the stage, unblock an objection, multi-thread to a missing buyer — and the one move that gets there.

## Output
```
# Brief — <Person>, <Company>
**Deal:** <stage> · **Health:** <band> · **ICP:** <score> <tier>

**Who you're meeting**
<title, role in the deal, champion/blocker if known>

**Where it stands**
<2-3 lines: last touch, what's open, what they're waiting on>

**Walk in for**
<the one goal of this conversation>
→ <the move that gets there, grounded in a fact>

**Have ready:** <the objection to preempt / the proof to bring>
```

## Close the loop

The things you tell them to raise on the call are decisions. Record them before the call; the verdict is whether they actually raised it.

Write the recommendation down before it is acted on (`decision.proposed` with a `decision_id`,
a `rationale`, and the `evidence_ids` you reasoned from), record what the user decided, and
stamp the same `decision_id` on the interaction when it fires. Full mechanic:
`../../references/decision-loop.md`.

Do not narrate this. It is bookkeeping, not part of the output.

## Rules
- **Show the evidence.** Each fact you cite carries its speaker and verbatim quote from
  `get_context`. In a pre-room read the quote IS the value: the user walks in knowing the actual
  words, not a summary of them. Never invent one to fill a gap.
- **Lead with what changes the conversation**, not a data dump — this is a pre-room read, keep it to what matters in the next hour.
- **One goal.** If the record suggests several, name the most important and hold the rest as backup.
