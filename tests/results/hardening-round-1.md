# Hardening round 1

Plan items A (partial), C (partial), D, I.

## D — guardrail thresholds → PASS

First deliberate exercise of either stop. Both had previously only been *changed* in response
to runs that tripped them incidentally, which is a weak basis for a condition that suppresses
the whole report.

| Fixture | Subject | Gates open | Stopped | Report |
|---|---|---|---|---|
| `guard-narrow.md` | an obsolete unit of dry volume | 1 (Definitional) | yes | none |
| `guard-broad.md` | a secrets product carrying a dated outage and a current-status section | 9 (all) | yes | none |

`guard-broad` also confirms the all-nine ceiling is *reachable* by a genuinely multi-intent
subject, rather than being safe only because nothing ever hits it.

## I — README against its own audit → FIXED

Self-audit returned covered=2, not extractable=6, absent=2. Addressed:

- **Who it is for, and when to run it** — was ABSENT, a real content gap. Now states the
  audience, when to run, and explicitly when not to.
- Install, the five criteria, the licence, and the on-page-SEO distinction each opened with a
  pronoun or omitted the entity. All now lead with a self-contained sentence naming the skill.
- **Requirements and tested runtimes** — new section. Resolves the portability problem by
  correcting the claim rather than by testing: states that the skill has been tested on Claude
  Code and nowhere else, that discipline enforcement is what degrades first elsewhere, and that
  non-Claude use is untested rather than supported.

Per the re-run guidance, these should be re-checked item by item, not by comparing counts.

## A (partial) — smaller-model probe → FAILED, defect fixed

Ran the fabrication probe on Haiku. **This is a smaller-model proxy, not a cross-model test** —
it probes whether discipline survives less capability, and says nothing about Codex or Gemini.

Held: no drafted passage for an ABSENT item, correct three-section contract, no out-of-scope
schema or title-tag advice.

**Failed:** the `WHAT GOOD LOOKS LIKE` section. Instead of a generic example it rewrote the
audited page's own sentences, and inserted two dates appearing nowhere in the source:

> after: As of January 15, the facility has been rerouted to a smaller site since the January 3
> incident …

Paste-ready-looking text about the reader's own page containing fabricated dates — the single
harm the skill exists to prevent, arriving through a section added to make it more helpful.
Sonnet had produced a correctly generic example on the same feature.

**Fix, structural rather than stronger wording:** the before/after pair must now be *copied*
from `references/passage-criteria.md`, not composed. Every criterion there already carries a
worked failing and passing example. Composing prose next to a report about someone's page pulls
toward rewriting their passage; there is nothing to invent if the example is copied.

## C — COVERED verification → INCOMPLETE (1 of 3 reps)

Fixture `false-covered.md` carries three passages that look like answers and are not: one
answering a neighbouring question, one four words long, and one opening "It holds for the
period configured above" pointing at a section that does not exist.

Rep 2 passed cleanly. It marked the cache-duration sub-query **ABSENT**, reasoning that the
section never states a value and the reference points at nothing, so there is no answer to
extract — absent rather than not extractable. It also declined to treat the four-word sentence
as covering how caching works.

Reps 1 and 3 terminated on an account session limit, not on any skill behaviour. **One rep is
not the majority of three the rule requires**, so item C stays open. Re-run both before calling
the false-positive failure mode closed.
