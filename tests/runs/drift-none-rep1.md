# Entity consistency: Kettleridge

Five sources compared: GitHub README, homepage hero, conference bio, docs landing page,
package registry description.

## Component comparison

CATEGORY NOUN — no drift
  load-testing tool     README, homepage, bio, docs, package registry (all five, identical wording)

AUDIENCE — no drift
  API teams              README, bio, docs, package registry
  (unstated)              homepage hero — omits the audience phrase but does not name a
                          different one; consistent with the length exception for a short
                          marketing hero, not a contradiction

DIFFERENTIATOR — no drift
  replays real/captured production traffic against a staging environment
                          README, homepage, docs, package registry — same underlying claim
                          at different levels of detail (docs and registry add that ordering
                          and inter-arrival timing are preserved; that is specificity, not a
                          different claim)
  (unstated)              conference bio — a one-sentence speaker-intro mention with no
                          product claim at all

  Considered flagging the bio's missing differentiator on its own, since
  references/entity-drift.md calls a fully-missing differentiator drift. Not flagging it
  here: nothing else in the bio conflicts with the other four sources (category noun and
  audience both match), the omission doesn't contradict the differentiator stated
  elsewhere, and it reads as ordinary brevity for a one-line speaker credit rather than a
  competing description of the product. Flagging it would be exactly the kind of
  every-surface-difference flag the module exists to avoid.

NAME FORM — no drift
  Kettleridge             README, homepage, bio, docs, package registry (identical
                          capitalization and spacing throughout)

## Differences checked and not flagged

- Length: bio (1 sentence) vs. docs landing (3 sentences) — expected variation by placement.
- Tone/register: homepage leads with an outcome-framed hook ("Test with the traffic you
  actually get"); README and docs lead with a plain definition. Same underlying claim.
- Reordering: bio states name-then-category ("Priya builds Kettleridge, a load-testing
  tool...") instead of entity-then-category. Stylistic.
- Added detail: docs and package registry add configurable replay volume, preserved
  ordering, and preserved inter-arrival timing, none of which appears in the README. This
  is specificity added on top of the same claim, not a contradiction.
- Verb/article choice: "replays real production traffic" / "replays your real production
  requests" / "replays captured production traffic" — synonymous phrasing, not a finding.

## Verdict

No drift found across category noun, audience, differentiator, or name form. The sources
describe the same entity consistently; differences are length, tone, ordering, and
specificity, which this module treats as acceptable variation, not drift.

## Canonical sentence

No factual conflict exists between sources, so no question needed asking before proposing
one.

> Kettleridge is a load-testing tool for API teams that replays real production traffic
> against a staging environment.

17 words. Names the entity, the category noun, the audience, and the differentiator. No
superlatives. Sourced directly from the README (Source 1) and corroborated without
contradiction by the homepage, bio, docs page, and package registry.
