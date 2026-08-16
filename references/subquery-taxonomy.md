# Sub-query taxonomy

Nine intent classes. Generate six to twelve sub-queries per audit from the classes whose
**gate** is open.

Each class carries a gate question. **A class contributes sub-queries only if its gate opens.**
Skip classes whose gate is closed — a pricing sub-query on a conceptual explainer is noise.
Six well-fitted sub-queries beat twelve padded ones. Do not pad to reach a count.

These are a plausible decomposition of the head query, not Google's. Nothing about the classes
below is disclosed in the antitrust record or in Google's documentation.

## Gate polarity

> Gates open on what the subject is and on what the page claims. **Never close a gate because
> the content does not cover it.** If a gate opens and the content says nothing, that is an
> ABSENT finding — which is the point of the audit.

Worked example of why this matters: a docs page contains no pricing anywhere. Under a gate
reading "does the page state a price?" the gate closes, no pricing sub-query is generated, and
pricing can never be reported ABSENT. A gap is by definition something the content does not
contain, so a gate that closes on content absence is blind to exactly the gaps worth reporting.

The clause about what the page claims exists because Evidence and Verification are properties
of what is being asserted, and an assertion can be introduced by the page about an otherwise
neutral subject. A gate may inspect what the content claims. A gate may never inspect whether
the content substantiates the claim — that is the finding, and findings belong in the report.

## The nine classes

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
| Verification | Does the subject, or an assertion the page makes, involve a **dated or datable event** whose occurrence a reader might doubt? |

**Two guardrails, both stops rather than findings.** If **two or fewer** gates open, do not
report: state which gates opened and ask whether the head query should be broader. The cause
is a head query that is too narrow, not thin content. If **all nine** gates open, do not
report: return to the head-query step and ask, because a subject opening every single gate is
usually more than one head query.

Both thresholds are deliberately at the extremes, because a stopped audit reports no gaps at
all — which is worse than one that runs slightly wide or slightly narrow.

Three open gates is a normal audit, not a starved one. A dated event typically opens only
Definitional, Verification, and Recency, and those three yield plenty of sub-queries between
them — what happened, who reported it, is it confirmed, what is the current status. Gates are
not counted 1:1 against the six-to-twelve range.

---

## Definitional

Asks what the thing is. The most reliably issued class, and the one content most often assumes
rather than states — an author who has written 2,000 words about a concept rarely goes back and
defines it in one liftable sentence.

- what is expand and contract migration
- what does reconciliation software do
- eBPF tracing meaning

**Gate closed:** an API parameter reference, a changelog, or a pricing table. The reader
arrived already knowing the term and is looking up a value, not an identity.

## Mechanism

Asks how or why it works. Splits into a *how* branch and a *why* branch, and the two often have
different answers in different sections.

- how does eBPF capture request traces without instrumentation
- why do database migrations lock production tables
- how does fuzzy transaction matching decide a threshold

**Gate closed:** a company's office relocation announcement. There is no mechanism a reader
would ask after — the subject is an event, not a system.

## Comparison

Asks how it sits against alternatives, including the alternative of doing nothing manually.

- expand and contract vs blue-green deploys
- alternatives to OpenTelemetry
- reconciliation software vs doing it in a spreadsheet

**Gate closed:** a specific historical event. A named flood in a named year has no alternatives
a reader would weigh it against.

## Qualification

Asks whether it is for this particular reader. The class most often missing entirely, because
"who this is not for" reads to authors like lost business.

- who is Ledgerline for
- when should you use eBPF tracing instead of SDK instrumentation
- is expand and contract worth it for a small team

**Gate closed:** the mechanism of an eclipse. Nothing about it is adopted, bought, or followed,
so there is no fit decision to make.

## Constraint

Asks where the boundary is: limits, requirements, incompatibilities, and price. The class most
likely to surface a genuine absence.

- does Cabletrace support Windows containers
- Ledgerline pricing
- what kernel version does eBPF tracing require

**Gate closed:** a definition of a mathematical term. It has no requirements, no
incompatibilities, and no price.

**Note:** when a Constraint sub-query lands in ABSENT, the sourced-claims rule in SKILL.md
applies with full force. Describe what the passage would need to contain. Do not write it.

## Evidence

Asks whether the claim holds up. Opens on the subject's own outcome claims **and** on outcome
claims the page introduces.

- does expand and contract actually prevent migration outages
- Ledgerline accuracy
- eBPF tracing overhead benchmarks

**Gate closed:** a page defining a unit of measurement. No outcome is claimed, by the subject
or by the page, that a reader would want proof of.

## Procedural

Asks how to actually do it. Distinct from Mechanism: mechanism explains, procedure instructs.

- how to migrate from OpenTelemetry
- getting started with Ledgerline
- how to backfill a column without locking the table

**Gate closed:** a profile of a historical figure. There is nothing for the reader to execute.

## Recency

Asks whether the subject's current state still matches what the page describes.

- latest X
- is X still accurate
- current status of X
- has X changed since this was written

Distinct from Verification: Recency asks whether the state still holds, not whether the event
occurred.

**Gate open:** a satellite service's supported-country list. Countries are added and removed, so
a reader must check what is true now.

**Gate closed:** the relationship between dew point and relative humidity. The physics does not
change, so there is no current state to check. Note this is a property of the subject — a page
about it could still be edited tomorrow, and that is not what this gate asks.

## Verification

Asks whether an asserted event actually happened, and on whose authority.

- is X confirmed
- who reported X
- source for X
- did X actually happen

**The gate needs a dated or datable event.** A capability, a specification, or a performance
figure is not an event, however concrete it sounds. "Clears 80 to 90 percent of volume" is a
specification — it invites Evidence ("does that hold up?"), not Verification ("did that
happen?"). If you cannot point at something that occurred at a time, this gate is closed.

**Gate open:** a report that a warehouse fire destroyed a freight terminal last Tuesday. A
reader can reasonably ask who established that, and whether it is confirmed.

**Gate closed:** a product docs page describing how three-pass transaction matching works and
what share of volume each pass clears. Nothing is asserted to have *occurred* — the subject is
a mechanism and its specifications, not an event.

---

## Worked decompositions

**Content:** a docs page for a transaction reconciliation tool.
**Head query:** *reconciliation software for finance teams with multiple payment processors*
**Gates open:** Definitional, Mechanism, Comparison, Qualification, Constraint, Procedural,
Recency. Verification and Evidence closed — the page asserts no event, and its match-rate
figure is a specification rather than an outcome claim inviting proof.

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
| is the connector list current | Recency |
| how long does implementation take | Procedural |

Note the pricing sub-query. The page contains no pricing at all — the Constraint gate opened on
what the subject *is*, not on what the page says, which is what makes the gap reportable.

**Content:** a narrative engineering blog post about migration failures.
**Head query:** *how to run database schema migrations without causing downtime*
**Gates open:** Definitional, Mechanism, Procedural, Constraint, Evidence. Comparison and
Qualification closed — the post argues a single pattern without positioning it against named
alternatives or a segmented audience. Recency and Verification closed — the pattern's validity
does not expire, and no event is asserted whose occurrence a reader would question.

| Sub-query | Class |
|---|---|
| what is the expand and contract migration pattern | Definitional |
| why do migrations lock large tables | Mechanism |
| how do you split a schema change into two deploys | Procedural |
| should backfills run inside migrations | Procedural |
| what does expand and contract cost you in practice | Constraint |
| does expand and contract actually prevent outages | Evidence |

## Composition, not genre

**Gates are determined by the subject, never by the kind of page.** No genre label appears
anywhere in this file, and none should be inferred from one.

| Subject of the head query | Gates that open |
|---|---|
| what the expand-and-contract migration pattern is | Definitional, Mechanism, Comparison, Procedural |
| a chemical plant fire that happened last Tuesday | Definitional, Verification, Recency |
| a regional product recall announced last Tuesday | Definitional, Verification, Recency, Constraint, Procedural, Qualification |
| why relative humidity misleads and dew point does not | Definitional, Mechanism, Comparison, Procedural |
| whether a payments method is available to merchants in a given country | Definitional, Constraint, Qualification, Recency, Procedural |

Rows two and three are both news. They differ by six gates versus three, because the subjects
differ — a recall is something a reader must act on and check eligibility for, while a fire is
something a reader only needs established. That difference is why no genre label appears in
this file.
