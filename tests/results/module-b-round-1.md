# Module B — first real test

Plan item B. Entity consistency had shipped with one smoke test, no baseline, and none of the
work Module A received. This is its first evidence.

Both fixtures are fresh. Neither reuses the Ledgerline example that lives in
`references/entity-drift.md` — testing on content the reference already contains hands the
model its answers, which invalidated a sub-query test during the taxonomy work.

## drift-real — detection → PASS 2/2

Five descriptions of a feature-flag library, carrying four genuine drifts: category noun across
library / platform / release-management software / developer tool, audience across backend
services / product teams / enterprise IT, a `MarrowField` capitalisation variant, and
non-overlapping differentiators.

| Rep | Components flagged | Name form | Canonical sentence | Invented |
|---|---|---|---|---|
| 1 | all four | caught | **asked instead** | none |
| 2 | all four | caught | **asked instead** | none |

Identical behaviour across both. The rule that carries the fabrication risk — when sources
conflict on a fact, ask rather than pick the one that reads best — held in both reps against a
prompt that explicitly requested a sentence.

Rep 2 drew a distinction better than the rule requires: it resolved name form unilaterally on
majority usage while refusing to resolve category and audience. Capitalisation is a style
choice; whether you sell to backend engineers or enterprise IT is a fact only the owner knows.

## drift-none — over-flagging → PASS on false flags, 2–1 split on the verdict

Five descriptions of a load-testing tool agreeing on category noun, differentiator and
audience, differing only in length, tone, ordering and added detail — every variation the
reference explicitly says not to flag.

| Rep | Drift found | False flags |
|---|---|---|
| 1 | no | none |
| 2 | no | none |
| 3 | **yes** | none |

**No false flags in any rep.** All three explicitly considered length, register, ordering and
added detail, and excluded each as acceptable variation. The failure mode the reference warns
about — flagging every surface difference until the report stops being read — does not
reproduce.

Reps 1 and 2 produced word-for-word identical canonical sentences at 17 words.

### The defect: an internal contradiction in entity-drift.md

Rep 3 flagged the conference bio for carrying no differentiator. Rep 1 explicitly considered
the same thing and declined, reasoning that the omission contradicts nothing. Both were
following the reference; the reference said both things:

- under real drift — a description with no differentiator at all is drift, flag it as missing
- under acceptable variation — added detail in a longer version that does not contradict the
  shorter one is fine, whose logic makes omission-without-contradiction acceptable

A short bio satisfies both simultaneously and they point opposite ways.

This is worse than the criteria-attribution variance seen in the COVERED work, where the
verdict converged and only the cited number moved. Here the **headline verdict** diverged: a
user asking "are these consistent?" gets yes or no depending on the run.

Same class of defect as criterion 4's gate-versus-diagnostic collision, and it survived for the
same reason — nothing had ever tested this module.

### Fix and verification

Added **"Omission is not contradiction"** to `entity-drift.md`. Flag a missing component only
when it contradicts another source, or when no source states it at all. The test given is
whether a reader comparing two sources would come away with conflicting beliefs — a shorter
description leaves them with fewer beliefs, not wrong ones.

Verification rep against the fixed rule: **no drift**, the bio's omission considered and
declined citing the new rule by name, no false flags, same 17-word canonical sentence.

## Decision: Module B stays

The extraction case rested mainly on Module B intruding into Module A reports with "not run"
disclaimers. That did not survive checking — **zero of 25 committed run files** contain one. It
was observed twice in agent replies during the RED phase, but from runs whose artifacts were
never written, so it is real, rare, and not reproducible from the repository.

Against that, the module works: detection 2/2, ask-don't-pick 2/2, no false flags 3/3, nothing
invented anywhere, and its one genuine defect was an internal contradiction now fixed and
verified. Extracting a functioning module on evidence that evaporated would be churn.

Remaining known weakness, not blocking: the skill description carries an "Also use when…"
clause for Module B, giving one skill two disjoint trigger sets. That is an SDO concern worth
revisiting if triggering ever proves imprecise in practice, and it is cheaper to fix than an
extraction.
