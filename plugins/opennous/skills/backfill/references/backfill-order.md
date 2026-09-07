# Backfill order — the staged SOP (why the sequence is the whole game)

A backfill is not "loop over every connected tool." The **order** decides whether you end up with
a clean account graph or a pile of duplicates. This file is the canonical strategy; the `backfill`
skill runs it stage by stage.

## The one law

**Account-*creating* sources run before account-*enriching* sources, and structured-stage sources
run before free-text ones.** Every later stage *matches into* the identity graph the earlier stages
built — it never forks it. Establish a person as early as possible (CRM row → outbound reply →
meeting attendee) so that when a later source mentions them, the engine resolves to the existing
record instead of creating a second one.

Corollaries:
- **Creators before enrichers.** CRM, outbound, and meeting notetakers *create* accounts. Gmail,
  Calendar, and Stripe mostly *enrich* what already exists.
- **Structured stage before free text.** A CRM/Stripe stage is ground truth; an email is a hint.
- **Enrich-only sources NEVER spawn accounts.** Gmail especially — otherwise every newsletter,
  vendor, and personal thread becomes a fake account.
- **Stage & closed/won come only from CRM or Stripe.** With neither, accounts stay stage-unknown —
  that is correct, not a gap.

## Stage 0 — Ask: is there a CRM?

Before anything, ask the user whether a CRM is connected (HubSpot / Attio / Pipedrive / Salesforce).
The answer changes the run: with a CRM you get the account backbone + pipeline stage for free; without
one, Stripe (Stage 5) becomes the way to learn what's actually closed.

## Stage 1 — CRM: the account backbone (creates accounts)

If a CRM is connected, import it first. It is the only source besides Stripe that carries **deal
stage**, and it gives you the canonical account set + contacts + owners in one structured pass.
- Import companies → accounts, contacts → people (keyed by email/domain), deals → `deal.stage`,
  `deal.value`, and the relationship owner.
- Everything downstream matches into these accounts.
- **Populates:** accounts, contacts, pipeline stage, deal value, owner.

## Stage 2 — Outbound tools: replies become contacts (creates accounts)

Instantly / HeyReach / Smartlead / Lemlist / EmailBison. These are creators because a **reply is a
real person** and the tool logs it. Run before meetings so that a person who later shows up in a
meeting is *matched*, not duplicated.
- Import contacted leads and, above all, **logged responses** — each reply → create/match the
  contact, record the interaction, and record `discovery` (how the relationship began: which campaign
  / channel).
- **Populates:** new contacts from replies, first-touch interactions, discovery source.

## Stage 3 — Meetings: notetaker + calendar (notetaker creates, calendar corroborates)

Two connectors, one block:
- **Notetaker** (Fireflies / Granola / Fathom) is the creator. For each transcript, run the full
  per-item procedure (see the `sync` skill): resolve external attendees by email, record identity
  attributes, record the interaction, extract claims/intel, extract insights, stash raw → git.
- **Calendar** (Google Calendar / Calendly / Cal.com) corroborates: it supplies the real meeting
  **date/time**, confirms attendee emails, and fills meetings the notetaker missed. Treat calendar as
  enrichment of the notetaker's meetings — create a contact from a calendar-only external attendee
  only when it's clearly a real external meeting.
- **Match first, create second.** An attendee already in the graph (from CRM / outbound) must resolve
  to that record.
- **Populates:** claims, intel, insights, accurate meeting timeline.

## Stage 4 — Gmail: enrich-only, NEVER create

Walk email history, but **only attach to accounts/people already in the graph** from Stages 1–3.
- For threads with a KNOWN contact: record interactions, discovery, and any durable intel.
- Do **not** create an account from an arbitrary sender. This is the entire reason CRM + outbound +
  meetings run first — they define "known," and Gmail only fills known.
- **Populates:** interaction history + intel on existing accounts. Never new accounts.

## Stage 5 — Stripe: the revenue truth (optional, last)

If there's no CRM (or to confirm one), ask the user to connect Stripe as the final, optional step.
Stripe tells you who actually **paid / closed-won** and the real amount, so accounts that had no
stage get one.
- **Populates:** closed/won stage, real deal amounts — especially when Stage 1 was skipped.

## Final pass — enrich, then score (this fills "not ICP'd, not enriched")

Ingestion order alone never scores accounts. After the stages, run one closing pass:
1. **Enrich firmographics** so accounts become *scoreable* — resolve each company's domain →
   industry / employee_count / etc. (whatever enrichment is available). An unenriched account has no
   features to score.
2. **Score against the ICP** — `score` every materialized account against the ICP model set in
   onboarding. This is what produces ICP fit + reasoning. If no ICP exists yet, say so and skip —
   don't block the report.

Scoring is last because it needs both an ICP model AND enriched features.

## Graceful degradation

Run only the stages whose source is actually connected (onboarding's Phase A discovered them). Skip
an absent stage and say so in the report ("no CRM connected — stages came from meetings + Gmail;
deal stages unknown without a CRM or Stripe"). Never invent a connector.
