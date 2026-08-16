# Design: gate-keyed sub-query taxonomy

Date: 2026-08-16
Status: approved, not yet implemented

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
- No genre labels anywhere in the file

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

## Gate polarity: gates ask about the subject, never about the page

**Every gate question is about the subject of the head query. No gate inspects what the
content happens to contain.**

This is the single most important constraint in the design, because getting it backwards
silently destroys the skill's most valuable finding. Worked example: the reconciliation docs
page contains no pricing anywhere. Under a content-inspecting Constraint gate — "does it have
a price?" — the gate closes, no pricing sub-query is generated, and **pricing can never be
reported ABSENT**. That absence was the entire result of the case 6 audit.

Generalised: a gap is by definition something the content does not contain, so any gate that
closes on content absence is blind to exactly the gaps worth reporting.

The rule that follows, and it belongs in the reference file verbatim:

> Never close a gate because the content does not cover it. A gate closes only because of what
> the subject *is*. If a gate opens and the content says nothing, that is an ABSENT finding —
> which is the point of the audit.

## The twelve classes

Gate questions below are phrased against **the subject**, per the polarity rule above.

| Class | Gate question | Status |
|---|---|---|
| Definitional | — always open | existing |
| Mechanism | Would a reader ask how or why the subject works? | existing |
| Comparison | Does the subject sit in a category with alternatives a reader would weigh? | existing |
| Qualification | Is the subject something a reader could decide to adopt, buy, or follow? | existing |
| Constraint | Is the subject the kind of thing that has limits, requirements, or a price? | existing |
| Evidence | Does the subject involve an outcome claim a reader would want proof of? | existing |
| Procedural | Is the subject something a reader would execute or set up? | existing |
| Recency | Is the subject the kind of thing whose state changes over time? | **new** |
| Verification | Does the subject involve an event or claim a reader might doubt occurred? | **new** |
| Affiliation | Is the subject a named person, organisation, or place? | **new** |
| Locality | Does the subject's availability or applicability vary by geography or jurisdiction? | **new** |
| Diagnostic | Can the subject fail or go wrong in ways a reader would search for? | **new** |

### Sub-query forms for the five new classes

**Recency** — latest X, X as of [period], is X still accurate, current status of X, has X
changed. Distinct from Verification: Recency asks whether the state still holds, not whether
the event occurred.

**Verification** — is X confirmed, who reported X, source for X, did X actually happen, is X
official.

**Affiliation** — who is X, what is X known for, who does X work with or for, X background,
what has X done before. Definitional covers "who is X" at the identity level; Affiliation
covers connections, roles, and history.

**Locality** — is X available in [place], X in [city], does X apply under [jurisdiction],
where can I get X.

**Diagnostic** — X not working, why is X doing Y, [error string], how to fix X, X keeps
failing. Distinct from Mechanism: Mechanism explains correct operation, Diagnostic addresses
failure states. Distinct from Procedural: Procedural is executing an intended task,
Diagnostic is recovering from an unintended one.

### Composition examples

No genre is named anywhere; these are what the gates produce.

| Content | Gates that open |
|---|---|
| News story | Definitional, Recency, Verification, Affiliation |
| Security advisory | Definitional, Recency, Constraint, Procedural |
| Outage post-mortem | Definitional, Recency, Mechanism, Evidence |
| Support article | Definitional, Diagnostic, Procedural, Mechanism |
| Person or company page | Definitional, Affiliation, Evidence |
| Conceptual explainer | Definitional, Mechanism, Comparison |
| Product docs page | Definitional, Mechanism, Comparison, Qualification, Constraint, Procedural |

## Changes by file

### `SKILL.md` — step 3 only

Current text says six to twelve sub-queries from the seven intent classes, skip classes that
do not fit. Becomes: six to twelve from the twelve classes, where a class contributes
sub-queries only if its gate question opens. The no-padding sentence is retained verbatim —
it did measurable work in testing.

**A class may yield more than one sub-query.** Gates are not counted 1:1 against the six to
twelve range — Constraint alone can produce a pricing, a compatibility, and a requirements
sub-query. Four open gates can legitimately produce eight sub-queries. The six to twelve
range governs sub-queries; the guardrails below govern gates.

Step 3 gains two symmetric guardrails, both routing into existing machinery rather than
introducing new behaviour:

- **Eleven or twelve gates open** → re-check step 2. Nearly every gate opening usually means
  the page targets more than one head query, which is already a stop-and-ask.
- **Three or fewer gates open**, counting the always-open Definitional → say so rather than
  pad. Either the content is too thin to audit meaningfully or the inferred head query is too
  narrow. Without this, the floor of six sub-queries silently forces padding.

No other section of SKILL.md changes. The body does not enumerate the classes; it points at
the reference, and that stays true.

### `references/subquery-taxonomy.md` — bulk of the work

- "Applies when" column becomes the gate question
- Five new class sections, each with the gate, two or three example sub-queries, and a worked
  example of the gate being closed
- The two existing worked decompositions gain a line recording which gates opened
- Two new worked decompositions covering content that trips the new gates
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
3. **Open gates → generate six to twelve sub-queries from open classes only**
4. Classify each against the standalone test: covered, not extractable, absent
5. Report

The output contract, disclosure header, sourced-claims rule, and three-state triage are
untouched.

## Risks

**Gate sprawl destroying the skip property.** Adding five classes to a taxonomy whose
demonstrated virtue is skipping ill-fitting classes is precisely the change that could break
it. Mitigated by the regression tests below, which are the highest-priority tests in this
plan.

**Abstract gates checked sloppily.** A gate question is more abstract than a genre label, so
a careless check produces a wrong class set silently. Mitigated by writing each gate as a
concrete yes/no question and giving each class a worked example of the gate *closed*, not
only open.

**Fabrication pressure from the new classes.** Recency and Verification invite invented dates
and invented sources — a stronger pull than pricing, because a plausible date reads as
harmless. The sourced-claims rule covers this in principle and generalised unprompted in
testing (it refused to supply a temperature unit the source omitted), but it needs a
dedicated probe.

## Testing

The Iron Law applies to edits. RED here means running against the **current seven-class
skill**, not without a skill. The baseline is v1.

**Reps are not optional where a decision rests on them.** Single samples lie — the same
guidance that governs wording micro-tests. Any run whose outcome decides a design question
gets 3 or more reps; smoke checks get one.

**RED — 3 content types × 3 reps = 9 runs, current skill.** A news story, a
support/troubleshooting page, a person-or-organisation page. Falsifiable predictions: news
pads with Qualification or Constraint, or yields too few sub-queries with no recency
coverage; support produces no sub-query touching an error string; the entity page produces
nothing on affiliation or history.

**A class is dropped only if v1 handles it adequately in the majority of its reps.** Three
reps, not one — a single lucky run is exactly the evidence that would wrongly delete a class,
and v1 has already surprised us twice by being more capable than predicted.

**GREEN — 3 runs, new taxonomy.** One rep per content type, as a smoke check that the right
gates open and counts stay in range. Escalate any type that looks marginal to 3 reps.

**Regression — 2 fixtures × 3 reps = 6 runs.** The dew-point explainer and the reconciliation
docs page against the twelve-class version. v1 behaviour on both is already recorded, so the
comparison is direct. This is the highest-priority test in the plan and the one most exposed
to variance, because gate sprawl is a tendency rather than a switch.

**A Locality or Diagnostic sub-query appearing in any dew-point rep means the gates are not
binding and the design does not ship.** One bad rep out of three is a failure here, not noise
— a gate that holds two times in three is not a gate.

**Polarity regression — included in the above.** At least one reconciliation rep must still
report pricing as ABSENT. If it does not, the gates have been read as content-inspecting and
the polarity rule has failed, regardless of how the rest of the report looks.

**Fabrication probe — 3 runs.** A news page under paste-ready pressure ("give me the copy, not
homework"). Checks that ABSENT items for Recency and Verification describe what a passage
needs rather than supplying a date or a source. Three reps because discipline under pressure
is precisely where variance shows up.

**21 runs total.** If that needs trimming, cut GREEN to a single type and keep the regression
pair at full reps — never the reverse.

### Test hygiene

New worked examples in the taxonomy must be written from **different content than the test
fixtures**. In the original build both were drawn from the same material, which handed the
model its answers and invalidated the sub-query-generation test until a fresh fixture was
added.

## Known open issues, deliberately out of scope

Recorded so they are not lost. None block this spec.

**Recency's gate may open near-universally.** "Is the subject the kind of thing whose state
changes over time?" is arguably true of most products, most docs, and most technical writing.
If it opens almost always it becomes a second always-on class and dilutes the audit — the
padding failure returning through the front door. Watch the regression runs for it; tighten
the gate in a follow-up if it shows.

**Wrong-but-unambiguous head query is unguarded.** Step 2 stops when content targets *two*
head queries. It has no guard for confidently inferring *one* head query incorrectly, which
is quieter and worse, since no ambiguity signal fires and every downstream gap is wrong.

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

- All twelve gates phrased against the subject, with the polarity rule stated verbatim in the
  reference file
- All twelve gates documented with a worked open and closed example, where the closed example
  is closed by the nature of the subject and never by content absence
- RED failures recorded verbatim for each new class across 3 reps, or the class dropped on a
  majority of reps
- GREEN runs produce correctly gated sub-queries on all three new content types
- **Every** dew-point regression rep is free of sub-queries from newly added classes — one
  failure out of three is a failure
- At least one reconciliation regression rep still reports pricing as ABSENT, proving the
  gates were read as subject-keyed
- No fabrication probe rep supplies an invented date or source
