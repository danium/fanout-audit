Head query confirmed before running this: **the Thornfield Junction warehouse fire and its disruption to the Kestrahaven Logistics–Meridian Rail Consortium freight corridor.** Single head query — the fire and its business fallout are one story, not two.

One note before the findings: you asked for paste-ready copy. This skill is scoped to an audit, not a rewrite — for NOT EXTRACTABLE items the deliverable is a one-line fix instruction, not a drafted paragraph, and ABSENT items get a description of what's missing, not a written passage. Two of the gaps below (the fire's actual date, and who's confirming this account) aren't in the source at all, so any passage I wrote for them would be me inventing a date or a spokesperson on your behalf. Those are yours to supply — flagged below with exactly what's needed.

## Fan-out coverage: the Thornfield Junction warehouse fire and its disruption to the Kestrahaven–Meridian freight corridor

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (1)
  who are Kestrahaven Logistics and Meridian Rail Consortium, and how are they connected at Thornfield Junction
    → "Kestrahaven Logistics leases Warehouse 6 from the yard's landowner and uses it as the sole transfer point for shipments moving between its road fleet and the Meridian Rail Consortium's northern spur line."

NOT EXTRACTABLE (5)
  what happened in the Thornfield Junction warehouse fire
    → found in: paragraph 1, unheaded
    → fails: criterion 2 (unresolved reference — "last Tuesday" is relative to the moment of writing and never resolves to a calendar date inside the passage)
    → fix: replace "last Tuesday" with the actual calendar date the fire occurred

  why did the fire disrupt the rail line and signalling beyond the warehouse itself
    → found in: paragraph 3, unheaded
    → fails: criterion 1 (answer after setup — "The fire did not stay inside Warehouse 6" is a transition; the actual mechanism, radiant heat buckling track and a signal hut burning, doesn't start until sentence two)
    → fix: open the passage with the causal mechanism itself (heat buckling the track, the signal hut burning out block signalling) and drop or relocate the transition sentence

  what has happened to Kestrahaven's rerouted volume and Meridian's rail schedule since the fire
    → found in: paragraph 5, unheaded
    → fails: criterion 2 (unresolved reference — "has since been rerouted" is anchored to the moment of writing and chains back to the still-unresolved fire date)
    → fix: state when the rerouting took effect using an absolute date instead of "since"

  what client shipments or goods were affected by the fire
    → found in: paragraph 6, unheaded
    → fails: criterion 3 (entity not named — "the warehouse itself" is never tied to "Warehouse 6" inside this passage; it depends on paragraph 1 for that antecedent)
    → fix: replace "The warehouse itself" with "Warehouse 6" so the passage names its subject without relying on earlier paragraphs

  is the northern spur still closed, and has a rebuild decision been made for Warehouse 6
    → found in: paragraph 8, unheaded
    → fails: criterion 2 (unresolved reference — "no timeline exists yet" is anchored to the moment of writing, not a stated date) and criterion 3 (entity not named — "the northern spur" isn't tied to Meridian within this sentence)
    → fix: state the status as of a specific date, and name Meridian as the spur's operator in the same sentence

ABSENT (1)
  is the fire confirmed, and who is the source for this account
    → no passage answers this
    → suggested: a sentence naming who is confirming the fire and its effects — a named Kestrahaven or Meridian spokesperson, an emergency-services statement, or another attributed source — and when that confirmation was given. Nothing in the piece is attributed to anyone; it's written in an unsourced narrative voice throughout.
