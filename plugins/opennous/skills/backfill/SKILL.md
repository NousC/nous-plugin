---
name: backfill
description: >
  Imports months of history into the Nous graph in bulk — walking every past meeting and
  email through the connected tools, extracting them, and filing them. Use when setting up
  a workspace, or when the user says "backfill my history", "import the last 6 months",
  "pull all my past calls". Runs on THIS agent's tokens. It is resumable and idempotent
  — safe to stop and re-run. For a single just-finished call, use sync instead.
---

# Nous backfill — import history in bulk

Populate the graph from history. Each item goes through the **same per-item procedure as
`sync`** — you extract, Nous resolves and scores. What this skill adds is the *loop*: a
window, batching, a watermark, resumability, and progress. Read
`../sync/references/claim-extraction.md` and `../sync/references/insight-extraction.md`
for the extraction rules — they are identical here.

## Setup
1. **Window** — default to the **last 6 months** unless the user asks for more/less.
2. **Sources** — the connected meeting + email tools (Fireflies/Granola/Gmail/…). If a required
   source isn't connected, say which one to add and continue with what's there.
3. **Watermark** — per (source), find where a previous run stopped (a `~/.nous/backfill.json`
   state file, or ask Nous). Resume from there; on a first run, start at the window's oldest edge.

## The loop (oldest → newest, in batches)
For each source, repeatedly:
1. **Pull the next batch** (e.g. 20–50 items) from the connector, from the watermark forward,
   within the window.
2. **For each item, run the `sync` per-item procedure** — including its step-2 attendee
   resolution, so every fact lands on the RIGHT person (resolved by email), never piled on one host
   entity:
   - resolve the item's external attendees → their identifiers (email → LinkedIn URL → domain)
   - stash the raw → `raw/<account-slug>/<YYYY-MM-DD>-<source>-<id>.md` (one folder per account; see `../sync/references/raw-storage.md`), compute `content_hash`
   - `record` the interaction (`observed_at` = the item's real date, `external_id` = `<id>:interaction`, `source_ref`)
   - extract facts → `record` each Intel fact **against its person**, `external_id` = `<id>:intel:<index>`
   - extract insights → `record_insight`
   - write a one-line brief → `briefs/<account>/<date>-<id>.md` (git only)
3. **Retry, then quarantine.** If an item fails on a transient error (connection closed, timeout,
   5xx), retry it up to 3 times; if it still fails, add its id to a `quarantine` list and continue —
   never let one bad item stop the run.
4. **Report progress** every ~50 items: `"420/1,200 meetings imported…"`.
5. **Checkpoint the watermark** after every batch, so a stop/crash never loses ground.

At the end, report the quarantine list so the user (or a re-run) can retry just those items.

## Cost & size discipline
- This runs on the user's tokens and a deep history can be thousands of items. Before a large run,
  give a rough size ("~1,200 meetings over 6 months") so they know the scale.
- Cap the work per invocation if it's very large; checkpoint and tell the user they can **re-run to
  continue** — because every write is idempotent, resuming never double-files anything.

## On completion
When the window is drained:
1. **Trigger the reporting distillation.** A backfill records the raw material for both reporting
   surfaces — objection/pain Intel (via `record`) that becomes `role-report`'s deal-blockers, and
   product/positioning/market/buyer insights (via `record_insight`) that become `market-read`'s
   themes — but the distilled layers are built server-side, not at write time. Company **themes**
   re-synthesize lazily the next time `market-read` reads them (no action). The **objection
   handlers** behind `role-report` are built by a weekly job, so after a bulk import ask Nous to run
   the intelligence pass now (the server's `runIntelligenceOnce`) rather than wait a week. If no
   trigger is exposed yet, tell the user role-report will populate on the next weekly run.
2. **Hand off to the report:** run `review-pipeline` to show what came in ("N accounts, M meetings,
   $X pipeline"). (When called from `onboard`, that skill owns the closing report — just return
   the counts.)

Reporting needs volume by design: the objection matcher needs ≥5 objections and theme synthesis
drops tiny 1–3-item themes — a thin backfill may not populate reporting until enough recurs.

## Rules
- **Attribution over volume.** Each item resolves its own attendees and files each fact on the right
  person (by email) — never dump a meeting's facts onto its host entity. One right beats ten wrong.
- **Retry, then quarantine — never stall.** Transient failures get up to 3 retries, then quarantine
  and continue. Idempotency makes a later re-run of the quarantined ids safe.
- **Idempotent + resumable, always.** Every observation carries a stable `external_id` derived from
  the source item id, so re-running skips everything already filed. NEVER restart from zero — read
  the watermark, and let `external_id` dedup catch overlaps.
- **`observed_at` is the item's REAL date**, never now — the whole point of backfill is a correct
  historical timeline.
- **Raw → git, structure → Nous.** Never send a transcript or a full brief to Nous.
- **Never invent.** A thin item may yield only its interaction and no facts — that's correct.
- **You never merge/resolve identities.** Observe against a precise `focus`; the engine resolves.
