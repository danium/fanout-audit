# Entity drift

Module B compares several descriptions of the same entity and reports where they disagree.

Decompose each description into three components, then compare the components rather than
the sentences:

| Component | Question it answers |
|---|---|
| Category noun | what kind of thing is this |
| Differentiator | what makes it distinct from others of that kind |
| Audience | who is it for |

A skill that flags every surface difference gets ignored after the second run. The
distinction below is what keeps it worth running.

---

## Real drift — flag it

### Category noun changes

The most consequential kind, and the most common. "Platform" in one place, "tool" in
another, "framework" in a third describes three different things to a reader assembling an
answer from multiple sources.

> README: "Ledgerline is an open-source **reconciliation engine** for finance teams"
> Homepage: "The reconciliation **platform** built for multi-processor finance teams"
> Directory: "LedgerLine provides automated **bookkeeping software** for small businesses"
> Bio: "Ledgerline, a **developer tool** for automating transaction matching"

Four category nouns. Engine and platform are close enough to be arguable. "Bookkeeping
software" is a different product category with a different buyer, and "developer tool"
contradicts a finance-team audience. Flag the set, not just the outlier.

### Differentiator changes or disappears

> README: "matches transactions across bank feeds, payment processors, and your general ledger"
> Homepage: "Close your books in hours, not weeks"
> Directory: "helping owners keep their accounts tidy without hiring a bookkeeper"

Three unrelated claims: multi-source matching, speed, and headcount avoidance. The first is
a capability, the second is an outcome, the third is a different outcome for a different
buyer. A description with no differentiator at all is also drift — flag it as missing.

### Audience contradicts

> "for finance teams" / "for multi-processor finance teams" / "for small businesses" /
> "for developers"

The first two are consistent, the second being a narrowing of the first. The third and
fourth each contradict them. Narrowing is not drift. Substitution is.

### The entity name itself varies in form

> Ledgerline / LedgerLine / Ledger Line / ledgerline

Flag it. Capitalisation and spacing variants split the entity across sources that a reader
would otherwise treat as corroborating each other. Pick one form and note where the others
appear.

---

## Acceptable variation — do not flag

### Length

A README first line, a homepage hero, and a docs landing paragraph will differ in length by
design. A one-line version that omits detail present in a longer one is not drift, provided
it does not contradict.

### Tone and register

> README: "Ledgerline is a reconciliation engine. It ingests transaction records, normalizes
> them, and matches them in three passes."
> Homepage: "Stop spending the first week of every month in a spreadsheet."

Different registers, same underlying claim. The homepage leads with the pain and the README
leads with the mechanism. Both describe a reconciliation engine for finance teams doing
multi-source matching. Not drift.

### Reordering the same components

Category-then-audience-then-differentiator versus differentiator-then-category-then-audience
is a stylistic choice.

### Added detail that does not contradict

A docs page that names the three matching passes where the README says only "matches
transactions" has added specificity, not changed the claim.

### Verb choice, article choice, and pluralisation

"Matches transactions" versus "for matching transactions" is not a finding.

---

## Reporting drift

State the component, the variants, and where each appeared. Do not rank the variants by
quality — the user knows which surface matters most to them.

```
CATEGORY NOUN — drift
  reconciliation engine    README, docs landing
  reconciliation platform  homepage hero
  bookkeeping software     directory listing
  developer tool           conference bio

  The directory listing and the bio place this in different categories with
  different buyers than the other three sources.

AUDIENCE — drift
  finance teams            README, homepage
  small business owners    directory listing
  developers               conference bio

NAME FORM — drift
  Ledgerline               README, homepage, docs, bio
  LedgerLine               directory listing

DIFFERENTIATOR — drift
  multi-source matching    README, docs landing
  speed of close           homepage hero
  avoiding a bookkeeper    directory listing
  (absent)                 conference bio
```

---

## The canonical sentence

Propose one sentence that the user can use everywhere.

- One sentence, under 25 words
- Names the entity, its category, and one differentiator
- No superlatives, no "leading", no "powerful", no "best-in-class"
- Must be true

**If the sources conflict on a fact, ask rather than pick.** The sources above disagree on
whether the audience is finance teams or small business owners. That is not a wording
problem you can resolve by choosing the more common variant — it is a question about what
the product actually is, and only the user can answer it. Ask, then write the sentence.

Example, once the audience question is settled in favour of finance teams:

> Ledgerline is a reconciliation engine that matches transactions across payment processors,
> bank feeds, and general ledgers for multi-processor finance teams.

23 words. Names the entity, the category noun, the differentiator, and the audience. Every
component is traceable to the README and the docs landing page — the sourced-claims rule in
SKILL.md applies here exactly as it does in Module A.
