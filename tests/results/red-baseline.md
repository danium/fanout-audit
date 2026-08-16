# RED baseline — v1 (seven classes) against the four new content types

11 reps completed across the four fixtures: news 3, support 3, availability 3, entity 2.
Rep counts are noted per class below, because the KEEP/DROP rule requires a majority of three.

## Evidence provenance — read this before trusting a verdict

The dispatch mechanism failed partway through this task (see ledger). A runner agent silently
dropped the write-to-file instruction from the prompts it passed on, so most reps returned
their audits as text rather than writing artifacts. Consequence:

| Fixture | Reps | Committed artifacts in `tests/runs/` |
|---|---|---|
| news | 3 | **none** — quotes below are from returned text only |
| support | 3 | **none** — quotes below are from returned text only |
| entity | 2 | **none** — quotes below are from returned text only |
| availability | 3 | `availability-rep1.md`, `availability-rep2.md` (rep3 text only) |

Quotes below are verbatim from what those reps returned, but **seven of the eleven runs left no
reproducible artifact**. Anyone re-checking these verdicts should re-run the affected fixtures
rather than treat this file as the primary record. The availability reps are the only ones with
artifacts, and they are also the ones whose verdict is most clear-cut.

This weakens the record; it does not change the verdicts, which were unanimous within each
fixture. It is recorded rather than quietly omitted because a dropped class is a design
decision and the evidence behind it should be checkable.

## Verdicts

| Class | Prediction | Result | Reps | Verdict |
|---|---|---|---|---|
| Verification | no sub-query asks who reported it or whether it is confirmed | **prediction held** | 3/3 | **KEEP** |
| Recency | no sub-query asks whether the state still holds | **partially held** | 3/3 | **KEEP (narrowed)** |
| Diagnostic | no sub-query touches the error string | **falsified** | 3/3 | **DROP** |
| Affiliation | nothing on roles, history, or connections | **falsified** | 2/2 | **DROP** |
| Locality | nothing keyed to geography or jurisdiction | **falsified** | 3/3 | **DROP** |

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

### Affiliation — DROP (falsified 2/2) — **rule deviation, stated plainly**

The KEEP/DROP rule requires a majority of three reps. Affiliation was dropped on **two**.

An attempt to run a third failed as a baseline: it was dispatched against the already-modified
nine-class skill rather than v1, so it tests the wrong thing and is not counted. Its output is
in `tests/runs/entity-rep3.md` and is consistent with the DROP — it produced "what was
Belmonte's professional background before Thrace & Wexford" with no Affiliation class present —
but consistency after the change is not baseline evidence, and treating it as such would be
exactly the single-sample reasoning this rule exists to prevent.

So: **the Affiliation DROP rests on two unanimous baseline reps, one short of the rule.** It is
the weakest of the three drops. Re-running two v1 entity reps would settle it; the v1 skill is
recoverable from git history at commit `217a45e`.

Both reps that did run:

Both entity reps produced role, history, and connection sub-queries:

- rep1: "where did Belmonte work before Thrace & Wexford", "who did Belmonte train under
  earlier in her career", "what is Belmonte's reputation in dispute resolution"
- rep3: "what is Belmonte's role at the Calloway Trust", "who is Rutger Fennimore and what
  was his relationship to Belmonte's career"

Definitional and Evidence between them already reach affiliation intent for a person
subject. rep3 additionally caught a genuine entity gap v1 was not expected to find — "what
kind of organization is Thrace & Wexford", noting the page "calls it 'the firm' throughout"
while describing details that "point in different directions and are never reconciled."

### Locality — DROP (falsified 3/3)

rep1 produced three more:

- "why isn't Corriewave available everywhere its satellites reach"
- "how do I find out if Corriewave is available at my address"
- "why do some Corriewave markets open before others"

rep3 produced:

- "is Corriewave available in my area" — classified NOT EXTRACTABLE, criterion 5
- "why does Corriewave open some markets before others"

rep2 produced four geography-keyed sub-queries, more than the proposed Locality class would
have prescribed:

- "is Corriewave available everywhere its satellites reach"
- "how do you check whether Corriewave is available at your address"
- "should a home buyer assume a property advertised with Corriewave will have service"
- "why do some Corriewave markets open for orders before others"

The third of these is the interesting one. It is a real-world availability question keyed to
a specific buyer situation, and nothing in the proposed Locality gate ("does the subject's
availability vary by geography or jurisdiction?") would have produced it. v1 reached it
through Qualification and Constraint. Adding Locality would not have improved this audit and
might have narrowed it.

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
