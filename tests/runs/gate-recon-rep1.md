Head query: reconciliation software for finance teams with multiple payment processors

## Stop: eight or nine gates open — no report generated

Gates open (9 of 9): Definitional, Mechanism, Comparison, Qualification, Constraint, Evidence,
Procedural, Recency, Verification.

Gates closed: none.

Definitional — the page explains what Ledgerline is for a reader who may not already know the
term ("Ledgerline is a reconciliation engine for finance teams that matches transactions
across bank feeds, payment processors, and your general ledger.").

Mechanism — "How matching works" describes the three-pass matching engine (exact, fuzzy,
aggregation).

Comparison — "Why reconciliation breaks" positions the subject against the spreadsheet
alternative a reader would otherwise weigh.

Qualification — the subject is software a finance team could decide to adopt; the page targets
"finance teams" with "multiple payment processors."

Constraint — "Connectors" names a specific, bounded list of supported processors, banks, and
general ledgers, with a generic-CSV fallback for anything not on the list.

Evidence — the page introduces its own outcome claims: "In practice this clears 80 to 90
percent of volume for most teams" and "Most teams complete this in under a week." Both invite
"does that actually hold" scrutiny.

Procedural — "Getting started" gives five numbered setup steps.

Recency — the connector list (which processors, banks, and general ledgers are supported) is
exactly the kind of fact that changes over time; a reader would need to check it is current.

Verification — "Security" asserts "SOC 2 Type II report available on request under NDA," a
certification/audit claim a reader could reasonably ask to have confirmed and by whom.

Per `references/subquery-taxonomy.md`: "Eight or nine gates open → do not report. Return to
step 2 and ask, because a subject that opens nearly every gate is usually more than one head
query." This is a stop, not a finding section — sub-queries were not generated or classified,
and no coverage report follows.

## What's likely happening

This single page appears to fuse at least two reader intents:

1. An explainer on what reconciliation software is and how transaction matching works
   (Definitional, Mechanism, Comparison).
2. A vendor-evaluation page for adopting Ledgerline specifically — connectors, accuracy,
   setup steps, and security/compliance (Qualification, Constraint, Evidence, Procedural,
   Recency, Verification).

## Question for the user

Should this audit be split into two head queries — for example "what is reconciliation
software and how does transaction matching work" versus "is Ledgerline a good fit and safe to
adopt for a multi-processor finance team" — or is one of these two clusters the actual intended
head query, with the audit narrowed to just that one? Answering this determines which gates
apply and lets the audit proceed to sub-query generation and classification.
