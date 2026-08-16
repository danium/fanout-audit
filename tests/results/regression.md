# Regression and polarity gate

Nine-class taxonomy against the two fixtures whose v1 behaviour is on record.

## Dew-point explainer — PASS 3/3

Expected: must open Definitional, Mechanism, Comparison, Procedural. Must stay closed Recency,
Verification.

| Rep | Gates open | Recency | Verification | Verdict |
|---|---|---|---|---|
| 1 | Definitional, Mechanism, Comparison, Procedural | closed | closed | PASS |
| 2 | Definitional, Mechanism, Comparison, Procedural | closed | closed | PASS |
| 3 | Definitional, Mechanism, Comparison, Procedural | closed | closed | PASS |

**Identical gate sets across three independent reps.** Convergence is the signal that the gate
questions bind rather than being reinterpreted per run — five different readings across five
reps would have meant the wording was not doing work.

No sub-query from either new class appeared in any rep, which was the condition for shipping.

Procedural opening is what keeps this fixture at four gates. At three it trips the low-gate
stop and produces no report, leaving the must-stay-closed check with nothing to check — the
pre-flight ruling that caught this is recorded in the ledger.

## Reconciliation docs — FAIL 2/3, then PASS 3/3 after fixes

### First run

| Rep | Gates open | Verification | Pricing ABSENT | Verdict |
|---|---|---|---|---|
| 1 | all nine | **open** | **no report produced** | FAIL |
| 2 | 7 | closed | yes | PASS |
| 3 | 7 | closed | yes | PASS |

Rep 1 opened every gate, tripped the high guardrail, and produced no report — so pricing was
never reported ABSENT and the polarity check failed. Two causes, both real defects:

**Verification over-opened.** Its gate accepted "an assertion the page makes", which rep 1 read
as covering a specification figure ("clears 80 to 90 percent of volume"). A specification
invites Evidence — does that hold up? — not Verification — did that happen? The gate now
requires a dated or datable event, with the distinction stated explicitly in the class section.

**The high guardrail was too tight.** This is a defect introduced by the class count dropping.
At twelve classes, the three dropped ones (Diagnostic, Affiliation, Locality) stayed reliably
closed on product content, holding a docs page near 7/12. Removing exactly those three raised
the open-ratio, so an 8-of-9 ceiling fires on legitimate pages. The threshold is now
all-nine-or-nothing: a thorough product page legitimately opens seven, and a stopped audit
reports no gaps at all, which is worse than a slightly wide one.

### After fixes

| Rep | Gates open | Verification | Pricing ABSENT | Verdict |
|---|---|---|---|---|
| 1 | 8 (Evidence opened) | closed | **yes** | PASS |
| 2 | 7 | closed | yes | PASS |
| 3 | 7 | closed | yes | PASS |

Rep 1 — the rep that failed — now closes Verification and runs to a report at eight gates.
Both fixes did precisely what they were designed to do, on the exact case that exposed them.

Evidence still varies between reps (open in rep 1, closed in 2 and 3). Tolerable: Evidence is
not in this fixture's must-stay-closed set, and eight of nine no longer trips the ceiling.

## Polarity check — PASS 3/3

Every reconciliation rep reported pricing as ABSENT. The fixture contains no pricing, billing
basis, plan inclusions, fee policy, or trial terms anywhere; the Constraint gate opened on what
the subject *is* rather than on what the page says, which is the entire point of the polarity
rule and the reason a genuine gap stays reportable.

## New-class value

Recency earned its place beyond passing the gate. Every reconciliation rep produced
`[ABSENT] is the connector list current` — a real gap that no v1 rep found in any run against
this fixture across the original build or this one.
