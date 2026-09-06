---
name: lead-triage
description: Scores a set of leads or accounts against the live ICP and sorts them into priority tiers, each with the reason and the recommended next action. Use when the user asks to triage, qualify, prioritize, rank, or tier leads or accounts, asks which leads to work first, or drops in a list of people/companies to sort.
---

# Lead triage

Take a set of leads and return a worked list: who to act on first, why, and what to do — scored against the user's own ICP, not generic firmographics.

## Tools
- `mcp__nous__score` — the live ICP fit + intent for one lead. It writes the judgment into the graph. Pass the lead's `attributes` (title, company, keywords) when the lead isn't in the graph yet, so it can score inline.
- `mcp__nous__query` — gather a set already in the graph (e.g. a stage, a recent-signal cohort) when the user asks to triage "my new leads" rather than pasting a list.
- `mcp__nous__get_account` — pull the reason behind a borderline score.

## Workflow
1. Get the set: use the list the user gave, or `query` for the cohort they named.
2. `score` each lead. For leads not yet in the graph, pass their attributes so scoring runs inline.
3. Sort into tiers by ICP fit and intent together — a high-fit / high-intent lead outranks high-fit / cold.
   - **Tier 1** — strong fit + warm/hot intent → work by hand now.
   - **Tier 2** — strong fit, cold, or moderate fit + intent → sequence/nurture.
   - **Tier 3 / skip** — weak fit or disqualified → deprioritize, say why.
4. For each Tier 1, give the one next action grounded in the score reason or a signal.

## Output
```
# Lead triage — <n> leads
**Tier 1 (work now)**
- <name>, <company> — ICP <score> <tier>, intent <band> — <reason> → <next action>

**Tier 2 (nurture)**
- <name>, <company> — ICP <score>, intent <band> — <reason>

**Tier 3 / skip**
- <name>, <company> — <why deprioritized>
```

## Rules
- Rank on fit AND intent, never fit alone.
- If a lead can't be scored (not enough info), say what attribute is missing rather than guessing a tier.
- Keep next actions specific to the score's reason — no boilerplate "reach out".
