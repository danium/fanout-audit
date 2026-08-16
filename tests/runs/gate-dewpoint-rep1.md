## Fan-out coverage: why is dew point a better measure of how muggy the air feels than relative humidity

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (1)
  what is relative humidity
    → "Relative humidity is a ratio."

NOT EXTRACTABLE (8)
  what is dew point
    → found in: paragraph 4, unheaded ("The dew point fixes this by measuring...")
    → fails: criterion 2 (opens "The dew point fixes this" — "this" has no antecedent inside the passage)
    → fix: open with "Dew point is the temperature to which air must be cooled, at constant pressure, to become saturated," and drop the "fixes this" framing

  why is relative humidity a misleading measure of mugginess
    → found in: paragraph 3, unheaded ("So relative humidity is a fraction whose denominator moves...")
    → fails: criterion 2 (opens "So," pointing to the temperature-capacity link explained in the prior paragraph, not restated here)
    → fix: state the reasoning inline — "Relative humidity is misleading because its denominator, the air's maximum moisture capacity, rises with temperature" — before giving the numeric example

  why does dew point stay stable while relative humidity changes with temperature
    → found in: paragraph 4, unheaded (same paragraph as "what is dew point")
    → fails: criterion 1 (the stability explanation is sentence 3, "Because it describes the water content itself..."); criterion 2 (sentence 1's unresolved "this")
    → fix: lead with "Dew point does not shift with temperature because it measures water content directly rather than as a ratio against a moving maximum" and move the formal definition after it

  why does high dew point make it harder for sweat to cool the body
    → found in: paragraph 7, unheaded ("The physiological reason the scale works is sweat...")
    → fails: criterion 1 (opens with the topic sentence "The physiological reason the scale works is sweat" instead of the mechanism itself); criterion 2 ("the scale" has no antecedent in this passage — it refers to the comfort scale in the prior paragraph)
    → fix: open with "High dew point slows sweat evaporation because cooling depends on the moisture gradient between skin and air, which dew point measures directly," and remove the "scale" reference

  why does condensation form on a surface colder than the dew point
    → found in: paragraph 8, unheaded ("There is one more place the distinction earns its keep...")
    → fails: criterion 1 (answer is sentence two; sentence one is a transition); criterion 2 ("the distinction" is unresolved)
    → fix: open with "Any surface colder than the dew point will collect water" and drop the transition sentence

  dew point vs relative humidity: which better predicts how muggy it feels
    → found in: paragraph 1, unheaded (opening paragraph)
    → fails: criterion 1 (the claim "the second number is the useful one" arrives in sentence three, after a rhetorical setup); criterion 5 (defers the actual reason — "the reason is worth understanding" — instead of containing it)
    → fix: state the claim and its reason together up front instead of deferring the explanation to later paragraphs

  when is relative humidity still the more useful number than dew point
    → found in: paragraph 9, unheaded ("None of this makes relative humidity useless...")
    → fails: criterion 1 and criterion 2 (opens "None of this," which has no antecedent inside the passage)
    → fix: open with "Relative humidity is the right number when the saturation ratio itself matters — for mould growth, material shrinkage, and static electricity" and drop the "None of this" backreference

  how do you stop a window or pipe from sweating (condensation)
    → found in: paragraph 8, unheaded (same paragraph as the condensation mechanism, final sentence)
    → fails: criterion 1 (the instruction "Lower the indoor dew point or raise the surface temperature" is the final sentence of a four-sentence paragraph that opens with a transition)
    → fix: lead with the instruction — "To stop condensation, lower the indoor dew point or raise the surface temperature" — then give the mechanism as support

ABSENT (1)
  how do you measure or find the dew point (instrument, formula, or report)
    → no passage answers this
    → suggested: a sentence naming how a reader would actually obtain a dew point reading — e.g., from a weather report, a hygrometer, or a calculation from temperature and relative humidity
