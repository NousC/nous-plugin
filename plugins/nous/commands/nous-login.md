---
description: Sign in to Nous in your browser and set your API key for the plugin
---

Get the user authenticated so the Nous MCP tools work. The plugin reads its key from the
plugin's `api_key` user config (see `plugin.json` → `userConfig`), injected into the MCP server's
`Authorization` header as `Bearer ${user_config.api_key}`.

## Steps

1. **Mint a key via the browser** — run in the shell:
   ```bash
   npx -y @opennous/cli login
   ```
   It prints a URL and opens the browser. The user signs in and approves; a fresh,
   workspace-scoped API key is minted and saved to `~/.nous/config.json`, and the command prints
   `Signed in`. If it times out or is denied, run it again.

2. **Read the minted key** so the user doesn't have to hunt for it — read `~/.nous/config.json`
   and pull the `api_key` (a `pk_…` value). Show ONLY that value to the user.

   If `npx` isn't available or login fails, fall back: tell the user to copy a key from
   **https://app.opennous.cloud/connect/api-keys**.

3. **Set it in the plugin config.** Tell the user to run `/plugin`, open **nous → configure**, and
   paste the key into **"Nous API key"** (it's stored encrypted). This is the one manual paste —
   the plugin can't write its own encrypted config.

4. **Apply it:** run `/reload-plugins`. If the tools still return `401 invalid_api_key`, tell the
   user to fully quit and relaunch Claude Code (an already-live MCP connection may cache the old
   header).

5. **Confirm it works:** call the `whoami` tool. Expect `You are acting as … Role(s): …`. Then tell
   the user what's set up and the natural next step (usually `/nous:nous-onboard` for a fresh
   workspace, or `/nous:focus` for the daily read).

## Notes
- The key is **workspace-scoped** — it acts as one workspace + identity. `whoami` shows which.
- Never print the full key into a shared/committed surface; it's a secret. Showing it once to the
  user so they can paste it into their own config is fine.
