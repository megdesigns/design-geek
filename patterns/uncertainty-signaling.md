# Uncertainty Signaling

**Pattern 01.** Serves [Principle 1](../principles.md#1-make-uncertainty-visible-and-actionable) (make uncertainty visible and actionable), [Principle 5](../principles.md#5-build-for-accessibility) (accessibility) and [Principle 6](../principles.md#6-avoid-generic-ai-slop) (avoid generic AI slop).

Design file: [Uncertainty Signal component set](https://www.figma.com/design/YqEqVIuX639b3zh9ljDinP/Research?node-id=9-32)

---

## Problem

A model returns everything in the same confident register. The interface inherits that flatness, so a grounded answer and a guess look identical, and the reader has no basis for deciding what to check.

Showing the raw score does not fix it. A percentage asks the reader to do the conversion work the interface should have done, and it presumes a fluency with probability that most people do not have. The common failure modes are an overconfident interface that gets trusted blindly, and a uniformly hedged one that gets ignored. Both leave the person without a way to allocate attention.

What the pattern delivers: a small number of named bands, each attached to a recommended behaviour, carried redundantly so that no single channel is load-bearing.

---

## When to use

Use this pattern when all of the following hold:

- The output varies meaningfully in reliability from one response to the next.
- You have a mapping from model output to band, and you have checked that the mapping tracks actual quality.
- The person can act differently depending on the band. If every band leads to the same behaviour, you do not need bands.

## When not to use

- **You cannot show that your confidence values track output quality.** Apple's guidance is explicit: if you do not know how your confidence values relate to result quality, convey no confidence at all. An unverified band is worse than silence, because it looks like information.
- **The result is itself statistical.** Forecasts, prices, polling, lab values, sports statistics. Here the interval or percentage is the literacy people already have, and replacing it with "medium" destroys information. Apple carves out exactly this exception for attributions.
- **Every output would carry the same band.** A constant disclaimer carries no information and trains people to stop reading it.
- **Below your floor.** Where confidence genuinely tracks quality and the output falls under a documented threshold, suppress it rather than band it. Shipping weak output with a warning attached is not the same as not shipping it.
- **The band is the only thing you would change.** If low confidence does not alter the presentation, the ranking, or the available actions, you have added a label, not a pattern.

---

## Variants

Four states. Each carries its meaning three times over: bar shape, text label, and colour. Remove any one of the three and the state is still legible.

| State | Indicator | Label | Supporting explanation |
|---|---|---|---|
| **High confidence** | Three filled bars | High confidence | Grounded in the sources listed below. Worth a glance at the citation before you act on it. |
| **Medium confidence** | Two filled, one empty | Medium confidence | Partly grounded. Check the specifics against a source before you rely on them. |
| **Low confidence** | One filled, two empty | Low confidence | A starting point, not an answer. Verify every claim before you use it. |
| **Cannot answer** | A single dash | Can't answer this | No reliable source covers this. Search the handbook directly, or ask the policy team. |

### High confidence

The output is traceable to a source the person can open. This is never the default state: it has to be earned by a verified mapping, otherwise it is the overconfidence problem wearing a badge.

### Medium confidence

The shape of the answer holds but the details do not. Pair this band with claim-level marks on the specific spans that are weak. A response-level band alone pushes the reader to re-check everything or nothing, which is the behaviour the pattern exists to prevent.

### Low confidence

Plausible but unverified. Consider showing two or three alternatives rather than one hedged answer. The People + AI Guidebook's n-best alternatives pattern is specifically useful here, because presenting candidates hands judgement back to the person instead of asking them to discount a number.

### Cannot answer

No grounded answer exists. Three things make this state work:

1. It is a designed state, not an error. The system is behaving correctly.
2. It names a next step, and that step is a real control, not a sentence. Apple's guidance on limitations is to explain why the inferior result occurred so the person can adjust; the Guidebook's framing is that no error should ever be a dead end.
3. It is not a softer Low. Downgrading "I cannot answer" into "low confidence, here is a guess" is the single most damaging substitution in this pattern, because it converts an honest limit into a plausible fabrication.

---

## Accessibility notes

**Colour is never the only carrier.** Bar count and written label both state the band, so the pattern survives greyscale, forced colours, and colour vision deficiency. This is the pattern's core requirement, not a refinement of it.

**Contrast.** Label text meets 4.5:1 against its tinted background. The bars meet 3:1 as meaningful graphics. The empty bars use a neutral track rather than a lighter tint of the accent, so filled and unfilled stay distinguishable at 3:1 rather than relying on a subtle hue shift.

**The tint is reinforcement, not information.** The component reads correctly with the background fill removed entirely. Test it that way.

**Markup.** The band lives in text. The bars are decorative once the label is present and are marked `aria-hidden`. The signal sits inside the same labelled region as the output it describes, so the band is announced before the content rather than after it.

| State | Announced as |
|---|---|
| High confidence | "High confidence. Grounded in the sources listed below." |
| Medium confidence | "Medium confidence. Partly grounded." |
| Low confidence | "Low confidence. A starting point, not an answer." |
| Cannot answer | "Cannot answer this. No reliable source covers this," followed by the next step as a focusable control. |

**Streaming.** If the band changes while the output streams, do not announce each change. Settle the band at completion and announce once. An unmanaged live region narrates the flicker and makes the output unusable.

**Motion.** Do not animate the bars. Any motion here is decorative and must respect reduced-motion.

**Keyboard.** The next step on the cannot-answer state is reachable by keyboard with visible focus. A message with no operable action fails [Principle 3](../principles.md#3-design-for-graceful-failure-and-a-manual-path).

---

## Why bands and not percentages

Google's People + AI Guidebook puts the question directly: what is someone supposed to do differently at 85.8% certain versus 87% certain? It calls numeric indicators risky because they presume an understanding of probability, and observes that a 97% match reads as a failure to anyone expecting 100%.

Apple arrives at the same place from the other direction. Displaying a bare "97% match" does not tell someone enough to make a choice; the guidance is to translate confidence into concepts people already hold, whether semantic categories, an ordering that implies confidence, or an outright recommendation such as "this is a good time to buy."

So a band is not a rounded percentage. It is a decision boundary with an action attached. That is also the test for whether a band earns its place: if two bands recommend the same behaviour, you have one band.

Both sources allow numeric display where the subject is numeric. The rule is about translation, not about hiding data.

---

## Good and bad examples

### Avoid

```
Confidence: 87.3%
```
Precision nobody can act on, and a decimal place implying a calibration you do not have.

```
[sparkle icon, amber] Here is your answer.
```
Colour and iconography only. Invisible to a screen reader and to anyone who cannot distinguish the hues.

```
I may make mistakes. Please verify important information.
```
Appended to every response. Carries no information, so people stop reading it, and it is absent exactly when it would have mattered.

```
Low confidence: the policy allows 15 days of carryover.
```
A hedged fabrication. If there is no source, this is a cannot-answer state, not a low-confidence one.

### Prefer

```
[▌▌▏] Medium confidence
      Partly grounded. Check the specifics against a source before you rely on them.
```
A band, a reason, and an implied action, all in text.

```
[▬] Can't answer this
    No reliable source covers this. [Search the handbook] or [ask the policy team]
```
A limit named plainly, with a route out instead of a dead end, and the route is a control.

```
Three possible readings of this clause:
  1. ... 2. ... 3. ...
Low confidence, so these are candidates rather than an answer.
```
N-best alternatives in place of one hedged answer.

---

## Acceptance criteria

- [ ] The mapping from model output to band is documented and verified against result quality.
- [ ] Each band states the behaviour it recommends, in product language.
- [ ] Bar shape and text label both carry the state; removing colour loses nothing.
- [ ] Label text meets 4.5:1, bars meet 3:1.
- [ ] A screen reader announces the band before the content it describes.
- [ ] Streaming output announces the band once, at completion.
- [ ] The cannot-answer state names a next step, and that step is keyboard operable.
- [ ] There is a documented floor below which output is suppressed rather than banded.

---

## Sources

[Apple Human Interface Guidelines: Generative AI](https://developer.apple.com/design/human-interface-guidelines/generative-ai) and [Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning), for the confidence, attribution and limitations patterns, and for the rule against conveying unverified confidence.

[Google People + AI Guidebook](https://pair.withgoogle.com/guidebook/), chapters [Explainability + Trust](https://pair.withgoogle.com/chapter/explainability-trust/) and [Errors + Graceful Failure](https://pair.withgoogle.com/chapter/errors-failing/), for trust calibration, the four confidence display types, n-best alternatives, and the failstate framing. Licensed CC BY-NC-SA 4.0; paraphrased here with attribution.
