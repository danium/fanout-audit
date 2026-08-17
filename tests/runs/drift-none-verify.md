# Entity consistency: Kettleridge

Sources compared (all pasted by the user in `tests/fixtures/drift-none.md`):

1. GitHub README, first line
2. kettleridge.dev homepage hero
3. Conference talk bio
4. Docs site landing page
5. Package registry description

Each description decomposed into category noun, differentiator, audience, and entity name
form, then compared component-by-component per `references/entity-drift.md`.

## Category noun

  load-testing tool   Source 1, 2, 3, 4, 5

All five sources use the identical term. No drift.

## Differentiator

  replays real/captured production traffic against a staging environment
                       Source 1, 2, 4, 5
  (absent)             Source 3 (conference bio)

Sources 1, 2, 4, and 5 all state the same underlying claim — production traffic is captured
and replayed against staging. Source 4 adds detail (strips credentials/PII, configurable
replay multiple, preserves ordering and inter-arrival timing) and Source 5 condenses to
"preserves timing and ordering" — both are added detail / length variation on the same claim,
not a different claim. Source 2's marketing framing ("the load you rehearse is the load you
serve") is register, not substance.

Source 3 (the conference bio) states no differentiator at all — it gives category and
audience, then pivots to the speaker's background. This is a missing component, not a
conflicting one: four other sources state the differentiator consistently, so this isn't the
"no source states it at all" case, and nothing in the bio contradicts what the other sources
say. Per the entity-drift.md rule, a bio dropping a component for length is the example case
for *not* flagging — flagged here it would be, in the reference's own words, "noise on
exactly the surface where brevity is correct." No drift.

## Audience

  API teams            Source 1, 3, 4, 5
  (unstated)           Source 2 (homepage hero)

Source 2 omits "for API teams" but states nothing that contradicts it — the omission reads as
marketing-copy brevity, not a different audience claim. No drift.

## Entity name form

  Kettleridge          Source 1, 2, 3, 4
  (unstated)           Source 5 (package registry)

Source 5's blurb never names the entity in the description text (the name is presumably
carried by the registry listing itself, e.g. the package title, outside the quoted text).
Where it is stated, the form is identical every time — no capitalization or spacing variants.
No drift.

## Length, tone, and ordering — checked, not flagged

Per the acceptable-variation rules: Source 4 is several times longer than Source 5, and
Source 2 leads with a pain-point hook instead of a definition. Neither changes the underlying
claim, so neither is reported as drift.

---

## Result

No drift found. All five descriptions agree on category (load-testing tool), the core
differentiator (replays production traffic against staging), and audience (API teams) where
stated. The only differences are omissions consistent with each source's format (bio, hero
copy, registry blurb) and are not contradictions.

## Canonical sentence

> Kettleridge is a load-testing tool for API teams that replays real production traffic
> against a staging environment.

17 words. Names the entity, the category noun, and the differentiator. Traceable almost
verbatim to Source 1 (README) and corroborated by Sources 2, 3, 4, and 5 — no fact conflicts
existed across sources, so nothing needed to be resolved by asking the user.
