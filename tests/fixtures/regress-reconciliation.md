# Ledgerline

Ledgerline is a reconciliation engine for finance teams that matches transactions across bank feeds, payment processors, and your general ledger.

## Why reconciliation breaks

Most finance teams reconcile in a spreadsheet. Someone exports a CSV from Stripe, another from the bank, a third from NetSuite, and then spends the first four days of the month lining up rows by hand. The work is not hard. It is just enormous, and it is enormous every single month.

The problem gets worse as payment surface area grows. A company with one processor and one bank account has two files to compare. A company with three processors, two currencies, and a handful of marketplace payouts has a combinatorial mess, and the spreadsheet approach stops scaling somewhere around the second currency.

## How matching works

Ledgerline ingests transaction records from each connected source and normalizes them into a common shape: amount, currency, timestamp, counterparty, and an opaque source reference. Matching then runs in three passes.

The first pass is exact matching. Records that agree on amount, currency, and date within a one-day window are paired immediately. In practice this clears 80 to 90 percent of volume for most teams.

The second pass is fuzzy matching. Remaining records are scored against each other on amount proximity, date proximity, and counterparty string similarity. Pairs above the confidence threshold are matched automatically; pairs below it are queued for review. The threshold is configurable per source, because a bank feed with clean counterparty names deserves a different threshold than a marketplace payout file that truncates merchant names to eight characters.

The third pass is aggregation matching, which handles the case where one deposit on the bank side corresponds to many individual transactions on the processor side. This is the case that breaks naive tools. Ledgerline searches for subsets of unmatched processor records that sum to an unmatched bank deposit, within a tolerance for fees.

## Connectors

Ledgerline connects to Stripe, Adyen, Braintree, PayPal, and Shopify Payments on the processor side. On the banking side it supports Plaid-backed feeds for most US and Canadian institutions, plus direct BAI2 and CAMT.053 file ingest for institutions that do not offer an API. General ledger connectors exist for NetSuite, Xero, and QuickBooks Online.

If your source is not on the list, Ledgerline accepts a generic CSV with a column mapping you define once and reuse.

## Handling exceptions

Unmatched records land in an exception queue. Each exception carries the candidate matches the engine considered and the reason it declined to auto-match: amount mismatch beyond tolerance, no counterparty overlap, date outside window, or ambiguity between two equally plausible candidates.

Reviewers can accept a candidate, split a record, or write off a difference with a reason code. Every decision is logged with the reviewer, the timestamp, and the rule state at the time, which is what auditors ask for.

## Getting started

1. Create a workspace and invite your finance team.
2. Connect at least one processor and one bank source.
3. Run a historical backfill over the last closed month. Ledgerline will match against a period you have already reconciled by hand, so you can compare its output to a known-good answer before trusting it forward.
4. Review the exception queue and tune thresholds per source.
5. Once the backfill matches your manual close, switch the current period to Ledgerline as the source of truth.

Most teams complete this in under a week.

## Security

All connector credentials are stored encrypted at rest. Ledgerline requests read-only scopes from every processor and bank connector; it never initiates transfers. SOC 2 Type II report available on request under NDA.
