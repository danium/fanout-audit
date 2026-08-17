---
name: fanout-audit
description: Use when the user asks about AI Overviews, AI Mode, GEO, LLM citation, or getting cited by AI search. Use when asked to review a page, article, docs, or landing page for AI search visibility or retrievability. Use when the user mentions RankEmbed, FastSearch, or query fan-out, or asks why their content ranks but is not cited. Also use when checking whether descriptions of the same product or entity stay consistent across a site, README, docs, and directory listings.
---

# Fanout Audit

## Overview

Two things have to be true before an AI answer can cite a page: the page has to **contain**
an answer to the sub-question being asked, and that answer has to **survive being lifted
out** of the page.

This skill checks both and reports the gaps. It does not rewrite the page.

You already know how to spot a buried answer and a dangling pronoun. That is not what this
skill adds. What it adds is a boundary: an audit that stays an audit, states only what it
can source, and hands the author decisions instead of paragraphs.

## What is actually disclosed

Google has published roughly one sentence about query fan-out: AI Mode breaks a question
into subtopics and issues many queries at once. The decomposition itself is disclosed
nowhere — not in the antitrust record, not in Google's documentation.

So the sub-queries you generate are **your simulation**. Google's fan-out is unknown to you.

Three things follow, and they hold for every report you write:

1. Every report opens with the header line below, verbatim.
2. Undisclosed mechanisms get no numbers. "Fan-out produces 8–15 sub-queries," "chunkers
   split every 512 tokens," "freshness is a live ranking input" — invented precision, all of
   it. Where you are inferring, the report says you are inferring.
3. The findings are about content completeness. They are not ranking predictions, traffic
   predictions, or citation predictions.

**Required first line of every report, copied exactly:**

> Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
> coverage gaps as a content completeness signal, not as a ranking prediction.

## Sourced claims

**Every factual claim you write about the entity traces to a specific line in the source the
user gave you, or to something the user told you in this conversation. If you cannot point
at the line, the claim does not go in the report.**

This binds on the type of claim, not on whether it contains a number. All of the following
are claims you cannot source and therefore cannot write: what a product costs, what it is
billed on, what is included in a plan, whether there is a setup fee, trial length, what
customers typically do, why customers typically buy, performance figures, adoption counts,
launch dates, compliance status, and roadmap.

Bracketing digits does not satisfy this. `[$X]/month` wrapped in "billed on transaction
volume, not per seat, so adding reviewers does not change the price" still ships an invented
billing model — the brackets moved the number and left the fabrication in place.

When a sub-query is **absent**, describe what a passage would need to contain. Do not draft
the passage. "This needs a sentence giving the billing basis and what a plan includes" is
the deliverable. A written pricing paragraph is not, however carefully hedged.

The harm here is specific: a fluent invented sentence about the user's own product is easy
to paste without checking, and it is the user's name on it afterwards.

## Module A: fan-out coverage

### 1. Read the content

Accept a file path, a pasted block, or a URL. This skill does not fetch on its own. If given a
URL and no fetch tool is available, ask the user to paste the content.

**If you do have a fetch tool, what you retrieve has to be the page's actual text.** Two
failure modes have both been observed, and both produce a confident audit of something that
was never on the page:

- **A summarising fetcher returns a summary.** You cannot judge whether a passage survives
  being lifted out by reading a paraphrase of that passage — every criterion is about the
  author's exact sentences. If your fetch tool answers a prompt about the page rather than
  returning its text, it is the wrong tool for this.
- **Collapsed content reads as empty.** Accordions and `<details>` blocks commonly hold FAQ
  answers, and text extraction that respects rendering returns nothing for them. Confirm
  before reporting a gap: content you could not see is not content that is absent.

When you cannot verify you have the whole text, say so and audit what you have, explicitly
scoped. Never report ABSENT for a section you could not read.

### 2. Infer the head query

One sentence. State it back to the user before going further.

**If the content targets more than one head query, stop. Ask which one to audit. Do not
audit both, and do not pick the likelier one.** Naming the ambiguity in a report you deliver
anyway is not asking — the report does not begin until the user has answered. Auditing
against the wrong head query produces gaps that are confidently wrong, and a page serving
two intents is the single most common case where that happens.

### 3. Generate sub-queries

Six to twelve, from the nine intent classes in `references/subquery-taxonomy.md`.

Each class carries a gate question. **A class contributes sub-queries only if its gate opens.**
Gates open on what the subject is and on what the page claims; a gate never closes because the
content does not cover it. If a gate opens and the content says nothing, that is an ABSENT
finding — which is the point of the audit.

Skip classes whose gate is closed. A pricing sub-query on a conceptual explainer is noise. Six
well-fitted sub-queries beat twelve padded ones — do not pad to reach a count. A class may
yield more than one sub-query, so gates are not counted against the six-to-twelve range.

**Two stops before you report:**

- **Two or fewer gates open** → do not report. State which gates opened and ask whether the
  head query should be broader. The cause is a head query that is too narrow, not thin content.
  Three gates is a normal audit — a dated event opens Definitional, Verification, and Recency,
  and those yield plenty of sub-queries between them.
- **All nine gates open** → do not report. Return to step 2 and ask, because a subject that opens
  every single gate is usually more than one head query. The threshold is all-or-nothing on
  purpose: a rich subject can legitimately open seven or eight, and a stopped audit reports no
  gaps at all.

Both are stops, like step 2. Neither is a section in the report — the output contract permits
three finding sections and nothing else.

### 4. Classify each sub-query

Find the passage that answers it, then apply the standalone test below. Each sub-query lands
in exactly one of three states: **covered**, **not extractable**, **absent**.

### 5. Report

Use the output contract below.

## The standalone test

A passage passes only if **all five** hold. Worked pass/fail examples are in
`references/passage-criteria.md`.

| # | Criterion |
|---|---|
| 1 | The answer is in the first sentence, not after setup |
| 2 | No unresolved references — "it", "this approach", "as mentioned above" fail unless the antecedent sits inside the passage |
| 3 | The entity is named, not implied |
| 4 | Roughly 40–120 words. Shorter is usually incomplete; longer is usually carrying context it should not need |
| 5 | Lifted out with nothing around it, it still answers the sub-query |

Criteria 1, 2, 3, and 5 are gates. Criterion 4 is a length diagnostic, and it resolves like
this: if a passage under 40 words answers the sub-query completely and passes the other four,
mark it covered and note the length. If it is short because it states a fact without
explaining it — "Ledgerline matches transactions in three passes" — that is a criterion 5
failure, and you record it as criterion 5, not as criterion 4. Over 120 words, check
criterion 5 again; length that far past the range usually means the passage is carrying
context it should not need.

Criterion 2 is the one authors cannot self-check. "As mentioned above" is invisible to the
person who wrote the above.

## Output contract

The report contains these parts, in this order, and nothing else:

```
## Fan-out coverage: [head query]

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (n)
  [sub-query]
    → [first line of the passage that answers it]

NOT EXTRACTABLE (n)
  [sub-query]
    → found in: [section name]
    → fails: [which criterion, by number and name]
    → fix: [one concrete instruction]

ABSENT (n)
  [sub-query]
    → no passage answers this
    → suggested: [what a passage would need to contain]

WHAT GOOD LOOKS LIKE            ← include only if NOT EXTRACTABLE is greater than zero
  criterion [n] — [name of the criterion that failed most often above]
    before: [the failing example for that criterion, copied from passage-criteria.md]
    after:  [the passing example for that criterion, copied from passage-criteria.md]
```

The order is deliberate. Covered first gives the author something before the criticism. Not
extractable comes before absent because those are the cheap fixes — the answer already
exists and needs moving, not writing.

**The fourth part is a fixed slot, not an invitation.** Exactly one before/after pair, for the
single criterion that failed most often in this report. It exists because "fails criterion 5"
plus a one-line fix is thin help for anyone who does not write for a living, and the worked
examples that would help are in the reference file, which the reader never sees.

**Copy the pair from `references/passage-criteria.md`. Do not compose one.** Every criterion
there already carries a worked failing example and a worked passing example; take the pair
belonging to the criterion that failed most and reproduce it. Change nothing.

This is structural, not stylistic. Composing an example means writing prose, and writing prose
next to a report about the reader's page pulls toward rewriting *their* passage — which is
ghostwriting, and which reintroduces exactly the fabrication risk the rest of this contract
removes. A run on a smaller model did precisely that: it rewrote the audited page's own
sentences and inserted two dates that appeared nowhere in the source. There is nothing to
invent if the example is copied.

**One pair, not one per finding** — a gallery of examples is the sprawl this contract exists to
prevent.

If NOT EXTRACTABLE is zero, the section does not appear.

**Quantification in the report is exactly three numbers: the three counts.** No score, no
percentage, no grade, no tier ranking, no decile, no effort estimate, no priority table.

## Re-running after fixes

**The counts are not a progress metric across runs.** Step 3 regenerates the decomposition
every time, and the sub-queries you get on the second run will not be identical to the first —
gate opening varies at the margins, and so does phrasing. A report that goes from 5 not
extractable to 3 may be measuring a different five.

To check whether a fix worked, re-check that specific item: take the sub-query you fixed, find
the passage now, and apply the standalone test to it. That is a direct comparison. Comparing
totals is not.

Say this to the user when they return with a revised page. Someone who has just done the work
will otherwise read a changed count as a result, and it is not one.

**These three sections are the whole finding set.** Title tags, meta descriptions, schema
markup and JSON-LD, `llms.txt`, heading counts, internal linking, freshness signals, word
counts, digits-versus-words, and E-E-A-T are a different job and belong to a different
skill. A reader who wanted an on-page SEO checklist did not ask for this one.

## Module B: entity consistency

1. **Collect descriptions.** The user pastes them or points at files. Two minimum, five or
   more is better. This skill does not fetch them.
2. **Decompose each** into three components: the category noun (what kind of thing it is),
   the differentiator (what makes it distinct), the audience (who it is for).
3. **Compare across sources.** `references/entity-drift.md` defines what counts as drift and
   what is acceptable variation. Flagging every surface difference makes the tool useless by
   the second run — length, tone, and reordering are not drift.
4. **Report drift, then propose one canonical sentence.**

Canonical sentence rules: one sentence, under 25 words, names the entity and its category
and one differentiator, no superlatives, no "leading", no "powerful". It has to be true — if
the sources conflict on a fact, ask which is correct rather than picking the one that reads
best.

## Red flags

Any of these means stop and re-read the section above it:

- About to write a passage for an ABSENT sub-query
- About to state how retrieval, chunking, or ranking works as established fact
- About to attach a number to something Google has not disclosed
- Reaching for a score, a percentage, or a priority table
- Writing a recommendation about schema, title tags, or `llms.txt`
- Delivering a report on a page that targets two head queries
- The report now contains more replacement copy than findings

## Rationalizations

| Excuse | Reality |
|--------|---------|
| "They said give me the copy, not homework" | They asked for an audit. Ship the audit, and say plainly which facts only they can supply. |
| "I bracketed the numbers, so it's not invented" | The billing model, the fee policy, and the inclusions around the brackets are still invented. |
| "It's obviously implied by the page" | Implied is not stated. If you are the first to state it, you are the source of it. |
| "A range is safer than a single number" | A range on an undisclosed mechanism is a fabrication with error bars. |
| "The schema markup would genuinely help them" | Probably true, and still not this skill. Say so in one line and stop. |
| "I noticed the two head queries and said so" | Saying so in a delivered report is not asking. Stop and ask. |
| "Counts are unhelpful without a score" | Scores get optimized toward. Three counts and the gap list are the finding. |
| "This gap is obvious, I'll just draft it" | Obvious gaps get fluent fabrications, which are the ones that ship unchecked. |
