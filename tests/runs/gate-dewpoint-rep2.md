## Fan-out coverage: why dew point, not relative humidity, is the better measure of how muggy the air feels

Sub-queries below are a simulated decomposition, not Google's actual fan-out. Treat
coverage gaps as a content completeness signal, not as a ranking prediction.

COVERED (1)
  what is relative humidity
    → "Relative humidity is a ratio."

NOT EXTRACTABLE (9)
  what is dew point
    → found in: paragraph 4, unheaded ("The dew point fixes this by measuring...")
    → fails: criterion 2 (opens "The dew point fixes this" — "this" refers to the
      relative-humidity problem described in the previous paragraph, not inside this passage)
    → fix: open with "Dew point is the temperature to which air must be cooled, at constant
      pressure, for it to become saturated," and drop the "fixes this" framing

  what dew point range feels comfortable vs oppressive
    → found in: paragraph 6, unheaded ("The rough scale most people converge on...")
    → fails: criterion 3 (the entity is never named in this passage — "dew point" is not
      stated anywhere in it, so the numbers below ten, ten to fifteen, sixteen to eighteen,
      above twenty, and above twenty-four have no named unit attached)
    → fix: rewrite as "A dew point below ten feels dry; ten to fifteen is comfortable;
      sixteen to eighteen feels sticky; above twenty is oppressive; above twenty-four is
      extreme," naming dew point directly

  why is relative humidity a misleading measure of mugginess across temperatures
    → found in: paragraph 3, unheaded ("So relative humidity is a fraction whose denominator
      moves...")
    → fails: criterion 2 (opens "So," presupposing the percentage-of-maximum-capacity
      explanation from the previous paragraph; "denominator" is never defined inside this
      passage itself)
    → fix: restate the mechanism inline — "Relative humidity is misleading because it is a
      percentage of the air's maximum moisture capacity, and that maximum rises with
      temperature" — before giving the ninety-percent example

  why doesn't dew point change as the temperature rises
    → found in: paragraph 5, unheaded ("That stability is what makes it comparable...")
    → fails: criterion 1 (opens "That stability is what makes it comparable," a callback
      rather than an answer); criterion 2 ("That stability" and "it" refer to the previous
      paragraph's claim, not to anything inside this passage)
    → fix: open with "Dew point stays the same as air temperature changes because it measures
      water content directly rather than as a ratio," then give the twenty-one-degree example

  why does a high dew point make it harder to cool down by sweating
    → found in: paragraph 7, unheaded ("The physiological reason the scale works is
      sweat...")
    → fails: criterion 1 (opens "The physiological reason the scale works is sweat,"
      referencing "the scale" instead of stating the mechanism)
    → fix: open with "Sweat cools the body by evaporation, which depends on the gap between
      the moisture on skin and the moisture in the air — a gap dew point measures directly,"
      then keep the rest of the paragraph as support

  why does condensation form on windows, pipes, or walls
    → found in: paragraph 8, unheaded ("There is one more place the distinction earns its
      keep, which is condensation...")
    → fails: criterion 1 (opens "There is one more place the distinction earns its keep,"
      a transition, not an answer)
    → fix: open with "Any surface colder than the dew point will collect water" and drop the
      transition sentence

  dew point vs relative humidity for measuring mugginess
    → found in: paragraph 1, unheaded (opening paragraph)
    → fails: criterion 1 (the claim "the second number is the useful one" arrives in sentence
      three, after two setup sentences); criterion 5 (the passage asserts dew point is more
      useful without stating why — "the reason is worth understanding" points forward to
      later paragraphs instead of explaining it here)
    → fix: state the reason in the same passage, e.g. "Dew point is a better measure of
      mugginess than relative humidity because it measures moisture directly instead of as a
      ratio that shifts with temperature"

  when is relative humidity still the more useful number than dew point
    → found in: paragraph 9, unheaded ("None of this makes relative humidity useless...")
    → fails: criterion 1 (the answer — that RH is right when the saturation ratio itself
      matters — arrives in sentence two, not the first); criterion 2 (opens "None of this,"
      an unresolved reference to the entire preceding argument)
    → fix: open with "Relative humidity is still the right number when the saturation ratio
      itself matters — for mould growth, material shrinkage, and static electricity" and
      drop the "None of this" backreference

  how to stop a window or pipe from sweating (condensation)
    → found in: paragraph 8, unheaded (same paragraph as the condensation mechanism, final
      sentence)
    → fails: criterion 1 (the instruction "Lower the indoor dew point or raise the surface
      temperature" is the last sentence of a paragraph that opens with a transition and a
      worked example)
    → fix: lead with the instruction — "To stop a window or pipe from sweating, lower the
      indoor dew point or raise the surface temperature" — then give the mechanism and
      example as support

ABSENT (1)
  how do you measure or find today's dew point
    → no passage answers this
    → suggested: a sentence naming how a reader would actually obtain a dew point reading —
      e.g., from a weather report, a hygrometer, or a calculation from temperature and
      relative humidity
