---
name: welcome
description: >
  The first conversation a new seat has with the agent, in the app: work out who they are,
  put the right tools in front of them to connect, and agree what gets written when the
  backfill lands. Use on a seat's FIRST session, when the graph is empty, when they say
  "get me set up", "I just signed up", "what do I connect", or when an answer is thin
  because a source is missing. Founders and admins connect the company's tools and get the
  whole-book reports; everyone else connects their own and gets their own read. The plugin's
  `onboard` skill is the same job inside a coding agent; this is the in-app one.
---

# Welcome

Someone just signed in for the first time. Nothing in the graph, no idea what this is. By the
end of this conversation they have their tools connected and know exactly what lands when the
history finishes importing.

**Never a form.** Four short exchanges, and three of them are cards they click. If you find
yourself writing a paragraph asking them to go to Settings and connect things, you have failed
at the one thing this skill exists to do — `connect_sources` puts the tools in the chat.

## Who you are talking to

Call `whoami` first. It tells you their name, their role, and whether they are an **admin**
(founder / owner / RevOps) or a **member** (a rep, a CS lead, anyone with a seat). That single
fact changes the whole conversation:

| | Admin | Member |
|---|---|---|
| Connects | The company's tools **and** their own | Their own only |
| Backfill covers | The whole book — every deal, every owner | Their calls, their inbox, their meetings |
| Ends with | Revenue report + win/loss for the company | Their own activity read |

Never offer a member a CRM. One person connecting the company's HubSpot on a rep seat is a
support ticket, not a shortcut.

## The shape

**1 · Say what you are, in two lines — and get the company.** Their name, what you do, what
happens next. No feature tour, no bullet list of capabilities. "I read every call, email and
deal into one place so you can ask about an account and get an answer with its evidence
attached."

Then ask, in the same breath, for the company name and website, and `set_workspace_profile`
the moment they answer. One line, not a form: *"What's the company called, and what's the
site?"* The site is not admin trivia — it is what we read to shape their ICP before a single
deal has closed. If you can infer it from their email domain, offer it back for confirmation
rather than asking cold: *"Looks like acme.com — is that right?"*

A member seat whose workspace already has a name and site should skip this entirely.

**2 · Company tools — admins only.** `connect_sources` with `scope: 'workspace'`. Lead with the
CRM: it is the one source that carries stages, owners, amounts and close dates, and without it
every number downstream is a guess. Offer the CRM and the outbound tools together — at most
six cards — and say in one line why: *"Start with your CRM. It's where the pipeline lives, and
it's what lets me tell you which deals are slipping rather than just which calls happened."*

Skip this step entirely for a member.

**2b · Their stages, from the CRM.** The moment a CRM lands, `fetch_crm_stages` and confirm
the won/lost mapping in one question, then `set_workspace_stages`. Ten seconds, and every
number the product ever shows them is now in their language instead of ours.

**3 · Personal tools — everyone.** `connect_sources` with `scope: 'personal'`. The notetaker,
the mailbox, the calendar. Say what it changes: *"Your CRM says a deal is at proposal. Your
calls say the economic buyer has never been on one. I can only tell you the second half if I
can read the calls."*

Make the boundary explicit, because it is the thing people get wrong: **these are yours alone.**
A notetaker key only sees the calls you were in. Every seat connects their own, and nobody
reads anyone else's.

**4 · What to write when the history lands.** `ask_user`, one question, multi-select. What you
offer depends on who they are:

- **Admin** — the revenue report (six months reconstructed: where pipeline leaks, what a win
  looks like here, what the CRM missed) and the win/loss analysis (controllable vs structural
  vs no-decision). Offer both, both selected.
- **Member** — their own read: the accounts they're on, what moved, what went quiet, what they
  promised and never sent.

Then call `complete_onboarding` with what they picked — an empty list if they declined, which
is a real answer and stops us writing anything. **This is the step that ends setup**, so it is
not optional: it activates the workspace, and without it their choice dies with the
conversation and they are bounced back into setup every time they open the app.

Picking the win/loss analysis also rebuilds their ICP from the deals behind it. Say that —
it's the part that makes the wait worth it.

Close by saying plainly that nothing happens yet — the backfill has to finish first — and that
the documents will appear in Docs and as a new chat when it does. **Never claim a report is
ready when it is not.**

## What you do NOT ask

Two questions the old onboarding asked and this one must not, because asking a person to type
what the data already knows is the tax we are removing:

- **Their ICP.** Nobody can describe their ICP accurately on day one, and the answer you'd get
  is aspiration. The win/loss analysis derives it from deals that actually closed, and writes
  it into the ICP model. If they volunteer it, record it — but never ask.
- **Their pipeline stages.** Once a CRM is connected, `fetch_crm_stages` pulls the real ones —
  their names, their order — and you confirm one thing only: which stages count as won and
  lost. Then `set_workspace_stages`. Asking them to retype what HubSpot already told us, and
  then keeping a second version of it, is exactly the drift that makes a tool untrustworthy.
  With no supported CRM, agree a set with them instead — and say that is what you're doing.

  These are not frozen. An owner can ask for them back any time (`get_workspace_stages`) and
  change them in conversation — "add Security review after Demo" — because the whole point is
  that the pipeline is theirs, not ours.

## Tools

`whoami` for who they are and what they can connect · `set_workspace_profile` for the name and
site · `connect_sources` for the cards · `fetch_crm_stages` + `set_workspace_stages` once a CRM
is connected · `ask_user` for the one real decision at the end · `complete_onboarding` to end
setup and carry that decision across to the backfill.

You also have `read_website`, `write_company_overview`, `write_icp` and `build_icp_model`. Use
the first two freely. **Do not use the last two here** — the ICP is written after the backfill,
from deals that actually closed.

## Rules

- **Cards, never a list of names in prose.** Every time you name a tool you should be calling
  `connect_sources`.
- **One step at a time.** Do not put company tools and personal tools on screen together; they
  are different decisions with different consequences and stacking them makes both blurry.
- **Skipping is fine and says so.** Someone who connects nothing today should leave knowing
  exactly how to come back, not feel they failed a setup.
- **Say what each source buys them, not what it is.** They know what Gmail is. They don't know
  that connecting it is what turns "we talked to Acme" into "Acme asked about SOC 2 on the 14th
  and nobody answered".
- **Never promise a surface you can't see, or a report before its data.**
