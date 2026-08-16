## Fan-out coverage: reconciliation software for finance teams with multiple payment processors

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (3)
  what is Ledgerline
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger." (20 words; short but complete)

  who is Ledgerline for
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger." (20 words; short but complete)

  what payment processors, banks, and general ledgers does Ledgerline connect to
    → "Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side."

NOT EXTRACTABLE (7)
  how does Ledgerline's transaction matching work
    → found in: "How matching works" section, per-pass paragraphs
    → fails: criterion 2 (the second- and third-pass paragraphs open with "The second pass," "The third pass," and "Remaining records," none of which have an antecedent inside their own paragraph)
    → fix: write one passage naming Ledgerline as the subject that describes all three passes together, replacing ordinal references like "the second pass" with self-contained phrasing such as "Ledgerline's fuzzy-matching pass"

  how does Ledgerline match one bank deposit to many processor transactions
    → found in: "How matching works" section, third-pass paragraph
    → fails: criterion 2 ("The third pass is aggregation matching" has no antecedent for "third" inside the passage)
    → fix: open with "Ledgerline's aggregation pass matches one bank deposit to many processor transactions by..." instead of "The third pass..."

  Ledgerline vs reconciling in a spreadsheet
    → found in: "Why reconciliation breaks" section
    → fails: criterion 5 (answers a neighboring question — why manual reconciliation is painful — not the comparison itself), criterion 3 (Ledgerline is never named anywhere in this section)
    → fix: add a passage in this section stating directly what Ledgerline does differently from the spreadsheet approach, naming Ledgerline as the subject

  what happens to transactions Ledgerline cannot match
    → found in: "Handling exceptions" section
    → fails: criterion 3 (the section refers only to "the engine," never to "Ledgerline" by name)
    → fix: replace "the engine" with "Ledgerline" in this section

  is Ledgerline SOC 2 compliant
    → found in: "Security" section
    → fails: criterion 1 (the SOC 2 line is the third sentence, after encryption and scope details)
    → fix: open the passage with the compliance answer, e.g. "Ledgerline is SOC 2 Type II compliant; a report is available on request under NDA," then follow with the encryption and scope details

  how long does Ledgerline implementation take
    → found in: "Getting started" section, closing sentence
    → fails: criterion 2 ("this" has no antecedent inside the sentence itself), criterion 3 (Ledgerline is not named in the sentence)
    → fix: rewrite as "Most teams complete Ledgerline's onboarding in under a week," naming both the entity and what "this" refers to

  how do you get started with Ledgerline
    → found in: "Getting started" section, numbered list
    → fails: criterion 3 (three of the five steps — create a workspace, connect a source, review the exception queue — never name Ledgerline; a chunker that splits per list item strips the subject from most of them)
    → fix: name Ledgerline as the subject of each step (e.g., "In Ledgerline, create a workspace...") instead of only steps 3 and 5

ABSENT (2)
  what does Ledgerline cost
    → no passage answers this
    → suggested: a passage stating the billing basis and what is included in a plan

  is Ledgerline's connector list current
    → no passage answers this
    → suggested: a date or "last verified" marker attached to the connector list (Stripe, Adyen, Braintree, PayPal, Shopify Payments, the Plaid-backed banks, NetSuite, Xero, QuickBooks Online)
