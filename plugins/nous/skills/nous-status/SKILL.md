---
name: nous-status
description: >
  Use when the user asks "is Nous set up / connected", "who am I / what's my
  role", "what's in my Nous / my graph", or right after installing the plugin to
  confirm it works. Reports who the key acts as, their scope and GTM role(s), and
  what's in the graph. A quick health check — not for day-to-day account work
  (use get_context, plan-account, or review-pipeline for that).
---

# Nous status

Confirm the plugin is wired, report who the agent acts as, and show what's in the graph.

## Steps
1. Call `mcp__nous__whoami`. This is the identity check:
   - If it **errors / 401**, the `NOUS_API_KEY` isn't set or is invalid — tell the user to set it
     (get a key at app.opennous.cloud → Settings → API Keys) and stop.
   - If it **returns**, note who the key acts as: the **workspace**, the **person**, their **scope**
     (admin = whole workspace · member = their own book + shared graph), and their **GTM role(s)**
     (e.g. AE, SDR, founder — a person can hold several).
2. Probe the graph with a single `mcp__nous__query` — e.g.
   `query(scope:{ kind:"state", property:"stage" }, return:"entities")` — to see whether it's
   **populated** (accounts across stages) or **empty**. Because the key is scoped, a member sees
   their own book here, an admin sees the whole workspace.
3. Report back, concisely:
   - **Acting as** — name · scope · role(s) · workspace.
   - **Graph** — roughly how many accounts / the stage spread, or "empty".
   - **Next step** — run onboarding/backfill if empty, or "ask me about an account" if populated.

## Rules
- This is a read-only health check — do not record or modify anything.
- Lead with the identity line from `whoami` — it's the thing that catches a mis-scoped or
  wrong-workspace key. Scope and role(s) tell the user (and later skills) whose view this is.
- If the graph looks empty, point the user to onboarding/backfill as the next step.
- Keep it to a few lines; this is a status check, not a report.
