# Sub-query taxonomy

Seven intent classes. Generate six to twelve sub-queries per audit by drawing from the
classes that fit the content.

**Skip classes that do not fit.** Not every class applies to every topic, and padding to
reach a count produces gaps that are not real gaps. A conceptual explainer with no product
attached has no pricing constraint to miss. A changelog has no qualification intent. Six
well-fitted sub-queries are a better audit than twelve padded ones.

These are a plausible decomposition of the head query, not Google's. Nothing about the
classes below is disclosed in the antitrust record or in Google's documentation.

| Class | Form | Applies when |
|---|---|---|
| Definitional | what is X, what does X mean | Almost always |
| Mechanism | how does X work, why does X happen | Technical or explanatory content |
| Comparison | X vs Y, alternatives to X, is X better than Y | Anything in a populated category |
| Qualification | who is X for, when should I use X | Products, tools, methods |
| Constraint | limitations of X, does X work for Z, X pricing | Anything with a boundary |
| Evidence | does X actually work, X results, X reviews | Claims that invite skepticism |
| Procedural | how to do X, getting started with X | Anything actionable |

---

## Definitional

Asks what the thing is. The most reliably issued class, and the one content most often
assumes rather than states — an author who has written 2,000 words about a concept rarely
goes back and defines it in one liftable sentence.

- what is expand and contract migration
- what does reconciliation software do
- eBPF tracing meaning

**Skip when:** the content is a reference table, a changelog, or an API parameter list, where
the reader arrives already knowing the term.

## Mechanism

Asks how or why it works. Splits into a *how* branch and a *why* branch, and the two often
have different answers in different sections.

- how does eBPF capture request traces without instrumentation
- why do database migrations lock production tables
- how does fuzzy transaction matching decide a threshold

**Skip when:** the content is purely procedural or commercial and makes no explanatory claim.

## Comparison

Asks how it sits against alternatives, including the alternative of doing nothing manually.

- expand and contract vs blue-green deploys
- alternatives to OpenTelemetry
- reconciliation software vs doing it in a spreadsheet

**Skip when:** the category genuinely has no populated field of alternatives. Rare — the
status quo usually counts as one, and "vs doing it by hand" is a real query.

## Qualification

Asks whether it is for this particular reader. The class most often missing entirely,
because "who this is not for" reads to authors like lost business.

- who is Ledgerline for
- when should you use eBPF tracing instead of SDK instrumentation
- is expand and contract worth it for a small team

**Skip when:** the content addresses a single, already-qualified audience it names up front.

## Constraint

Asks where the boundary is: limits, requirements, incompatibilities, and price. The class
most likely to surface a genuine absence.

- does Cabletrace support Windows containers
- Ledgerline pricing
- what kernel version does eBPF tracing require
- does expand and contract work with a single database replica

**Skip when:** nothing about the topic has a boundary worth stating. Rare in practice.

**Note:** when a constraint sub-query lands in ABSENT, the sourced-claims rule in SKILL.md
applies with full force. Describe what the passage would need to contain. Do not write it.

## Evidence

Asks whether the claim holds up. Triggered by content that asserts an outcome.

- does expand and contract actually prevent migration outages
- Ledgerline accuracy
- eBPF tracing overhead benchmarks

**Skip when:** the content makes no outcome claim — a definition page invites no skepticism.

## Procedural

Asks how to actually do it. Distinct from Mechanism: mechanism explains, procedure
instructs.

- how to migrate from OpenTelemetry
- getting started with Ledgerline
- how to backfill a column without locking the table

**Skip when:** the content is conceptual and there is nothing to execute.

---

## Worked decomposition

**Content:** a docs page for a transaction reconciliation tool.
**Head query:** *reconciliation software for finance teams with multiple payment processors*

| Sub-query | Class |
|---|---|
| what is reconciliation software | Definitional |
| how does automated transaction matching work | Mechanism |
| how are many-to-one bank deposits matched to transactions | Mechanism |
| reconciliation software vs reconciling in a spreadsheet | Comparison |
| who is this reconciliation tool for | Qualification |
| what does reconciliation software cost | Constraint |
| what payment processors and banks does it connect to | Constraint |
| what happens to transactions it cannot match | Constraint |
| how long does implementation take | Procedural |

Nine sub-queries. Evidence is skipped: the page states a match rate but makes no outcome
claim inviting scrutiny, so an evidence sub-query would be manufactured rather than found.

**Content:** a narrative engineering blog post about migration failures.
**Head query:** *how to run database schema migrations without causing downtime*

| Sub-query | Class |
|---|---|
| what is the expand and contract migration pattern | Definitional |
| why do migrations lock large tables | Mechanism |
| how do you split a schema change into two deploys | Procedural |
| should backfills run inside migrations | Procedural |
| what does expand and contract cost you in practice | Constraint |
| does expand and contract actually prevent outages | Evidence |

Six sub-queries. Comparison and Qualification are skipped: the post is a single-pattern
argument that neither positions against named alternatives nor addresses a segmented
audience, so both classes would produce gaps the author had no reason to fill.
