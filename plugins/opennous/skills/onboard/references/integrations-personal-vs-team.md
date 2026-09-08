# Integrations — personal vs team-shared

Every source falls into one of two kinds, and the difference drives onboarding, permissions, and what
the reports can honestly claim.

## Team-shared (the account / deal / revenue layer)
Configured **once by an admin/founder**, and it applies to the whole workspace — because these are
already the team's shared systems of record.
- **CRM** — HubSpot / Attio / Pipedrive / Salesforce → accounts, contacts, **deal stages**, deal
  values, **owners**.
- **Stripe** → closed-won / real revenue.
- **Outbound** — Instantly / HeyReach / Smartlead / Lemlist / EmailBison → reply-sourced contacts +
  discovery, across the team's campaigns.
- **Slack** (messaging) → workspace-wide.

Connecting one of these lights up **the whole team's accounts and who owns them immediately** — even
accounts owned by a rep who hasn't onboarded yet. What's missing for those accounts is only the
*conversation* layer.

## Personal (the conversation layer)
Connected **by each member, on their own tokens** — one person's calls and inbox are theirs.
- **Meeting notetaker** — Fireflies / Granola / Fathom.
- **Email / calendar** — Gmail / Outlook / Google Calendar.
- **LinkedIn** — each rep's own account.

These fill in **seat by seat**: a rep's backfill lands their conversations onto the shared accounts,
and identity resolution merges them into the same records.

## What this means

**Onboarding is role-aware:**
- An **admin/founder** connects the **team-shared** backbone (CRM + outbound + Stripe) once, then their
  own personal sources. (This is what Phase B recommends to them.)
- A **member** usually finds the team-shared sources **already connected** — they just connect their
  **personal** meetings + email and backfill their own slice. Don't ask a member to connect the CRM.

**The report can be precise about coverage** (Revenue Report §7/§8):
- Team-shared connected? → you have the whole team's accounts + owners + stages.
- Personal connected? → you have *your* conversations. Other reps' conversations are missing until they
  onboard — and you can name exactly which accounts are affected via CRM ownership: *"Account X is
  owned by [Jane] (from the CRM); her conversations aren't backfilled yet — invite her."*

**In the app (Integrations page):** two sections — **Team integrations** (admin-configured, shared;
members see them as already-connected) and **My integrations** (each member connects their own). Only
admins/founders can configure the team-shared ones.
