## Fan-out coverage: Varrowline, a scheduling engine for field service teams

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (5)
  what is Varrowline
    → Varrowline is a scheduling engine for field service teams.

  how does Varrowline assign jobs to technicians
    → Every morning Varrowline builds a route for each technician from the jobs due that day.

  what systems does Varrowline integrate with
    → Varrowline reads work orders from ServiceTitan, Jobber, and Housecall Pro, and writes completed job records back to the same system.

  does Varrowline track certification renewal windows
    → Varrowline tracks certifications per technician and refuses to assign a job whose work order requires a certification the technician does not hold or whose certification has expired.

  how do you get started with Varrowline
    → Import your technician roster and your job history as CSV.

NOT EXTRACTABLE (2)
  how does Varrowline handle reassignment when a job overruns or a technician calls in
    → found in: "How assignment works", second (unheaded) paragraph
    → fails: criterion 3 (entity not named — the passage calls it only "the engine";
             "Varrowline" never appears in this paragraph)
    → fix: replace "the engine" with "Varrowline" and open the passage naming it directly,
           e.g. "Varrowline reassigns jobs continuously through the day."

  what's the difference between the Varrowline plans (Field, Route, Depot)
    → found in: "Plans" table and the "All plans include..." sentence beneath it
    → fails: criterion 3 (entity not named). Checked at row level, not just as a block:
             the header row, the Field row, the Route row, and the Depot row each name
             only the plan tier ("Field", "Route", "Depot") and feature values — none
             contains "Varrowline," so every row fails identically, not just the table
             as a whole.
    → fix: add a lead sentence naming Varrowline before the table (e.g. "Varrowline
           offers three plans:"), and convert each row to a full sentence naming
           Varrowline as the subject, e.g. "The Varrowline Route plan supports up to
           50 technicians, live reassignment, and read-only parts inventory sync."

ABSENT (4)
  how is using Varrowline different from manually dispatching field service jobs
    → no passage answers this
    → suggested: a passage naming Varrowline and stating which dispatch decisions it
      takes over from a human dispatcher versus what a dispatcher still does by hand.

  who is Varrowline for
    → no passage answers this
    → suggested: a sentence naming Varrowline and stating the size or type of field
      service operation it is built for, and any case it is not meant for.

  what does Varrowline cost
    → no passage answers this
    → suggested: a sentence giving the price or billing basis for the Field, Route, and
      Depot plans, and the per-technician overage rate.

  is Varrowline's integration list current
    → no passage answers this
    → suggested: a date or version marker next to the integrations list (ServiceTitan,
      Jobber, Housecall Pro, Fishbowl, NetSuite) that a reader can check freshness against.
