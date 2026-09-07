# Claim extraction (facts about the contact)

Ported from Nous's server-side extractor. Apply this to a transcript/email **yourself** — you are
the model. Produce the JSON, then write it via `record`. Extract about the EXTERNAL people (the
prospect/customer side), never about us.

## STEP 0 — identity attributes FIRST (before any Intel)

Before the Intel below, record the **structured attributes** for each external attendee. This is what
gives a person a name and a company on the record (without it they show as a bare email), AND it is
what lets identity resolution attach repeat meetings to the same record. Record these as
`kind:'state'` observations via `record` (NOT as intel notes):

**On the person** (`about:'person'`):
- `first_name`, `last_name` — the person's real name, from the transcript / attendee list / email
  signature. If you genuinely can't find it, skip it — never guess.
- `job_title`, `seniority` — only if actually stated (you'll often have these from the same lines that
  produced the `authority` intel).

**On their company** (`about:'company'`, focus = the person — the engine routes it to the company and
creates/links the company entity):
- `domain` — **derive from the work email** (`jack.cane@revenanas.com` → `revenanas.com`). This is the
  reliable key that identifies + creates the company. Record it whenever there's a work email.
- `company` — the company **name**, ONLY if you actually find it in the transcript/metadata
  (e.g. "Alibaba Cloud"). **Never use the person's name as the company.** If the name isn't stated,
  leave it empty — the `domain` alone lets the engine create the company; the name can be enriched later.

Do NOT record identity attributes for our own side (the operator/teammates — see the internal-domain
guard in the sync skill). Skip a free-email domain (gmail.com, outlook.com, …) as a company `domain`.

Everything below (the Intel) is recorded IN ADDITION to these attributes, not instead of them.

## What to record

For each external attendee, record facts drawn from what THEY reveal about themselves, their
company, needs, constraints, opinions, or plans. ALSO record facts about the RELATIONSHIP between
the attendee and us: how they found us or why they reached out (a video, post, referral, event),
anything they offered us or intros they can make, and any objection/critique they raised about our
product or how we differ from alternatives.

Do NOT turn OUR OWN questions or statements into facts about them (if we asked "what's behind your
product?", that is not a fact that they're interested in our product). But an offer, an intro, a
critique, or a reason-for-reaching-out that they genuinely voice IS a fact and must be recorded.

## The three bars — a fact qualifies ONLY if it passes ALL THREE

1. **DURABLE** — still true weeks or months from now. A meeting time, availability, or reschedule is
   NOT durable. EXCEPTION: a buying signal (asked for the agreement/contract, accepted/agreed to
   pricing, gave a verbal commit, asked to start/onboard) is decision-critical the instant it
   happens — record it as `buying_signal` even though it's momentary.
2. **DECISION-RELEVANT** — it would change how someone sells to or works with them: their budget,
   authority, pain, goals, stack, or buying timeline — OR how the relationship should be worked (how
   they found us, what they offered, the objection they raised).
3. **SPECIFIC** — carries the concrete detail or the WHY, not a vague label. "Evaluating Clay vs
   Apollo because Apollo's data went stale", not "looking at tools".

## NEVER record (noise, or it lives elsewhere)
- Meeting logistics: scheduling, availability, reschedules, "has a call on X", invites.
- Generic sentiment, small talk, greetings, pleasantries.
- Anything true today but meaningless next week.

## Categories — tag each fact with exactly one `category`

- `status_quo` — how they work today (tools, vendor, process, stack). e.g. "Acme runs outbound on Apollo and Instantly today."
- `goal` — an initiative, priority, or outcome they're chasing. e.g. "Wants to consolidate enrichment vendors before end of Q3."
- `pain` — a stated problem/frustration with the concrete reason. e.g. "Clay's list-building is bottlenecked by manual work."
- `objection` — a concern/pushback/challenge to us (price, security, timing, switching cost, competitor loyalty, "how are you different"). e.g. "Questioned how we differ from Fireflies that already syncs notes to the CRM."
- `authority` — buying role and decision power (champion, blocker, economic buyer, end user). e.g. "Owns the GTM-tooling budget; spend over $50k needs VP sign-off."
- `budget` — budget size, procurement, or a commercial constraint. e.g. "Has roughly $30k a year earmarked for GTM data tooling."
- `timeline` — a buying/project timeline tied to a business reason (never a meeting time). e.g. "Evaluating vendors this quarter, driven by a budget review."
- `buying_signal` — explicit deal-progression: asked for the agreement/contract, accepted pricing, verbal commit, asked to start/onboard. e.g. "Asked us to send the agreement and confirmed they'll prepay $2.5k to start."
- `preference` — how to work with them (channel, cadence, style, format). e.g. "Strongly prefers tools with a native API over no-code builders."
- `competitor` — a competing tool they use or evaluated, why, how loyal. e.g. "Currently on Clay and frustrated with its pricing at scale."
- `discovery` — how the relationship began (content/channel/post/referral/event) and why they reached out; always from THEIR side. e.g. "Priya found us through our YouTube video on open-source GTM and reached out on LinkedIn."
- `relationship` — a durable connection (reports-to, referred-by, mutual connection, community, or an offer to introduce). e.g. "Offered to introduce us to seed- and YC-stage founders in his network."
- `general` — durable, decision-relevant context fitting none of the above. e.g. "Plans to hire 2 SDRs once the team passes $50k MRR."

## Field rules
- Each fact is ONE self-contained sentence that **names the person explicitly — no pronouns**
  (never "you/your", never speak as them with "I/my"). "us/our/we" for our own side is fine.
- `about` = "person" (about the attendee) or "company" (about their company; inherited by the whole account).
- `label` = a 2–4 word Title Case tag naming the SUBSTANCE (not the person's name). For a competitor
  use the vendor name ("Pearl Lemon"); for an objection name it ("Outbound intent doubts"); for a
  pain/goal name it ("Stale Apollo data").
- Context tags (only for the category that carries them):
  - `competitor`: `entity` = the vendor name; `stance` ∈ evaluating | incumbent | past_failure | mentioned.
  - `objection`: `status` ∈ open | resolved | addressed; `hardness` ∈ hard | soft.

## Discipline
- Extract EVERY fact that clears all three bars — no target number. A thin message yields none or
  one; a rich meeting yields many. NEVER pad, NEVER split one fact into several, NEVER restate one
  fact in different words. Quality over quantity. If nothing clears the bar, return `[]`.
- Hard ceiling ~12 facts for a meeting (a safety limit, not a goal).

## Output — ONLY valid JSON
```json
[{"content":"...","label":"2-4 word tag","category":"<key>","about":"person|company","entity":"<competitor only>","stance":"<competitor only>","status":"<objection only>","hardness":"<objection only>"}]
```
If nothing meaningful: `[]`
