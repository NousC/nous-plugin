---
name: focus
description: The morning command surface — what needs you today, scoped to your own book. Mirrors the Studio homepage: upcoming meetings, accounts to focus on, who to follow up on, and open action items — ranked into a short worklist you can run every morning. Use when the user asks what to focus on today, what needs their attention, who to follow up on, what's on their plate, "my day", "my morning", or their action items. For the whole portfolio use `review-pipeline`; for the raw delta use `whats-changed`.
---

# Focus

The daily driver a member runs every morning: the same command surface as the Studio homepage, in the agent. Upcoming meetings, the accounts that need you, who to follow up on, and your open action items — scoped to *your* book, ranked by what needs you now.

## The engine owns the "what needs you" logic — read it, don't reinvent it
The Studio worklist (which accounts are flagged, when a follow-up is due, what's cooling) is computed server-side by the engine. Read that curated worklist through `query`; do **not** rebuild the follow-up logic from raw activity, or the skill drifts from what the app shows.

## Tools
- `whoami` — FIRST. The caller's identity and `scope`. Everything below is *theirs* — a member gets their own book, scoped `mine`.
- `query` — the curated Studio worklist, scoped to the caller: `scope: { attention: "mine" }` returns the four sections the engine already assembled (upcoming meetings, accounts-to-focus-on / flags, follow-ups + replies due, open action items). If that scope isn't available yet, fall back to assembling from `query` (recent replies/signals, gone-quiet via `without`) and say the list isn't the full engine-curated worklist.
- `get_context` — expand the top item into the concrete next move when the user wants to act.

## Workflow
1. **Establish the seat.** `whoami` → who the caller is; scope everything to them.
2. **Pull the worklist** with `query` (`attention: "mine"`). Keep the four sections intact — they're already curated and ranked by the engine.
3. **Present the morning read**, in this order (most time-sensitive first): meetings today → who to follow up on (a cooling reply is urgent) → accounts to focus on → open action items.
4. **Give each item its one move**, and hand off: `brief` for a meeting, `reach-out` for a reply/follow-up, `plan-account` for a flagged account.

## Output
```
# Today — <Name>

**Upcoming**
- <day> <time> — <meeting> with <person>, <account> → run `brief`

**Follow up on**
- <person>, <account> — <reply to answer / follow-up due> (<when>) → run `reach-out`

**Accounts to focus on**
- <account> — <why the engine flagged it> → <the move>

**Open action items**
- <task> — <due / context>
```

## Rules
- **Attribute inline, don't quote.** A worklist is scanned in seconds. Short attribution only.
- **Read the engine's worklist**, don't recompute it — the follow-up/attention logic is the engine's, so the agent and the Studio homepage always agree.
- **Scoped to the caller.** Use `whoami`; a member sees their own book. If the `mine` worklist scope isn't live yet, say the read spans the accessible book, not owned-only.
- **Rank by what needs them now**, and cap it — today is a handful of actions; the rest is `review-pipeline`.
- **Ground each item and its move** in the engine's worklist, not in raw activity.
