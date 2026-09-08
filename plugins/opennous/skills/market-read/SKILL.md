---
name: market-read
description: The founder's company-wide voice-of-market — what buyers are telling you across every account, distilled into Product, Positioning, Market, and Buyer signals with mention counts and evidence. Use when the founder (or an admin) asks what the market/buyers are saying, what patterns are showing up across calls, what to fix in the product or the pitch, or for a company-level voice-of-customer read. This is the strategic, whole-company surface — for a single seat's insights, use `role-report`.
---

# Market read

The closed loop no CRM has, at the company level: read the distilled buyer signal across every account and turn it into what *we* should change about the product and the story. This is the founder surface — the strategic Product / Positioning / Market / Buyer view, not one rep's feed.

## Who sees this
This is a **founder / admin** surface. Call `whoami` first. If the caller's `scope` is not `admin` (and `founder` is not among their roles), don't run the company read — tell them the company voice-of-market is founder-only and hand off to **`role-report`** for the insights routed to their own seat.

## Tools
- `whoami` — confirm the caller's scope (`admin`/`member`) and workspace. Gate on `admin`.
- `query` — read the distilled company insights: `scope: { reporting: "company" }` returns the external **themes** grouped by category (`product` · `positioning` · `market` · `buyer`), each with its mention count and evidence. Do not rebuild these from raw — the server already distilled them (the same data the app's company Reporting shows).
- `record_insight` — optional: when the read surfaces a genuinely new synthesized meta-theme, file it back (`category`, one sentence in our voice, a representative quote).

## Workflow
1. **Gate.** `whoami`; proceed only for admin/founder scope, else hand off to `role-report`.
2. **Pull the four categories** with `query` (`reporting: "company"`). Keep each theme's mention count and its evidence.
3. **Rank within each category** by mention count, and flag the one loudest signal overall — the theme most worth acting on this week.
4. **Turn signal into a move.** For each top theme, say what it implies we change (product build, positioning line, pricing, target) — grounded in the count and the evidence, never invented.
5. **File** any new synthesized meta-theme with `record_insight` so it persists.

## Output
```
# Company market read
**Loudest signal:** <the one theme to act on, and why>

**Product**
- <theme> — <n> mentions → <what it implies we build/fix>

**Positioning**
- <theme> — <n> mentions → <the line to sharpen>

**Market**
- <theme> — <n> mentions → <the shift it points to>

**Buyer**
- <theme> — <n> mentions → <who's buying / what triggers it>
```

## Rules
- **Quote the buyer.** Insights already carry the verbatim line and who said it. A theme with its
  real quote is a finding; the same theme without one is an assertion.
- **Founder surface only** — gate on `whoami` scope; never expose the company read to a plain member.
- **Read the distilled themes, don't reconstruct.** The counts and evidence come from the server's distillation; reproducing them from raw Intel drifts from what the app shows.
- **Pattern, not anecdote** — rank by mention count; a single mention is an account note, not a company signal.
- **`record_insight` sentences are about *us*,** never a fact about a contact.
