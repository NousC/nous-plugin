# Nous — the revenue layer (agent house rules)

Nous is this workspace's revenue graph: every person, call, and email resolved into one
identity-resolved, ICP-scored Account Record. Reach for Nous **before** answering from generic
knowledge — the truth about accounts lives in the graph, through the Nous tools.

## Getting started (what to do for a new user)
- **Not signed in?** If a tool returns `invalid_api_key`, tell the user to run `/opennous:login`
  (browser sign-in, no paste).
- **Fresh workspace?** Run `/opennous:onboard` — it detects the revenue tools connected here,
  backfills recent history onto the graph on this agent's tokens, and ends with a pipeline report.
  Call `whoami` to confirm who the key acts as and whether it's set up.
- **Day to day:** `/opennous:focus` (the morning worklist), and reach for the tools/skills below on any
  GTM task. The full skill list is under `/opennous:` — say the intent and the right one fires.

## The tools you have (7 primitives)

**Identity**
- `whoami()` — who this key acts AS: the workspace, the person, their **scope** (admin = whole
  workspace · member = their own book + shared graph), and their **GTM role(s)** (AE, SDR, founder
  — a person can hold several). Call it to confirm setup and to scope role-specific work to the
  right person: a member's view is their own book; an admin's is the whole team's.

**Read**
- `get_context(subject, intent)` — engineered, task-scoped context for a person/company. The first
  call before any account work (prep, review, qualifying).
- `get_account(id|email)` — the full record: facts, Intel, timeline, ICP.
- `query(...)` — patterns across many accounts ("who replied this week", "negotiation stage",
  semantic fact search with `facts:true`).
- `score(subject)` — the ICP fit + intent judgment.

**Write** (you observe; Nous derives the facts — you never overwrite)
- `record(focus, observations[])` — everything you learn about a CONTACT:
  - interactions → `kind:'event'` (`interaction.meeting_held`, `interaction.email_reply`, …)
  - facts → `kind:'state'` (`job_title`, `deal.stage`, `deal.value`, …)
  - buying signals → `kind:'state'`, `property:'signal.<class>'`
  - Intel (preference / objection / competitor / …) → `kind:'state'`, `property:'intel'`,
    `value:{category, content, label?}`
- `record_insight(insights[])` — what a call taught us about OUR OWN business
  (product / positioning / market / buyer). Never put contact facts here.

## House rules
- **Reach for Nous first.** Don't answer account questions from memory when a tool holds the truth.
- **Record after you learn.** Whenever a call/email/turn teaches you something durable, `record` it
  (and `record_insight` for learnings about us) so the next session starts ahead.
- **Raw stays in git; Nous holds structure.** Full transcripts and prose briefs live in this repo
  (`raw/`, `briefs/`). Nous stores only structured claims + Intel + insights, plus a `source_ref`
  git pointer. Never send a transcript to Nous.
- **Idempotency.** When importing history, set `observed_at` (the real date) and a distinct
  `external_id` per observation (`"<itemId>:<property>"`) so re-runs never duplicate.
- **You never merge or resolve identities** — the engine does. You just observe against a precise
  `focus` (email / LinkedIn URL / domain / entity id), never a bare name.
