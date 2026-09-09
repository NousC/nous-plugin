---
name: automate
description: >
  Turns on the built-in automations that work your calls while you sleep — after every call, draft
  the follow-up and coach it; every Sunday, a coaching report. Installs the GitHub Actions that run
  the plugin headlessly on your own tokens, in your connected repo. Use when the user says "turn on
  automations", "automate my follow-ups", "set up the after-call automation", "make it run after
  every call", or asks how the automations in Settings actually run. One-time setup; idempotent.
---

# Automate — install the built-in automations

The built-in automations (Settings → Automations) run in **your** connected repo's GitHub Actions,
on **your** Anthropic + Nous keys. This skill installs the machinery once: the workflow files, a
self-contained copy of the routines, the config, and the two repo secrets. After this, a call ending
fires the after-call run with no one touching anything.

**You are running inside the user's repo.** Write the files straight into it and commit. Idempotent —
re-running reconciles (overwrites the installed files, never duplicates).

## Phase 0 · Orient
Call `whoami`. You need to be signed in. Then confirm this repo is the user's **connected Nous repo**:
check `git remote -v` and that a repo is connected in the app (Settings → Repo). **If no repo is
connected, stop** and tell the user to connect one at https://app.opennous.cloud/settings?section=repo
first — the automations have nowhere to run without it.

## Phase 1 · Install the machinery (write + commit)
Write these into the repo, then commit them:
1. **The workflows** — copy `templates/nous-after-call.yml` → `.github/workflows/nous-after-call.yml`,
   and `templates/nous-weekly.yml` → `.github/workflows/nous-weekly.yml`.
2. **The runnable routines** — the CI runs headless with no plugin installed, so it reads the routine
   from the repo. Write a self-contained copy of the **after-call** routine to
   `.nous/skills/after-call.md` (distil this plugin's `after-call` skill into a direct instruction:
   read the scored call from `get_account` on the payload's `entity_id`, write the coaching review to
   `coaching/<date>-<account>.md` and the channel-aware follow-up draft to
   `drafts/<account>/<date>-follow-up.md`, never send). Same for `weekly-coaching` →
   `.nous/skills/weekly-coaching.md` when the user wants the weekly report.
3. **The config** — copy `templates/automation.json` → `.nous/automation.json` (set `after_call.enabled`
   / `weekly_coaching.enabled` to what the user asked for).
Commit with a clear message (e.g. `chore: install Nous automations`).

## Phase 2 · Set the two secrets
The headless run needs two repo **Action secrets**: `ANTHROPIC_API_KEY` (the user's — this is the
"your tokens" part that runs `claude`) and `NOUS_API_KEY` (workspace-scoped, for the plugin's MCP).
- **If the GitHub CLI is available and authed** (`gh auth status` ok): set them with
  `gh secret set ANTHROPIC_API_KEY` and `gh secret set NOUS_API_KEY` (read the Nous key from
  `~/.nous/config.json`; ask the user for the Anthropic key, never echo it). Confirm with
  `gh secret list`.
- **Otherwise guide once:** print the two secret names and the link
  `https://github.com/<owner>/<repo>/settings/secrets/actions`, and have the user paste them (a
  30-second one-time step). Do not put either key in the chat or a file.

## Phase 3 · Arm it
The dispatch that triggers the after-call run is gated by the app toggle. Tell the user to switch
**After every call** on at https://app.opennous.cloud/settings?section=sequences (Settings →
Automations) — or confirm it's already on. That flag is what makes Nous fire the workflow when a
call ends.

## Phase 4 · Confirm
Tell the user, in a couple of lines: what's now installed, what will happen on the next call (the
after-call run drafts the follow-up + writes the coaching review into `drafts/` and `coaching/`),
that they can run it by hand from the repo's **Actions** tab (`workflow_dispatch`), and how to turn
it off (toggle in Settings → Automations, or delete the workflow files). Offer to open a test run.

## Rules
- **Their repo, their tokens.** Everything runs in the user's own GitHub Actions on their keys. Nous
  never runs the generative work.
- **Secrets are pointers, never content.** Set them via `gh` or the GitHub UI; never write a key into
  a file or the chat.
- **Idempotent.** Re-running overwrites the installed files and reconciles the config; it never
  duplicates workflows or double-commits unchanged files.
- **Draft, never send.** The installed routines only write files; sending stays the user's call.
