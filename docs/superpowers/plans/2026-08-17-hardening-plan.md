# Hardening plan

Date: 2026-08-17
Status: proposed, not started

Twelve open items, collected from the final branch review, the deferred spec issues, and an
unknown-unknowns pass. Ordered by what they cost if left alone, not by effort.

The governing question for each: **does this make the skill claim something it cannot support,
produce a wrong audit, or merely leave a limit unstated?** Those are three different problems
and only the first two are urgent.

---

## Tier 1 — Claims we make and cannot support

These are the ones that matter most, because they are promises to users rather than internal
weaknesses.

### A. Cross-model portability is asserted with zero evidence

`README.md` tells Codex, Copilot CLI and Gemini CLI users to install this skill. Every test run
in the repository — roughly 35 across two sessions — was Claude. The skill is almost entirely
discipline enforcement: refusing to draft absent passages, refusing to sprawl into on-page SEO,
stopping to ask on an ambiguous head query. Discipline is the first property to degrade on a
different or smaller model.

**Do:** run the fabrication probe and one regression fixture on a non-Claude model. The
fabrication probe is the right choice because it is the sharpest discipline test we have —
paste-ready pressure against a page with real gaps.

**Verify:** probe passes 3/3 with no invented date or source, and the regression fixture closes
Recency and Verification on the dew-point subject.

**If it fails:** narrow the README's install instructions to the runtimes actually tested and
say why. An honest scope beats a broken promise.

**Effort:** small, if a non-Claude runtime is available. This is the single highest-value item
in the plan.

### B. Module B ships on one smoke test

Entity consistency never received a RED baseline, never received gate questions, and received
nothing from the gate-keying work. One smoke test exists from the original build. It is roughly
half the skill's documented surface.

**Do:** decide first whether Module B stays. It solves a real problem — descriptions drifting
across a site, README, docs and directory listings — but it shares almost nothing with Module A
except the sourced-claims rule.

- **If it stays:** give it the same treatment Module A received. RED baseline on drift fixtures
  (3 reps), a decision on whether drift detection needs gates at all, and a regression fixture
  that proves it does not flag acceptable variation.
- **If it goes:** extract it to its own skill. Two loosely coupled modules in one skill make the
  description harder to write and the triggering less precise.

**Verify:** drift report correctly classifies a narrowing as not-drift, and the canonical
sentence rule holds — asks rather than picks when sources conflict on a fact.

**Effort:** medium either way. The decision is the hard part.

---

## Tier 2 — Correctness gaps that produce wrong audits

### C. COVERED is never verified

Every discipline mechanism in the skill points at the negative findings. Nothing tests whether
COVERED items are *correctly* covered, and the report format shows only the passage's first
line, which structurally hides whether the whole passage was tested. One 21-word passage was
already marked covered during the original build.

This is the most expensive wrong answer the tool can give: it tells an author they are fine.

**Do:** build a fixture with three passages that look answering but fail on inspection — an
answer that addresses a neighbouring question, one that depends on an earlier section, one that
states a fact without explaining it. Run 3 reps.

**Verify:** all three land in NOT EXTRACTABLE, not COVERED.

**If it fails:** consider requiring the report to show enough of the passage to justify the
verdict, rather than only its first line.

**Effort:** small. Highest value in this tier.

### D. Neither guardrail threshold has a dedicated test

Both stops — two-or-fewer gates and all-nine gates — were changed reactively, in response to
runs that tripped them incidentally. The fixtures that would exercise them deliberately
(`guard-narrow.md`, `guard-broad.md`) were specified in the previous plan and never built.

**Do:** build both fixtures. A subject so narrow only two gates open; a subject broad enough to
open all nine.

**Verify:** each produces a stop and no report, and neither fires on the existing regression
fixtures.

**Effort:** small.

### E. A wrong-but-unambiguous head query is unguarded

Step 2 stops when content targets *two* head queries. It has no guard for confidently inferring
*one* head query incorrectly — quieter and worse, because no ambiguity signal fires and every
downstream gap is confidently wrong.

**Do:** the skill already states the head query back before proceeding. Make that a required
pause rather than an announcement, or accept the risk and document it. Prefer the cheap
version: require the inferred head query on its own line before any gate work, phrased so a
wrong inference is obvious to the reader.

**Verify:** a fixture whose subject is easy to misread produces a stated head query the reader
can reject.

**Effort:** small.

---

## Tier 3 — Limits to state, not to fix

Each of these is a genuine boundary. Attempting to fix them would expand the skill past what it
can support.

### F. There is no "wrong answer" state

A passage that is extractable, complete and factually false is marked COVERED. The README now
says extractability and credibility are independent axes, but the report itself carries no way
to flag it.

**Do:** nothing structural. Do **not** add a truth-check state — verifying claims about an
arbitrary entity is a different tool and the skill would do it badly. Consider one line in the
report footer restating what a clean report does and does not mean.

### G. The skill cannot tell you the question has no audience

The head query is inferred *from the content*, so a page on a topic nobody searches for is
audited against its own premise and can come back clean. This is structurally unfalsifiable
from inside the skill.

**Do:** state it in the README's "what it does not do" list. Keyword demand is a different job.

### H. Non-prose pages are undefined

Calculators, video-led pages, image galleries, JS-rendered apps. The skill assumes retrievable
text exists.

**Do:** add a stop condition — if the page's substantive content is not text, say so and stop
rather than auditing the surrounding copy as though it were the page.

---

### I. The README fails the skill's own audit

Running the skill against its own README returned **covered=2, not extractable=6, absent=2**.
Full run in `tests/runs/self-readme-rep1.md`.

Not extractable: the five criteria, the install procedure, why the decomposition is simulated,
how this differs from an on-page SEO tool, the licence, and the runtimes it works with.
Absent: **who it is for and when to run it**, and whether the compatibility list is current.

Two of those findings — the runtimes one and the currency one — are item **A** arriving from a
different direction. A was found by asking what had never been tested; these were found by
reading the page. Same defect, two independent methods.

The one COVERED finding is the disclosed-facts section, which was rewritten after the earlier
review flagged an unsourced claim presented as record. That fix is visible to the audit; the
rest of the README has not had the same treatment.

**Do:** fix the README against its own report. Start with "who is it for", which is a real gap
rather than an extractability problem — the README describes what the skill does and never says
who should run it or when.

**Verify:** re-check each fixed item individually. Per the re-run guidance, do **not** compare
counts across runs — the decomposition regenerates.

**Effort:** small. Worth doing early, because a tool whose own front page fails its standard is
hard to argue for.

## Tier 4 — Record hygiene

- **Affiliation was dropped on two baseline reps** where the rule requires three. Recoverable:
  v1 is at commit `217a45e`. Two reps settles it.
- **Seven of eleven RED runs left no artifact**, because a runner dropped the write-to-file
  instruction. Verdicts were unanimous but are not reproducible from the repo.
- **`docs/` ships to users.** The documented install clones the whole repo into the skills
  directory, so specs and plans land in every user's skills folder. Consider a release branch
  or moving `docs/` out of the installed path.
- **Caveat decay.** A user on their twentieth report has stopped reading the disclosure header.
  No fix proposed; noted so it is not rediscovered.

---

## Explicitly not doing

Recorded so these stay decided:

- **No score, percentage or grade.** Scores get optimised toward, which is the failure the
  three-state triage exists to avoid.
- **No truth or fact-checking state.** See F.
- **No on-page SEO findings.** Title tags, schema, `llms.txt`, internal linking. The output
  contract excludes them and every baseline test showed agents sprawl there by default.
- **No fetching built into the skill.** Guidance on acquiring text correctly is in step 1; the
  skill still does not fetch on its own.
- **No numbers imported from practitioner sources** about undisclosed mechanisms, however
  confidently those sources state them.

---

## Sequencing

1. **A** — cross-model probe. It is a live promise and the cheapest way to find out if it holds.
2. **C** — COVERED verification. Cheapest correctness fix with the worst failure mode behind it.
3. **D** — guardrail fixtures. Closes the last untested behaviour in Module A.
4. **B** — the Module B decision, then whichever branch follows.
5. **E**, then Tier 3 documentation together in one pass.
6. Tier 4 as it becomes convenient.

Items A, C and D together are a short session and would leave Module A with no untested
behaviour. B is the larger question and deserves its own.
