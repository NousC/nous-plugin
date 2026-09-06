# Nous — the revenue layer for your coding agent

Nous turns your calls and emails into a live, identity-resolved, ICP-scored **account graph** —
extracted on your *own* agent's tokens and filed to Nous. Your CRM lives where you already work:
Claude Code, Codex, or any MCP-capable agent.

- **You extract, we resolve.** Your agent reads a transcript, extracts the facts, signals, Intel,
  and insights, and files them through two write doors. Nous does the identity resolution, ICP
  scoring, and memory — the part that compounds.
- **Raw stays yours.** Full transcripts and briefs live in your git repo; Nous holds only the
  structured graph plus a pointer back to the raw.

## Install (Claude Code)

```bash
/plugin marketplace add NousC/nous-plugin
/plugin install nous@nous
```

Then sign in (browser, no copy-paste), and reload:

```bash
/nous:nous-login
/reload-plugins
```

`/nous:nous-login` mints a workspace-scoped key and helps you set it in the plugin config (stored
encrypted). Manual alternative: paste a key from app.opennous.cloud/connect/api-keys into
`/plugin` → nous → configure. Full steps in [INSTALL.md](./INSTALL.md).

Confirm it works: run `/nous:whoami` → *"You are acting as … Role(s): …"*.

## What's inside

| Piece | What |
|---|---|
| `plugins/nous/.claude-plugin/plugin.json` | wires the hosted Nous MCP (`mcp.opennous.cloud/mcp?surface=plugin`) via the `api_key` plugin config, and declares that config |
| `plugins/nous/commands/nous-login.md` | `/nous:nous-login` — browser sign-in + key setup |
| `plugins/nous/CLAUDE.md` | the router + house rules ("reach for Nous first"; raw → git; you observe, Nous derives) |
| `plugins/nous/skills/` | the 19 skills — setup, daily (`focus`), accounts/deals, pipeline, and reporting |

### The 7 primitives

**Identity:** `whoami` (workspace · scope admin/member · GTM role[s])
**Read:** `get_context` · `get_account` · `query` · `score`
**Write:** `record` (contact facts, interactions, signals, Intel) · `record_insight` (learnings about your own product/positioning/market/buyer)

## Repo layout

```
.claude-plugin/marketplace.json     # this repo is a plugin marketplace
plugins/nous/
  .claude-plugin/plugin.json        # the plugin manifest
  .mcp.json                         # MCP server (hosted Nous)
  CLAUDE.md                         # router / house rules
  skills/<name>/SKILL.md            # one folder per skill
```

## Status

Live. The MCP surface is exactly **7 primitives** (the six data tools + `whoami`), and the skills are in:
setup & ingestion (`nous-onboard`, `nous-status`, `nous-sync`, `nous-backfill`) and revenue plays
(`plan-account`, `triage-leads`, `review-pipeline`, `whats-changed`, `win-loss`). Eight more are
locked and building next — `build-record`, `brief`, `reach-out`, `map-committee`, `objection-prep`,
`forecast`, `ask-nous`, `market-read`.

There is one MCP server and only one. Full plan, tool signatures, and the locked skill catalog:
[revenue-plugin overview](https://github.com/NousC/opennous/blob/main/docs/revenue-plugin/README.md).

---

## License

MIT — the skills, prompts, and config here are transparent and forkable. The Nous **engine**
(identity resolution, ICP scoring, memory) is the hosted, proprietary service they call.
