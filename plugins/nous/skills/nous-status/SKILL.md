---
name: nous-status
description: >
  Use when the user asks "is Nous set up / connected", "what's in my Nous / my
  graph", "what can Nous do", or right after installing the plugin to confirm it
  works. Confirms the plugin is wired and responding, and reports what's in the
  graph and the single most useful next step. A quick health check — not for
  day-to-day account work (use get_context, plan-account, or review-pipeline).
---

# Nous status

Confirm the plugin is wired to a workspace and responding, then report what's in the graph.

## Steps
1. Probe the graph with a single `mcp__nous__query` — e.g.
   `query(scope:{ kind:"state", property:"stage" }, return:"entities")`.
   - If it **errors / 401**, the `NOUS_API_KEY` isn't set or is invalid. Tell the user to set it
     (get a key at app.opennous.cloud → Settings → API Keys) and stop.
   - If it **returns**, the key resolves and the workspace is reachable — the plugin is wired.
2. Read the result to judge whether the graph is **populated** (accounts across stages, recent
   activity) or **empty**. A second `query(scope:{ property:"interaction", since_days:30 }, return:"entities")`
   gives a quick sense of recent movement.
3. Report back, concisely:
   - **Connected** — the key resolves and Nous is responding.
   - **What's in the graph** — roughly how many accounts, the stage spread, whether anything's moved lately (or "empty").
   - **The single most useful next step** — run onboarding/backfill if the graph is empty, or "ask me about an account" if it's populated.

## Rules
- This is a read-only health check — do not record or modify anything.
- If the graph looks empty (no accounts), point the user to onboarding/backfill as the next step.
- Keep it to a few lines; this is a status check, not a report.

## What this skill can't confirm (yet)
The six-primitive surface has no identity tool, so this check **cannot** report *who* the key acts
as, its **scope (admin vs member)**, or which **integrations are connected**. It confirms the plugin
works and what's in the graph — not the actor behind the key. Verifying identity/scope needs a
`whoami` primitive (a pending surface decision); until then, check the acting identity + connected
sources in the app (Settings → API Keys / Sources).
