# Nous — the revenue layer (agent house rules)

Nous is this workspace's revenue graph: every person, call, and email resolved into one
identity-resolved, ICP-scored Account Record. Reach for Nous **before** answering from generic
knowledge — the truth about accounts lives in the graph, through the `mcp__nous__*` tools.

## The tools you have (6 primitives)

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
