# Closing the loop on your own advice

Shared by every skill that recommends something. A recommendation is a data point only if it
is written down *before* anyone acts on it — advice that lives in chat scrollback and then
gets acted on teaches the engine nothing, because it looks identical to a thing that happened
out of nowhere.

**What counts as a decision:** something the user can say yes or no to and then *act* on. A
summary is not a decision. A list of five plays is five decisions. If no plausible action
follows, do not write one — a table full of rows that never resolve drags every rate toward
meaning nothing.

Three writes, all through `record`. None of them costs the user a round trip.

## 1. Write down what you proposed — at draft time, not after

```
record(focus: "dana@acme.io", observations: [{
  kind: "state",
  property: "decision.proposed",
  value: {
    decision_id: "cc-20260908-a41f",     // any unique string; reuse it for the next two writes
    proposal: "follow up naming the security review they raised on the 3rd",
    rationale: "the security review is the real blocker at this stage, not price",
    evidence_ids: ["<the claim/note id you actually reasoned from>"],
    recipient: "dana@acme.io",           // how the confirmation finds its way back
    action_type: "email_send",           // email_send | linkedin_message | campaign_add
    category: "outreach",
    surface: "plugin",
    agent: "claude-code"
  }
}])
```

**`proposal` is what you are doing. `rationale` is why you think it works.** Keep them apart.
The second is a claim that can turn out to be wrong, and separating it is what lets Nous grade
*reasoning* rather than only actions.

**`evidence_ids` are the facts you actually reasoned from** — the claim, note, or observation
ids behind the call. You are already citing them to the user (a fact is never shown unsourced);
pass the same references here. This is what makes "which kinds of evidence are worth acting on"
answerable at all. Without it we learn whether follow-ups work, but never whether *security
objections are worth reacting to* — and the second question is the more useful one.

Be honest about it: list what genuinely drove the recommendation, not everything you read. A
padded basis is worse than a thin one, because it teaches the engine that irrelevant evidence
predicts outcomes.

`recipient` matters more than it looks. When the send actually happens, a hook fires on the
send tool and reports it to Nous — but that hook has never heard of your `decision_id`. It
knows who the message went to. The recipient is what lets the two halves find each other.

## 2. Record what the human decided

Same `decision_id`, and the verdict is the property name so you cannot report one thing and
mean another:

```
property: "decision.accepted"   // they sent it as drafted
property: "decision.edited"     // they rewrote it first
property: "decision.rejected"   // value: { decision_id, verdict_note: "too pushy for this stage" }
```

If the user shows you what they actually sent, pass `drafted_body` and `sent_body` in the
value — Nous measures how much of your draft survived instead of taking your word for it.
That number is the single most useful thing this loop collects: it is how the engine learns
which of its recommendations experienced operators consistently correct.

A rejection is not a failure to log. It is the label. Record it.

## 3. Stamp the decision on the action

When you log the send, carry the id:

```
record(focus: "dana@acme.io", observations: [{
  kind: "event",
  property: "interaction.email_sent",
  value: { description: "follow-up on the security review" },
  decision_id: "cc-20260908-a41f"
}])
```

Now whatever comes back — a reply, a booked meeting, a bounce, or silence — is attributed to
the recommendation that caused it. Without the stamp the send is an orphan: real, recorded,
and ungradeable.

## What you do not have to do

Do not try to detect whether the action succeeded, and do not ask the user to confirm it twice.
If they act through a connected tool the loop closes on its own, from the tool's own response
and from the provider's webhook. Your job is only to say what you recommended, why, and what
they said back.

And do not narrate any of this to the user. The three writes are bookkeeping; they belong in
the tool calls, not in the conversation.
