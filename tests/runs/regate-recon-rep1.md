## Fan-out coverage: reconciliation software for finance teams with multiple payment processors

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (2)
  what is Ledgerline
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger."

  what payment processors and banks does Ledgerline connect to
    → "Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side."

NOT EXTRACTABLE (6)
  how does Ledgerline match transactions
    → found in: "How matching works", opening paragraph
    → fails: criterion 5 (states "Matching then runs in three passes" without explaining what any pass does — lifted alone it answers nothing about how matching works)
    → fix: fold a one-line summary of each pass's job into this paragraph, or replace the "three passes" sentence with three short defining clauses

  how does fuzzy transaction matching decide a threshold
    → found in: "How matching works", second-pass paragraph
    → fails: criterion 2 (opens "The second pass," assuming an undescribed first pass; "Remaining records" carries the same assumption), criterion 3 (Ledgerline is never named in this paragraph)
    → fix: open with "Ledgerline's fuzzy-match pass..." instead of "The second pass," naming the product so the paragraph stands without the passage before it

  how does Ledgerline match many-to-one bank deposits
    → found in: "How matching works", third-pass paragraph
    → fails: criterion 2 (opens "The third pass," assuming two undescribed prior passes)
    → fix: replace "The third pass is aggregation matching" with a self-contained opener, e.g. "Ledgerline's aggregation pass matches many processor records to one bank deposit by..."

  what happens to transactions Ledgerline cannot match
    → found in: "Handling exceptions", first paragraph
    → fails: criterion 3 (passage refers to "the engine," never names Ledgerline)
    → fix: replace "the engine" with "Ledgerline" in the first sentence

  how much transaction volume does Ledgerline match automatically
    → found in: "How matching works", first-pass paragraph
    → fails: criterion 1 (the 80-90 percent figure lands in sentence three, not the first), criterion 2 (opens "The first pass," assuming an unstated sequence), criterion 3 (Ledgerline is never named in this paragraph)
    → fix: open with "Ledgerline's exact-match pass clears 80 to 90 percent of volume for most teams," naming the product and leading with the figure, then explain the matching rule after

  how do you get started with Ledgerline
    → found in: "Getting started", numbered list
    → fails: criterion 3 (the list opens "Create a workspace..." with no entity named; Ledgerline appears only in steps 3 and 5, so a chunk beginning at step 1 or 2 names no product)
    → fix: add a lead-in sentence before the numbered list — "Getting started with Ledgerline:" — so the entity travels with the list even if separated from its heading

ABSENT (4)
  Ledgerline vs reconciling in a spreadsheet
    → no passage answers this
    → suggested: a passage naming Ledgerline and stating directly what it replaces or changes relative to the manual spreadsheet process already described in "Why reconciliation breaks" — that section describes spreadsheet pain without naming the product, and no passage connects the two

  who is Ledgerline for
    → no passage answers this
    → suggested: a sentence giving a concrete fit signal — a processor-count, currency-count, or transaction-volume threshold where Ledgerline becomes worthwhile — beyond the generic "for finance teams" phrase already used to answer the definitional query; optionally, who it is not built for

  Ledgerline pricing
    → no passage answers this
    → suggested: a sentence giving the billing basis and what is included at each plan, or a link to where pricing is disclosed

  is the connector list current
    → no passage answers this
    → suggested: a last-updated date or changelog reference near the connector list (Stripe, Adyen, Braintree, PayPal, Shopify Payments, Plaid-backed feeds, NetSuite, Xero, QuickBooks Online) so a reader can judge whether it still reflects current support
