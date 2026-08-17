# Entity consistency: Marrowfield

Five sources compared, decomposed into category noun, differentiator, audience, and name
form, per `references/entity-drift.md`.

## Sources decomposed

| # | Source | Category noun | Differentiator | Audience | Name form |
|---|---|---|---|---|---|
| 1 | GitHub README | feature-flag library | evaluates flags locally, no network call on the hot path | backend services | Marrowfield |
| 2 | marrowfield.io homepage hero | experimentation platform | "ship confidently, measure everything, roll back in one click" | product teams | (name not used in the excerpt) |
| 3 | startup directory listing | release management software | controls rollout of internal applications across large user populations | enterprise IT departments / operations teams | MarrowField |
| 4 | founder's conference bio | developer tool | gradual rollouts | developers (implied) | Marrowfield |
| 5 | docs site landing page | feature-flag library | loads a ruleset at startup, keeps it in memory, evaluates every flag check locally against it | not stated | Marrowfield |

## Drift found

```
CATEGORY NOUN — drift
  feature-flag library          README, docs landing page
  experimentation platform      homepage hero
  release management software   directory listing
  developer tool                 conference bio

  Feature-flag library repeats only between the README and the docs page — the two
  most technical, first-party sources, and their descriptions of the mechanism agree.
  The other three sources each name a different category, and none of the three
  matches any other: an experimentation platform, an enterprise release-management
  tool, and an unspecified "developer tool." This is four categories across five
  sources, not one category stated four ways.

DIFFERENTIATOR — drift
  local evaluation, no network call on the hot path      README
  ruleset loaded at startup, cached, evaluated locally    docs landing page
    (same underlying claim as the README, with more detail — not drift between
    these two, per the "added detail" rule)
  "ship confidently, measure everything, roll back in one click"   homepage hero
  controls rollout of internal apps across large user populations  directory listing
  gradual rollouts                                        conference bio

  README and docs describe one mechanism (local, in-memory evaluation). The
  homepage, directory, and bio each describe something else, and not the same
  something else as each other — an outcome claim that includes "measure
  everything" (experimentation/analytics, not mentioned anywhere else), an
  enterprise rollout-control claim, and an unspecified "gradual rollouts."

AUDIENCE — drift
  backend services                                README
  product teams                                   homepage hero
  enterprise IT departments / operations teams     directory listing
  developers (implied)                             conference bio
  (not stated)                                      docs landing page

  Four distinct audiences across the sources that state one at all, and none is a
  narrowing of another — backend services, product teams, enterprise IT, and
  developers are different buying centers, not degrees of the same one.

NAME FORM — drift
  Marrowfield       README, conference bio, docs landing page
  MarrowField        directory listing
  (name not used)    homepage hero
```

## Canonical sentence — not proposed yet

Category noun and audience aren't wording differences here — they're conflicting
claims about what the product is and who buys it. Per the canonical-sentence rule
("if the sources conflict on a fact, ask which is correct rather than picking the
one that reads best"), I'm asking instead of resolving these by picking the
majority or the best-sounding option:

1. **What is Marrowfield, fundamentally?** The README and docs page (the two
   first-party technical sources) agree it's a feature-flag library that evaluates
   flags locally against an in-memory ruleset. The homepage, directory listing, and
   bio each describe a different kind of product — an experimentation platform, a
   release-management tool, or an unspecified developer tool. Is the library
   description the accurate one, and the other three are marketing/listing drift
   that should be corrected — or does Marrowfield now do more than the README and
   docs describe?
2. **Who is it for?** Backend services/engineers, product teams, enterprise IT
   departments, or developers generally? These four don't reduce to one audience.
3. **Name form:** standardize on "Marrowfield" (3 of 5 sources) and fix the
   directory listing's "MarrowField"? The homepage doesn't use the name at all in
   the given excerpt, which is worth noting but is a gap, not a form conflict.

Once (1) and (2) are answered, a canonical sentence can be written from the
README/docs description, which is the only pair that already agrees end to end —
I haven't drafted one here because doing so before that answer would mean picking
a fact rather than reporting that one is unresolved.
