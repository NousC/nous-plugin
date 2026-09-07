# Authoring the ICP (the scoring model) — for onboarding

The ICP is the **scoring model** every account is judged against. In onboarding you author it with the
user and write it via the `set_icp` tool (body_md = the markdown below). The engine compiles it; `score`
and get_context then judge accounts against it. Write it ONCE per workspace; `whoami` → `setup.has_icp`
tells you whether one already exists (if it does, read/confirm before replacing).

## How to gather it (fast, in onboarding)
Draw a first draft from what you already have, then confirm with the user — don't make them fill a form:
1. Use the backfilled accounts + the company insights (`record_insight` themes) to infer the pattern.
2. Ask 2–3 sharp questions: *who's your ideal customer (role + company type)? what size/geography? what
   signals mean "in-market" for you?*
3. If the user already has a canonical ICP file (e.g. `context/nous/icp/icp.md`), **read it and reuse it**
   verbatim rather than inventing a second one — reconcile, don't duplicate.

## Required shape (body_md)
```markdown
# ICP (the scoring model)

One score, one question: how likely is this person/company to become a customer?

## How the score works
score = 40 base + best-matching PERSONA weight + FIRMOGRAPHIC weights + observed SIGNAL boosts/cuts,
clamped 0–100. A hard disqualifier caps the score at 25. Static evidence (persona + firmographics)
sets the base; signals only exist once observed — an absent signal never lowers a score.

## The buyer
<2–4 sentences: who the ideal customer is, in the user's words>

## 1. Personas (strongest single match applies)
- <best-fit role/persona> +35
- <next> +28
- <adjacent> +12
- No clear fit +0

## 2. Firmographics (additive, when known)
- <target geography> +12   ·  <secondary> +4
- <target company size> +12  ·  <adjacent sizes> +6
- <target company type> +8

## 3. Signals (observed only — boost or cut, never penalize for absence)
Boosts: <in-market signal> +12; <momentum signal> +8; <fit signal> +6
Cuts: <not-in-market> -10; <wrong-fit> -25

## 4. Hard disqualifiers (cap the score at 25)
- <e.g. no relevant role/signal at all>
- <e.g. wrong market entirely>

## Anchors
Fit: <a real example of a great-fit customer>.
```

## Rules
- Weights are the user's judgment — propose sensible defaults (like above), let them adjust, don't invent
  precise numbers they didn't sanction if they push back.
- Keep it ONE ocean/one score. Don't build multiple ICPs.
- After `set_icp` succeeds, proceed to score the backfilled accounts (onboard Phase D).
