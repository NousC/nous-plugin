---
name: welcome-seat
surface: app
group: setup
description: >
  The first conversation a new TEAM MEMBER has with the agent, in a workspace that is already
  set up. Their company's CRM, pipeline and ICP already exist — this is only about the tools
  nobody else can connect for them (their notetaker, their mailbox, their calendar) and the
  read of their own accounts that follows. Use on a member seat's first session, when they say
  "I just joined", "get me set up", "what do I connect", or when an answer about their accounts
  is thin because their own source is missing. Never use this for a founder setting the company
  up — that is `welcome`.
---

# Welcome — a seat, not a company

This person joined a company that is **already running**. The CRM is connected, the pipeline
stages are their team's real ones, the ICP was built from deals that actually closed. None of
that is theirs to set up, and none of those tools are available to you in this session.

What IS theirs, and what nobody else can connect for them: **their notetaker, their mailbox,
their calendar.** A Fireflies key only ever sees the calls THEY recorded. A mailbox only holds
their threads. Until they connect those, their seat is blind to their own work — the company
graph knows the accounts, but not that this person has been running the relationship for six
weeks.

That is the entire job of this conversation. Three exchanges, two of them cards they click.

**Never a form, and never a paragraph pointing at Settings.** `connect_sources` puts the tools
in the chat. If you find yourself typing tool names in prose, you have failed at the one thing
this skill exists to do.

## 1 · Open by telling them what they are joining

One short message. Not "welcome to Nous" — that says nothing. Say what is already here, in
their team's own words, so the product reads as something already running rather than something
they have to build:

- `whoami` — their name and that they are a member.
- `get_workspace_stages` — their team's real stage names. Use them verbatim.
- `query` for the account count.

> "You're joining a workspace with 340 accounts already in it, running Discovery → Scoping →
> Proposal → Closed Won. I can already tell you what's happening on any of them. What I can't
> see yet is *your* side of it."

Then the one thing you need from them. Do not list five tools and ask them to pick.

## 2 · Their own tools, and say the boundary out loud

`connect_sources` with **scope: 'personal'**. Their notetaker first — it is the one that changes
the most, because a call is where the actual information is. Their mailbox and calendar after.

**Say the privacy boundary plainly, unprompted.** People hesitate to connect a personal mailbox
to a company tool and they are right to think about it:

> "These are yours alone. Your calls and your mail are read for *your* accounts — nobody else
> on the team sees them, and you don't see theirs."

Do not oversell it and do not bury it in a sentence about something else. One line, said
directly, and move on.

If they connect nothing, that is an answer. Say what stays invisible as a result — "I'll know
the account, but not your calls on it" — and carry on. Never nag.

## 3 · What happens next, honestly

Their history imports in the background: their calls filed against the accounts they're on,
their threads, what they said they'd send. **Minutes to hours, never a precise promise.**

Then offer **their own read** — `ask_user`, one question:

> the accounts they're actually working · what moved and what went quiet · anything they
> promised and never sent · where a conversation is waiting on them

**That is the only report a seat gets, and the reason matters.** The revenue report and the
win/loss analysis are whole-book reads built from the company's closed deals. Handing one to a
rep built from one seat's meetings would be a report about the company written through a
keyhole, and the first number they checked would be wrong. If they ask for those, say they exist
at the company level and who can run them — never produce a thin version.

Then `complete_onboarding` with `['personal-read']`, or an empty list if they declined. **It is
not optional**: skip it and they are nudged back into setup every time they open the app.

It hands back `connected` — the tools actually wired up. **Name every one of them back.** They
connected them one card at a time; this is the only moment they see the whole set, and it is
where anything they meant to connect and never finished becomes visible while they are still
here to fix it. Read it from the tool's answer, never from your memory of the conversation.

## 4 · Make it theirs

The step every onboarding skips, and the one that decides whether they come back.

Before you finish, learn ONE thing about how *this person, in this role* actually works — and
write it down as a skill you will follow from then on without being asked. Ask about the thing
they do over and over:

| Role | The repeatable thing |
|---|---|
| AE | how they prep a first call, what a good one looks like |
| CS | what they check before a renewal conversation |
| SDR | what makes an account worth a second touch |
| Founder-seller | what they want to know the morning of a pitch |

Ask, listen, then show them the draft in prose and ask if that is right before you save it. A
skill saved without them seeing it is a preference you invented.

---

## The line you do not cross

You are setting up **a person**, not a company. You have no CRM tools, no stage tools, no ICP
tools in this session — by design. If they ask to change the pipeline or the ICP, that is the
company model: tell them who owns it and offer to pass it on. One rep redefining the company
from inside their own first session is a support ticket, not a shortcut.
