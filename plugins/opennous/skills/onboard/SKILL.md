---
name: onboard
description: >
  Onboards a new Nous workspace end-to-end inside the coding agent — detecting the
  revenue tools connected here, filling gaps, backfilling the last 6 months onto the
  graph (on this agent's tokens), and ending with a pipeline report. Use the FIRST time
  a user sets up Nous, or when they say "onboard me", "set up my workspace", "build my
  accounts", "import my history", or ask how to get started. Not for day-to-day work
  once set up — use sync, plan-account, or review-pipeline instead.
---

# Nous onboarding (strict SOP)

Stand up the user's Nous graph entirely inside this agent — no Nous UI, nothing connected to our
platform. You extract on THIS agent's tokens; Nous resolves identities and scores. Follow the
phases **in order**. Do not advance until a phase's **Exit** is met. Announce each phase to the
user in one short line.

## Phase 0 · Orient (fast)
Call `whoami` — confirm you're signed in and see the workspace/role you're acting as. If a tool
returns `invalid_api_key`, stop and tell the user to run `/opennous:login` first, then resume.
Idempotent: if the graph already has accounts (a quick `query` shows activity), tell the user
they're already set up and hand to `/opennous:focus` instead of re-onboarding.

**Exit:** signed in, acting as a known workspace, and the graph is empty (or the user asked to
re-run anyway).

## Phase A · Discover
List the MCP servers / connectors available in this session and classify each into a Nous category:
- **Meetings** → Fireflies, Granola, Fathom, Otter
- **Email** → Gmail, Outlook
- **Calendar** → Google Calendar, Outlook Calendar
- **CRM** → HubSpot, Attio, Pipedrive, Salesforce
- **Messaging** → Slack
- **Social** → LinkedIn

**Exit:** you have a coverage map (which categories are present, which are missing).

## Phase B · Gap-check & recommend the backbone — STOP for a decision
Never silently jump to a thin backfill. This is a decision point: check the floor, then actively
recommend the account-creating sources that are missing, and **wait for the user to choose**.

**Sources split into team-shared vs personal — recommend by the caller's role** (`whoami` → scope; see
`references/integrations-personal-vs-team.md`). Team-shared (CRM · Stripe · outbound) is the
account/deal/revenue backbone an **admin/founder** connects once for the whole team; personal
(notetaker · email · calendar) is each member's own conversation layer.
- **Admin/founder:** recommend connecting the team-shared backbone (CRM + outbound) — it lights up the
  whole team's accounts + owners — plus their own personal sources.
- **Member:** the team-shared sources are usually already connected by an admin — **don't ask them to
  connect the CRM.** Have them connect their **personal** meetings + email and backfill their slice.

**Required (block if missing):** Meetings + Email — the floor for building accounts. If either is
missing, tell the user exactly what to connect ("Connect a meeting-notes tool — Fireflies or
Granola") and wait. Never invent a connector that isn't present.

**Recommend the account-creating backbone — then STOP.** Meetings + Email alone build a *thin* graph:
people and conversations, but **no deal stages, no pipeline $, and no contacts from outbound replies.**
Before backfilling, recommend the missing CREATORS. **CRM and outbound are BOTH strongly recommended,
with EQUAL weight — neither is secondary, neither is "first."** Recommend whichever the user is
missing, each as its own first-class option, with the concrete tradeoff:
- **No CRM? (recommended)** Connect one (HubSpot / Attio / Pipedrive / Salesforce). The backbone —
  canonical accounts, the **pipeline stages**, deal values, and owners. Only a CRM or Stripe carries
  stage, so without one there is no funnel and no pipeline $.
- **No outbound tool? (recommended, equally)** If they run outbound, connect it (Instantly / HeyReach
  / Smartlead / Lemlist / EmailBison). It creates contacts from **logged replies** plus the discovery
  source that meetings and email never capture — the top of the funnel.
- **No Stripe? (optional)** The fallback for closed/won when there's no CRM.

**The recommended path is to connect BOTH a CRM and an outbound tool** — that's the strongest setup,
not one or the other. Make **"connect both" the first, recommended option**; then each one
individually (both still recommended); then Stripe as the optional fallback; then "proceed with
what's connected." Do NOT mark only CRM as recommended — CRM and outbound are recommended together.
Then **wait** — do NOT proceed until they answer. Frame it like: *"For the full picture I'd connect
BOTH a CRM and an outbound tool before I backfill (that's the strongest setup): the CRM gives accounts
+ stages + pipeline $, the outbound tool gives reply-sourced contacts + discovery. Connect both, just
one, or proceed with meetings + email?"* An example menu shape:
1. **Connect both a CRM and an outbound tool (recommended)** — the full backbone
2. Connect a CRM (HubSpot / Attio / Pipedrive / Salesforce)
3. Connect an outbound tool (Instantly / HeyReach / Smartlead / Lemlist / EmailBison)
4. Connect Stripe (optional — closed/won only)
5. Proceed with meetings + email (no stages, no pipeline $)

**Exit:** the user has either connected a recommended source (then re-run Phase A to pick it up) or
**explicitly** chosen to proceed with what's connected, knowing the tradeoff.

## Phase C · Backfill (last 6 months) — raw lands in git as it goes
**Audit raw storage first (idempotent):** ensure `raw/` and a `.nous/raw.json` marker
(`{ workspace_id, convention, repo }`) exist in this repo — create them once if missing, leave them
if present. Raw is filed per the locked convention — **one folder per account**,
`raw/<account-slug>/<date>-<source>-<externalId>.md`. Full spec: `../sync/references/raw-storage.md`.

Then run the **`backfill`** skill over the connected sources (default window: 6 months). It pulls
history through the connectors, extracts on the user's tokens, **writes each item's raw into its
account folder**, and files structure via `record` / `record_insight` carrying a `source_ref` git
pointer to that raw. Idempotent and resumable — accounts and their raw folders materialize as it
goes; a re-run overwrites the same paths, never duplicates. Report progress as it runs.

**Exit:** the 6-month window is fully processed for every required source (backfill reports done),
with each item's raw written under `raw/<account-slug>/`.

## Phase D · Set up the ICP, then materialize & report
Once backfill is drained:
1. **Set up the ICP — this is 100% part of onboarding, done here by you.** Check `whoami` →
   `setup.has_icp`. If there's no ICP yet, author it WITH the user and write it via the **`set_icp`**
   tool — draft from the backfilled accounts + the company insights + 2–3 sharp questions, or reuse
   their canonical `context/nous/icp/icp.md` verbatim if they have one (reconcile, never invent a
   second). Full authoring guide + required shape: `references/icp-authoring.md`. If an ICP already
   exists, read/confirm before replacing it. This is not optional and not deferred to the app.
2. `score` the newly materialized accounts against the now-set ICP.
3. **Write the Revenue Report — the payoff of onboarding.** This is the first time the user sees their
   whole revenue motion unified, so it's a **retrospective revelation, not a to-do list**: the state of
   their revenue over the window, what stood out, deals won/lost/stalled, the leads and follow-ups
   nobody was tracking, how their pipeline behaves, the market/positioning intelligence from the calls,
   and honest team-coverage (this is one seat's slice — drive them to invite the team). Gather the
   material with `query`/`get_account`/`review-pipeline`, then write it to
   **`reports/revenue-report-<YYYY-MM-DD>.md`** (in the working dir, NOT under `raw/`). Follow the full
   spec and the eight sections in **`references/revenue-report.md`**. Every claim sourced; honest about
   what's missing; pipeline $ only if a CRM/Stripe fed it (else make "no tracked outcomes" a finding).
4. Show the user the **executive summary** inline (a few sentences that land the "oh") + the report
   path. Don't paste the whole report — the summary plus "full Revenue Report at <path>".

**Exit:** the ICP is set (`set_icp` succeeded), accounts are scored, and the Revenue Report is written
to `reports/` and its executive summary shown.

## Phase E · Handoff + one optional last step
Tell the user their history is in and they can work now. Offer a couple of openers **as plain things
they can just say — never tell them to type a skill command.** You know the skills; you route the
intent yourself. Ground the openers in their real data, e.g. *"want me to plan the WindSeeker AI
account?"* or *"ask 'who's gone quiet?' and Taimoor will come up."* — not "run `/opennous:plan-account`".

Then, as the FINAL and OPTIONAL step: **"Connect a repo in OpenNous to keep raw flowing ongoingly."**
Everything above already works without it — the backfill's raw is already in this repo. This wires the
*ongoing, server-side* push (new meetings/emails auto-filed to the same repo, so the raw stays
complete without anyone running anything). **Give the user the direct link and tell them to connect it
there:** https://app.opennous.cloud/settings?section=repo . Audit first: if a repo is already
connected, say so and skip it. If not, offer to pre-fill the repo you detected from `git remote` —
they paste a fine-grained GitHub token on that page once (a secret they create; it's stored
encrypted, so it belongs in the app, not this chat).

**Exit:** the optional connect-repo step is offered (and done, or knowingly deferred). Stop.

## Rules
- **No tool connected to our platform in this flow, and no UI** — everything happens here.
- **Backfill runs on the user's tokens** — by design.
- **Idempotent + resumable** — if onboarding is interrupted, re-running continues from the watermark
  and never double-files (see `backfill`).
- **You never merge/resolve identities** — the engine materializes accounts from what you record.
- Keep phase announcements to one line each; save the detail for the closing report.
