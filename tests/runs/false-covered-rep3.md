## Fan-out coverage: how Havenlock's networked door controllers work, including their offline and credential-caching behavior

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (1)
  what are Havenlock door controllers
    → Havenlock builds networked door controllers for commercial buildings.

NOT EXTRACTABLE (3)
  how do Havenlock controllers keep working when the building network is offline
    → found in: "Offline behaviour" section
    → fails: criterion 1 (opens with generic industry context — "Buildings lose network
      connectivity more often than vendors admit" — not the answer), criterion 3 (Havenlock
      is never named in this section; the closing sentence describes "a door" generically)
    → fix: move the "holds the access rules locally, and keeps working when the building
      network drops" fact from the intro into this section, name Havenlock in the opening
      sentence, and drop the generic industry-context opener

  how does Havenlock's credential caching work
    → found in: "Credential caching" section
    → fails: criterion 5 (states that caching happens, locally, without explaining the
      mechanism — nothing about what triggers a cache update, how it's stored, or how it
      stays in sync; the passage is a bare fact, not an explanation of "how")
    → fix: add sentences explaining what triggers a credential cache update and what
      "locally" means operationally (stored on-device vs. synced from a central system)

  what happens to a Havenlock controller if power is cut, and what are its power
  requirements
    → found in: "Power" section
    → fails: criterion 3 (the passage says "Each controller," never naming Havenlock)
    → fix: replace "Each controller" with "Havenlock controllers" so the passage names the
      entity without relying on the page heading

ABSENT (5)
  how long does the credential cache last before the controller falls back to a default
  rule
    → no passage answers this
    → suggested: a passage stating the actual cache duration (a number, with its unit) and
      whether it is fixed or configurable, including the default. The "How long the cache
      lasts" section reads as if it answers this, but it only points to "the period
      configured above" and "the mechanism described in the previous section" — neither
      exists anywhere else on the page, so no duration value is actually stated

  how do Havenlock's controllers compare to other access-control approaches (e.g.,
  cloud-dependent or centrally-controlled systems)
    → no passage answers this
    → suggested: a passage naming at least one alternative approach and stating how
      Havenlock's local-caching design differs from it on a specific dimension, such as
      where the access decision is made during an outage

  who are Havenlock door controllers designed for
    → no passage answers this
    → suggested: a passage stating the building type, size, or use case Havenlock targets,
      and any situation it is not suited for

  does Havenlock's local caching actually prevent fail-open or fail-locked situations
  during outages
    → no passage answers this
    → suggested: a passage giving evidence for the "keeps working when the building network
      drops" claim — what was tested, which failure modes were eliminated, or third-party
      validation

  how do you configure the credential cache duration or the default door-group fallback
  rule
    → no passage answers this
    → suggested: a passage naming where this is configured (e.g., an admin console or
      config file) and the steps to set it

WHAT GOOD LOOKS LIKE
  criterion 3 — the entity is named, not implied
    before: The device pairs over Bluetooth and stores its settings on the unit itself, so
            it keeps working if the phone app is closed.
    after:  Nimbus locks pair over Bluetooth and store their settings on the lock itself, so
            a Nimbus lock keeps working if the phone app is closed.
