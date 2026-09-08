---
name: reach-out
description: Drafts a personalized first-touch or follow-up grounded in the account record — the real hook (a recent signal, an open thread, the ICP-relevant angle), in the user's own writing voice, with one clear ask. Use when the user says "draft an email to", "write a follow-up", "reach out to", "draft outreach for", or "write a note to" someone. Drafts only — sending stays in the user's own email tool; logs the touch after it's sent.
---

# Reach out

Turn what Nous knows into a message that could only have been written for this person, in a voice
the recipient would recognise as the user's. Two things carry it: the grounding, which is a real fact
from the record, and the voice, which is theirs.

## Tools
- `get_context` — pass `intent: "draft_email"` (or `"follow_up"`) with the recipient. Returns the angle: recent signals, open objections to preempt, where the relationship stands, ICP fit.
- `get_account` — pull a specific thread to reference (a prior conversation, a stated goal) when `get_context` doesn't surface it.
- `record` — AFTER the user confirms they sent it, log the touch as `kind:'event', property:'interaction.email_sent'` (or `linkedin_message`) so the timeline stays true. Don't record a draft that wasn't sent.

## Workflow
1. **Frame the touch:** first-touch or follow-up? What's the purpose (book a call, revive a stalled thread, answer a question)? Ask if unclear.
2. **Read the actual thread history. This is not optional and the graph is not enough.**
   A follow-up that ignores what was already said, or how many times, is broken however well it is
   written. The graph's timeline is a summary, not the correspondence. Before drafting:
   - Search every connected channel for real messages with this person: email (`from:`/`to:` their
     address), and any messaging connector present (LinkedIn, Slack, WhatsApp).
   - Establish four things: **what you last said, when, whether they replied, and how many
     unanswered messages are already outstanding.**
   - **If you cannot retrieve the history, say so plainly and do not invent continuity.** Never
     write "as I mentioned", "following up on my last note", or reference a promise you cannot see.
     Tell the user which channel you could not read and ask them to paste the last exchange.
   - Four unanswered messages and a live reply are completely different situations. Four unanswered
     means acknowledge the silence and lower the ask, never repeat the same question again.

3. **Get the hook.** `get_context` (`draft_email` intent). Find the single most specific, current thing to open on — a signal, a mutual thread, the pain their ICP profile implies. If nothing is on file, say so and keep the draft honest rather than inventing familiarity.
4. **Load the user's voice. Do this BEFORE writing a word.** Walk the ladder in
   `../../references/language.md` §3 and stop at the first hit: their `CLAUDE.md` / `AGENTS.md` and
   any writing file it points at → a voice file in the repo (`brand.md`, `ai-slop.md`, `voice.md`,
   a style guide) → five to ten messages they actually **sent**, if an email connector is present →
   nothing, so write plain and short. Never skip this step. A draft written without looking is the
   single most common failure of this skill.
5. **Say what you found**, in one line above the draft: *"Using your `ai-slop.md` floor (no em
   dashes, no 'X not Y')."* The user can correct a standard they didn't intend to inherit.
6. **Draft the body in THEIR voice**, specific to the record, with ONE clear ask. Short. Preempt an
   open objection only if it's natural.
7. **Offer to log it.** When the user says it's sent, `record` the interaction.

## The body is not a report
Everything you print is normally in Nous's register (see CLAUDE.md, "How you write"). **The message
body is the exception, and it releases that register entirely.** An email does not carry a number in
every line, does not lead with the finding, and does have a greeting and a sign-off. Carrying the
report register into a draft is exactly what makes a message read like a briefing.

What still holds inside the body: the floor in `../../references/language.md` §2 (no em dash as a
connector, no "X, not Y", no fragments for emphasis, no rule of three, no consultant verbs), and
never inventing a fact. Everything else about the body is the user's voice, not yours.

## Output
The shell is Nous's register. The body is theirs. The line between them is the whole point.

```
**To:** <recipient> · **Why now:** <the fact this is grounded in>     ← register
**Voice:** <what you found, or "no voice file found — writing plain">  ← register

Subject: <subject>                                                     ← their voice
                                                                       ← their voice
<the draft body — tight, specific, one ask>                            ← their voice

---
Grounded in: <the record fact used>. Want me to log this once you send it?   ← register
```

## Rules
- **Find the voice before you draft.** Step 4 is not optional. Writing first and adjusting later
  produces a model-voiced message with the user's words sprinkled on top.
- **Their standard outranks your instincts, inside the body only.** If their file bans em dashes,
  the body has none. It never reaches the shell, the why-now line, or any report.
- **No voice found → plain and short.** Do not invent a personality, perform casualness, or reach
  for warmth the record doesn't support. Never imitate a voice from a single sample.
- **Ground the personalization in a real fact.** If the record is thin, a short honest note beats
  fake familiarity. Courtesy openings are a matter of the user's own register, not a ban.
- **One call to action, one question, and never an "or".** Do not offer a choice ("already building
  this, or still figuring it out?"). Ask the single thing you want answered, or make the single
  offer. A two-answer question is cold-outbound craft and does not belong in a warm follow-up.
- **Don't send.** Drafting is yours; sending is the user's own tool. Only `record` the touch after they confirm it went out.
