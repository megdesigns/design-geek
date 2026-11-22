# Graceful Failure & Manual Fallback

**Pattern 02.** Serves [Principle 3](../principles.md#3-design-for-graceful-failure-and-a-manual-path) (graceful failure and a manual path), [Principle 2](../principles.md#2-preserve-control-and-reversibility) (control and reversibility), [Principle 5](../principles.md#5-build-for-accessibility) (accessibility) and [Principle 6](../principles.md#6-avoid-generic-ai-slop) (avoid generic AI slop).

Design file: [Fallback Block and Output State](https://www.figma.com/design/YqEqVIuX639b3zh9ljDinP/Research?node-id=18-84)

---

## Problem

Failure is designed last, if at all. The happy path gets the attention and everything else inherits a spinner that stops, a red box carrying a stack trace, or a cheerful apology with no way out. The person is left holding a dead end and no idea whether their work survived.

The deeper problem is structural. When the AI path is the only path, every model limitation becomes a product limitation. A feature with no manual route stops working when the model does, and the person has no way to finish the task they came to do.

Both source guidelines treat this as foundational rather than as polish. The People + AI Guidebook's Mental Models chapter says to make sure there is a default experience that does not rely on AI. Its Feedback + Control chapter calls the manual method a safe and useful fallback. Apple's generative AI guidance asks for a great experience even when generative features are unavailable or the person opts out, and to offer a non-AI fallback where possible.

---

## When to use

- The AI feature can fail, return nothing, or return something it cannot stand behind, which is all of them.
- A manual route to the same outcome exists, or can exist. If it genuinely cannot, say so explicitly rather than implying one.
- The person has something to lose, such as a draft, a queue position, or time already spent.

## When not to use

- **As a replacement for fixing the failure.** A well-designed fallback for an error that fires on a third of requests is a well-designed symptom. Fix the rate first.
- **For a context error.** When the output is correct but wrong for this moment, the person perceives an error the system cannot detect. That is handled by making the assumption visible and correctable inside the output, not by a failure block. See [Variants](#variants) below.
- **As a blanket disclaimer.** A standing "AI can make mistakes" notice is not this pattern. It names no limit, offers no route, and is absent exactly when it would have mattered.
- **Where the honest answer is to remove the feature.** If the manual path is always better, ship the manual path.

---

## Anatomy

Every state carries four things, in this order. Drop any one and it stops being a fallback and becomes an error message.

| | |
|---|---|
| **1. Name the limit** | Specifically, in plain language, in the product's own vocabulary. "Nothing in the policy library covers contractor leave," not "Unable to process request." |
| **2. Say what survived** | The unasked question in every failure is whether work was lost. Answer it before it is asked: the draft is saved, nothing was sent, changes are stored locally. |
| **3. Primary action is manual** | Exactly one primary, and it never involves the model. Search, browse, write it yourself. This is the non-AI path made visible at the moment it is needed. |
| **4. Secondary options** | Retry, refine the input, hand off to a person. Zero to two, never competing with the primary for weight. |

---

## Variants

### Failure states

| State | What happened | Behaviour |
|---|---|---|
| **Low confidence** | The system produced something it cannot stand behind | Output stays on screen, block sits beneath it. Overlaps [Uncertainty Signaling](uncertainty-signaling.md) |
| **Cannot answer** | No grounded answer exists, often correctly | Replaces the output rather than sitting beside it |
| **Error** | The system broke, as distinct from reaching a limit of knowledge | Must state what was preserved |
| **Offline** | Degraded mode. The product works, the AI part does not | Name what still functions |

**Low confidence.** The Guidebook's framing is a relevance error: high confidence, wrong context, or a result the system cannot justify. The output remains because discarding it wastes work the person may still find useful. The primary action is the manual route, not regenerate.

**Cannot answer.** A failstate in the Guidebook's sense, often a true negative: the system is correct that no answer exists, and the person expected one anyway. This replaces the output because leaving a guess on screen beside "I cannot answer" is the worst of both. It must name what is missing.

**Error.** A system failure. Timeouts, rate limits, service outages. The distinguishing requirement is preservation: say what survived, because that is what the person is actually worried about. Apple's guidance on describing what happened in plain language and offering a clear next step applies directly.

**Offline.** Degraded, not dead. The failure here is implying total failure when only one capability is gone. Name what still works.

**No context-error variant, deliberately.** The Guidebook names context errors as the AI-specific addition to the classic user-error and system-error split: the output is technically right, but it rests on an assumption that was wrong for this person or this moment. The system cannot detect it, so no failure block can fire. The answer is to surface the assumption in the output and make it correctable, which belongs to a different pattern.

### Surrounding states

Not failures, but they fail the same way when nobody designs them.

| State | Requirement |
|---|---|
| **Loading** | Name the work in progress, not "Processing". Give a time sense and an escape. |
| **Partial** | Mark the gap explicitly in text. Partial output read as complete is a silent failure. |
| **Empty** | One obvious next action. Not an error, and it must not borrow error styling. |

Apple's guidance on progress feedback is specific here: "Finding substitutions for ingredients" rather than "Processing", because it reduces uncertainty and makes waiting feel purposeful.

---

## Action hierarchy

The rule that makes this pattern work, and the one most often broken: **the primary action is the manual path, not "try again."**

Retry as primary teaches people that the way out of AI failure is more AI. It is also the action least likely to succeed, since nothing about the situation has changed between the failure and the retry. Demote it to secondary and give the top slot to the route that does not depend on the model.

Human handoff earns a place when a person can actually resolve it and the route is real. A "contact support" link pointing at an unmonitored form is worse than no link, because it converts an honest dead end into a hidden one.

The Guidebook's framing of returning control is worth keeping in view: the person needs awareness of the situation, what they need to do next, and how to do it. It also warns that handing control back can itself be risky, which matters more as stakes rise.

---

## Copy rules

- **Name the limit.** Say what the system could not do and why, as far as you can without handing someone a way to game it. The Guidebook's spam-filter caution sets that boundary: do not over-explain the mechanism where the explanation is itself exploitable.
- **No blame.** A context error is the system's assumption, not the person's mistake. Nothing here says "invalid input."
- **No apology as content.** "Sorry, something went wrong" occupies the space a next step should hold. If you apologise, do it in four words, then say what to do.
- **Boring on purpose.** The Guidebook's phrasing is to make failure safe, boring, and a natural part of the product. No alarm colours for a state that is working as intended.
- **Real nouns and real numbers.** "14 handbook sections", "timed out after 30 seconds". Specificity is what separates a designed state from a template, and it is the first thing sanded off in review.

---

## Accessibility notes

**Announce once, as a status.** The failure block is an `aria-live="polite"` region, or it receives focus when it replaces output that had focus. It never announces mid-stream and never announces twice.

**Place focus deliberately.** When a block replaces output, move focus to the block heading, so a screen reader user is not stranded in a region that no longer exists. When it sits beside output, leave focus alone and let the live region do the work. Moving focus unprompted in the second case interrupts reading.

**Every route out is a real control.** Primary, secondary and link are all buttons or links, reachable in reading order in that order, with visible focus. A link styled as body text fails this, as does an action that only appears on hover.

**Colour is not the signal.** Each state carries a distinct glyph in a filled tile, plus a distinct headline. Error is not red-only, offline is not grey-only. Remove colour entirely and the four states stay distinguishable.

**Contrast.** Headline text meets 4.5:1 on its tint. Glyphs meet 3:1 as meaningful graphics. The secondary action border meets 3:1, so the control boundary survives forced-colours mode.

**Progress is not only a bar.** The text says what is happening and roughly how long. A bar with no text is unreadable non-visually. Throttle progress announcements rather than firing them continuously.

**Motion.** No spinner as the sole indicator, and any animation on the progress bar respects `prefers-reduced-motion`.

---

## Platforms

Both component sets carry a `Platform` property with `Web` and `Mobile`. The states and their meaning are identical across platforms. Four things change.

| | Web | Mobile |
|---|---|---|
| Block width | 560 | 370 |
| Actions | A row, primary first | A full width stack in priority order |
| Touch target | 37px | 48px |
| Body / headline type | 14 / 16 | 15 / 17 |

**Actions stack rather than wrap.** A wrapped row puts the secondary action above the primary as often as not. Stacking in priority order keeps the manual route as the first thing the thumb reaches.

**Type goes up, not down.** The common instinct is to shrink text on small screens. Reading distance does not shrink with the viewport, so the opposite is correct.

**Explanation copy is cut, not truncated.** At 360px the explanation competes with the output it describes. What survives is the limit and the route; the elaboration goes. No ellipsis, no "show more", nothing hidden behind a tap that was visible on web.

**Touch targets are 48px**, above the 44px WCAG 2.2 AA target-size minimum, because a failure state is where people tap in a hurry.

---

## Implementation notes

**Markup shape.** A `role="status"` container holding a heading, a paragraph, and an action group. `role="alert"` is wrong for most of these: alert is assertive and interrupts, which suits a genuine error but not low confidence, empty, or offline. Reserve assertive for the case where acting on stale output would cause harm.

**Where the block mounts.** Low confidence appends after the output node, so the output keeps its position and focus stays put. Cannot answer, error and offline replace the output node, which means the previously focused element is gone, which is what triggers the focus move to the heading.

**Timing.** Do not render a loading state for work that completes under roughly 300ms, or the interface flickers. Once shown, hold it for a minimum duration so it does not flash. If the operation exceeds the stated estimate, update the text rather than letting a stale estimate sit there.

**Cancel must actually cancel.** Abort the request, do not just hide the indicator. A cancel that leaves the work running and the tokens burning is a lie in the interface.

**Preservation is a claim you have to back.** "Your draft is saved" means the draft is persisted before the AI call is made, not after it returns. If persistence happens on success only, the message is false in exactly the case where it is shown.

**Retry needs a changed variable.** An identical retry against an identical request usually fails identically. Either change something (a shorter context, a different model, a longer timeout) or label it honestly and expect the same result. Rate-limit the retry so it cannot be hammered.

**Offline detection.** `navigator.onLine` reports the network interface, not reachability, so it produces false positives on captive portals. Treat a failed request as the signal and use the online event only to prompt a re-check.

**Error taxonomy in code.** Map transport failures, model refusals, empty results and low-confidence results to separate states at the boundary, not in the view. Collapsing them into one catch block is how every failure ends up saying "something went wrong."

**Component API.** In the design file the pattern is two component sets. `Fallback Block` takes a `State` property with four values. `Output State` takes a `State` property with three. Both compose instances of an `Action` component whose `Kind` is Primary, Secondary or Link, so the one-primary rule is visible in the structure rather than written in a comment. All three sets also carry a `Platform` property, so the web and mobile layouts are one component rather than two that drift apart.

---

## Good and bad examples

### Avoid

```
Something went wrong. Please try again.
```
Names no limit, preserves no context, and the only route out is the one that just failed.

```
[spinner, then the last frame stays on screen]
```
The person cannot tell whether it failed, is slow, or finished empty.

```
[ Try again ]  [ Search manually ]
```
Retry as primary. Teaches that the way out of AI failure is more AI, and nothing has changed to make it succeed.

```
⚠ No documents in this folder
```
An empty state wearing error styling. Alarm for a state that is working as intended.

### Prefer

```
I can't answer this one
Nothing in the policy library covers contractor leave. Guessing here
would cost you more time than it saves.

[ Browse policies ]  [ Rephrase the question ]  Ask the policy team
```
Limit named, manual route leading, retry available but not leading, human handoff present and real.

```
The assistant didn't respond
The request timed out after 30 seconds. Your draft is saved and
nothing was sent.

[ Write it yourself ]  [ Try again ]  Report this
```
Answers the question the person has not asked yet.

```
Reading 14 handbook sections
About 10 seconds left. You can keep editing while this runs.
[progress bar]  Cancel
```
Specific progress with an escape, instead of a shimmer.

---

## Acceptance criteria

- [ ] Every AI feature has a manual path, and that path is exercised in testing rather than assumed.
- [ ] Each state names the specific limit in the product's own vocabulary.
- [ ] Each state says what was preserved, and that claim is true at the point the state renders.
- [ ] The primary action does not involve the model. Exactly one primary per block.
- [ ] No terminal state. Every state offers at least one action.
- [ ] Loading, partial, empty, error and offline are all specified, not inherited from a default.
- [ ] The block announces once via a status region, and focus moves to the heading only when the block replaced focused output.
- [ ] Removing colour leaves all states distinguishable.
- [ ] Cancel aborts the underlying request.

---

## Sources

[Apple Human Interface Guidelines: Generative AI](https://developer.apple.com/design/human-interface-guidelines/generative-ai) and [Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning), for the non-AI fallback, specific progress feedback, describing what happened in plain language with a clear next step, and the limitations pattern.

[Google People + AI Guidebook](https://pair.withgoogle.com/guidebook/), chapters [Errors + Graceful Failure](https://pair.withgoogle.com/chapter/errors-failing/), [Mental Models](https://pair.withgoogle.com/chapter/mental-models/) and [Feedback + Control](https://pair.withgoogle.com/chapter/feedback-controls/), for the error taxonomy, the default non-AI experience, returning control to the person, making failure safe and boring, and the caution on over-explaining exploitable mechanisms. Licensed CC BY-NC-SA 4.0; paraphrased here with attribution.
