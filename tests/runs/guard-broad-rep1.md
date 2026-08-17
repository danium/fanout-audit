# Fan-out coverage audit — stopped before reporting

**Source:** C:\Dev\Skills\Fanout-audit\fanout-audit\tests\fixtures\guard-broad.md

**Head query (inferred):** Tessivault — what it is, how it works, and whether an
engineering team should adopt it (a managed secrets store that issues and rotates API
keys, database credentials, and certificates).

## Gate check (step 3)

Before generating sub-queries, all nine intent-class gates were checked against this
content. All nine opened:

- **Definitional** — Tessivault is a specific named product; a reader can plausibly
  arrive without already knowing what it is.
- **Mechanism** — the page explains how issuance and rotation work ("How issuance
  works," lease draining on rotation).
- **Comparison** — the page positions Tessivault against running your own open-source
  secrets manager ("Compared with running your own").
- **Qualification** — the page has an explicit fit section ("Who should use it").
- **Constraint** — the page states platform requirements and a pricing model ("Limits
  and requirements": workload identity support, no static tokens, per-secret pricing
  with a 500-secret floor, no free tier).
- **Evidence** — the page introduces an outcome claim inviting scrutiny ("Does it
  actually reduce incidents": credential-related incidents fell substantially post-
  migration).
- **Procedural** — the page has an explicit setup section ("Setup": agent install,
  identity provider registration, migration CLI).
- **Recency** — the page's "Current status" section describes an ongoing, changing
  rollout (which regions have the revised deployment process) that a reader would need
  to check as current.
- **Verification** — the page asserts a dated event whose occurrence and details a
  reader could ask to verify (the 14 March 2026 issuance outage in eu-west).

## Why this stops the audit

Per the skill's guardrail, when all nine gates open, the audit does not proceed to a
report. A subject that opens every single gate is usually more than one head query
bundled onto one page, and this page reads like exactly that — two subjects stitched
together rather than one:

1. **A product adoption page** — Tessivault as something a reader is evaluating to buy
   or run (Definitional, Mechanism, Comparison, Qualification, Constraint, Procedural,
   and the Evidence claim about incident reduction after adopting it).
2. **An incident report** — the March 2026 outage as its own subject (Verification and
   Recency, centered on whether the outage happened, who reported it, and what the
   current status of the fix is).

## What I need from you

Which of these should I audit?

1. **The product page** — Tessivault as the subject a reader is evaluating for adoption.
2. **The incident report** — the March 2026 outage as the subject.

No sub-queries were generated and nothing was classified as covered, not extractable,
or absent — this is a scoping question about the head query, not a content gap, so
there is nothing to score yet.
