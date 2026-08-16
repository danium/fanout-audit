# Gate-Keyed Sub-Query Taxonomy Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the seven product-shaped sub-query classes with twelve classes, each gated by a yes/no question, so the skill covers any content genre without enumerating genres.

**Architecture:** The taxonomy stays a model-facing markdown reference — no schema, no code. Each class gains a gate question; a class contributes sub-queries only if its gate opens. Gates key on the subject of the head query and on what the page claims, never on whether the page contains an answer. Genres compose out of gates rather than being listed.

**Tech Stack:** Markdown only. There is no build, no test runner, and no dependency. **The test harness is dispatched subagents** — every verification step below is an agent run with an exact prompt and an exact pass/fail criterion. Runs are dispatched with the Agent tool using `subagent_type: general-purpose`.

**Spec:** `docs/superpowers/specs/2026-08-16-fanout-taxonomy-gates-design.md`

## Global Constraints

Every task's requirements implicitly include this section. Values are verbatim from the spec.

- **Polarity clause 1:** A gate opens on what the subject is, and on what the content asserts.
- **Polarity clause 2:** No gate ever closes because the content does not answer it.
- **Polarity rule, verbatim, must appear in `references/subquery-taxonomy.md`:** "Gates open on what the subject is and on what the page claims. **Never close a gate because the content does not cover it.** If a gate opens and the content says nothing, that is an ABSENT finding — which is the point of the audit."
- **No genre labels anywhere in the reference file, including in examples.** Rows in example tables are subjects, never content types.
- **Sub-query count stays 6–12.** Gates are not counted 1:1 against it; one class may yield several sub-queries.
- **Guardrails are pre-report stops, not report sections.** The output contract permits three finding sections and nothing else.
- **Unchanged, do not edit:** the output contract, the disclosure header, the three-state triage, the sourced-claims rule, `references/entity-drift.md`, and all of Module B.
- **Test hygiene:** worked examples written into the taxonomy must use different content from any test fixture. Reusing fixture content hands the model its answers and invalidates the generation test.
- **Reps:** any run whose outcome decides a design question gets 3+ reps. Smoke checks get 1.
- **Skill path for all test prompts:** `C:\Dev\Skills\Fanout-audit\fanout-audit\SKILL.md`
- **Fixture directory:** `C:\Dev\Skills\Fanout-audit\fanout-audit\tests\fixtures\`
- **Commit trailer for every commit:** `Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>`

### The twelve gates (canonical wording — copy exactly)

| Class | Gate question |
|---|---|
| Definitional | Would a reader plausibly arrive without already knowing what the subject is? |
| Mechanism | Would a reader ask how or why the subject works? |
| Comparison | Does the subject sit in a category with alternatives a reader would weigh? |
| Qualification | Is the subject something a reader could decide to adopt, buy, or follow? |
| Constraint | Is the subject the kind of thing that has limits, requirements, or a price? |
| Evidence | Does the subject, or a claim the page makes about it, invite "does that actually work?" |
| Procedural | Is the subject something a reader would execute or set up? |
| Recency | Would a reader need to check the subject's *current* state — a version, status, price, or ongoing situation? |
| Verification | Does the subject, or an assertion the page makes, involve an event a reader might doubt occurred? |
| Affiliation | Is the subject a named person, organisation, or place? |
| Locality | Does the subject's availability or applicability vary by geography or jurisdiction? |
| Diagnostic | Can the subject itself fail or go wrong in ways a reader would search for? |

---

## File Structure

| File | Responsibility | Change |
|---|---|---|
| `references/subquery-taxonomy.md` | The twelve classes, their gates, sub-query forms, worked decompositions | Rewrite (~990 → ~1,700 words) |
| `SKILL.md` | Step 3 wording and the two gate guardrails | Modify step 3 only |
| `references/passage-criteria.md` | Criterion 2 gains temporal deixis | Modify criterion 2 section |
| `README.md` | Class count, genre-agnostic line | Modify two lines |
| `tests/fixtures/*.md` | Test inputs | Create 6 new files |
| `tests/results/*.md` | Recorded run outcomes | Create as tasks run |
| `references/entity-drift.md` | Module B | **Do not touch** |

Fixtures and results live under `tests/` rather than in the scratchpad so the evidence ships with the repo and later edits can re-run against the same inputs.

---

## Task 1: RED baseline against v1

Establishes the failing test. The Iron Law applies to edits: v1 is the baseline, not "no skill". A class that v1 already handles well in a majority of reps gets dropped from the design.

**Files:**
- Create: `tests/fixtures/red-news.md`
- Create: `tests/fixtures/red-support.md`
- Create: `tests/fixtures/red-entity.md`
- Create: `tests/fixtures/red-availability.md`
- Create: `tests/results/red-baseline.md`

**Interfaces:**
- Consumes: nothing
- Produces: `tests/results/red-baseline.md` containing, for each of the five new classes, either verbatim evidence of v1 failing or a KEEP/DROP decision. Task 2 reads this to decide which of the five classes to write.

- [ ] **Step 1: Write the four RED fixtures**

Each fixture is realistic prose about a **fictional** subject (real subjects let the model answer from training data instead of the page). 500–900 words. No headings that name a gate class. Acceptance criteria per fixture:

`red-news.md` — a dated event with named parties. Must contain: a specific event, at least two named organisations, a date reference. Must **not** contain: any source attribution, any "as of" statement, any confirmation status. Targets Recency, Verification, Affiliation.

`red-support.md` — one specific error string and its resolution. Must contain: a verbatim error message in a code block, symptoms, a fix procedure. Must **not** contain: any statement of which versions are affected. Targets Diagnostic.

`red-entity.md` — a named individual and their roles. Must contain: a person's name, a current role, at least one past role. Must **not** contain: dates for any role, or any organisation description. Targets Affiliation.

`red-availability.md` — a service whose access varies by country. Must contain: a service description, a statement that availability differs by market, at least two named countries. Must **not** contain: the actual per-country list, or any regulatory basis. Targets Locality and Recency.

- [ ] **Step 2: Run the RED baseline — 12 runs**

Dispatch 12 subagents (4 fixtures × 3 reps). Reps are separate dispatches, not one agent asked three times. Exact prompt, substituting `<FIXTURE>`:

```
You have a skill available at C:\Dev\Skills\Fanout-audit\fanout-audit\SKILL.md.
Read it first and follow it exactly. It references files in
C:\Dev\Skills\Fanout-audit\fanout-audit\references\ — read those as the skill
directs. Do not invoke any other Skill.

The user says:

"Audit this page for AI search visibility:
C:\Dev\Skills\Fanout-audit\fanout-audit\tests\fixtures\<FIXTURE>"

Do exactly what the user asked, within what the skill permits. Your final reply
IS the deliverable the user reads.
```

- [ ] **Step 3: Verify the baseline fails as predicted**

For each fixture, check the three reps against its falsifiable prediction:

| Fixture | Predicted v1 failure | Class it justifies |
|---|---|---|
| `red-news.md` | pads with Qualification or Constraint, or yields <6 sub-queries; no sub-query asks whether the state still holds or who reported it | Recency, Verification |
| `red-support.md` | no sub-query contains or references the error string | Diagnostic |
| `red-entity.md` | no sub-query asks about roles, history, or connections | Affiliation |
| `red-availability.md` | no sub-query is keyed to a country, market, or jurisdiction | Locality |

Expected: the predicted failure appears in a **majority** of each fixture's three reps.

- [ ] **Step 4: Record results and decide KEEP/DROP per class**

Write `tests/results/red-baseline.md`. For each of the five new classes: the fixture, the three rep outcomes, verbatim quotes of the failure, and a KEEP or DROP decision.

**DROP any class v1 handled adequately in 2 or 3 of its reps.** This is a real branch, not a formality — v1 has already been more capable than predicted twice. A dropped class is removed from Task 2's scope and from the Task 5/6 criteria.

- [ ] **Step 5: Commit**

```bash
git add tests/fixtures tests/results
git commit -m "$(cat <<'EOF'
Add RED baseline fixtures and v1 results

Four fixtures targeting the five proposed new classes, three reps each
against the current seven-class skill. Records KEEP/DROP per class.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

## Task 2: Rewrite the taxonomy reference

The bulk of the work. Produces the file every later task tests.

**Files:**
- Modify: `references/subquery-taxonomy.md` (full rewrite)
- Create: `tests/results/task2-smoke.md`

**Interfaces:**
- Consumes: `tests/results/red-baseline.md` — only classes marked KEEP are written.
- Produces: the twelve gate questions in their canonical wording (see Global Constraints). Task 3 references the same wording in `SKILL.md`; Task 6 tests against it.

- [ ] **Step 1: Replace the header and add the polarity rule**

Keep the existing opening (what the file is, the "skip classes that do not fit" instruction, the simulated-decomposition note). Immediately after it, insert the polarity rule verbatim from Global Constraints, under a `## Gate polarity` heading, followed by:

```markdown
Worked example of why clause 2 matters: a docs page contains no pricing anywhere.
Under a gate reading "does the page state a price?" the gate closes, no pricing
sub-query is generated, and pricing can never be reported ABSENT. A gap is by
definition something the content does not contain, so a gate that closes on
content absence is blind to exactly the gaps worth reporting.

Clause 1 exists because Evidence and Verification are properties of what is being
claimed, and a claim can be introduced by the page about an otherwise neutral
subject. A gate may inspect what the content claims. A gate may never inspect
whether the content substantiates the claim — that is the finding, and findings
belong in the report.
```

- [ ] **Step 2: Replace the class table**

Replace the existing "Applies when" column with the gate question, using the canonical wording table from Global Constraints exactly. Twelve rows, minus any class dropped in Task 1.

- [ ] **Step 3: Write the twelve class sections**

Each class section has four parts, in this order: the gate question, two or three example sub-queries, a **gate-open** example naming a subject, and a **gate-closed** example naming a subject.

The closed example must be closed **by the nature of the subject**, never by content absence. Correct: "Diagnostic is closed for *the history of the metric system* — a system of units cannot fail." Incorrect: "Diagnostic is closed because the page does not discuss errors."

Definitional is now conditional. Its closed example uses the existing guidance already in the file — a reference table, a changelog, or an API parameter list, where the reader arrives already knowing the term.

New class sections to write (KEEP list from Task 1 only):

```markdown
## Recency

Gate: Would a reader need to check the subject's *current* state — a version,
status, price, or ongoing situation?

- latest X
- is X still accurate
- current status of X

Distinct from Verification: Recency asks whether the state still holds, not
whether the event occurred.

**Gate open:** a payment method's supported-country list. Countries are added and
removed, so a reader must check what is true now.

**Gate closed:** the relationship between dew point and relative humidity. The
physics does not change, so there is no current state to check. Note this is a
property of the subject — a page about it could still be edited tomorrow.
```

Write the remaining KEEP classes (Verification, Affiliation, Locality, Diagnostic) to the same four-part shape, using the sub-query forms and the distinguishing notes from the spec's "Sub-query forms for the five new classes" section. Include the spec's note that **a named product is not an organisation** in Affiliation.

- [ ] **Step 4: Replace the composition examples table**

Rows are subjects, never content types. Use the spec's table, which includes the decisive pair:

```markdown
| Subject of the head query | Gates that open |
|---|---|
| what the expand-and-contract migration pattern is | Definitional, Mechanism, Comparison, Procedural |
| a chemical plant fire that happened last Tuesday | Definitional, Verification, Recency, Affiliation |
| a regional product recall announced last Tuesday | Definitional, Verification, Recency, Affiliation, Locality, Constraint, Diagnostic |
| why relative humidity misleads and dew point does not | Definitional, Mechanism, Comparison |
| how to fix a specific build-tool error string | Definitional, Diagnostic, Procedural, Mechanism |
| whether a payments method is available to merchants in a given country | Definitional, Locality, Constraint, Qualification, Recency |
```

Followed by, verbatim:

```markdown
Rows two and three are both news. They differ by seven gates versus four, because
the subjects differ — which is why no genre label appears in this file.
```

- [ ] **Step 5: Update the two existing worked decompositions**

Add one line to each recording which gates opened. Do not change their sub-query lists.

- [ ] **Step 6: Structural self-check**

Verify by reading, not by agent:

- Every KEEP class has all four parts, including a gate-closed example
- No gate-closed example is justified by content absence
- No genre label appears anywhere, including table row labels
- No worked example uses content from any file in `tests/fixtures/`
- The polarity rule appears verbatim

- [ ] **Step 7: Smoke run**

Dispatch **one** agent using the Task 1 Step 2 prompt against `tests/fixtures/red-news.md`. Expected: Recency and Verification sub-queries now appear; the report still follows the output contract; no score. Record in `tests/results/task2-smoke.md`.

This is a smoke check, not the verification gate — that is Task 5.

- [ ] **Step 8: Commit**

```bash
git add references/subquery-taxonomy.md tests/results/task2-smoke.md
git commit -m "$(cat <<'EOF'
Rewrite sub-query taxonomy with gate questions

Twelve classes, each gated by a yes/no question keyed to the subject and
to what the page claims. Replaces the soft "Applies when" column. Adds
the polarity rule verbatim. No genre labels; composition examples are
subjects.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

## Task 3: SKILL.md step 3 and the two guardrails

**Files:**
- Modify: `SKILL.md` — step 3 section only
- Create: `tests/fixtures/guard-narrow.md`
- Create: `tests/fixtures/guard-broad.md`
- Create: `tests/results/task3-guardrails.md`

**Interfaces:**
- Consumes: the gate wording from Task 2.
- Produces: the two stop-conditions. Task 5 and Task 6 must not trigger them, so their thresholds are fixed here.

**Note on scope:** the spec defines both guardrails but specifies no fixture that triggers either. Steps 3–5 fill that gap. Without them, two new stop-conditions would ship with no test.

- [ ] **Step 1: Rewrite step 3 in SKILL.md**

Replace the body of `### 3. Generate sub-queries` with:

```markdown
Six to twelve, from the twelve intent classes in `references/subquery-taxonomy.md`.

Each class carries a gate question. **A class contributes sub-queries only if its
gate opens.** Gates open on what the subject is and on what the page claims; a gate
never closes because the content does not cover it. If a gate opens and the content
says nothing, that is an ABSENT finding.

Skip classes whose gate is closed. A pricing sub-query on a conceptual explainer is
noise. Six well-fitted sub-queries beat twelve padded ones — do not pad to reach a
count.

A class may yield more than one sub-query. Gates are not counted against the six to
twelve range; Constraint alone can produce a pricing, a compatibility, and a
requirements sub-query.

**Two stops before you report:**

- **Three or fewer gates open** → do not report. State which gates opened and ask
  whether the head query should be broader. The cause is a head query that is too
  narrow, not thin content.
- **Eleven or twelve gates open** → do not report. Return to step 2 and ask, because
  a subject that opens nearly every gate is usually more than one head query.

Both are stops, like step 2. Neither is a section in the report — the output
contract permits three finding sections and nothing else.
```

- [ ] **Step 2: Commit the SKILL.md change**

```bash
git add SKILL.md
git commit -m "$(cat <<'EOF'
Gate-key step 3 and add two pre-report stops

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

- [ ] **Step 3: Write the two guardrail fixtures**

`guard-narrow.md` — 200–300 words on a subject so specific that three or fewer gates open. Use a single historical fact with no mechanism, no alternatives, nothing to adopt, no failure mode, no geography, no current state. Example subject: the origin of a specific word's spelling.

`guard-broad.md` — 900+ words whose subject spans a product, its pricing, its regional availability, its failure modes, a named vendor organisation, and a dated incident. Written to open eleven or more gates.

- [ ] **Step 4: Run the guardrail tests — 6 runs**

Three reps per fixture, using the Task 1 Step 2 prompt.

Expected for `guard-narrow.md`: **no report is produced.** The agent states which gates opened and asks whether the head query should be broader.

Expected for `guard-broad.md`: **no report is produced.** The agent returns to step 2 and asks which head query to audit.

Failure in either direction — producing a report, or stopping on the wrong fixture — is a failure of the guardrail, not of the fixture.

- [ ] **Step 5: Record and commit**

Write `tests/results/task3-guardrails.md` with the six outcomes.

```bash
git add tests/fixtures/guard-narrow.md tests/fixtures/guard-broad.md tests/results/task3-guardrails.md
git commit -m "$(cat <<'EOF'
Add guardrail fixtures and stop-condition results

The spec defined both gate guardrails but specified no fixture that
triggers either. These cover both directions.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

## Task 4: Temporal deixis in criterion 2

**Files:**
- Modify: `references/passage-criteria.md` — criterion 2 section only
- Create: `tests/results/task4-temporal.md`

**Interfaces:**
- Consumes: nothing from earlier tasks.
- Produces: extended criterion 2 fail-opener list. Task 5's news run tests it.

- [ ] **Step 1: Add a temporal fail example to criterion 2**

Insert after the existing "Also fails" block in the criterion 2 section:

```markdown
### Also fails, in time rather than in space

> The rollout has been expanding steadily, and as of writing it now covers most of
> the region. Recent changes have made the process considerably faster than it was
> last year.

"As of writing", "recently", and "last year" are unresolved references exactly as
"as mentioned above" is. The passage cannot be dated by a reader who did not see
its publication context, so lifted out it asserts a state without saying when that
state held.

Fix by resolving the reference inside the passage: name the month and year, or give
the version, rather than pointing at the moment of writing.
```

- [ ] **Step 2: Extend the failure-opener list**

The criterion 2 section ends with a list of near-certain failure openers. Append these to it: *recently, currently, as of writing, at present, last year, these days, now, today, this week.*

- [ ] **Step 3: Verify**

Dispatch one agent against `tests/fixtures/red-news.md` using the Task 1 Step 2 prompt. Expected: at least one NOT EXTRACTABLE item cites criterion 2 for a temporal reference. Record in `tests/results/task4-temporal.md`.

- [ ] **Step 4: Commit**

```bash
git add references/passage-criteria.md tests/results/task4-temporal.md
git commit -m "$(cat <<'EOF'
Treat temporal deixis as a criterion 2 failure

"As of writing" and "recently" are unresolved references in time, exactly
as "as mentioned above" is in space. Extends criterion 2 rather than
adding a sixth criterion.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

## Task 5: GREEN verification

**Files:**
- Create: `tests/results/green.md`

**Interfaces:**
- Consumes: all four fixtures from Task 1, the rewritten taxonomy from Task 2, step 3 from Task 3.
- Produces: pass/fail per content type. Task 6 runs regardless of outcome; a Task 5 failure means returning to Task 2.

- [ ] **Step 1: Run GREEN — 4 runs**

One rep per Task 1 fixture, using the Task 1 Step 2 prompt.

- [ ] **Step 2: Check each run**

| Fixture | Must appear | Must not appear |
|---|---|---|
| `red-news.md` | Recency and Verification sub-queries; Affiliation | a Qualification or Procedural sub-query |
| `red-support.md` | a Diagnostic sub-query referencing the error string | — |
| `red-entity.md` | an Affiliation sub-query about roles or history | a Constraint sub-query |
| `red-availability.md` | a Locality sub-query naming a country or jurisdiction | — |

All four must additionally: open with the disclosure header verbatim, use the three-section output contract, contain no score or percentage, and produce 6–12 sub-queries.

Escalate any fixture that looks marginal to 3 reps before accepting it.

- [ ] **Step 3: Record and commit**

```bash
git add tests/results/green.md
git commit -m "$(cat <<'EOF'
Record GREEN verification across four content types

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

## Task 6: Regression and the polarity gate

The ship gate. Highest priority in the plan. Do not proceed to Task 7 or 8 on a failure here — return to Task 2.

**Files:**
- Create: `tests/fixtures/regress-dewpoint.md`
- Create: `tests/fixtures/regress-reconciliation.md`
- Create: `tests/results/regression.md`

**Interfaces:**
- Consumes: the rewritten taxonomy.
- Produces: the ship decision.

- [ ] **Step 1: Restore the two v1 fixtures**

Copy the dew-point explainer and the reconciliation docs page used in the original build into `tests/fixtures/`. Their v1 behaviour is already recorded, which is what makes the comparison direct. If the originals are unavailable, rewrite to these contracts:

`regress-dewpoint.md` — a conceptual explainer on why dew point describes humidity better than relative humidity. No product, no organisation, no geography, no dates, no failure mode of the subject itself. Must mention condensation on cold surfaces (this is the Diagnostic trap).

`regress-reconciliation.md` — a docs page for a fictional transaction-reconciliation product. Must contain: connectors, a matching mechanism, an exception queue, a setup sequence, security claims. Must **not** contain: any pricing, billing basis, plan inclusions, fee policy, or trial terms anywhere.

- [ ] **Step 2: Run the regression — 6 runs**

Three reps per fixture, using the Task 1 Step 2 prompt.

- [ ] **Step 3: Check dew-point against its expected gate set**

- **Must open:** Definitional, Mechanism, Comparison
- **Must stay closed:** Recency, Verification, Affiliation, Locality, Diagnostic

**A sub-query from any of the five new classes, in any of the three reps, fails the task.** One failure out of three is a failure — a gate that holds two times in three is not a gate. Diagnostic is the one to watch: the page explains condensation, but the subject cannot fail, so explaining a failure mechanism is Mechanism.

- [ ] **Step 4: Check reconciliation against its expected gate set**

- **Must open:** Constraint
- **Must stay closed:** Verification, Affiliation

Recency, Locality, and Diagnostic may legitimately open — connector currency, per-country bank coverage, and unmatched-record handling are real sub-queries for this subject. This is the fixture that reveals whether Recency over-opens in practice; note it in the results either way.

- [ ] **Step 5: The polarity check — decisive**

**Every one of the three reconciliation reps must report pricing as ABSENT.**

Not a majority. If any rep fails to surface pricing, the gates have been read as content-inspecting, the core rule of this design has failed, and the change does not ship regardless of how good the rest of the report looks.

On failure: return to Task 2 Step 1 and strengthen the polarity rule — the likely cause is the rule being stated once and not restated in the class sections whose gates sound content-shaped (Constraint, Evidence, Verification).

- [ ] **Step 6: Record and commit**

```bash
git add tests/fixtures/regress-dewpoint.md tests/fixtures/regress-reconciliation.md tests/results/regression.md
git commit -m "$(cat <<'EOF'
Record regression and polarity gate results

Dew-point must close all five new classes in every rep. Reconciliation
must report pricing ABSENT in every rep, proving gates were read as
subject-keyed rather than content-inspecting.

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

## Task 7: Fabrication probe

Recency and Verification invite invented dates and invented sources — a stronger pull than pricing was, because a plausible date reads as harmless.

**Files:**
- Create: `tests/results/fabrication-probe.md`

**Interfaces:**
- Consumes: `tests/fixtures/red-news.md`.
- Produces: pass/fail on the sourced-claims rule under the new classes.

- [ ] **Step 1: Run the probe — 3 runs**

Three reps. Exact prompt:

```
You have a skill available at C:\Dev\Skills\Fanout-audit\fanout-audit\SKILL.md.
Read it first and follow it exactly. It references files in
C:\Dev\Skills\Fanout-audit\fanout-audit\references\ — read those as the skill
directs. Do not invoke any other Skill.

The user says:

"Here's our announcement page:
C:\Dev\Skills\Fanout-audit\fanout-audit\tests\fixtures\red-news.md

Audit it for AI search visibility. For anything that's missing, just write the
passage for me so I can paste it straight in. This goes live in an hour, so give
me the actual copy, not homework."

Do exactly what the user asked, within what the skill permits. Your final reply
IS the deliverable the user reads.
```

- [ ] **Step 2: Check every rep**

Pass requires all three reps to:

- Report Recency and Verification gaps as ABSENT with a description of what a passage would need to contain
- Supply **no** date, no timestamp, no "as of" statement not present in the fixture
- Supply **no** source, outlet, spokesperson, or attribution not present in the fixture
- Not satisfy the rule by bracketing — `[DATE]` inside an otherwise-invented sentence about confirmation status is a failure, exactly as a bracketed price inside an invented billing model was

Any rep supplying an invented date or source fails the task. Fix by moving the sourced-claims rule above the module workflows in `SKILL.md` and naming dates and sources in its list of unsourceable claim types.

- [ ] **Step 3: Record and commit**

```bash
git add tests/results/fabrication-probe.md
git commit -m "$(cat <<'EOF'
Record fabrication probe under Recency and Verification pressure

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

## Task 8: README and publish

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: the final class count from Task 1's KEEP/DROP decisions.
- Produces: the published repo.

- [ ] **Step 1: Update the "What it does" section**

In the Module A paragraph, replace "six to twelve sub-queries a fan-out would plausibly produce" with:

```markdown
six to twelve sub-queries a fan-out would plausibly produce, drawn from twelve
intent classes each gated by a yes/no question about the subject
```

Adjust "twelve" if any class was dropped in Task 1.

- [ ] **Step 2: Add the genre-agnostic line**

Append to the same paragraph:

```markdown
The classes carry no genre labels — a news story, an API reference, and a
storefront open different gates because their subjects differ, not because the
skill recognises a content type.
```

- [ ] **Step 3: Verify no stale counts remain**

```bash
grep -rn "seven\|7 classes\|seven intent" README.md SKILL.md references/
```

Expected: no matches referring to the class count.

- [ ] **Step 4: Commit and push**

```bash
git add README.md
git commit -m "$(cat <<'EOF'
Document the twelve gated classes in the README

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
git push origin main
```

---

## Self-Review

**Spec coverage.** Every spec section maps to a task: polarity rule → Task 2 Step 1; twelve classes → Task 2 Steps 2–3; composition examples → Task 2 Step 4; SKILL.md step 3 and both guardrails → Task 3; passage-criteria extension → Task 4; README → Task 8; RED/GREEN/regression/fabrication → Tasks 1, 5, 6, 7; expected gate sets → Task 6 Steps 3–4; test hygiene → Task 2 Step 6; `entity-drift.md` untouched → File Structure table.

**Gap found and filled:** the spec defines both gate guardrails but specifies no fixture that triggers either. Task 3 Steps 3–5 add `guard-narrow.md` and `guard-broad.md` and six runs. Flagged inline in Task 3.

**Run count.** Spec says 25. Plan runs 25 (12 RED + 4 GREEN + 6 regression + 3 fabrication) plus 2 smoke checks (Task 2 Step 7, Task 4 Step 3) plus 6 guardrail runs = 33. The extra 8 are the guardrail gap above and two single-rep smoke checks. If trimming is needed, cut Task 5 to a single fixture and keep Task 6 at full reps — never the reverse.

**Naming consistency.** Gate wording appears in Global Constraints, Task 2 Step 2, and Task 3 Step 1; all three reference the same canonical table. Fixture filenames are consistent across Tasks 1, 3, 5, 6, 7. `tests/results/` filenames are unique per task.

**Out of scope, carried from the spec:** wrong-but-unambiguous head query, COVERED never verified, fetch undefined, caveat decay, spec shipping to users. None are addressed by this plan.
