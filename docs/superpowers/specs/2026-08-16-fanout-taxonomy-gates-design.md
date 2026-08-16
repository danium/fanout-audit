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

## The twelve classes

| Class | Gate question | Status |
|---|---|---|
| Definitional | — always open | existing |
| Mechanism | Does it explain how or why something works? | existing |
| Comparison | Are there alternatives a reader would weigh against this? | existing |
| Qualification | Could a reader decide to use, buy, or follow this? | existing |
| Constraint | Does it have limits, requirements, incompatibilities, or a price? | existing |
| Evidence | Does it claim an outcome that invites "does that actually work?" | existing |
| Procedural | Is there something the reader would execute? | existing |
| Recency | Would this be stale or wrong if read a year from now? | **new** |
| Verification | Does it assert something happened that a reader might doubt occurred? | **new** |
| Affiliation | Is it about a named person, organisation, or place? | **new** |
| Locality | Does applicability depend on geography or jurisdiction? | **new** |
| Diagnostic | Does it address something going wrong? | **new** |

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

**RED — 3 runs, current skill.** A news story, a support/troubleshooting page, a
person-or-organisation page. Falsifiable predictions: news pads with Qualification or
Constraint, or yields too few sub-queries with no recency coverage; support produces no
sub-query touching an error string; the entity page produces nothing on affiliation or
history. **If v1 handles any of these adequately, that class is dropped from the design.**

**GREEN — 3 runs, new taxonomy.** Same three inputs. Verify the correct gates open, the
sub-queries are well-fitted, and counts stay in range.

**Regression — 2 runs.** The dew-point explainer and the reconciliation docs page against
the twelve-class version. v1 behaviour on both is already recorded, so the comparison is
direct. **A Locality or Diagnostic sub-query appearing in the dew-point audit means the
gates are not binding and the design does not ship.**

**Fabrication probe — 1 run.** A news page under paste-ready pressure ("give me the copy, not
homework"). Checks that ABSENT items for Recency and Verification describe what a passage
needs rather than supplying a date or a source.

Nine runs total.

### Test hygiene

New worked examples in the taxonomy must be written from **different content than the test
fixtures**. In the original build both were drawn from the same material, which handed the
model its answers and invalidated the sub-query-generation test until a fresh fixture was
added.

## Done when

- All twelve gates documented with a worked open and closed example
- RED failures recorded verbatim for each new class, or the class dropped
- GREEN runs produce correctly gated sub-queries on all three new content types
- Both regression runs match v1 behaviour, with no sub-queries from newly added classes
- The fabrication probe produces no invented date or source
