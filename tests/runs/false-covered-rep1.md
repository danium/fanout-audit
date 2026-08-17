## Fan-out coverage: Havenlock networked door controllers for commercial buildings

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

Head query stated back: "Havenlock networked door controllers for commercial buildings" — what
they are and how they behave (offline continuity, credential caching, power). The page targets
this one entity throughout; no second head query was detected.

Gates open: Definitional, Mechanism, Comparison, Qualification, Constraint, Evidence,
Procedural, Recency (8 of 9). Verification is closed — nothing on the page asserts a dated or
datable event.

COVERED (1)
  what are Havenlock door controllers
    → Havenlock builds networked door controllers for commercial buildings.
    (27 words — under the 40-word guideline but complete: names the entity, states the category,
    and states the core behavior. Marked covered per the length-diagnostic rule, not penalized
    for criterion 4.)

NOT EXTRACTABLE (2)
  how do Havenlock controllers make access decisions when the network is down
    → found in: "Credential caching" section
    → fails: criterion 5 (states that credentials are cached locally but never explains how that,
      combined with the access rules the intro says are "held locally," produces an actual offline
      access decision — a fact stated, not explained)
    → fix: merge this sentence with the intro's "holds the access rules locally" clause and add
            one sentence on how a presented credential is checked against the local cache to
            decide access without the network

  does Havenlock's power-loss backup actually complete an in-progress unlock
    → found in: "Power" section
    → fails: criterion 3 (subject is "each controller," Havenlock is never named in this
      passage), criterion 5 (restates the capability claim with no supporting detail, so lifted
      out it repeats the claim rather than substantiating it)
    → fix: name Havenlock as the subject of the sentence, and state what stores the charge and
            for how long, rather than asserting the outcome alone

ABSENT (5)
  how do Havenlock's local door controllers compare to cloud-dependent access control systems
    → no passage answers this
    → suggested: a passage naming Havenlock and stating how its locally-cached, offline-capable
      model differs from access control systems that require a live network or cloud connection
      to make access decisions

  what kinds of buildings or teams are Havenlock controllers suited for
    → no passage answers this (the phrase "for commercial buildings" in the opening sentence
      scopes the product category — it does not address fit, or who should or should not adopt it)
    → suggested: a passage naming Havenlock and stating what building types, security
      requirements, or team situations it fits, and ideally what it does not fit

  how long does a Havenlock controller cache credentials before falling back to a default rule
    → no passage answers this. The "How long the cache lasts" section reads as an answer but
      is not one: "It holds for the period configured above" points at a period that is never
      configured or stated anywhere in the document — not in this section, not in "Credential
      caching," nowhere. "This is the same mechanism described in the previous section" is also
      broken: the previous section ("Credential caching") is a single sentence that describes no
      mechanism. This is a dangling reference to content that was never written, not a passage
      that merely needs to be moved or reworded — so it is scored ABSENT, not NOT EXTRACTABLE.
    → suggested: a passage naming Havenlock and giving the actual cache duration — a specific
      figure, or, if it is admin-configurable, a statement of the default and the configurable
      range — that does not depend on a value the page never states

  how do you configure the credential cache duration on a Havenlock controller
    → no passage answers this
    → suggested: a passage naming Havenlock and giving the steps or the location (e.g., which
      settings screen or interface) for setting the cache duration

  is Havenlock's documented offline and caching behavior current, or has it changed in a newer
  firmware or version
    → no passage answers this
    → suggested: a passage naming Havenlock and stating the firmware/software version, or a
      last-reviewed date, that the described behavior applies to

WHAT GOOD LOOKS LIKE
  criterion 5 — lifted out with nothing around it, it still answers the sub-query
    before: "The system caches results locally."
    after:  "The system caches results locally, storing each response for 24 hours so it can
             answer repeat requests instantly and keep working even if the connection to the
             server drops."
