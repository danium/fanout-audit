## Fan-out coverage: reconciliation software for finance teams with multiple payment processors

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (3)
  what is Ledgerline
    → Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger.

  what payment processors and banks does Ledgerline connect to
    → Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side.

  how to get started with Ledgerline
    → Create a workspace and invite your finance team.

NOT EXTRACTABLE (4)
  how does automated transaction matching work
    → found in: "How matching works" section, opening paragraph
    → fails: criterion 5 (states matching runs in three passes without explaining what they do; the explanation lives in three separate later paragraphs, none of which is self-contained)
    → fix: write one consolidated passage naming Ledgerline that briefly explains what each of the three passes does, within roughly 90 words

  how are many-to-one bank deposits matched to transactions
    → found in: "How matching works" section, third-pass paragraph
    → fails: criterion 2 ("The third pass" is an ordinal reference to a first and second pass that are not established inside this passage)
    → fix: open with "Ledgerline's aggregation pass..." instead of "The third pass" so the passage does not depend on the two preceding paragraphs

  what happens to transactions Ledgerline cannot match
    → found in: "Handling exceptions" section
    → fails: criterion 3 (entity never named — "the engine" is used in place of "Ledgerline," and the second paragraph has no subject at all)
    → fix: open the first sentence with "Ledgerline" as the subject and replace "the engine" with "Ledgerline"

  how long does implementation take
    → found in: "Getting started" section, closing sentence
    → fails: criterion 1 (the answer, "under a week," appears after five setup steps rather than first)
    → fix: state the timeframe in the opening sentence of the section, before the numbered steps

ABSENT (4)
  Ledgerline vs reconciling in a spreadsheet
    → no passage answers this
    → suggested: a passage naming Ledgerline directly against the manual spreadsheet workflow, stating what Ledgerline specifically replaces or automates that the spreadsheet process requires by hand

  is Ledgerline worth it for a team with only one payment processor
    → no passage answers this
    → suggested: a statement of the complexity level (processor count, currency count, or volume) at which Ledgerline is intended to help, and whether simpler setups are better served by another approach

  what does Ledgerline cost
    → no passage answers this
    → suggested: a sentence giving the billing basis and what is included in a plan

  is the connector list current
    → no passage answers this
    → suggested: a last-verified or last-updated date on the connector list, or a link to a live-maintained connector status page
