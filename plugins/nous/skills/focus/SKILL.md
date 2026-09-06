---
name: focus
description: The morning driver — what YOU need to focus on today, scoped to the accounts you own. Pulls what's waiting on you (replies, buying signals), what's time-bound (today's meetings, commitments due), what's slipping, and who's gone quiet, then ranks it into a short do-this-now list. Use when the user asks what to focus on today, what's on their plate, their priorities, "my day", "where should I spend my time", or "what needs me". This is the personal, owned-book read; for the whole portfolio use `review-pipeline`, for the raw delta use `whats-changed`.
---

# Focus

Answer one question: what should *I* do today? Scoped to the caller's own book, ranked by what actually needs them now — not the whole pipeline, not a data dump.

## Tools
- `mcp__nous__whoami` — FIRST. Who the caller is, their `scope`, and GTM role(s). This scopes the whole read to *them* — a member sees their own book, not the company's.
- `mcp__nous__query` — the caller's priorities. Scope to the accounts they own (`scope: { owner: "me" }` once available; a member key is already scoped server-side, so absent that filter you still get the caller's accessible book — say so if it can't be narrowed to owned-only). Pull, in order:
  - **Waiting on you** — replies received and fresh buying signals with no response yet.
  - **Time-bound** — meetings today and commitments/next-steps due.
  - **Slipping** — deals whose signals contradict their stage, or stalled in-stage too long.
  - **Gone quiet** — owned accounts that were live and cooled (use `without`: activity earlier MINUS recent).
- `mcp__nous__get_context` — expand the top item into the actual next move when the user wants to act on it.

## Workflow
1. **Establish the seat.** `whoami` → identity + scope + role(s). Everything below is scoped to this caller's book.
2. **Gather the four buckets** with `query`, scoped to owned accounts. Keep each item's account, what happened, and when.
3. **Rank by urgency, not volume:** waiting-on-you (a reply going cold) outranks a time-bound meeting, which outranks a slipping deal, which outranks a quiet account. Cap the list — today is 3–6 real actions, not everything.
4. **Give each item its one move**, grounded in the record. Offer to open the top one (`get_context`) or hand off (`brief` for a meeting, `reach-out` for a waiting reply).

## Output
```
# Today — <n> things  (<Name>, <role>)

**Waiting on you**
- <account/person> — <reply/signal> (<when>) → <the move>

**On your calendar / due today**
- <meeting or commitment> → <prep: run `brief`>

**Slipping**
- <account> — <stage vs signal contradiction> → <the move>

**Gone quiet**
- <account> — last touch <when>, was <stage> → <re-engage or let go>
```

## Rules
- **Scoped to the caller's book.** Use `whoami`; a member sees their owned accounts, never the whole company. If the owned-only filter isn't available yet, say the list spans the accessible book, not just owned.
- **Rank by what needs a response now**, not by chronology or count — a cooling reply beats a routine sync.
- **Cap it.** Today is a handful of real actions; push the rest to `review-pipeline`.
- **Ground every line and its move** in the record; absolute times for anything scheduled, relative for past activity.
