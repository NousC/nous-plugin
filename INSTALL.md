# Install Nous

Nous is the revenue layer for your coding agent — an identity-resolved, ICP-scored account graph
you build and query from Claude Code. Install the plugin, sign in, then (optionally) onboard your
workspace.

## 1 — Install the plugin

```bash
/plugin marketplace add NousC/nous-plugin
/plugin install nous@nous
```

## 2 — Authenticate

The easiest way — browser sign-in, no hunting for a key:

```bash
/nous:nous-login
```

It signs you in and mints a workspace-scoped key saved to `~/.nous/config.json`; the plugin's MCP
server (`@opennous/mcp`, trimmed to the 7 primitives via `NOUS_SURFACE=plugin`) reads it live — no
paste, no restart.

**Manual alternative:** copy a key from **https://app.opennous.cloud/connect/api-keys**, run
`/plugin` → **nous → configure**, and paste it into **"Nous API key"** (stored encrypted).

Then apply it:

```bash
/reload-plugins
```

If the tools still return `401 invalid_api_key` right after setting the key, fully quit and relaunch
Claude Code — a live MCP connection can cache the old header.

## 3 — Verify

Run `/nous:whoami` (or ask the agent to call the `whoami` tool). You should see
**"You are acting as … Role(s): …"** with the workspace the key acts as. If it errors, the key isn't
set or is invalid — re-run step 2.

## 4 — Onboard (recommended)

Run **`/nous:nous-onboard`** (or say *"onboard my Nous workspace"*). Nous detects the revenue tools
you already have connected (Fireflies, Gmail, Calendar, HubSpot/Attio, LinkedIn), backfills recent
history onto the graph **on your own tokens**, and builds your pipeline report. Extraction runs here,
in your agent — nothing is connected in our UI.

## What you get

**7 primitives:** `whoami` · `get_context` · `get_account` · `query` · `score` · `record` ·
`record_insight`

**Skills (19):**
- Setup — `nous-onboard` · `nous-status` · `nous-sync` · `nous-backfill`
- Daily — `focus` (your morning worklist) · `whats-changed` · `ask-nous`
- Accounts & deals — `build-record` · `plan-account` · `brief` · `reach-out` · `map-committee` ·
  `objection-prep`
- Pipeline & reporting — `review-pipeline` · `triage-leads` · `forecast` · `win-loss` ·
  `market-read` (founder) · `role-report` (per seat)

**How it works:** you (this agent) extract facts and insights from calls/emails on your own tokens;
Nous does the identity resolution, ICP scoring, and memory — the part that compounds. Raw data stays
in your git; only the structured graph enters Nous.

Need help? → https://docs.opennous.cloud/mcp/introduction
