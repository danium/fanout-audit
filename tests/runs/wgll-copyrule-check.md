## Fan-out coverage: How do Havenlock door controllers work?

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (1)
  what is Havenlock
    → Havenlock builds networked door controllers for commercial buildings.

NOT EXTRACTABLE (4)
  why does it matter that a door controller keeps deciding access when the network is down
    → found in: "Offline behaviour" section
    → fails: criterion 1 (the answer — "A door that stops deciding... fails open... or fails
      locked..." — is the third sentence; the first two sentences are setup about vendors and
      cable/switch failures), criterion 3 (Havenlock is never named anywhere in this section)
    → fix: open the passage with the consequence sentence, and name Havenlock's controller as
      the subject that avoids it, instead of the generic "a door"

  how does Havenlock's credential caching work
    → found in: "Credential caching" section
    → fails: criterion 5 ("Havenlock controllers cache credentials locally" states the fact
      and explains nothing — what is cached, how, or why — the same shape as "Ledgerline
      matches transactions in three passes")
    → fix: add the mechanism detail this section is missing so the passage is a complete
      answer on its own, rather than a lead-in to the next section

  how long does the credential cache last on a Havenlock controller
    → found in: "How long the cache lasts" section
    → fails: criterion 2 ("the period configured above" and "described in the previous
      section" both point outside the passage — and neither antecedent is a stated duration
      anywhere on the page), criterion 3 ("the controller" — Havenlock is not named in this
      section)
    → fix: state the cache duration directly in this section instead of pointing to "above,"
      and replace "the controller" with "a Havenlock controller"

  what are a Havenlock controller's power / backup power specifications
    → found in: "Power" section
    → fails: criterion 3 ("Each controller draws power over Ethernet..." — Havenlock is not
      named anywhere in this section)
    → fix: change "Each controller" to "Each Havenlock controller" so the passage identifies
      its subject on its own

ABSENT (5)
  what does Havenlock cost
    → no passage answers this
    → suggested: a passage stating the cost or pricing basis for a Havenlock controller or
      system

  how do Havenlock's controllers compare to other access control systems
    → no passage answers this
    → suggested: a passage naming at least one alternative approach (e.g. cloud-dependent or
      centrally-controlled access systems) and stating how Havenlock's local-decision approach
      differs from it

  who is Havenlock for
    → no passage answers this
    → suggested: a passage stating which building types, portfolio sizes, or security
      requirements Havenlock suits or does not suit

  does Havenlock's offline behavior actually prevent fail-open or fail-locked incidents
    → no passage answers this
    → suggested: a passage giving evidence, testing, or field data supporting the claim that
      Havenlock controllers keep making correct access decisions during a network outage

  how do you configure the credential cache duration on a Havenlock controller
    → no passage answers this
    → suggested: a passage or section explaining where and how to set the local credential
      cache duration for a door group

WHAT GOOD LOOKS LIKE
  criterion 3 — the entity is named, not implied
    before: - Full request waterfalls across HTTP, gRPC, and Postgres
            - Automatic service dependency maps built from observed traffic
            - p50, p95, and p99 latency per endpoint, per service, per version
    after:  Cabletrace connects to Kubernetes clusters running version 1.24 or later and to bare-metal Linux hosts, in both cases requiring Linux kernel 5.8 or newer. Cabletrace traces HTTP, gRPC, and Postgres traffic, and reports p50, p95, and p99 latency per endpoint, per service, and per version. Cabletrace does not support Windows containers or kernels older than 5.8, because the eBPF features it depends on are not present in them.
