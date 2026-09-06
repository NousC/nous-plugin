---
name: nous-onboard
description: >
  Onboards a new Nous workspace end-to-end inside the coding agent — detecting the
  revenue tools connected here, filling gaps, backfilling the last 6 months onto the
  graph (on this agent's tokens), and ending with a pipeline report. Use the FIRST time
  a user sets up Nous, or when they say "onboard me", "set up my workspace", "build my
  accounts", "import my history", or ask how to get started. Not for day-to-day work
  once set up — use nous-sync, account-plan, or pipeline-review instead.
---

# Nous onboarding (strict SOP)

Stand up the user's Nous graph entirely inside this agent — no Nous UI, nothing connected to our
platform. You extract on THIS agent's tokens; Nous resolves identities and scores. Follow the
phases **in order**. Do not advance until a phase's **Exit** is met. Announce each phase to the
user in one short line.

## Phase A · Discover
List the MCP servers / connectors available in this session and classify each into a Nous category:
- **Meetings** → Fireflies, Granola, Fathom, Otter
- **Email** → Gmail, Outlook
- **Calendar** → Google Calendar, Outlook Calendar
- **CRM** → HubSpot, Attio, Pipedrive, Salesforce
- **Messaging** → Slack
- **Social** → LinkedIn

**Exit:** you have a coverage map (which categories are present, which are missing).

## Phase B · Gap-check & suggest
**Meetings and Email are REQUIRED** — that's where accounts come from. If a required category is
missing, stop and tell the user exactly what to connect ("Connect a meeting-notes tool — Fireflies
or Granola — so I can build your account history"), then wait. CRM / Calendar / Slack / Social are
optional: note them, don't block. Never invent a connector that isn't present.

**Exit:** required categories are covered, or the user explicitly says "skip and continue".

## Phase C · Backfill (last 6 months)
Run the **`nous-backfill`** skill over the connected sources (default window: 6 months). It pulls
history through the connectors, extracts on the user's tokens, and files via `record` /
`record_insight`. Idempotent and resumable — accounts materialize as it goes. Report progress as it
runs.

**Exit:** the 6-month window is fully processed for every required source (backfill reports done).

## Phase D · Materialize & report
Once backfill is drained:
1. `score` the newly materialized accounts against the ICP. If no ICP is set up yet, say so and
   offer `icp-tune` — but don't block the report.
2. Run **`pipeline-review`** + **`build-dashboard`** (pipeline-board template) to produce the
   payoff: an interactive pipeline report written to `dashboards/`. If those skills aren't available
   yet, produce a concise text summary instead.
3. Tell the user, in a few lines: **"Imported N accounts, M meetings, P emails over 6 months —
   $X in open pipeline"**, and the path to their report.

**Exit:** the report (or summary) is generated and shown.

## Phase E · Handoff
Tell the user their history is in and they can work now — suggest a couple of openers
("try `account-plan` on your top deal", or "ask who's gone quiet"). Then explain the one remaining
step for *ongoing, automatic* updates: **connect ongoing ingestion in the Nous app** (~2 min) —
that's where webhooks and the git raw-data folder get wired, so new meetings/emails flow in without
running anything.

**Exit:** the next step is communicated. Stop.

## Rules
- **No tool connected to our platform in this flow, and no UI** — everything happens here.
- **Backfill runs on the user's tokens** — by design.
- **Idempotent + resumable** — if onboarding is interrupted, re-running continues from the watermark
  and never double-files (see `nous-backfill`).
- **You never merge/resolve identities** — the engine materializes accounts from what you record.
- Keep phase announcements to one line each; save the detail for the closing report.
