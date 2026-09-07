---
name: reach-out
description: Drafts a personalized first-touch or follow-up grounded in the account record — the real hook (a recent signal, an open thread, the ICP-relevant angle), in the user's voice, with one clear ask. Use when the user says "draft an email to", "write a follow-up", "reach out to", "draft outreach for", or "write a note to" someone. Drafts only — sending stays in the user's own email tool; logs the touch after it's sent.
---

# Reach out

Turn what Nous knows into a message that could only have been written for this person. The value is the grounding — a real fact from the record, not a template.

## Tools
- `mcp__nous__get_context` — pass `intent: "outreach"` with the recipient. Returns the angle: recent signals, open objections to preempt, where the relationship stands, ICP fit.
- `mcp__nous__get_account` — pull a specific thread to reference (a prior conversation, a stated goal) when `get_context` doesn't surface it.
- `mcp__nous__record` — AFTER the user confirms they sent it, log the touch as `kind:'event', property:'interaction.email_sent'` (or `linkedin_message`) so the timeline stays true. Don't record a draft that wasn't sent.

## Workflow
1. **Frame the touch:** first-touch or follow-up? What's the purpose (book a call, revive a stalled thread, answer a question)? Ask if unclear.
2. **Get the hook.** `get_context` (outreach intent). Find the single most specific, current thing to open on — a signal, a mutual thread, the pain their ICP profile implies. If nothing is on file, say so and keep the draft honest rather than inventing familiarity.
3. **Draft in the user's voice**, specific to the record, with ONE clear ask. Short. Preempt an open objection only if it's natural.
4. **Offer to log it.** When the user says it's sent, `record` the interaction.

## Output
```
**To:** <recipient> · **Why now:** <the fact this is grounded in>

Subject: <subject>

<the draft body — tight, specific, one ask>

---
Grounded in: <the record fact used>. Want me to log this once you send it?
```

## Rules
- **Ground the personalization in a real fact** — no generic "hope you're well"; if the record is thin, a short honest note beats fake familiarity.
- **One ask** per message.
- **Don't send.** Drafting is yours; sending is the user's own tool. Only `record` the touch after they confirm it went out.
- Match the user's voice and length; don't pad to sound formal.
