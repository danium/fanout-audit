## Fan-out coverage: reconciliation software for finance teams with multiple payment processors

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (4)
  what is Ledgerline
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger."

  who is Ledgerline for
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger."

  what payment processors and banks does Ledgerline connect to
    → "Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side."

  how do you get started with Ledgerline
    → "1. Create a workspace and invite your finance team."

NOT EXTRACTABLE (3)
  how does Ledgerline's fuzzy matching decide a threshold
    → found in: "How matching works," second-pass paragraph
    → fails: criterion 1 (the threshold logic arrives in sentence 3, not the first sentence), criterion 2 (opens "The second pass" and "Remaining records," both assuming a first pass that is not described inside this passage), criterion 3 (Ledgerline is never named anywhere in this paragraph)
    → fix: open with "Ledgerline's fuzzy matching pass scores unmatched records on amount proximity, date proximity, and counterparty string similarity, matching pairs above a per-source confidence threshold and queueing the rest for review," naming Ledgerline and dropping the ordinal and pointer references

  how does Ledgerline match a bank deposit that corresponds to many transactions
    → found in: "How matching works," third-pass paragraph
    → fails: criterion 2 (opens "The third pass is aggregation matching," an ordinal reference to a first and second pass that are not present in this passage)
    → fix: open with "Aggregation matching handles the case where one deposit on the bank side corresponds to many individual transactions on the processor side" and drop the "third pass" framing

  what happens to transactions Ledgerline cannot match
    → found in: "Handling exceptions" section
    → fails: criterion 3 (the passage says "the engine," never "Ledgerline")
    → fix: replace "the engine" with "Ledgerline" in the first two sentences of the section

ABSENT (3)
  Ledgerline vs reconciling in a spreadsheet
    → no passage answers this
    → suggested: a passage that names Ledgerline directly against manual/spreadsheet reconciliation and states what specifically changes — the "Why reconciliation breaks" section describes spreadsheet pain points but never names Ledgerline or states the contrast

  what does Ledgerline cost
    → no passage answers this
    → suggested: a passage giving the billing basis and what a plan includes

  is Ledgerline's connector list current
    → no passage answers this
    → suggested: a dated "last verified" note or version marker on the Connectors section so a reader can tell whether the processor, bank, and general-ledger list still reflects current support
