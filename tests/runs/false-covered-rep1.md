## Fan-out coverage: how Havenlock door controllers handle network and power loss

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (1)
  what is a havenlock door controller
    → Havenlock builds networked door controllers for commercial buildings.

NOT EXTRACTABLE (3)
  how do havenlock controllers keep deciding access when the network goes down
    → found in: "Offline behaviour" section
    → fails: criterion 1 (opens with network-reliability framing — "Buildings lose network
            connectivity more often than vendors admit" — not the answer), criterion 5 (the
            passage only frames the fail-open/fail-locked stakes; it never states what
            Havenlock's controller actually does, which sits in a different section)
    → fix: open this section with what the controller does when the network drops (holds
            access rules and cached credentials locally), then use the fail-open/fail-locked
            framing as the reason it matters

  how does credential caching work on a havenlock controller
    → found in: "Credential caching" section
    → fails: criterion 5 (states the fact without explaining it — "Havenlock controllers
            cache credentials locally" gives no detail on what is cached or how that enables
            an access decision)
    → fix: expand the sentence into a self-contained passage stating what is cached and how
            it is used to decide access without a network connection

  what are the power requirements for a havenlock controller
    → found in: "Power" section
    → fails: criterion 3 (entity implied, not named — "Each controller draws power over
            Ethernet" never says "Havenlock")
    → fix: change "Each controller" to "Each Havenlock controller" so the passage names the
            entity if lifted out alone

ABSENT (6)
  havenlock local credential caching vs cloud-dependent access control
    → no passage answers this
    → suggested: a passage naming the alternative approach (e.g., cloud-dependent or
            centralized access control) and stating concretely how Havenlock's offline
            behavior differs from it

  is havenlock a good fit for buildings with unreliable network connectivity
    → no passage answers this
    → suggested: a passage stating what kind of building or reader this suits, or does not
            suit

  how long does a havenlock controller cache credentials before falling back to the default
  rule
    → no passage answers this
    → suggested: the actual cache duration — a time period, or the specific event that ends
            it — stated as a fact. The "How long the cache lasts" section reads as though it
            answers this ("It holds for the period configured above") but no period is
            configured anywhere earlier in the content, and "the same mechanism described in
            the previous section" does not describe a duration either — the previous section
            only states that caching happens. The section title matches the sub-query; the
            content under it does not contain the fact

  does a havenlock controller's power backup actually complete an in-progress unlock when
  power is cut
    → no passage answers this
    → suggested: what supports the claim — a figure, a test, or a mechanism description —
            beyond restating the claim itself

  how do you configure the credential cache duration on a havenlock controller
    → no passage answers this
    → suggested: where the cache duration is set and the steps to change it

  does this offline and caching behavior apply to havenlock's current controller models
    → no passage answers this
    → suggested: which model line or firmware version this description applies to, or when
            it was last confirmed accurate

WHAT GOOD LOOKS LIKE
  criterion 5 — it survives being lifted out
    before: "Acme Sync runs backups on a schedule."
    after:  "Acme Sync backs up the database every six hours, snapshotting it and uploading
            the encrypted snapshot to cloud storage, then pruning any snapshot older than 30
            days."
