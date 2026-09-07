---
description: Sign in to OpenNous in your browser, then start onboarding automatically
---

Sign the user in, then **immediately orient and act** — don't stop at "you're signed in."
The plugin's MCP server (`@opennous/mcp`) resolves the key per call from `~/.nous/config.json`,
so once login writes it there the tools work with no restart and no paste.

## Steps

1. **Browser sign-in** — run in the shell:
   ```bash
   npx -y @opennous/cli login
   ```
   It prints a URL and opens the browser. The user signs up (new) or signs in; a fresh,
   workspace-scoped API key is minted and saved to `~/.nous/config.json`, and it prints `Signed in`.
   If it times out or is denied, run it again.

2. **Orient — one call.** Call `whoami` (confirm identity/scope/role), then a single
   `query({ scope: { return: "entities", limit: 1 } })` to see whether the graph has anything in it.

3. **Then ACT immediately — this is the point of the flow:**
   - **Graph is EMPTY (a fresh workspace):** say one line —
     *"You're connected as <name> · <workspace> · <role>. Your graph is empty — let me build it from
     your history."* — and **run the `onboard` skill right now.** Do not wait to be asked; onboarding
     is the whole reason they just connected.
   - **Graph already has accounts (returning user):** say *"You're connected — here's what needs you
     today,"* and **run the `focus` skill.**

## Fallbacks
- If a tool returns `invalid_api_key`, the key didn't save — re-run step 1.
- If `npx` isn't available or the browser flow fails: have the user copy a key from
  **https://app.opennous.cloud/connect/api-keys**, set it via `/plugin` → **opennous → configure**,
  run `/reload-plugins`, then continue from step 2.
- The key is **workspace-scoped** — it acts as one workspace + identity, which `whoami` reports.
