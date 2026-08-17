## Fan-out coverage: Thornfield Junction warehouse fire and supply chain disruption

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (4)
  what happened at Thornfield Junction
    → A fire broke out shortly after dawn last Tuesday inside Warehouse 6 at the Thornfield Junction freight yard...

  who was involved in the warehouse fire
    → Kestrahaven Logistics leases Warehouse 6 from the yard's landowner and uses it as the sole transfer point for shipments moving between its road fleet and the Meridian Rail Consortium's northern spur line.

  what products or shipments were destroyed
    → The warehouse itself held goods belonging to several of Kestrahaven's shipping clients, including packaged foodstuffs nearing their sell-by window and a shipment of household appliances destined for a coastal retail chain.

  how long will the northern spur be closed
    → No timeline exists yet for reopening the northern spur, and no decision has been made on whether Warehouse 6 will be rebuilt on the same footprint or relocated elsewhere in the yard.

NOT EXTRACTABLE (3)
  what area is affected by the Thornfield Junction disruption
    → found in: paragraphs 3–5
    → fails: criterion 5 (lifted out, does not fully specify geographic scope of supply chain impact)
    → fix: combine infrastructure affected (northern spur line) and geographic region served (e.g., "northern coastal port towns") into one passage naming both

  is the Thornfield Junction disruption still affecting supply chains
    → found in: paragraph 5
    → fails: criterion 2 (unresolved temporal references: "has since been rerouted," "most of the day" point to the moment of writing, not an absolute date)
    → fix: replace relative time markers with publication date and specify what day/date the observed impacts occurred

  what is the current status of the northern spur reopening
    → found in: paragraph 7
    → fails: criterion 2 (unresolved temporal reference: "yet" lacks date anchor showing when the state was assessed)
    → fix: open with "As of [publication date]" and replace "no timeline exists yet" with explicit date framing

ABSENT (2)
  who reported or confirmed the Thornfield Junction warehouse fire
    → no passage answers this
    → suggested: attribution to a named official, emergency authority, or company representative stating who confirmed the fire occurred and on what date

  is the fire confirmed by a credible source
    → no passage answers this
    → suggested: a sentence citing an official investigation, emergency services statement, or third-party verification, including the date of that verification

WHAT GOOD LOOKS LIKE
  criterion 2 — no unresolved references

    before: The facility has been rerouted to a smaller site since the incident, and discussions about reopening are still in progress. Workers report disruptions through most of the morning.

    after: As of January 15, the facility has been rerouted to a smaller site since the January 3 incident, with no reopening timeline established as of that date. On January 3, workers reported disruptions through the morning of the fire.
