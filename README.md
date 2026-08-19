# fanout-audit

fanout-audit is a Claude Code skill that checks whether a page answers likely AI-search
sub-queries in passages that survive being lifted out.

It audits content for retrievability by an LLM grounding pass, using the two mechanisms from
the Google antitrust record that are actually checkable offline. It runs locally, fetches
nothing, and writes no copy for you.

## The caveat, up front

fanout-audit simulates a plausible query decomposition rather than reproducing Google's,
because the decomposition itself — which sub-queries get issued for a given question, and how
they are chosen — is disclosed nowhere. What Google has published has grown from one sentence
to a few: Search Central now documents that both AI Overviews and AI Mode "may use a 'query
fan-out' technique — issuing multiple related searches across subtopics and data sources,"
and Google states that Deep Search "can issue hundreds of searches." None of that reveals the
decomposition.

Every report fanout-audit produces opens by saying so, because a tool that quietly implies it
knows Google's fan-out is the exact failure it exists to argue against.

Coverage gaps are a **content completeness signal**. They are not a ranking prediction.

## What it does

**Module A — fan-out coverage.** Infers the head query your content targets, decomposes it
into six to twelve sub-queries a fan-out would plausibly produce, and checks whether your
content contains a self-contained passage answering each. Every sub-query lands in one of
three states: covered, present but not extractable, or absent.

If you have the real queries an engine issued — from an API's grounding metadata or an
analytics surface that exposes them — paste them: fanout-audit audits those instead of
simulating, and the report header says so. Observed queries beat simulation whenever they
exist.

Otherwise sub-queries are drawn from nine intent classes, each gated by a yes/no question
about the subject and about what the page claims. The classes carry no genre labels — a news report, an
API reference, and a storefront open different gates because their subjects differ, not because
the skill recognises a content type. Gates also never close because your page omits something,
which is what makes a genuine gap reportable rather than invisible.

**Module B — entity consistency.** Given several descriptions of the same entity — site,
docs, README, directory listings, a conference bio — it detects drift in how you describe
yourself and proposes one canonical sentence.

## Who it is for, and when to run it

fanout-audit is for people who own the content and can edit it: technical writers, docs
owners, founders writing their own pages, and content teams. It assumes you can change
sentences on the page, because every finding it produces is an edit instruction.

Run it when a page targets a question you want to be the answer to, and you want to know
whether your answer would survive being quoted away from the rest of the page. It is most
useful on comparison pages, product docs, how-to guides, and pricing pages — content where a
specific question has a specific answer somewhere in the prose.

**It is not worth running** on a page with no question behind it, on content you cannot edit,
or as a way to decide what to write next. It audits the answer you already wrote; it does not
tell you which questions are worth answering, and it cannot tell you whether anyone is asking
the question your page targets.

One page takes a few minutes. There is no setup, no account, and no data leaves your machine.

## What it does not do

- No rank tracking, keyword volume, or SERP scraping
- No backlink analysis
- No claim to reproduce Google's actual fan-out decomposition
- No score out of 100, no percentage, no grade. Scores invite optimization toward the score
- No fetching. Paste the content or point at a file
- **No writing your facts for you.** When a sub-query is absent, it tells you what a passage
  would need to contain and stops. It will not draft a pricing paragraph, a customer count,
  or a benchmark number that you did not supply

That last one is the point where this skill is most likely to cause harm if it slipped, so
it is enforced rather than suggested. A fluent invented sentence about your own product is
easy to paste without checking, and it goes out under your name.

fanout-audit is not an on-page SEO tool and differs from one in what it examines: it reads
your sentences and asks whether each answer survives being lifted off the page. An on-page SEO
tool reads your markup. Title tags, meta descriptions, schema, `llms.txt`, heading counts and
internal linking are all outside fanout-audit's output contract, and a report that wandered
into them would be a different job done badly.

## Install

Install fanout-audit by cloning it into your agent runtime's skills directory:

```bash
git clone https://github.com/danium/fanout-audit ~/.claude/skills/fanout-audit
```

Then either ask for fanout-audit by name, or just describe the problem — it triggers on AI
Overviews, AI Mode, GEO, LLM citation, AI search visibility, query fan-out, and "why does my
content rank but never get cited".

## Requirements and tested runtimes

fanout-audit is markdown only. It has no dependencies, no build step, and makes no network
calls, so it needs nothing beyond an agent runtime that loads skills from a directory.

**It has been tested on Claude Code and nowhere else.** Every verification run in this
repository was Claude. The skill is largely discipline enforcement — refusing to draft absent
passages, declining out-of-scope findings, stopping on an ambiguous head query — and those are
the properties most likely to degrade on a different or smaller model. Codex, Copilot CLI and
Gemini CLI read `~/.agents/skills/`, so installing there should work, but no verification run
has been done on any of them. Treat non-Claude use as untested rather than supported.

Compatibility statement last verified August 2026.

## The standalone test

Half of the reasoning behind this test is in the record and half is inference, so here is
which is which.

**From the record.** Google's antitrust filing states that FastSearch "delivers results more
quickly than Search because it retrieves fewer documents, but the resulting quality is lower
than Search's fully ranked web results" — lower, but good enough for grounding.

**Observable, though not disclosed.** Links out of Google AI Overviews have carried
scroll-to-text fragments — a `#:~:text=` parameter pointing at a specific sentence on the
destination page. Two honesty notes on that. The fragment syntax is Chrome's generic
text-fragment feature, in browsers since 2020, so its presence shows the product pointing at
individual passages — it is not an AI-specific mechanism. And product behaviour changes
without notice, so check current Overview links yourself before leaning on this; the
observation holds only as long as it reproduces. When present, it is behaviour anyone can
verify, not a claim about internals.

**Not from the record.** Nothing disclosed describes how candidates are chunked or scored, or
states any preference for text that stands alone. That step remains an inference: if a
grounding pass works from a smaller and lower-quality candidate set, and if what comes back is
pointed at sentence-level, then a passage carrying its own context is likelier to be usable
than one that depends on its surroundings.

So treat the standalone test as a defensible inference with an observable foot to stand on,
not a documented mechanism. It is useful because passages that pass it are better writing for
a reader arriving cold, which is true regardless of how any retrieval system works.

**What this skill does not measure.** Whether a passage is *true*, *sourced*, or *deserves* to
be cited. Extractability and credibility are independent axes — well-shaped content with no
verifiable trail behind its claims gets retrieved perfectly well. A clean report here means
your answers can be lifted, not that they should be trusted.

fanout-audit judges every passage against five criteria: the answer leads, references resolve
inside the passage, the entity is named, the length is sane, and the passage still answers the
sub-query once lifted out. Criteria 1, 2, 3 and 5 are gates. Criterion 4 is a length
diagnostic — it never fails a passage on its own, it tells you which other criterion to check.

| # | Criterion |
|---|---|
| 1 | The answer is in the first sentence, not after setup |
| 2 | No unresolved references — "it", "this approach", "as mentioned above" fail unless the antecedent sits inside the passage |
| 3 | The entity is named, not implied |
| 4 | Roughly 40–120 words |
| 5 | Lifted out with nothing around it, it still answers the sub-query |

Criterion 2 is the one authors cannot self-check, because "as mentioned above" is invisible
to the person who wrote the above.

## Worked example

**Input:** a narrative engineering blog post about database migration failures. The post is
good. It explains the expand-and-contract pattern correctly and in detail. It names the
pattern exactly once, in an aside, two-thirds of the way down.

**Output** (abridged — the real report covered six sub-queries):

```
## Fan-out coverage: how to run database schema migrations without causing downtime

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (0)

NOT EXTRACTABLE (6)
  should backfills run inside migrations
    → found in: paragraph 8, unheaded
    → fails: criterion 1 (answer is in sentence two), criterion 2 (opens
             "There is one more piece")
    → fix: delete the transition sentence and open with the answer already
           sitting in sentence two, naming migrations in it

  what expand and contract costs you in practice
    → found in: paragraph 7 ("It is not free.")
    → fails: criterion 1, criterion 2 ("It" and "Developers hate this" have no
             antecedent inside the passage), criterion 3 (pattern never named)
    → fix: replace "It is not free" with a first sentence naming the pattern and
           stating the cost the next sentence already gives

ABSENT (0)
```

Nothing was missing from that post. Every answer was already written; six of six could not
survive being lifted off the page. Those are the cheap fixes, which is why not-extractable is
reported before absent — and why there is no score, because "0 covered" would read as a
failing grade on a post whose only problem is where its sentences start.

## Background

This skill accompanies [an article by Daniil Sokolov](https://x.com/daniilsokolov/status/2088613085162483785).

The disclosed facts under this skill are worth being exact about, since the skill's own rule
is that a claim you cannot point at a source for does not get written down.

**Query fan-out**, in Google's own words, announcing AI Mode in May 2025: "Under the hood, AI
Mode uses our query fan-out technique, breaking down your question into subtopics and issuing
a multitude of queries simultaneously on your behalf."
([blog.google](https://blog.google/products-and-platforms/products/search/ai-mode-search/))

**Search Central documentation** now states it for both surfaces: "Both AI Overviews and AI
Mode may use a 'query fan-out' technique — issuing multiple related searches across subtopics
and data sources."
([developers.google.com](https://developers.google.com/search/docs/appearance/ai-features))
"Data sources" is worth noticing: fan-out is not described as web-only.

**Deep Search**, per Google's own announcement, "can issue hundreds of searches" for a complex
question. That is a Google-published number, quotable with attribution — the no-invented-
numbers rule bans numbers nobody published, not numbers Google did.
([blog.google](https://blog.google/products/search/deep-search-business-calling-google-search/))

**FastSearch**, from the DOJ v. Google filing: it "delivers results more quickly than Search
because it retrieves fewer documents, but the resulting quality is lower than Search's fully
ranked web results," and is "based on RankEmbed signals." Reported during the 2025 remedies
trial. ([Search Engine Land](https://searchengineland.com/google-fastsearch-464557))

**Per-request outputs are partially observable.** Gemini API grounding responses return
`groundingMetadata.webSearchQueries` — "an array of the search queries used" for that request
([ai.google.dev](https://ai.google.dev/gemini-api/docs/generate-content/google-search)). That
is first-party output for one request on one surface, not the algorithm; it tells you what was
asked that time, not how the decomposition is produced.

What remains **undisclosed** is the part this skill simulates: how the decomposition is
chosen, how candidates are chunked or scored, and whether standalone text is favoured. Where
this skill goes beyond the sourced statements above it is inferring, and it says so rather
than borrowing their authority. If you have real observed queries — from an API's grounding
metadata or an analytics surface that exposes them — auditing against those beats any
simulation, and you should prefer them.

## Licence

fanout-audit is released under the MIT licence, which permits commercial use, modification and
redistribution provided the copyright notice is retained. Full text in [LICENSE](LICENSE).
