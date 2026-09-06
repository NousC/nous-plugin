---
name: build-record
description: Researches a net-new person or company from scratch — LinkedIn, website, news — extracts the facts on your own tokens, and files them to Nous as a resolved, ICP-scored account record. Use when the user drops a name, email, domain, or LinkedIn URL that isn't in the graph yet, or asks to research, look up, enrich, profile, or "add" a prospect or account, or to build a record where none exists.
---

# Build record

Create the account record where no CRM has one. You do the research on your own tokens, extract the structure, and file it to Nous — Nous resolves the identity and scores it. Raw research stays in the user's git; Nous holds only the structured facts plus a pointer back.

## Tools
- `mcp__nous__get_account` — FIRST, check whether a record already exists for the email / domain / LinkedIn URL. If it does, this becomes an enrichment (add only what's missing), not a new record.
- `mcp__nous__record` — file the extracted facts against a precise `focus` (email / linkedin_url / domain). Person and company facts are separate observations.
- `mcp__nous__score` — score the freshly built record against the live ICP.

## Workflow
1. **Check for a duplicate.** Call `get_account` with the identifier the user gave. If a record exists, read it and research only the gaps. Never fork a second record for the same person.
2. **Research on your own tokens.** Use your own web/search/browse ability (not a Nous tool) to gather: for a person — name, title, seniority, department, company, location, LinkedIn URL; for a company — domain, description, what they do, size/keywords, and any recent, dated signal (funding, hiring, launch, leadership change).
3. **Save the raw to git.** Write your research notes to the user's repo (`raw/research/<slug>.md`) so the observations can carry a `source_ref: git://…` pointer + `content_hash`. Never send the prose to Nous.
4. **Record the facts.** Call `record` with `focus` = the precise identifier:
   - Person facts as `kind:'state'` (`first_name`, `last_name`, `job_title`, `seniority`, `department`, `company`, `linkedin_url`, `city`, `country`), each `method:'inference'` or `'extraction'`, `source:'agent'`.
   - Company facts against the domain (`domain`, `description`, `keywords`).
   - Any dated signal as `kind:'state', property:'signal.<class>'` with the `value` shape.
   - Set a distinct `external_id` per observation (`research:<slug>:<property>`) so a re-run doesn't duplicate.
5. **Score it.** Call `score` on the new record for ICP fit + intent.
6. Report the record and what to do with it.

## Output
```
# <Person / Company> — record built
**ICP:** <score>/100 <tier> · **Intent:** <band>
**Who:** <title> at <company> · <location>
**Company:** <one line on what they do>
**Signal:** <the dated signal, if any>
**Filed:** <n> facts recorded · raw → raw/research/<slug>.md

**Why they fit (or don't)**
<2-3 sentences grounded in the ICP reason>

**Next move**
<the one action, tied to a fact you found>
```

## Rules
- **Check before you create** — always `get_account` first; enrich an existing record rather than forking a duplicate.
- **Raw stays in git.** File research to the repo and pass `source_ref` + `content_hash`; never send prose to Nous.
- **Extract, don't guess.** Record only what the research actually supports; mark inferences with `method:'inference'`. If you can't find a fact, leave it out — don't invent a title or a signal.
- **Precise focus, never a bare name.** File against an email, LinkedIn URL, or domain so the resolver attaches correctly. You never merge or resolve identities — Nous does.
