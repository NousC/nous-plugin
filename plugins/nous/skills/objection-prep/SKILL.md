---
name: objection-prep
description: Preps the objections you'll hear on a specific deal and the answer to each — drawn from the objections already recorded on that account, ordered by how hard and how unaddressed they are, with the counter grounded in your own positioning and proof. Use when the user asks what objections they'll face, to handle pushback on a deal, what the account is worried about, or to prep for a tough conversation. For the cross-account objection battlecard, that's `role-report`.
---

# Objection prep

The objections on THIS deal and how to answer them — pulled from what's actually been recorded on the account, not a generic list. One deal; the real concerns.

## Tools
- `mcp__nous__get_account` — the recorded objection Intel (category `objection`, with `status` open/resolved and `hardness` hard/soft), plus competitors in play and their stance.
- `mcp__nous__get_context` — pass `intent: "account_review"` to pull the deal state and our relevant positioning when the account record is large.

## Workflow
1. **Resolve the account** and `get_account`; collect the objection Intel, competitors, and deal stage.
2. **Filter to what matters:** open objections first (resolved ones are context), hard before soft.
3. **Answer each** with the specific feature, proof, or positioning that addresses it, and one line on how to phrase it — grounded in our real story, never invented. If we have no strong answer on file, say so plainly (that's a real finding to fix, not a gap to paper over).
4. **Order by risk** — hardest, most deal-blocking, still-open first.

## Output
```
# Objection prep — <Account>
**Stage:** <stage> · **Competitors:** <in play, if any>

1. "<what they'll say>"  — <hard/soft · open>
   → **Counter:** <the answer, grounded in our positioning/proof>
   → **Proof:** <the single most specific thing to point to>

2. "<next objection>" …

**No strong answer yet:** <any objection we can't currently counter>
```

## Rules
- **Only real objections** — pull from what's recorded on the account; don't manufacture pushback that hasn't been raised.
- **Flag the unanswerable.** Where our positioning genuinely doesn't counter it, say so — an honest gap beats a hollow answer.
- **Tie every counter to our actual story** (feature, proof, positioning), not a generic rebuttal.
- Order by hardness + open status, so the user preps the deal-breaker first.
