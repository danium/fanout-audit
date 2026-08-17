## Fan-out coverage: Havenlock networked door controllers for commercial buildings

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (2)
  what are Havenlock door controllers
    → "Havenlock builds networked door controllers for commercial buildings."

  how do Havenlock controllers keep working when the building network drops
    → "A controller sits behind each door, holds the access rules locally, and keeps working when the building network drops."

NOT EXTRACTABLE (3)
  how does Havenlock's credential caching work
    → found in: "Credential caching" section
    → fails: criterion 5 (states the fact — credentials are cached locally — without explaining what is cached, how it is used, or how it enables offline decisions)
    → fix: expand the sentence to say what is cached and how the controller uses it to decide access while offline

  what happens when the Havenlock credential cache expires
    → found in: "How long the cache lasts" section
    → fails: criterion 2 (opens on unresolved "It," and points at "the period configured above" and "the previous section," neither of which resolves inside the passage), criterion 3 ("the controller" — Havenlock is never named in this passage)
    → fix: rewrite as a self-contained sentence that names Havenlock directly and states the fallback rule without pointing at "above" or an earlier section

  what are the power requirements for a Havenlock controller
    → found in: "Power" section
    → fails: criterion 3 ("each controller" — Havenlock is never named in this passage)
    → fix: name Havenlock explicitly in this sentence instead of relying on "each controller"

ABSENT (5)
  how long does the Havenlock credential cache last
    → no passage answers this
    → suggested: the actual cache duration as a concrete value (a number of hours/days), stated directly — not a reference to "the period configured above," which does not appear anywhere else on the page

  how does Havenlock compare to cloud-dependent door access control systems
    → no passage answers this
    → suggested: a sentence naming at least one category of alternative (e.g., cloud-only controllers) and stating the specific difference in offline behavior

  is Havenlock a good fit for buildings with frequent network outages
    → no passage answers this
    → suggested: a sentence stating which building types or conditions Havenlock suits (and does not suit), beyond the generic "commercial buildings"

  does Havenlock's offline design actually prevent fail-open or fail-locked situations
    → no passage answers this
    → suggested: evidence beyond the bare claim — a certification, test methodology, or deployment result supporting the offline-behavior claim

  how do you configure the credential cache duration on a Havenlock controller
    → no passage answers this
    → suggested: configuration steps or a pointer to where the setting lives, since "configured above" implies a setting the page never actually shows

WHAT GOOD LOOKS LIKE
  criterion 3 — the entity is named
    before: The device holds enough charge to finish the current operation if power is lost.
    after:  Vantage holds enough charge to finish the current operation if power is lost.
