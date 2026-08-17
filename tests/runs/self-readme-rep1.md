## Fan-out coverage: what is fanout-audit and how does it work

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

**Head query (stated back before decomposition):** what is fanout-audit and how does it
work — the README's single subject throughout: install, mechanism, rationale, licence, and
background all attach to this one entity, so this was audited as one head query, not split.

**Gates opened (8 of 9):** Definitional, Mechanism, Comparison, Qualification, Constraint,
Procedural, Recency, Verification. Evidence stayed closed — the page repeatedly disclaims
outcome/ranking claims ("not a ranking prediction," "does not measure... whether a passage
is true, sourced, or deserves to be cited"), so there is no claimed outcome for a reader to
seek proof of.

COVERED (2)
  what is fanout-audit
    → "fanout-audit is a Claude Code skill that checks whether a page answers likely
      AI-search sub-queries in passages that survive being lifted out."

  is the quoted Google AI Mode statement and the FastSearch DOJ-filing quote sourced and
  verifiable
    → "FastSearch, from the DOJ v. Google filing: it 'delivers results more quickly than
      Search because it retrieves fewer documents, but the resulting quality is lower than
      Search's fully ranked web results,' and is 'based on RankEmbed signals.' Reported
      during the 2025 remedies trial."

NOT EXTRACTABLE (6)
  what five criteria does fanout-audit's standalone test use to judge a passage
    → found in: "The standalone test" section, criteria-table intro
    → fails: criterion 2 (opens "Criteria 1, 2, 3, and 5 are gates" — "Criteria" has no
      antecedent inside the passage), criterion 3 (neither "fanout-audit" nor "the
      standalone test" is named in the passage itself)
    → fix: open with "fanout-audit's standalone test applies five criteria to every
      passage to decide whether it is covered, not extractable, or absent:" before the
      table

  why does fanout-audit simulate sub-queries instead of using Google's actual fan-out
    → found in: "The caveat, up front" section
    → fails: criterion 1 (the direct answer, "So this skill simulates a plausible
      decomposition," is sentence three, after two sentences of setup), criterion 3
      ("this skill" is used throughout instead of "fanout-audit")
    → fix: open with "fanout-audit simulates a plausible decomposition instead of
      Google's actual fan-out because Google discloses only one sentence about how fan-out
      works and never publishes the decomposition itself," then keep the supporting detail

  how is fanout-audit different from an on-page SEO audit tool
    → found in: "What it does not do" section, closing line
    → fails: criterion 2 ("It is also deliberately not an on-page SEO tool" — "It" has no
      antecedent inside the passage), criterion 3 (entity not named)
    → fix: replace "It is also deliberately not an on-page SEO tool" with "fanout-audit is
      not an on-page SEO tool — it does not check title tags, meta descriptions, schema
      markup, llms.txt, heading counts, or internal linking"

  what does fanout-audit cost / what license is it under
    → found in: "Licence" section
    → fails: criterion 5 (states a fact — "MIT" — without explaining it; answers a
      neighbouring question, licence type, rather than cost)
    → fix: state directly that fanout-audit is free to use, rather than leaving cost to be
      inferred from the licence name alone

  what runtimes or tools does fanout-audit require or work with
    → found in: "Install" section, compatibility line
    → fails: criterion 1 (opens with "Personal skills live in your runtime's skills
      directory," which doesn't name fanout-audit or state the requirement), criterion 3
      (the entity appears only inside a URL/path string, never as a stated subject)
    → fix: open with "fanout-audit runs as a skill in Claude Code, and also works with
      Codex, Copilot CLI, and Gemini CLI, which look for skills in ~/.agents/skills/"

  how do you install fanout-audit
    → found in: "Install" section, clone command
    → fails: criterion 1 (the instruction sentence precedes the command with generic
      context — "Personal skills live in your runtime's skills directory" — rather than
      naming the tool and the action)
    → fix: lead with "To install fanout-audit, clone it into your runtime's skills
      directory:" immediately before the existing command

ABSENT (2)
  who is fanout-audit for, or when should someone run it
    → no passage answers this
    → suggested: a sentence naming the intended user (e.g. who writes or edits the content
      being audited) and the situation where running it is worthwhile — and, optionally,
      who it is not for

  is the compatibility list (or the disclosed-facts summary) still current
    → no passage answers this
    → suggested: a last-updated date, version marker, or a pointer to how a reader can
      check whether the Codex/Copilot CLI/Gemini CLI compatibility list or the summarized
      Google disclosures have changed since this was written

WHAT GOOD LOOKS LIKE
  criterion 3 — the entity is named, not implied
    before: "It runs entirely on your machine and never sends your files anywhere."
    after:  "Vaultbackup runs entirely on your machine and never sends your files
            anywhere."
