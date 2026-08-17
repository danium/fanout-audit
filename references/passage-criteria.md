# The standalone test

Criteria 1, 2, 3, and 5 are gates — one failure among them is a failure. Criterion 4 is a
length diagnostic; see its section below for how it resolves.

| # | Criterion |
|---|---|
| 1 | The answer is in the first sentence, not after setup |
| 2 | No unresolved references — "it", "this approach", "as mentioned above" fail unless the antecedent sits inside the passage |
| 3 | The entity is named, not implied |
| 4 | Roughly 40–120 words |
| 5 | Lifted out with nothing around it, it still answers the sub-query |

The fail examples below matter more than the pass examples. Criterion 2 in particular is
invisible to the person who wrote the text, because they wrote the antecedent too and their
eye supplies it automatically.

---

## Criterion 1 — the answer is in the first sentence

**Sub-query:** *should backfills run inside migrations*

### Fails

> There is one more piece. Backfills do not belong in migrations at all. A backfill is a
> data operation, not a schema operation, and it should run as a batched background job that
> can be paused, resumed, and monitored.

The answer is correct and it is in sentence two. Sentence one is a transition. A passage
retrieved and quoted in isolation leads with "There is one more piece," which answers
nothing and signals to a reader that they have arrived mid-argument.

### Passes

> Backfills should not run inside database migrations. A backfill is a data operation rather
> than a schema operation, and running one inside a migration transaction holds a lock for
> the full duration of the backfill. Run it instead as a batched background job that can be
> paused, resumed, and monitored — processing in fixed-size chunks, sleeping between
> batches, and backing off automatically when replication lag crosses a threshold.

Answer first, then the reason, then the method.

**The fix instruction for this failure is always the same shape:** move the answer to the
front of the passage. It rarely requires new facts, which is why NOT EXTRACTABLE items are
the cheap ones.

---

## Criterion 2 — no unresolved references

**Sub-query:** *how do you split a schema change into two deploys*

### Fails

> The thing that actually worked was splitting every schema change into two deploys that are
> individually reversible. In the first deploy, you make the additive change only. In the
> second deploy, which happens after the first has been stable for at least one full business
> day, you switch the application to read from the new shape and only then remove the old one.

"The thing that actually worked" points at four paragraphs that will not travel with this
one. Worked for whom, against what problem, compared to what else that did not work? Quoted
alone, the passage opens by referring to something the reader cannot see.

### Also fails

> As mentioned above, the second pass is fuzzy matching. Remaining records are scored against
> each other on amount proximity, date proximity, and counterparty string similarity.

Two failures. "As mentioned above" is an explicit pointer out of the passage, and "the second
pass" and "remaining records" both assume a first pass the passage never describes.

### Passes

> Expand and contract splits a database schema change into two separately deployed steps, each
> of which can be reversed on its own. The expand deploy makes only additive changes — a
> nullable column, a new table, an index created concurrently — while the application still
> reads the old shape. The contract deploy, run once the expand deploy has been stable for at
> least one full business day, switches reads to the new shape and removes the old one.

Every reference resolves inside the passage. "The expand deploy" is defined by the sentence
it appears in, not by an earlier section.

### Also fails, in time rather than in space

**Sub-query:** *is the northern rail spur still closed*

> The spur has been closed since the fire, and no timeline exists yet for reopening it. Crews
> have recently begun clearing the site, and the operator said last week that a decision on
> rebuilding is still some way off.

"Since the fire", "recently", and "last week" are unresolved references exactly as "as
mentioned above" is — they resolve against the moment of writing, which does not travel with
the passage. Lifted out, this asserts a state without saying when that state held, and a reader
arriving a year later cannot tell whether any of it is still true.

Fix by resolving the reference inside the passage: name the month and year, or give the
version, rather than pointing at the moment of writing.

**Watch for these openers.** Each is a near-certain criterion 2 failure: *That, This, It,
There is one more, The thing that, As mentioned, As we saw, Building on this, Instead, The
second, Remaining, Such, These* — and, for the temporal form: *recently, currently, as of
writing, at present, last year, last week, these days, now, today, still.*

---

## Criterion 3 — the entity is named

**Sub-query:** *what does this tool connect to*

### Fails

> - Full request waterfalls across HTTP, gRPC, and Postgres
> - Automatic service dependency maps built from observed traffic
> - p50, p95, and p99 latency per endpoint, per service, per version

The densest facts on the page, attributable to nobody. Retrieved as a chunk without its
parent heading, this describes a capability set belonging to no named product. A model
either drops it or credits it to whichever entity the surrounding context suggests — which
may be a competitor.

Bullet fragments fail this criterion routinely, because bullets drop the subject by
convention.

### Passes

> Cabletrace connects to Kubernetes clusters running version 1.24 or later and to bare-metal
> Linux hosts, in both cases requiring Linux kernel 5.8 or newer. Cabletrace traces HTTP,
> gRPC, and Postgres traffic, and reports p50, p95, and p99 latency per endpoint, per
> service, and per version. Cabletrace does not support Windows containers or kernels older
> than 5.8, because the eBPF features it depends on are not present in them.

Named three times, once per claim cluster. This reads slightly repetitively to a human
reading top to bottom, and that is the correct trade — the passage is being written for
retrieval in isolation, not for flow.

---

## Criterion 4 — roughly 40 to 120 words

### Fails, too short

> Ledgerline matches transactions in three passes.

Nine words. True, names the entity, answers nothing. A reader arriving at this from a
mechanism query learns that three passes exist and nothing about what they do.

### Fails, too long

A 400-word section that opens with the answer and then spends the remainder on background,
a customer story, and a tangent about spreadsheet history. It carries context the answer does
not need, which is the tell: the passage is not self-contained, it is self-*sufficient* by
brute force. Chunkers will split it, and the split will not respect the boundary between the
answer and the padding.

### Passes

> Ledgerline matches transactions in three passes. The exact pass pairs records agreeing on
> amount, currency, and date within a one-day window, which clears most volume for typical
> teams. The fuzzy pass scores remaining records on amount proximity, date proximity, and
> counterparty string similarity, auto-matching those above a per-source confidence threshold
> and queueing the rest for review. The aggregation pass searches for subsets of unmatched
> processor records that sum to a single unmatched bank deposit, within a tolerance for fees.

Roughly 90 words. Complete, and nothing in it is scaffolding.

### How this criterion resolves

The range is a diagnostic, not a gate. Length on its own never fails a passage — it tells you
which other criterion to check.

- **Under 40 words, complete:** a crisp definitional answer can be shorter than 40 words and
  still name the entity, answer the sub-query, and resolve its own references. Mark it
  covered and note the length. Do not record a criterion 4 failure.
- **Under 40 words, incomplete:** the passage states a fact without explaining it, as in the
  nine-word example above. That is a **criterion 5** failure — lifted out, it does not answer
  the sub-query — and you record it as criterion 5.
- **Over 120 words:** re-run criterion 5. Length that far past the range usually means the
  passage is carrying setup, narrative, or a tangent it should not need, and criterion 5 is
  where that shows up.

The reason for routing it this way: "too short" and "too long" are symptoms, and the report is
more useful to an author when it names the disease. "Fails criterion 4, 21 words" tells them
to pad. "Fails criterion 5, states the fact without explaining it" tells them what to add.

---

## Criterion 5 — it survives being lifted out

The composite test. Delete everything around the passage and read it cold, as someone who
has never seen the page, arriving from the sub-query.

Three questions:

1. Does it answer the sub-query, or does it answer a *neighbouring* question? A passage
   explaining why manual reconciliation breaks does not answer what reconciliation software
   is, however adjacent.
2. Could a reader attribute it? If the entity is unnamed, no.
3. Does anything in it assume a fact stated elsewhere on the page?

### Fails

> Cabletrace will show spans your manual instrumentation missed, and your manual
> instrumentation will show application-internal spans that Cabletrace cannot see, because it
> observes syscalls and not function calls. This asymmetry is the main thing to understand
> before you commit.

Well written, genuinely useful, and it fails. It assumes the reader knows a comparison is
underway, knows what "your manual instrumentation" refers to, and knows what they are
committing to. It answers a question the surrounding page asked, not one a searcher asked.

### Passes

> Migrating from OpenTelemetry to Cabletrace does not preserve trace IDs. A request that
> begins under OpenTelemetry instrumentation and completes under Cabletrace appears as two
> separate traces for the duration of the transition. Teams running alerting keyed on trace
> continuity should expect noise during the overlap window and silence those alerts in
> advance.

Names both entities, answers the sub-query in the first sentence, resolves its own
references, and states the consequence. Nothing outside it is required.

---

### A completeness shape for criterion 5

"Still answers the sub-query" is the vaguest of the five. When a passage is arguably complete
and arguably not, check whether it carries three things:

| Part | Question |
|---|---|
| Claim | What is being asserted? |
| Evidence | What figure, mechanism, or example supports it? |
| Qualification | Under what conditions does it hold, or stop holding? |

A passage with the claim alone is the classic criterion 5 failure — it states a fact without
explaining it, and a reader arriving cold learns that something is true without learning
enough to use it. A passage with claim and evidence but no qualification usually reads as
overclaiming once lifted away from the hedges in its neighbours.

This is a diagnostic, not a gate. Plenty of good passages carry only claim and evidence, and a
definitional passage may need nothing else. Use it when you are unsure whether to call
criterion 5, not as a fourth thing to enforce.

## When the passage is a table row

The five criteria are written for prose, and applying them to prose is most of the job. But
comparison data, pricing, specifications, and support matrices often live in tables, and the
answer to a sub-query is sometimes a single row.

**Apply the criteria to the row, not to the table.** A table is a container; the retrievable
unit is smaller. The failure this exposes is specific and common:

| Plan | Included sources | Retention |
|---|---|---|
| Starter | 3 | 30 days |
| Growth | 10 | 12 months |

Lifted alone, the row `Growth | 10 | 12 months` answers nothing. "10" and "12 months" are
values whose attribute names sit in a header that did not travel with them, and no entity is
named anywhere in the row. That is a **criterion 2** failure (unresolved reference — the header
is the antecedent) and a **criterion 3** failure (entity implied, not named), recorded as
those, not as a new criterion.

Two fixes, and the second is usually better:

- Restate the attribute inside each row, so a row reads as a fact rather than a value.
- Put one prose sentence immediately above or below the table stating the comparison's
  conclusion, naming the entity and the dimension compared. The table then carries the detail
  and the sentence carries the answer.

**Do not treat "this should be a table" as a finding.** Whether prose or markup extracts better
is not something this skill knows, and recommending markup changes is the on-page SEO job the
output contract excludes. The finding is always about whether the answer survives being lifted
out — a table row that names its own attributes passes, and a paragraph that does the same
passes equally.

## Recording a failure

In the NOT EXTRACTABLE section, name the criterion by number and give one concrete
instruction:

```
NOT EXTRACTABLE (2)
  should backfills run inside migrations
    → found in: paragraph 8, unheaded
    → fails: criterion 1 (answer after setup), criterion 2 (opens "There is one more piece")
    → fix: open the passage with "Backfills should not run inside database migrations."
            and drop the transition sentence

  what does the tool connect to
    → found in: "What you get" bullet list
    → fails: criterion 3 (entity never named in the block)
    → fix: convert the bullets to full sentences, each naming the product as subject
```

The fix is an instruction, not a draft. One line, imperative, pointing at what to change.
