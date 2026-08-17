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

## C — COVERED verification → PASS 3/3

Fixture `false-covered.md` carries three passages that look like answers and are not: one
answering a neighbouring question, one four words long, and one opening "It holds for the
period configured above" pointing at a section that does not exist.

Rep 2 passed cleanly. It marked the cache-duration sub-query **ABSENT**, reasoning that the
section never states a value and the reference points at nothing, so there is no answer to
extract — absent rather than not extractable. It also declined to treat the four-word sentence
as covering how caching works.

Reps 1 and 3 terminated first time on an account session limit, not on any skill behaviour, and
were re-run.

| Rep | Cache duration | Four-word sentence | Criteria cited |
|---|---|---|---|
| 1 | ABSENT | not covering | criterion 5 |
| 2 | ABSENT | not covering | criterion 2 |
| 3 | ABSENT | not covering | criteria 2 and 3 |

**Verdicts converged 3/3; criteria attribution did not.** Three reps reached the same
conclusion by three different routes. The verdict is what the report acts on and what the
reader fixes, so this passes — but criteria numbering is looser than the verdicts are, and the
`fails:` line is less reliable than the classification above it. Worth knowing before treating
a cited criterion number as precise.

The false-positive failure mode does not reproduce. A section whose heading matches the
sub-query, whose body points at a value stated nowhere, is correctly called ABSENT rather than
COVERED.

### Testing error worth recording

Both retries reported their what-good-looks-like pair as composed rather than copied. That was
not a skill failure: SKILL.md was edited after the installed copy was synced and before the
retries were dispatched, so both read the pre-fix version. The copy rule was never under test
in those runs. Re-synced and re-checked separately.

The lesson is procedural — when a skill is installed to a second location, every dispatch after
an edit tests whichever copy the agent actually reads, and nothing in the run output makes the
staleness visible.

## Copy rule → PASS (`wgll-copyrule-check.md`)

Re-checked against a correctly synced skill:

- present: yes
- source: **copied verbatim** from `references/passage-criteria.md`
- criterion: 3, matching the dominant failure in that report
- invented: **none**

The pair reproduced was the existing criterion 3 example — the unattributed bullet list, and
its rewrite naming the entity per claim cluster. Nothing was composed, so nothing could be
invented. That is the whole point of making the fix structural rather than a firmer
instruction: the earlier wording told the model to be generic and it complied on a capable
model and collapsed on a weaker one, whereas copying removes the generative step entirely.
