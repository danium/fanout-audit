# External review round — three accepted changes, verified

An external review (run on a non-Claude model) proposed nine changes. Three were accepted
after verification of the claims behind them; the rest were rejected, deferred, or already
covered. This file records the verification runs for the three that shipped.

## Verification of the review's factual claims, first

Acted on only after checking:

| Claim | Verdict |
|---|---|
| Search Central documents fan-out for both surfaces, incl. "data sources" | Confirmed, quoted in README |
| Google published "hundreds of searches" for Deep Search | Confirmed, quoted in README |
| Gemini API returns the queries used (`groundingMetadata.webSearchQueries`) | Confirmed, cited in README |
| `#:~:text=` is generic Chrome tech (2020), not AI-specific | Confirmed, README demoted accordingly |
| Google stopped sending fragments on Overview links (~May 2026) | **Not confirmed** — not imported |
| Our spec "still says seven classes" | **False** — all instances are correct historical context |
| WGLL examples "always Ledgerline" | **False** — pairs span migrations, an event, two products, onboarding |

## 1. Observed-query mode → PASS

User pastes real queries (e.g. from `groundingMetadata.webSearchQueries`); the skill audits
those and generates nothing. Test supplied six queries against the reconciliation fixture,
including the trap `Ledgerline vs Blackline` — a competitor appearing nowhere on the page.

- Observed-queries header line used instead of the simulation line
- Zero queries generated beyond the six supplied; no gate or stop machinery invoked
- Trap held: `Ledgerline vs Blackline` → ABSENT, with no invented comparison
- Pricing → ABSENT: polarity behaviour intact in the new mode
- Verdicts consistent with all prior runs on this fixture (SOC 2 → NOT EXTRACTABLE)

## 2. User-named head query → PASS

Run against `two-heads.md` — the two-intent Cabletrace page whose documented baseline
behaviour is stop-and-ask. User named "how to migrate from OpenTelemetry to Cabletrace".

- Did not stop: the named query suppressed the ambiguity stop, as specified
- Used verbatim; not re-inferred or broadened
- All eleven sub-queries scoped to the migration half; the one Definitional sub-query drew on
  the overview half only to orient a migration reader, which is the gate working as designed

Closes hardening-plan item E for the named path. The inferred path keeps its existing guard
(head query stated back before proceeding).

## 3. Draft-from-supplied-facts → PASS

The riskiest change: it relaxes wording that held through every fabrication probe. The bet was
that it is not actually a relaxation — the sourced-claims rule always permitted facts from the
user's own message, and the flat "do not draft" overshot it.

User supplied four pricing facts and asked for the passage, against a page with several other
ABSENT gaps.

- Passage drafted from exactly the four supplied facts, entity named, answer first
- **Facts beyond supplied: none** — no billing basis, seat policy, or overage invented, which
  was the original baseline failure this rule was built against
- **Other ABSENT items drafted: none** — permission stayed scoped to the one item with facts
  behind it rather than generalising into "drafting is allowed now"

## Rejected from the same review, for the record

- **Unique-claim flag on COVERED items** — "likely corroborated" is a guess about the rest of
  the web with nothing behind it, and the output contract permits three sections only. The
  sound fragment (contradiction across owned surfaces) is already Module B's job.
- **Transformation layer on simulated queries** — premise (fan-out is mostly query
  transformations) is third-party patent inference, unverified; it risks reinflating counts
  toward padding; and observed-query mode obsoletes it, since real queries contain their own
  transforms. Recorded as an open question pending calibration evidence, not implemented.
