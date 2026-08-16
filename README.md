# fanout-audit

fanout-audit is a Claude Code skill that checks whether a page answers likely AI-search
sub-queries in passages that survive being lifted out.

It audits content for retrievability by an LLM grounding pass, using the two mechanisms from
the Google antitrust record that are actually checkable offline. It runs locally, fetches
nothing, and writes no copy for you.

## The caveat, up front

Google has published roughly one sentence about query fan-out: AI Mode breaks a question into
subtopics and issues many queries at once. **The decomposition itself is disclosed nowhere** —
not in the trial record, not in Google's documentation.

So this skill simulates a plausible decomposition. It does not replicate Google's, and it
cannot. Every report it produces opens with that statement, because a tool that quietly
implies it knows Google's fan-out is the exact failure it exists to argue against.

Coverage gaps are a **content completeness signal**. They are not a ranking prediction.

## What it does

**Module A — fan-out coverage.** Infers the head query your content targets, decomposes it
into six to twelve sub-queries a fan-out would plausibly produce, and checks whether your
content contains a self-contained passage answering each. Every sub-query lands in one of
three states: covered, present but not extractable, or absent.

**Module B — entity consistency.** Given several descriptions of the same entity — site,
docs, README, directory listings, a conference bio — it detects drift in how you describe
yourself and proposes one canonical sentence.

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

It is also deliberately not an on-page SEO tool. Title tags, meta descriptions, schema
markup, `llms.txt`, heading counts, and internal linking are a different job.

## Install

Personal skills live in your runtime's skills directory:

```bash
git clone https://github.com/danium/fanout-audit ~/.claude/skills/fanout-audit
```

Codex, Copilot CLI, and Gemini CLI also recognise `~/.agents/skills/`.

Then ask for it by name, or just describe the problem — the skill triggers on AI Overviews,
AI Mode, GEO, LLM citation, AI search visibility, query fan-out, and "why does my content
rank but never get cited".

## The standalone test

Half of the reasoning behind this test is in the record and half is inference, so here is
which is which.

**From the record.** Google's antitrust filing states that FastSearch "delivers results more
quickly than Search because it retrieves fewer documents, but the resulting quality is lower
than Search's fully ranked web results" — lower, but good enough for grounding.

**Not from the record.** Nothing disclosed describes passage- or chunk-level retrieval, or any
preference for text that stands alone. That step is an inference: if a grounding pass works
from a smaller and lower-quality candidate set, a passage carrying its own context is likelier
to be usable than one that depends on its surroundings.

So treat the standalone test as a defensible inference, not a documented mechanism. It is
useful because passages that pass it are better writing for a reader arriving cold, which is
true regardless of how any retrieval system works.

Criteria 1, 2, 3, and 5 are gates. Criterion 4 is a length diagnostic — it never fails a
passage on its own, it tells you which other criterion to check.

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

Two disclosed facts sit under this skill, and it is worth being exact about both, since the
skill's own rule is that a claim you cannot point at a source for does not get written down.

**Query fan-out**, in Google's own words, announcing AI Mode in May 2025: "Under the hood, AI
Mode uses our query fan-out technique, breaking down your question into subtopics and issuing
a multitude of queries simultaneously on your behalf."
([blog.google](https://blog.google/products-and-platforms/products/search/ai-mode-search/))

**FastSearch**, from the DOJ v. Google filing: it "delivers results more quickly than Search
because it retrieves fewer documents, but the resulting quality is lower than Search's fully
ranked web results," and is "based on RankEmbed signals." Reported during the 2025 remedies
trial. ([Search Engine Land](https://searchengineland.com/google-fastsearch-464557))

That is the whole disclosed surface. The actual fan-out decomposition, how many sub-queries
are issued, how candidates are chunked or scored, and whether standalone text is favoured are
all **undisclosed**. Where this skill goes beyond the two quotes above it is inferring, and it
says so rather than borrowing their authority.

## Licence

MIT. See [LICENSE](LICENSE).
