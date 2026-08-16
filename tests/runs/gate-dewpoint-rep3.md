## Fan-out coverage: why dew point, not relative humidity, is the right number for judging how muggy or uncomfortable the air feels

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (2)
  what is relative humidity
    → "Relative humidity is a ratio. It tells you how much water vapour the air is holding as a percentage of the maximum it could hold at its current temperature." (about 29 words; short but complete)

  why does relative humidity give misleading comparisons across temperatures
    → "So relative humidity is a fraction whose denominator moves."

NOT EXTRACTABLE (6)
  what is dew point
    → found in: paragraph 4, unheaded
    → fails: criterion 1 (answer in first sentence) — the sentence opens with "fixes this," and the actual definition doesn't land until sentence two; criterion 2 (no unresolved references) — "this" refers to the relative-humidity problem described two paragraphs earlier, outside this passage
    → fix: open with "Dew point is the temperature to which air must be cooled, at constant pressure, for it to become saturated and begin condensing," naming dew point directly and dropping "fixes this"

  what dew point range feels comfortable vs oppressive
    → found in: paragraph 6, unheaded
    → fails: criterion 3 (entity named) — "dew point" is never stated in this passage; the numbers (ten, fifteen, eighteen, twenty, twenty-four) have no named unit or entity attached
    → fix: rewrite as "Dew point below ten degrees feels dry; ten to fifteen is comfortable; sixteen to eighteen feels sticky; above twenty is oppressive; above twenty-four is extreme," naming dew point in the sentence itself

  why is dew point a stable measure that doesn't shift with temperature
    → found in: paragraph 5, unheaded
    → fails: criterion 2 (no unresolved references) — opens with "That stability," pointing at the mechanism described in the previous paragraph; criterion 3 (entity named) — the passage says "it," never restating "dew point"
    → fix: open with "Dew point stays comparable across temperature because it measures water content directly rather than as a ratio," naming dew point and dropping "That stability"

  why does a high dew point make it harder for the body to cool itself through sweat
    → found in: paragraph 7, unheaded
    → fails: criterion 1 (answer in first sentence) — the opening sentence names the topic ("the physiological reason ... is sweat") without giving the mechanism; criterion 2 (no unresolved references) — "the scale" refers to the comfort scale introduced in the prior paragraph
    → fix: open with "Cooling by evaporation depends on the gradient between the moisture on your skin and the moisture in the air, which dew point measures directly," dropping the "physiological reason the scale works" lead-in

  dew point vs relative humidity — which is the better number for judging mugginess
    → found in: paragraph 1, unheaded
    → fails: criterion 5 (survives being lifted out) — the passage names dew point as "the useful one" but explicitly defers the reason ("the reason is worth understanding") instead of stating it, so lifted alone it asserts a verdict without support
    → fix: add a clause inside this passage stating why dew point wins the comparison (e.g., "because it measures moisture directly rather than as a ratio that shifts with temperature")

  how do you stop a window or cold surface from sweating with condensation
    → found in: paragraph 8, unheaded
    → fails: criterion 1 (answer in first sentence) — opens with "There is one more place the distinction earns its keep," a transition that answers nothing; criterion 2 (no unresolved references) — "the distinction" refers to the dew-point-vs-humidity contrast established earlier in the article
    → fix: open with "Any surface colder than the dew point will collect water — lower the indoor dew point or raise the surface temperature to stop it," dropping the "one more place" transition

ABSENT (0)
