---
name: team-report
description: The founder/admin's TEAM-WIDE revenue report — rolls up every seat's accounts and activity into one unified pipeline across all reps, with per-rep coverage, ownership, and cross-team patterns. Use when a founder or admin asks for the team pipeline, "how's the whole team doing", a team revenue review, or per-rep coverage. Different from the per-seat onboarding Revenue Report (one person's backfill) and from focus/review-pipeline (today's work) — this is the whole team's retrospective + state.
---

# Team Revenue Report

The workspace-wide view: every seat's accounts and conversations merged into one pipeline. Where the
per-seat **Revenue Report** (in `onboard`) is one person's slice, this is the **whole team**, and it's
for the founder/admin. Runnable anytime — it gets richer as more seats backfill, never gated on
"everyone's in."

## Who can run it
Admin/founder scope only — check `whoami` (scope = admin). A member gets their own book, not the team;
if a member asks, hand them their per-seat view instead. The team roll-up needs the whole-workspace
read an admin key has.

## Tools
- `whoami` — confirm admin scope + see the roster (members + their GTM roles).
- `query` with **`scope.reporting: "company"`** — the team-wide roll-up (pipeline, stages, activity
  across all seats). This is the team lens; a per-rep view is `scope.reporting: "role"` /
  `scope.attention: "mine"` for a named seat.
- `get_account` — drill into a specific account (who owns it, cross-rep touches).
- `score` — ICP fit when ranking.

## What to produce
Same retrospective spirit as the Revenue Report, but team-wide. Write to
`reports/team-report-<YYYY-MM-DD>.md`.

1. **Executive summary** — the state of the *team's* revenue in a few sentences.
2. **The team pipeline** — the unified funnel/stage shape across all reps (only if a CRM/Stripe feeds
   stages; else say so and lead with activity + ICP). Total open, at-risk, won/lost over the window.
3. **Per-rep coverage & book** — for each seat: how many accounts they own, their activity, what
   they've backfilled, and **who hasn't onboarded** (their accounts show as CRM shells without
   conversations). This is the founder's "is my team actually in here, and following up?" view.
4. **Leaks across the team** — deals gone quiet, unaddressed objections, and **dropped commitments by
   rep** (from `commitment_made`). Found revenue, attributed to who owns it.
5. **Heating up** — buying signals + intent across the whole book, by owner.
6. **Market & positioning intelligence** — the team-wide `record_insight` roll-up (product ·
   positioning · market · buyer) — patterns no single rep sees alone.
7. **Coverage + trust** — which team-shared sources (CRM/Stripe/outbound) are connected, which seats'
   personal sources are in vs missing, and the provenance note.

## Rules
- **Team-shared vs personal** (`../onboard/references/integrations-personal-vs-team.md`): CRM/Stripe/
  outbound give the whole team's accounts + owners at once; each rep's conversations arrive per seat.
  An account can exist (from the CRM) with its owning rep's conversations not yet backfilled — say so,
  never imply the account is fully covered.
- **Attribute everything to an owner.** Every leak, every deal, every commitment names the rep who owns
  it (`relationship_owner`) — the point of a team report is accountability + coverage.
- **Never fabricate a funnel or $** without a CRM/Stripe stage source — make its absence a finding.
- **Every claim sourced.** Same trust posture as the per-seat report.
- Runnable anytime; state the coverage ("N of M seats backfilled") so the founder reads it correctly.
