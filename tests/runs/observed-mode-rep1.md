## Fan-out coverage: Ledgerline (reconciliation software for finance teams)

Sub-queries below are observed queries supplied by the user, not a simulated decomposition.
Treat coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (2)
  what is Ledgerline
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger."
    (20 words — short but complete: names the category, the audience, and the function. Marked covered per the criterion 4 under-40-words-complete rule, not flagged as a length failure.)

  what banks and processors does Ledgerline connect to
    → "Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side."
    (Full passage continues with the banking-side and general-ledger connectors in the same paragraph; entity named, self-contained, ~54 words.)

NOT EXTRACTABLE (2)
  how does Ledgerline match one bank deposit to many transactions
    → found in: "How matching works", the aggregation-matching paragraph
    → fails: criterion 1 (the actual mechanism — "searches for subsets of unmatched processor records that sum to an unmatched bank deposit" — is the third sentence; sentence 1 just labels the pass and restates the scenario, sentence 2 ("This is the case that breaks naive tools") is editorial and answers nothing); criterion 2 ("The third pass" is an unresolved ordinal reference — it assumes the first- and second-pass paragraphs above it, which are not part of this passage)
    → fix: open with the mechanism itself and drop the ordinal label — state that Ledgerline matches one deposit to many transactions by searching for subsets of unmatched processor records that sum to the deposit (within a fee tolerance), then move "this is the case that breaks naive tools" later or cut it

  is Ledgerline SOC 2 certified
    → found in: "Security" section
    → fails: criterion 1 (the SOC 2 fact is the third sentence, after two sentences about credential encryption and read-only connector scopes that don't address the query); criterion 3 (the sentence carrying the answer — "SOC 2 Type II report available on request under NDA." — has no subject; Ledgerline is named in the sentence before it, not in this one)
    → fix: lead the Security section (or a standalone sentence) with "Ledgerline has a SOC 2 Type II report, available on request under NDA," naming the entity and stating the fact first instead of trailing it after unrelated encryption/scope sentences

ABSENT (2)
  Ledgerline pricing
    → no passage answers this
    → suggested: a passage stating what Ledgerline costs, what the price is billed on (e.g., per seat, per transaction volume, flat fee), and what is included — nothing on the page currently addresses cost

  Ledgerline vs Blackline
    → no passage answers this
    → suggested: a passage naming both Ledgerline and Blackline together and stating at least one concrete, checkable point of difference (e.g., a connector, a matching mechanism, a pricing basis) — Blackline is not mentioned anywhere on the page

WHAT GOOD LOOKS LIKE
  criterion 1 — the answer is in the first sentence
    before: "There is one more piece. Backfills do not belong in migrations at all. A backfill is a data operation, not a schema operation, and it should run as a batched background job that can be paused, resumed, and monitored."
    after:  "Backfills should not run inside database migrations. A backfill is a data operation rather than a schema operation, and running one inside a migration transaction holds a lock for the full duration of the backfill. Run it instead as a batched background job that can be paused, resumed, and monitored — processing in fixed-size chunks, sleeping between batches, and backing off automatically when replication lag crosses a threshold."
