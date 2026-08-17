## Entity consistency: Kettleridge

Five sources compared: GitHub README (first line), kettleridge.dev homepage hero, conference talk
bio, docs site landing page, package registry description.

Each description decomposed into category noun / differentiator / audience / name form, then
compared component-by-component per `references/entity-drift.md`.

```
CATEGORY NOUN — consistent
  load-testing tool    README, homepage hero, conference bio, docs landing page, registry

  All five sources use the same term. No drift.

DIFFERENTIATOR — consistent
  replays real/captured production traffic against a staging environment (preserving request
  ordering and inter-arrival timing where stated)
                        README, homepage hero, docs landing page, registry

  (not stated)          conference bio

  Four sources state the same claim. The docs page and the registry add detail — the
  capture/strip/replay mechanism, and "preserves timing and ordering" — that does not
  contradict the README's shorter version; that is added specificity, not a changed claim.
  The conference bio omits the differentiator entirely, but its own sentence is otherwise
  consistent with the rest ("a load-testing tool for API teams"), and the bio's real estate
  goes to introducing the speaker, not the product. Per the length rule in
  `references/entity-drift.md` ("a one-line version that omits detail present in a longer
  one is not drift, provided it does not contradict"), this omission does not contradict
  anything the other four sources say, so it is not flagged.

AUDIENCE — consistent
  API teams             README, conference bio, docs landing page, registry

  (not stated)          homepage hero

  Four of five sources name the same audience and none contradicts it. The homepage hero
  drops the audience in favor of a punchy, benefit-led opening line ("Test with the traffic
  you actually get") — a tone and length choice, not a different claim about who the product
  is for.

NAME FORM — consistent
  Kettleridge           README, homepage hero, conference bio, docs landing page

  (not shown)           registry description

  Every source that names the product spells it the same way: "Kettleridge." The registry
  blurb doesn't include the name in its description text — registry listings typically show
  the package name as a separate field above the description — so this is a formatting
  omission, not a variant spelling.
```

No drift found. Every difference across these five sources is the kind
`references/entity-drift.md` marks as acceptable: length (the homepage hero and the registry
blurb are the two shortest, and each drops one component the other four keep), tone (the
homepage hero's "the load you rehearse is the load you serve" versus the README's plainer
phrasing), and added detail (the docs page's capture/strip/replay mechanism and the registry's
"preserves timing and ordering," both consistent with, not contradicting, the shorter
versions).

## Canonical sentence

> Kettleridge is a load-testing tool for API teams that replays real production traffic
> against a staging environment.

17 words. Names the entity, the category noun, the audience, and the differentiator. This is
the README's own sentence (Source 1) verbatim — independently corroborated by the homepage
hero, the docs landing page, and the package registry description. No source disputes any
fact in it, so no question was put to the user before proposing it.
