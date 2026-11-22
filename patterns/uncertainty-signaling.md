# Uncertainty Signaling

**Pattern 01.** Serves [Principle 1](../principles.md#1-make-uncertainty-visible-and-actionable) (make uncertainty visible and actionable), [Principle 5](../principles.md#5-build-for-accessibility) (accessibility) and [Principle 6](../principles.md#6-avoid-generic-ai-slop) (avoid generic AI slop).

Design file: [Uncertainty Signal component set](https://www.figma.com/design/YqEqVIuX639b3zh9ljDinP/Research?node-id=9-32)

This pattern is the deeper, public continuation of an internal AI pattern exploration carried out at CSL Behring, which covered components and states for AI interaction and remained internal.

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

The encoding is ordinal and carries three separate signals. Filled bars count the evidence, and empty bars are **outlined rather than greyed**, so the difference between a filled and an unfilled bar is a shape rather than a tint. The container stroke carries the second signal: 1px solid for a settled band, 2px dashed for Low because the result is provisional, 2px solid for the terminal state. A 3px rail on the leading edge, coloured by state, carries the third. The rail is the fastest of the three to read at a distance, and it is what separates Medium from Low at a glance rather than saturation.

High is deliberately the quietest of the four. Reliable should not read as loud, or the band becomes the overconfidence it exists to prevent.

| State | Indicator | Label | Supporting explanation |
|---|---|---|---|
| **High confidence** | Three filled bars, 1px solid border, state rail | High confidence | Grounded in the sources listed below. Worth a glance at the citation before you act on it. |
| **Medium confidence** | Two filled, one outlined, 1px solid border, state rail | Medium confidence | Partly grounded. Check the specifics against a source before you rely on them. |
| **Low confidence** | One filled, two outlined, 2px dashed border, state rail | Low confidence | A starting point, not an answer. Verify every claim before you use it. |
| **Cannot answer** | A single dash, no bars, 2px solid border, state rail | Can't answer this | No reliable source covers this. Search the handbook directly, or ask the policy team. |

### High confidence

The output is traceable to a source the person can open. This is never the default state: it has to be earned by a verified mapping, otherwise it is the overconfidence problem wearing a badge.

### Medium confidence

The shape of the answer holds but the details do not. Pair this band with claim-level marks on the specific spans that are weak. A response-level band alone pushes the reader to re-check everything or nothing, which is the behaviour the pattern exists to prevent.

### Low confidence

Plausible but unverified. Consider showing two or three alternatives rather than one hedged answer. The People + AI Guidebook's n-best alternatives pattern is specifically useful here, because presenting candidates hands judgement back to the person instead of asking them to discount a number.

### Cannot answer

**A designed terminal state, not a fourth confidence band.** It is built differently on purpose, and the difference is structural rather than decorative:

- **Layout changes.** The three bands are horizontal, indicator beside text. This one is vertical: heading, explanation, then the control. The silhouette differs before a single word is read.
- **The bars are gone.** Bars count evidence. There is no evidence to count, so showing one lit bar would imply a weak answer exists.
- **It carries a real control**, an instance of the Primary action, not a sentence suggesting what to do. On mobile that control is full width at 48px.
- **The border is 2px solid**, matching Low in weight but not in treatment, so it reads as decided rather than provisional.

Three rules keep it honest:

1. It is a designed state, not an error. The system is behaving correctly and the UI should not apologise.
2. It must name what is missing, specifically. "No reliable source covers this" is the content, not the framing.
3. **It is not a softer Low.** Downgrading "I cannot answer" into "low confidence, here is a guess" is the single most damaging substitution in this pattern, because it converts an honest limit into a plausible fabrication.

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

## Platforms

The component set carries a `Platform` property with `Web` and `Mobile`. The four states, their thresholds and their announced strings are identical across both.

| | Web | Mobile |
|---|---|---|
| Block width | 420 | 358 |
| Label / explanation | 15 / 13 | 15 / 14 |
| Bar size | 3.5 x 7/11/15 | unchanged |
| Terminal control | inline, 37px | full width, 48px |

**The explanation earns less room on mobile, not smaller type.** A signal that is harder to read than the output it qualifies has inverted its own purpose, so the explanation text steps up from 13 to 14 while the sentence itself gets shorter.

**The bars do not shrink.** They are the non-colour carrier of the state, and they are already near the floor of what reads reliably at 4px wide.

**Nothing truncates.** Every text node fills its container and grows in height, so a longer string wraps rather than clipping. There is no ellipsis anywhere in this pattern, because a clipped uncertainty signal is a wrong uncertainty signal.

358px is the block width inside a 390px viewport with 16px gutters.

---

## Tokens

Every state resolves to the same four colour roles plus a stroke width. Later patterns reuse this set rather than inventing their own, which is why the roles are named by function and not by hue.

| State | Colour tokens | Stroke | Bars |
|---|---|---|---|
| High | `color/confidence/high/{fg, bg, border, bar}` | `border/width/default`, solid | 3 filled |
| Medium | `color/confidence/medium/{fg, bg, border, bar}` | `border/width/default`, solid | 2 filled, 1 outlined |
| Low | `color/confidence/low/{fg, bg, border, bar}` | `border/width/emphasis`, dashed | 1 filled, 2 outlined |
| Cannot answer | `color/confidence/unknown/{fg, bg, border, bar}` | `border/width/emphasis`, solid | none |

Shared by all four: `color/track/empty` for the outlined bar strokes, `color/text/secondary` for the explanation line.

Two deliberate choices in the token model:

- **`bar` is its own role**, aliased to the same primitive as `fg`. Today a bar and its label are the same colour. Keeping them separate means bar treatment can diverge later without editing a component.
- **`unknown` is not a confidence level.** It is named for what it is rather than extending the high/medium/low scale, so nobody is tempted to order it beneath Low.

The dash pattern on Low is set on the component rather than tokenised, because Figma has no variable type for it. That is a known gap, noted here so it is a decision rather than an oversight.

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
- [ ] Medium and Low differ by more than saturation: bar count and stroke treatment both change.
- [ ] Empty bars are outlined, not greyed, so filled and unfilled differ in shape.
- [ ] Cannot answer uses a different layout and carries a real control, not a sentence.
- [ ] Every state binds the four confidence roles; no hardcoded colour values in the component.
- [ ] Label text meets 4.5:1, bars meet 3:1.
- [ ] A screen reader announces the band before the content it describes.
- [ ] Streaming output announces the band once, at completion.
- [ ] The cannot-answer state names a next step, and that step is keyboard operable.
- [ ] There is a documented floor below which output is suppressed rather than banded.

---

## Sources

[Apple Human Interface Guidelines: Generative AI](https://developer.apple.com/design/human-interface-guidelines/generative-ai) and [Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning), for the confidence, attribution and limitations patterns, and for the rule against conveying unverified confidence.

[Google People + AI Guidebook](https://pair.withgoogle.com/guidebook/), chapters [Explainability + Trust](https://pair.withgoogle.com/chapter/explainability-trust/) and [Errors + Graceful Failure](https://pair.withgoogle.com/chapter/errors-failing/), for trust calibration, the four confidence display types, n-best alternatives, and the failstate framing. Licensed CC BY-NC-SA 4.0; paraphrased here with attribution.
