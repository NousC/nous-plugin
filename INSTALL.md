# Install Nous

Nous is the revenue layer for your coding agent — an identity-resolved, ICP-scored account graph
you build and query from Claude Code, Codex, or Cursor. Follow these steps to connect it, then
(optionally) onboard your workspace.

## 1 — Add the Nous MCP server

Register the hosted Nous tools with this agent:

```bash
claude mcp add nous -- npx -y @opennous/mcp
```

For Codex or Cursor, add an MCP server with command `npx -y @opennous/mcp` in your agent's config.

## 2 — Authenticate

Get your API key at **https://app.opennous.cloud/connect/api-keys**, then either:

```bash
export NOUS_API_KEY=pk_your_key        # picked up on the next call
# or
npx @opennous/cli auth login --key pk_your_key
```

## 3 — Verify

Confirm the connection by calling the Nous tools — e.g. ask this agent to run `mcp__nous__whoami`.
You should see the workspace the key acts as. If it errors, the key isn't set or is invalid.

## 4 — Onboard (recommended)

Say **"onboard my Nous workspace."** Nous detects the revenue tools you already have connected
(Fireflies, Gmail, Calendar, HubSpot/Attio, LinkedIn), backfills the last 6 months onto the graph
**on your own tokens**, and builds your pipeline report. Nothing is connected in our UI — extraction
runs here, in your agent.

## What you get

**Tools (6):** `get_context` · `get_account` · `query` · `score` · `record` · `record_insight`

**Skills:** `nous-onboard` · `nous-sync` · `nous-backfill` · `account-plan` · `pipeline-review` ·
`lead-triage` · `win-loss-review` · `whats-changed`

**How it works:** you (this agent) extract facts and insights from calls/emails on your tokens; Nous
does the identity resolution, ICP scoring, and memory — the part that compounds. Raw data stays in
your git; only the structured graph enters Nous.

Need help? → https://docs.opennous.cloud/mcp/introduction
