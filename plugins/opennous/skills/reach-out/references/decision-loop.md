# Closing the loop on your own advice

Every recommendation you make is a data point, but only if it is written down *before* anyone
acts on it. A draft that lives in chat scrollback and then gets sent teaches the engine
nothing — it looks identical to an email that arrived out of nowhere.

Three writes, all through `record`. None of them costs the user a round trip.

## 1. Write down what you proposed — at draft time, not after

```
record(focus: "dana@acme.io", observations: [{
  kind: "state",
  property: "decision.proposed",
  value: {
    decision_id: "cc-20260908-a41f",     // any unique string; reuse it for the next two writes
    proposal: "follow up naming the security review they raised on the 3rd",
    recipient: "dana@acme.io",           // how the confirmation finds its way back
    action_type: "email_send",           // email_send | linkedin_message | campaign_add
    category: "outreach",
    surface: "plugin",
    agent: "claude-code"
  }
}])
```

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

Do not try to detect whether the send succeeded, and do not ask the user to confirm it twice.
If they send from a connected tool the loop closes on its own, from the tool's own response
and from the provider's webhook. Your job is only to say what you recommended and what they
said back.
