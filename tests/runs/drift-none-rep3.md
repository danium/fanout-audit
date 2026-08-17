## Entity consistency: Kettleridge

Sources compared (5):
1. GitHub README, first line
2. kettleridge.dev homepage hero
3. Conference talk bio
4. Docs site landing page
5. Package registry description

CATEGORY NOUN — consistent
  load-testing tool    Source 1, 2, 3, 4, 5

NAME FORM — consistent
  Kettleridge    Source 1, 2, 3, 4
  (name not repeated in the description text)    Source 5 — package registry copy typically sits under the package's own title, so this is not a competing form, just a field that doesn't restate the name.

AUDIENCE — consistent
  API teams    Source 1, 3, 4, 5
  (unstated, not contradicted)    Source 2 — the hero leads with a pain-point line and never names an audience; it does not name a different one, so this is omission, not conflict.

DIFFERENTIATOR — drift
  replays real production traffic against a staging environment, preserving request ordering and timing
    → Source 1 ("replays real production traffic against a staging environment")
    → Source 2 ("replays your real production requests against staging")
    → Source 4 (captures a window of requests, strips credentials/PII, replays at a chosen volume multiple, preserves ordering and inter-arrival timing)
    → Source 5 ("Replays captured production traffic against staging. Preserves timing and ordering.")
  (absent)    Source 3 — conference bio

  The conference bio states the category noun and audience ("a load-testing tool for API teams") but no differentiator — nothing that distinguishes Kettleridge from any other load-testing tool. A reader who saw only the bio would not know it replays production traffic at all. Per the skill's rule, a description with no differentiator is flagged as missing rather than treated as ordinary length variation, because bio brevity elsewhere (e.g. omitting "API teams" in Source 2) still preserved a distinguishing claim, while this one drops it entirely.

No other drift found. The following differences were noted but are acceptable variation, not drift:
  - Length: README/registry are one or two lines; docs page is a full paragraph.
  - Register: homepage hero leads with the pain ("the load you rehearse is the load you serve"); docs page and registry lead with the mechanism.
  - Ordering: sources sequence category/audience/differentiator differently.
  - Added detail: the docs page's mention of credential-stripping, PII removal, and configurable replay volume adds specificity to the same underlying claim rather than changing it.

CANONICAL SENTENCE

> Kettleridge is a load-testing tool for API teams that replays real production traffic against a staging environment, preserving request timing and ordering.

22 words. Names the entity, the category noun (load-testing tool), the audience (API teams), and the differentiator (replays production traffic against staging, preserving timing/ordering). No superlatives. Traceable to Source 1 for the entity/category/audience/core-replay claim and to Sources 4 and 5 for the timing-and-ordering preservation detail.

No factual conflict existed across the sources (audience and differentiator omissions in Sources 2 and 3 do not contradict the other sources), so no clarifying question was needed before proposing this sentence.
