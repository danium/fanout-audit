# Design: gate-keyed sub-query taxonomy

Date: 2026-08-16
Status: **implemented as nine classes, not twelve.** See the revision note below.

## Revision note — what RED changed

This spec proposed five new classes. The RED baseline falsified three of them: the existing
seven-class taxonomy already produced the sub-queries Diagnostic, Affiliation, and Locality
were meant to add.

| Proposed class | Result | Reps |
|---|---|---|
| Diagnostic | **dropped** — every support rep named the error string unprompted | 3/3 falsified |
| Affiliation | **dropped** — both entity reps produced role and history sub-queries | 2/2 falsified |
| Locality | **dropped** — all three availability reps produced geography-keyed sub-queries | 3/3 falsified |
| Recency | kept, in the narrowed current-state form | 3/3 held |
| Verification | kept | 3/3 held |

Evidence in `tests/results/red-baseline.md`. The sections below are amended in place where the
count matters — class table, guardrail thresholds, expected gate sets, and done-when. Sections
describing the *approach* (gate polarity, composition-not-genre, the rejected alternatives) are
unchanged, because RED did not challenge them.

The design rationale survives the shrinkage: gates keyed to the subject still cover any genre
without enumerating genres. There are simply fewer classes to gate than predicted, because the
seven existing ones reach further than the spec assumed.

## Problem

The seven-class sub-query taxonomy in `references/subquery-taxonomy.md` is product- and
explainer-shaped. Four of the seven classes presuppose content the reader could adopt:

| Class | Presupposes |
|---|---|
| Qualification | a thing you would adopt, with a fit decision |
| Constraint | a boundary, limits, or a price |
| Evidence | a claim inviting skepticism |
| Procedural | something you would execute |

News content satisfies almost none of that. There is no fit decision, no price, nothing to
execute. Definitional and Mechanism partly survive; the other five are either skipped —
leaving the audit with too few sub-queries — or padded, which is the failure the file
already warns against.

The sub-queries news actually fans out to have a different shape: what happened, when, is it
confirmed, who is affected, what happens next. Two of those have no home in the current
seven: **recency** and **verification**.

News is not the only gap. Support content fans out to error strings and symptoms. Content
about a named person or organisation fans out to affiliation and history. Geographically
bound content fans out to jurisdiction and availability.

## Goal

Cover any content genre without enumerating genres, while preserving the property that made
the current file work in testing: it skips classes that do not fit instead of padding to
reach a count.

## Non-goals

- No change to Module B (entity consistency)
- No change to the output contract, the disclosure header, the three-state triage, or the
  sourced-claims rule
- No machine-readable schema. The taxonomy stays a model-facing markdown reference; a tool
  built on this skill invokes the skill rather than parsing the taxonomy
- No genre labels anywhere in the file, including in examples

## Approach: gate questions, not genres

Rejected alternatives:

**Genre-keyed with a fallback** (~8 named genres, each with a class list). Easier to read and
closer to how authors describe their own content, but an outage post-mortem is arguably news,
arguably explainer, arguably support, and picking wrong produces confidently wrong gaps — the
same failure the skill already spends a stop-condition on at step 2. Every unlisted genre
lands in the fallback, so "universal" silently means "the eight we enumerated".

**Hybrid core plus genre supplements.** Inherits the genre-picking ambiguity for the
supplements while adding a second concept to hold.

**Chosen: each class carries a hard yes/no gate question.** A class contributes sub-queries
only if its gate opens. Genres compose out of gates, so unlisted content types are handled
rather than enumerated. This also makes explicit a judgment the skill already asks for
implicitly — the current file says "skip classes that do not fit", which is a fit test
written softly.

When the properties were made concrete they mapped 1:1 onto classes, so there is no
two-level structure. The gate question replaces the existing soft "Applies when" column.

## Gate polarity

Two clauses, and both are load-bearing. The first prevents the audit going blind to gaps;
the second prevents it going blind to claims.

> **1. A gate opens on what the subject is, and on what the content asserts.**
> **2. No gate ever closes because the content does not answer it.**

Clause 2 exists because getting it backwards silently destroys the skill's most valuable
finding. Worked example: the reconciliation docs page contains no pricing anywhere. Under a
gate reading "does the page state a price?" the gate closes, no pricing sub-query is
generated, and **pricing can never be reported ABSENT**. That absence was the entire result of
the case 6 audit. Generalised: a gap is by definition something the content does not contain,
so any gate that closes on content absence is blind to exactly the gaps worth reporting.

Clause 1 exists because two classes cannot be keyed to the subject alone. **Evidence** and
**Verification** are properties of what is being claimed, and a claim can be introduced by the
page about an otherwise neutral subject. A physics explainer has no inherent efficacy claim,
but if it asserts that a particular intervention works, a reader wants proof of that — and a
purely subject-keyed Evidence gate would never open, making the gap unreachable.

The distinction that keeps both clauses consistent: **a gate may inspect what the content
claims; it may never inspect whether the content substantiates the claim.** Observing "this
page asserts an outcome" is an applicability fact. Observing "this page does not prove it" is
the finding, and findings belong in the report, not in the gate.

Wording for the reference file, verbatim:

> Gates open on what the subject is and on what the page claims. **Never close a gate because
> the content does not cover it.** If a gate opens and the content says nothing, that is an
> ABSENT finding — which is the point of the audit.

## The twelve classes

| Class | Gate question | Status |
|---|---|---|
| Definitional | Would a reader plausibly arrive without already knowing what the subject is? | existing |
| Mechanism | Would a reader ask how or why the subject works? | existing |
| Comparison | Does the subject sit in a category with alternatives a reader would weigh? | existing |
| Qualification | Is the subject something a reader could decide to adopt, buy, or follow? | existing |
| Constraint | Is the subject the kind of thing that has limits, requirements, or a price? | existing |
| Evidence | Does the subject, **or a claim the page makes about it**, invite "does that actually work?" | existing |
| Procedural | Is the subject something a reader would execute or set up? | existing |
| Recency | Would a reader need to check the subject's *current* state — a version, status, price, or ongoing situation? | **new** |
| Verification | Does the subject, **or an assertion the page makes**, involve an event a reader might doubt occurred? | **new** |
| ~~Affiliation~~ | ~~Is the subject a named person, organisation, or place?~~ | **dropped — RED** |
| ~~Locality~~ | ~~Does the subject's availability or applicability vary by geography or jurisdiction?~~ | **dropped — RED** |
| ~~Diagnostic~~ | ~~Can the subject itself fail or go wrong in ways a reader would search for?~~ | **dropped — RED** |

Three gates were tightened during review and the reasons matter for implementation:

**Definitional is no longer always-open.** The current taxonomy explicitly skips it for
reference tables, changelogs, and API parameter lists, where the reader arrives already
knowing the term. An always-open rule would manufacture a definition gap for those subjects
and would also make the "worked open and closed example for all twelve" requirement
impossible to satisfy.

**Recency asks about checkable current state**, not "changes over time". The looser phrasing
would open for nearly any product, doc, or technical page, making it a second always-on class
and reintroducing padding through the front door.

**Diagnostic asks whether the subject itself can fail.** A page may *explain* a failure —
condensation, an outage — without the subject being a thing that fails. Explaining a failure
mechanism is Mechanism.

### Sub-query forms for the five new classes

**Recency** — latest X, X as of [period], is X still accurate, current status of X, has X
changed. Distinct from Verification: Recency asks whether the state still holds, not whether
the event occurred.

**Verification** — is X confirmed, who reported X, source for X, did X actually happen, is X
official.

**Affiliation** — who is X, what is X known for, who does X work with or for, X background,
what has X done before. Definitional covers "who is X" at the identity level; Affiliation
covers connections, roles, and history. **A named product is not an organisation** —
Definitional and Qualification cover products.

**Locality** — is X available in [place], X in [city], does X apply under [jurisdiction],
where can I get X.

**Diagnostic** — X not working, why is X doing Y, [error string], how to fix X, X keeps
failing. Distinct from Mechanism: Mechanism explains correct operation, Diagnostic addresses
failure states. Distinct from Procedural: Procedural is executing an intended task,
Diagnostic is recovering from an unintended one.

### Composition examples

**Gates are determined by the subject, never by the kind of page.** The pairs below are the
point of the design: same broad kind of content, different subjects, different gate sets.

| Subject of the head query | Gates that open |
|---|---|
| what the expand-and-contract migration pattern is | Definitional, Mechanism, Comparison, Procedural |
| whether a named vendor's tracing agent supports Windows containers | Definitional, Constraint, Qualification |
| a chemical plant fire that happened last Tuesday | Definitional, Verification, Recency, Affiliation |
| a regional product recall announced last Tuesday | Definitional, Verification, Recency, Affiliation, Locality, Constraint, Diagnostic |
| who the CFO of a named company is | Definitional, Affiliation, Recency |
| why relative humidity misleads and dew point does not | Definitional, Mechanism, Comparison |
| how to fix a specific build-tool error string | Definitional, Diagnostic, Procedural, Mechanism |
| whether a payments method is available to merchants in a given country | Definitional, Locality, Constraint, Qualification, Recency |

Rows three and four are both "news". They differ by seven gates versus four, because the
subjects differ — which is precisely why no genre label appears in the reference file.

## Changes by file

### `SKILL.md` — step 3 and one stop-condition

Current text says six to twelve sub-queries from the seven intent classes, skip classes that
do not fit. Becomes: six to twelve from the twelve classes, where a class contributes
sub-queries only if its gate question opens. The no-padding sentence is retained verbatim —
it did measurable work in testing.

**A class may yield more than one sub-query.** Gates are not counted 1:1 against the six to
twelve range — Constraint alone can produce a pricing, a compatibility, and a requirements
sub-query. Four open gates can legitimately produce eight sub-queries. The six to twelve range
governs sub-queries; the guardrails below govern gates.

Step 3 gains two guardrails. Both are **pre-report stops**, not report content — the output
contract permits three finding sections and nothing else, and neither guardrail gets a slot in
it. Both mirror the existing step 2 stop-and-ask.

- **Eight or nine gates open** → do not report. Nearly every gate opening usually means the
  subject is broader than one head query. Return to step 2 and ask. (Scaled from eleven-or-twelve
  when the class count dropped to nine.)
- **Three or fewer gates open** → do not report. State which gates opened and ask whether the
  head query should be broader. **The cause is a head query that is too narrow, not thin
  content** — gates do not inspect whether the content is thin, and phrasing this as a content
  judgment would contradict the polarity rule.

No other section of SKILL.md changes. The body does not enumerate the classes; it points at
the reference, and that stays true.

### `references/subquery-taxonomy.md` — bulk of the work

- "Applies when" column becomes the gate question
- The polarity rule stated verbatim near the top
- Five new class sections, each with the gate, two or three example sub-queries, and a worked
  example of the gate **closed** — closed by the nature of the subject, never by content
  absence
- Definitional's existing "skip when" guidance is preserved and becomes its gate
- The two existing worked decompositions gain a line recording which gates opened
- Two new worked decompositions covering subjects that trip the new gates
- Approximately 990 → 1,700 words

### `references/passage-criteria.md` — one extension

Recency content fails extractability in a way the current criteria nearly catch. "Recently",
"currently", "last year", "at time of writing" and "as of now" are unresolved references in
time, exactly parallel to "as mentioned above" being unresolved in space.

This is **criterion 2**, not a sixth criterion. Add temporal deixis to criterion 2's fail
examples and to its list of near-certain failure openers.

### `README.md`

Update the class count and add one line noting coverage is genre-agnostic.

### `references/entity-drift.md`

Unchanged.

## Data flow

Unchanged except where marked:

1. Read the content
2. Infer the head query; stop and ask if more than one
3. **Open gates → stop and ask if 3 or fewer, or 11 or more → generate six to twelve
   sub-queries from open classes only**
4. Classify each against the standalone test: covered, not extractable, absent
5. Report

The output contract, disclosure header, sourced-claims rule, and three-state triage are
untouched.

## Risks

**Gate sprawl destroying the skip property.** Adding five classes to a taxonomy whose
demonstrated virtue is skipping ill-fitting classes is precisely the change that could break
it. Mitigated by the regression tests below, which are the highest-priority tests in this plan.

**Abstract gates checked sloppily.** A gate question is more abstract than a genre label, so a
careless check produces a wrong class set silently. Mitigated by writing each gate as a
concrete yes/no question and giving each class a worked example of the gate *closed*.

**Class overlap.** Diagnostic against Mechanism, Verification against Evidence, and Affiliation
against Definitional all have fuzzy boundaries. Each carries a distinguishing note above, but
overlap is a live source of variance and is the reason expected gate sets are stated per
regression fixture rather than left to judgment.

**Fabrication pressure from the new classes.** Recency and Verification invite invented dates
and invented sources — a stronger pull than pricing, because a plausible date reads as
harmless. The sourced-claims rule covers this in principle and generalised unprompted in
testing (it refused to supply a temperature unit the source omitted), but it needs a dedicated
probe.

## Testing

The Iron Law applies to edits. RED here means running against the **current seven-class
skill**, not without a skill. The baseline is v1.

**Reps are not optional where a decision rests on them.** Single samples lie. Any run whose
outcome decides a design question gets 3 or more reps; smoke checks get one.

### Fixtures

Four RED content types, one per new class cluster. **Locality gets its own fixture** — none of
the other three exercises it, so without this the requirement to record failures for each new
class is unsatisfiable and Locality would ship untested.

| Fixture | Subject | New classes exercised |
|---|---|---|
| News story | a dated event with named parties | Recency, Verification, Affiliation |
| Support page | a specific error string and its fix | Diagnostic |
| Person or organisation page | a named individual and their roles | Affiliation |
| Availability page | a service whose access varies by country or jurisdiction | Locality, Recency |

### Phases

**RED — 4 content types × 3 reps = 12 runs, current skill.** Falsifiable predictions: news pads
with Qualification or Constraint, or yields too few sub-queries with no recency coverage;
support produces no sub-query touching an error string; the entity page produces nothing on
affiliation or history; the availability page produces nothing keyed to geography or
jurisdiction.

**A class is dropped only if v1 handles it adequately in the majority of its reps.** Three reps,
not one — a single lucky run is exactly the evidence that would wrongly delete a class, and v1
has already surprised us twice by being more capable than predicted.

**GREEN — 4 runs, new taxonomy.** One rep per content type, as a smoke check that the right
gates open and counts stay in range. Escalate any type that looks marginal to 3 reps.

**Regression — 2 fixtures × 3 reps = 6 runs.** The dew-point explainer and the reconciliation
docs page against the twelve-class version. v1 behaviour on both is already recorded, so the
comparison is direct. This is the highest-priority test in the plan and the one most exposed to
variance, because gate sprawl is a tendency rather than a switch.

**Fabrication probe — 3 runs.** A news page under paste-ready pressure ("give me the copy, not
homework"). Checks that ABSENT items for Recency and Verification describe what a passage needs
rather than supplying a date or a source. Three reps because discipline under pressure is
precisely where variance shows up.

**25 runs total.** If that needs trimming, cut GREEN to a single type and keep the regression
pair at full reps — never the reverse.

### Expected gate sets for the regression fixtures

Stated explicitly so the regression is decidable rather than a judgment call. Gates not listed
in either column may go either way without failing the run.

**Dew-point explainer** — subject: why relative humidity misleads and dew point does not.

- **Must open:** Definitional, Mechanism, Comparison, **Procedural**
- **Must stay closed:** Recency, Verification

Procedural is in the must-open list as a pre-flight ruling, not an afterthought. Without it the
expected set is exactly three gates, which trips the low-gate guardrail and produces **no
report at all** — leaving the must-stay-closed check with nothing to check, while looking like
a guardrail working correctly. The justification is on file: v1's recorded audit of this
fixture generated "how do you lower the indoor dew point" as an ABSENT sub-query, so a reader
does act on this subject.

**A sub-query from either new class in any dew-point rep means the gates are not binding and
the design does not ship.** One failure out of three is a failure — a gate that holds two times
in three is not a gate.

**Reconciliation docs page** — subject: reconciliation software for multi-processor finance
teams.

- **Must open:** Constraint
- **Must stay closed:** Verification

Recency may legitimately open here — connector currency is a real sub-query for this subject —
and this is the fixture that will reveal whether Recency over-opens in practice.

**Polarity regression — the decisive check.** **Every** reconciliation rep must report pricing
as ABSENT. Not a majority: the polarity rule is the core of this design, and allowing it to
fail twice in three runs while treating one dew-point failure as fatal is indefensible. If any
rep fails to surface pricing, the gates have been read as content-inspecting and the design
does not ship regardless of how the rest of the report looks.

### Test hygiene

New worked examples in the taxonomy must be written from **different content than the test
fixtures**. In the original build both were drawn from the same material, which handed the
model its answers and invalidated the sub-query-generation test until a fresh fixture was
added.

## Known open issues, deliberately out of scope

Recorded so they are not lost. None block this spec.

**Wrong-but-unambiguous head query is unguarded.** Step 2 stops when content targets *two* head
queries. It has no guard for confidently inferring *one* head query incorrectly, which is
quieter and worse, since no ambiguity signal fires and every downstream gap is wrong.

**COVERED is never verified.** All discipline points at the negative findings. Nothing tests
whether covered items are correctly covered, and the report format shows only the passage's
first line, which structurally hides whether the whole passage was tested.

**Fetch is undefined rather than disabled.** The skill says it does not fetch and to ask the
user to paste, but agents running it often have a fetch tool and nothing tells them not to use
it. Behaviour on a URL is therefore harness-dependent and untested.

**Caveat decay on repeat use.** A user on their twentieth report has stopped reading the
disclosure header. Nothing addresses this.

**The spec ships to users.** The documented install clones the repo into the skills directory,
so `docs/superpowers/specs/` lands in every user's skills folder.

## Done when

- All twelve gates phrased per the polarity rule, with both clauses stated verbatim in the
  reference file
- All twelve gates documented with a worked open and closed example, where the closed example
  is closed by the nature of the subject and never by content absence — including Definitional,
  which is now conditional
- RED failures recorded verbatim for each proposed class across 3 reps, or the class dropped on
  a majority of reps — **done: three of five dropped**
- **Every** dew-point regression rep is free of sub-queries from Recency and Verification
- **Every** reconciliation regression rep reports pricing as ABSENT
- No fabrication probe rep supplies an invented date or source
