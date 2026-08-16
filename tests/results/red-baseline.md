# RED baseline — v1 (seven classes) against the four new content types

9 of 12 reps completed. All four fixtures covered; news and support have the full 3 reps,
entity has 2, availability has 1. The remaining 3 were killed when the dispatch mechanism
ran away (see ledger). Rep counts are noted per class below, because the KEEP/DROP rule
requires a majority of three.

## Verdicts

| Class | Prediction | Result | Reps | Verdict |
|---|---|---|---|---|
| Verification | no sub-query asks who reported it or whether it is confirmed | **prediction held** | 3/3 | **KEEP** |
| Recency | no sub-query asks whether the state still holds | **partially held** | 3/3 | **KEEP (narrowed)** |
| Diagnostic | no sub-query touches the error string | **falsified** | 3/3 | **DROP** |
| Affiliation | nothing on roles, history, or connections | **falsified** | 2/2 | **DROP** |
| Locality | nothing keyed to geography or jurisdiction | **falsified** | 1/1 | **DROP (provisional)** |

## Evidence

### Diagnostic — DROP (falsified 3/3)

Every support rep produced a sub-query naming the error string directly:

- rep1: "what does ERR_SHARD_LOCK_TIMEOUT mean"
- rep2: "what does ERR_SHARD_LOCK_TIMEOUT mean" (reported ABSENT — v1 found the gap)
- rep3: "why does Emberreef give up and log ERR_SHARD_LOCK_TIMEOUT instead of resolving
  the stuck rebalance itself"

v1 also produced failure-mode sub-queries unprompted — "what happens if you force-unlock a
shard whose holder is not actually dead" (all three reps) — which is Diagnostic intent
arriving through the existing Mechanism and Constraint classes. Adding a Diagnostic class
would relabel work v1 already does.

### Affiliation — DROP (falsified 2/2)

Both entity reps produced role, history, and connection sub-queries:

- rep1: "where did Belmonte work before Thrace & Wexford", "who did Belmonte train under
  earlier in her career", "what is Belmonte's reputation in dispute resolution"
- rep3: "what is Belmonte's role at the Calloway Trust", "who is Rutger Fennimore and what
  was his relationship to Belmonte's career"

Definitional and Evidence between them already reach affiliation intent for a person
subject. rep3 additionally caught a genuine entity gap v1 was not expected to find — "what
kind of organization is Thrace & Wexford", noting the page "calls it 'the firm' throughout"
while describing details that "point in different directions and are never reconciled."

### Locality — DROP, provisional (falsified 1/1, insufficient reps)

The single availability rep produced:

- "is Corriewave available in my area" — classified NOT EXTRACTABLE, criterion 5
- "why does Corriewave open some markets before others"

One rep is not the majority-of-three the plan requires. The verdict is provisional and the
two missing reps should be run before Locality is deleted from the design.

### Verification — KEEP (prediction held 3/3)

No news rep asked who reported the event, whether it was confirmed, or what the source was.
All three instead converged on causation — "what caused the warehouse fire" — reported ABSENT
in 3/3. Causation is not verification: the gap "no source is attributed to any claim in this
report" went unfound by every rep.

### Recency — KEEP, narrowed (prediction partially held 3/3)

Reps did produce forward-looking timeline sub-queries — "how long will the northern spur line
be closed", "is there a timeline for reopening the northern spur or rebuilding Warehouse 6" —
so v1 is not blind to time.

What no rep produced is the *current-state* question the gate is written for: whether the
page's own claims still hold when read later. The fixture is dense with unresolved temporal
deixis ("last Tuesday", "has since been rerouted", "no timeline exists yet") and no rep
flagged any of it as a staleness problem.

Recency survives, but only in the narrowed form already in the spec — checkable current
state — not as "content about time".

## Consequence for the design

Three of five proposed classes are already handled by the seven-class taxonomy. This is the
fourth time in this project that v1 has proven more capable than predicted.

The twelve-class design becomes a **nine-class** design: the existing seven plus Verification
and Recency. That is a materially smaller change than the spec describes, and it invalidates
several spec artifacts that assume five new classes — the composition-example table, the
regional-recall row that opens Locality and Diagnostic, the Locality fixture requirement, and
the dew-point must-stay-closed list.

This is a spec-level change, not a task-level one, and it is escalated rather than absorbed.
