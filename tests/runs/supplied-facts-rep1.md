## Fan-out coverage: reconciliation software for finance teams with multiple payment processors

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (4)
  what is Ledgerline
    → "Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger."

  how does Ledgerline match transactions
    → "Ledgerline ingests transaction records from each connected source and normalizes them into a common shape: amount, currency, timestamp, counterparty, and an opaque source reference."

  what payment processors and banks does Ledgerline connect to
    → "Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side."

  how do I get started with Ledgerline
    → "1. Create a workspace and invite your finance team."

NOT EXTRACTABLE (3)
  how does Ledgerline handle many-to-one bank deposits
    → found in: "How matching works," third-pass paragraph
    → fails: criterion 2 (opens "The third pass is aggregation matching" — no first or second pass is named inside this paragraph, so "third" has no antecedent if the paragraph travels alone)
    → fix: open with "Ledgerline's aggregation-matching pass handles..." instead of "The third pass..."

  Ledgerline vs reconciling in a spreadsheet
    → found in: "Why reconciliation breaks"
    → fails: criterion 3 (Ledgerline is never named anywhere in this section), criterion 5 (the passage explains why manual reconciliation is painful but never states what Ledgerline itself does differently — it answers a neighbouring question)
    → fix: add a sentence naming Ledgerline and stating directly what it replaces in the manual CSV-matching process described here

  what happens to transactions Ledgerline can't match
    → found in: "Handling exceptions"
    → fails: criterion 3 ("the engine" and "it" stand in for the product throughout; Ledgerline is never named in this section)
    → fix: replace "the engine" with "Ledgerline" in the section's opening sentence

ABSENT (3)
  who is Ledgerline for
    → no passage answers this
    → suggested: a sentence stating what team size, transaction volume, or processor count makes Ledgerline a fit — and ideally where a spreadsheet is still fine

  Ledgerline pricing
    → no passage answers this
    → suggested: a passage stating plan prices, what each plan includes, setup fee status, and trial length — drafted below from facts you supplied

  is Ledgerline's connector list current
    → no passage answers this
    → suggested: a date or version marker on the Connectors section so a reader can tell if the list is current

WHAT GOOD LOOKS LIKE
  criterion 3 — the entity is named
    before: - Full request waterfalls across HTTP, gRPC, and Postgres
            - Automatic service dependency maps built from observed traffic
            - p50, p95, and p99 latency per endpoint, per service, per version
    after:  Cabletrace connects to Kubernetes clusters running version 1.24 or later and to bare-metal Linux hosts, in both cases requiring Linux kernel 5.8 or newer. Cabletrace traces HTTP, gRPC, and Postgres traffic, and reports p50, p95, and p99 latency per endpoint, per service, and per version. Cabletrace does not support Windows containers or kernels older than 5.8, because the eBPF features it depends on are not present in them.

---

## Pricing passage (drafted from facts you supplied)

This is not part of the audit above — the audit reports pricing as ABSENT because the page
currently contains none. You supplied the facts in chat, so drafting is permitted from those
facts only. Nothing below traces to anything except what you told me.

> Ledgerline pricing has two plans, Starter and Growth. Starter costs $49 per month and
> includes 3 connected sources. Growth costs $199 per month and includes 10 connected
> sources. There is no setup fee, and Ledgerline offers a 14-day free trial.

40 words. Passes the standalone test: answer in the first sentence, no unresolved references,
Ledgerline named, and it holds up lifted out with nothing around it.
