---
name: map-committee
description: Maps the buying committee on a deal — who's involved, their role and stance (champion, blocker, economic buyer, technical evaluator, end user), who's engaged, and which roles are missing. Use when the user asks who's on the deal, for a stakeholder map, who the decision-makers are, whether they're single-threaded, or who they're missing. Surfaces coverage gaps and how to multi-thread.
---

# Map committee

Turn the account's people into a committee map: who covers which role, who's engaged, and — the part that wins or loses deals — who's missing. Single-threaded is a risk to name, not hide.

## Tools
- `get_account` — the account's known contacts, their titles, engagement, and any recorded stance (champion / blocker) or authority Intel.
- `query` — find other contacts at the same company/domain who aren't engaged yet, to fill a gap the known set doesn't cover.

## Workflow
1. **Resolve the account** and `get_account` for its people, roles, engagement, and recorded stance.
2. **Map to committee roles:** economic buyer, champion, technical evaluator, blocker, end user. Place each known person; mark engaged vs silent.
3. **Find the gaps** — no economic buyer engaged, only one contact (single-threaded), a known blocker with no counter, a champion who's gone quiet.
4. **Fill them:** `query` the domain for contacts who could cover a missing role, and name the move to reach them (a warm intro from the champion, a direct touch).

## Output
```
# Buying committee — <Account>
| Role | Person | Stance | Engaged |
|---|---|---|---|
| Economic buyer | <name / MISSING> | <champion/blocker/neutral> | <yes/no> |
| Champion | … | | |
| Technical evaluator | … | | |
| Blocker | … | | |

**Gaps**
- <the missing role or the single-thread risk>

**Multi-thread next**
- <who to reach and how, grounded in the record>
```

## Artifact (Claude Code only)
After the map, render it as a **branded HTML artifact** and publish it. Copy
**`../../references/artifact-template.html`** and follow **`../../references/artifact-design.md`**
exactly. The **shell and rules are shared** with every OpenNous artifact (masthead, `Generated for
{name}, {company}`, full-UUID account/person links, footnote source citations for a stance, sentence
case, no em dash or colon, numerals, footer). The **body is this skill's own, and the visual IS the
point**: render the committee as a **node diagram** — the people placed by role (economic buyer,
champion, technical evaluator, blocker, end user), engaged vs silent shown in the node, and **missing
roles as empty slots** so the coverage gap is visible at a glance. Then the gaps and the multi-thread
move below it. The text map stays the answer. Not on Claude Code? Skip the artifact.

## Rules
- **Name gaps, don't fill them with guesses** — a role with no known person is `MISSING`, not an invented name.
- **Single-threaded is a flagged risk** even when the one contact is a strong champion.
- **Quote the evidence for a stance.** Champion or blocker is a strong claim, so show the line
  that supports it and who said it.
- **Ground stance in recorded Intel** (authority, champion/blocker signals); don't infer power from title alone.
