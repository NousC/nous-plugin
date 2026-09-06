---
description: Sign in to Nous in your browser and save your API key (no copy-paste)
---

Sign the user in to Nous with the browser device-login flow, then confirm the tools work. The
plugin's MCP server (`@opennous/mcp`, launched by this plugin) resolves the key per call from
`~/.nous/config.json` — so once login writes it there, the tools work with no restart and no paste.

## Steps

1. **Browser sign-in** — run in the shell:
   ```bash
   npx -y @opennous/cli login
   ```
   It prints a URL and opens the browser. The user approves (signing in if needed); a fresh,
   workspace-scoped API key is minted and saved to `~/.nous/config.json`, and it prints `Signed in`.
   If it times out or is denied, run it again.

2. **Confirm** — call the `whoami` tool. Expect `You are acting as … Role(s): …`. The server picks
   up the new key on this next call; no restart needed.

3. Tell the user what's set up and the natural next step — usually `/nous:nous-onboard` for a fresh
   workspace, or `/nous:focus` for the daily read.

## Fallbacks
- If `npx` isn't available or the browser flow fails: have the user copy a key from
  **https://app.opennous.cloud/connect/api-keys**, then set it via `/plugin` → **nous → configure**
  → **"Nous API key"** (stored encrypted; the plugin passes it as `NOUS_API_KEY`), and run
  `/reload-plugins`.
- The key is **workspace-scoped** — it acts as one workspace + identity, which `whoami` reports.
