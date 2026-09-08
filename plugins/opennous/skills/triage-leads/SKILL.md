---
name: triage-leads
description: Scores a set of leads or accounts against the live ICP in ONE batch call and sorts them into priority tiers — each with the layered fit/pain/intent/ability read, what's still unknown, and the engine's suggested play. Use when the user asks to triage, qualify, prioritize, rank, or tier leads or accounts, asks which to work first, or drops in a list of people/companies to sort.
---

# Lead triage

Take a set of leads and return a worked list: who to act on first, why, and what to do — scored
against the user's own ICP, and read through the engine's **layered** judgment, not a single number.

## Tools
- `score` — the ICP + intent judgment. **Score in BATCH, never one call per lead:**
  - **External list the user pasted (not in the graph)** → pass `leads` (up to **1000** attribute
    objects in ONE call, nothing written): each is `{ job_title, company_type, industry,
    employee_count, keywords|headline, location, ref }`. Returns the tier distribution so you can size
    the list fast and decide which to bring in. Give each a `ref` so you can name them back.
  - **A cohort already in the graph** → pass `identifiers` (up to **100** per call — chunk a bigger
    list). Returns, per lead: ICP fit 0-100 + tier, intent + band, AND the **layered read** —
    `layers {fit, pain, intent, ability}`, a `missing` worklist (what's still unknown — unknown is
    never 0), and a suggested `play`.
  - **One cold lead** → `identifier` + `attributes`.
- `query` — gather the in-graph cohort when the user says "my new leads / this stage / recent signals"
  instead of pasting a list.
- `get_account` — drill into a borderline lead for the reason behind its score.

## Workflow
1. **Get the set.** Use the pasted list, or `query` for the cohort they named.
2. **Score in one batch.** External list → one `leads` call. In-graph cohort → `identifiers` in
   chunks of 100. **Never loop a single `score` per lead.**
3. **Read the LAYERS, not just fit.** Rank on the engine's `priority` (fit + pain + intent + ability
   together), not fit alone. For each lead carry the four layers, what's `missing`, and the `play`.
4. **Tier + next action from the play:**
   - **Tier 1** — high priority (strong fit + real pain/intent) → the play is usually "work now"; give
     the one concrete next action grounded in the score reason or a live signal.
   - **Tier 2** — strong fit but cold, or a `missing` layer (e.g. intent unknown) → the play is
     "enrich/nurture"; name the missing layer to resolve.
   - **Tier 3 / skip** — weak fit or disqualified → deprioritize, say why.
5. For an external `leads` pass, report the distribution and recommend which slice to bring into the
   graph for the full per-lead layered read.

## Output
```
# Lead triage — <n> leads
**Tier 1 (work now)**
- <name>, <company> — priority <p> · fit <f>/pain <p>/intent <i>/ability <a> — <reason> → <next action>

**Tier 2 (enrich / nurture)**
- <name>, <company> — fit <f>, missing: <layer> — <play>

**Tier 3 / skip**
- <name>, <company> — <why deprioritized>
```
(For a raw external `leads` pass: `Scored N/M inline — tiers: tier 1 x · tier 2 y · tier 3 z. Bring the tier-1s in for the full read.`)

## Close the loop

A tier assignment is a recommendation about where the user spends their time, so it is a decision. Its evidence is the ICP signals that drove the tier.

Write the recommendation down before it is acted on (`decision.proposed` with a `decision_id`,
a `rationale`, and the `evidence_ids` you reasoned from), record what the user decided, and
stamp the same `decision_id` on the interaction when it fires. Full mechanic:
`../../references/decision-loop.md`.

Do not narrate this. It is bookkeeping, not part of the output.

## Rules
- **Attribute inline, don't quote.** A tiered list stays scannable.
- **Batch, never loop.** One `leads` call for an external list, `identifiers` in 100s for a cohort.
- **Rank on the layered priority**, never fit alone — surface fit/pain/intent/ability every time.
- **Name what's missing.** If a layer is unknown, say so and give the engine's play to resolve it —
  don't guess a tier past what's known.
- Keep next actions specific to the score's reason — no boilerplate "reach out".
