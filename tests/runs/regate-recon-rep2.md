## Fan-out coverage: reconciliation software for finance teams with multiple payment processors

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (4)
  what is Ledgerline
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger." (20 words; short but complete)

  who is Ledgerline for
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger." (audience stated: "for finance teams")

  what payment processors, banks, and general ledgers does Ledgerline connect to
    → "Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side."

  how do you get started with Ledgerline
    → "Create a workspace and invite your finance team." (step 1 of the "Getting started" list; Ledgerline is named in steps 3 and 5 of the same list)

NOT EXTRACTABLE (5)
  how does Ledgerline's transaction matching work
    → found in: "How matching works" section, opening paragraph
    → fails: criterion 5 ("Matching then runs in three passes" states the fact without explaining it; the actual explanation is split across three separate paragraphs, so no single passage covers the mechanism)
    → fix: write one passage naming Ledgerline as the subject that describes all three passes together, in place of the current one-line preview

  how does Ledgerline match one bank deposit to many processor transactions
    → found in: "How matching works" section, third-pass paragraph
    → fails: criterion 2 ("The third pass is aggregation matching" opens with an ordinal that assumes passes one and two, described only in the preceding paragraphs)
    → fix: open with "Ledgerline's aggregation pass matches..." instead of "The third pass is..."

  what happens to transactions Ledgerline cannot match
    → found in: "Handling exceptions" section
    → fails: criterion 3 (the section says "the engine," never "Ledgerline," in both paragraphs)
    → fix: replace "the engine" with "Ledgerline" in the opening paragraph

  is Ledgerline SOC 2 compliant
    → found in: "Security" section
    → fails: criterion 1 (the SOC 2 line is the third sentence, after unrelated encryption and scope details)
    → fix: open the passage with the compliance answer, e.g. "Ledgerline has a SOC 2 Type II report, available on request under NDA," then follow with the encryption and scope details

  how long does Ledgerline implementation take
    → found in: "Getting started" section, closing sentence
    → fails: criterion 2 ("this" has no antecedent inside the sentence itself), criterion 3 (Ledgerline is not named in the sentence)
    → fix: rewrite as "Most teams complete Ledgerline's getting-started process in under a week," naming the entity and dropping the dangling "this"

ABSENT (3)
  Ledgerline vs reconciling in a spreadsheet
    → no passage answers this
    → suggested: a passage that directly names Ledgerline against manual/spreadsheet reconciliation and states what Ledgerline automates that the spreadsheet workflow (described in "Why reconciliation breaks") requires by hand

  what does Ledgerline cost
    → no passage answers this
    → suggested: a sentence stating what Ledgerline costs and its billing basis, and what a plan includes

  is Ledgerline's connector list current
    → no passage answers this
    → suggested: a last-verified date, version marker, or link to a canonical/live connector list, so a reader can tell whether the processor, bank, and general-ledger list is still accurate
