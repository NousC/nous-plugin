---
name: nous-status
description: >
  Use when the user asks "is Nous set up / connected", "what's in my Nous / my
  graph", "what can Nous do", or right after installing the plugin to confirm it
  works. Reports who the API key acts as, what's connected, and what to do next.
  A quick health check — not for day-to-day account work (use get_context,
  account-plan, or pipeline-review for that).
---

# Nous status

Confirm the plugin is wired to the workspace and report its state.

## Steps
1. Call `mcp__nous__whoami` to confirm the API key resolves and see which workspace and identity
   the agent is acting as. If it errors, the `NOUS_API_KEY` isn't set or is invalid — tell the user
   to set it (get a key at app.opennous.cloud → Settings → API Keys) and stop.
2. Call `mcp__nous__get_workspace_status` to read what's connected and the ranked next steps.
3. Report back, concisely:
   - who the key acts as + the workspace name
   - what's connected (integrations) and what's missing
   - the single most useful next step (usually: run onboarding / backfill if the graph is empty,
     or "ask me about an account" if it's populated)

## Rules
- This is a read-only health check — do not record or modify anything.
- If the graph looks empty (no accounts), point the user to onboarding/backfill as the next step.
- Keep it to a few lines; this is a status check, not a report.
