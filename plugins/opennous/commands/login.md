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

3. **Then ACT immediately — this is the point of the flow. Do NOT print a "next steps" list or ask
   permission.** Your very next action is a skill, not a suggestion:
   - **Graph is EMPTY (fresh workspace):** say ONE line — *"You're connected as <name> · <workspace>
     · <role>. Your graph is empty — building it from your history now."* — then **immediately invoke
     the `onboard` skill.** Do not stop, do not offer options, do not wait for a "yes". Onboarding is
     the whole reason they connected; just start it.
   - **Graph already has accounts (returning user):** say *"You're connected — here's what needs you
     today,"* then **immediately invoke the `focus` skill.**

   Presenting a menu ("Natural next steps: /opennous:onboard …") instead of running the skill is a
   failure of this command. Run it.

## Fallbacks
- If a tool returns `invalid_api_key`, the key didn't save — re-run step 1.
- If `npx` isn't available or the browser flow fails: have the user copy a key from
  **https://app.opennous.cloud/connect/api-keys**, set it via `/plugin` → **opennous → configure**,
  run `/reload-plugins`, then continue from step 2.
- The key is **workspace-scoped** — it acts as one workspace + identity, which `whoami` reports.
