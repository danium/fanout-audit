## Entity consistency: Marrowfield

Five sources compared: GitHub README (first line), marrowfield.io homepage hero, startup
directory listing, founder's conference bio, docs site landing page.

Each description decomposed into category noun / differentiator / audience / name form,
then compared component-by-component per `references/entity-drift.md`.

```
CATEGORY NOUN — drift
  feature-flag library          GitHub README, docs landing page
  experimentation platform      homepage hero
  release management software   startup directory listing
  developer tool                founder's conference bio

  Four category nouns across five sources. "Feature-flag library" and "developer tool" are
  arguably close. "Experimentation platform" (implies A/B testing and measurement) and
  "release management software" for enterprise IT are different products with different
  buyers than the other three.

DIFFERENTIATOR — drift
  evaluates flags locally, no network call on the hot path        GitHub README
  loads a ruleset at startup, evaluates every check locally       docs landing page
                                                                   (consistent with README,
                                                                   adds mechanism detail)
  ship confidently / measure everything / roll back in one click  homepage hero
  control rollout of internal apps across large user populations  directory listing
  gradual rollouts (unspecified)                                  conference bio

  README and docs agree on a specific capability claim (local evaluation, no network call).
  The homepage substitutes an outcome claim, the directory names a different capability for
  a different buyer, and the bio is too vague to confirm or contradict either.

AUDIENCE — drift
  backend services / developers            GitHub README, conference bio (implied by
                                            "developer tool")
  product teams                            homepage hero
  enterprise IT departments / ops teams    directory listing
  (not stated)                             docs landing page

  Backend services and developers are consistent with each other. Product teams and
  enterprise IT departments each contradict that pair and each other — three distinct
  buyers, not a narrowing of one audience.

NAME FORM — drift
  Marrowfield                        GitHub README, conference bio, docs landing page
  MarrowField                        startup directory listing
  (name not shown in visible copy)   homepage hero

  Capitalisation variant in the directory listing only. "Marrowfield" is the form used in 3
  of the 4 sources that name the entity at all.
```

## Canonical sentence — blocked on two facts, not proposed

The category noun and the audience are not a wording choice here: the sources describe four
different products for three different buyers, and picking the most common or best-sounding
variant would mean stating something as fact that no source agrees on. Per the skill's rule
("if the sources conflict on a fact, ask which is correct rather than picking"), that call
belongs to you:

1. **What kind of thing is Marrowfield** — a feature-flag library (README, docs), an
   experimentation platform (homepage), release management software (directory), or a
   developer tool (bio)?
2. **Who is it for** — backend services/developers (README, bio), product teams (homepage),
   or enterprise IT/ops (directory)?

Once those two are answered, the sentence is close to assembled already: the README and docs
independently agree on the same mechanism — local evaluation against an in-memory ruleset,
no network call on the hot path — which can be reused as the differentiator as-is.

**Not blocked — fix independently of the above:**

- **Name form**: standardize on "Marrowfield" (3 of the 4 sources that name the entity use
  it; "MarrowField" in the directory listing is the outlier) and correct the directory
  listing.
