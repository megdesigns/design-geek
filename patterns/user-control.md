# User Control & Reversibility

**Pattern 03.** Serves [Principle 2](../principles.md#2-preserve-control-and-reversibility) (control and reversibility), [Principle 3](../principles.md#3-design-for-graceful-failure-and-a-manual-path) (graceful failure), [Principle 5](../principles.md#5-build-for-accessibility) (accessibility) and [Principle 6](../principles.md#6-avoid-generic-ai-slop) (avoid generic AI slop).

Design file: [AI Output Block and Approval Step](https://www.figma.com/design/YqEqVIuX639b3zh9ljDinP/Research?node-id=49-274)

---

## Problem

Generated output arrives as a finished object. The person can accept it or throw it away, and nothing in between. So they copy it into another window, fix it there, and paste it back. The system never learns what was wrong, the correction is invisible to everyone downstream, and the feature looks like it worked.

The second failure is quieter. An agent acts, the action lands, and the person finds out afterwards. Consent arrives as a notification rather than as a decision.

Both sources treat this as foundational. Apple's generative AI guidance asks that people remain in charge of decision making, that they can dismiss, revert or retry, and that Edit, Undo, Retry and Adjust sit near generated content with a clear signal when a correction takes effect. The People + AI Guidebook frames it as automation versus augmentation, with human-in-the-loop as the default and preview, test, edit or undo available for anything automated.

---

## The four affordances

Control is not one control. These do different jobs and a feature usually needs all four.

| | |
|---|---|
| **Edit in place** | The generated text is an editable field, not a read-only block. Correction costs one click, not a round trip through another app. |
| **Undo at scope** | Undo reverses exactly what the action did. An edit undoes one edit; a bulk apply undoes the whole batch in one gesture. |
| **Approval with scope** | Before a consequential action, show what will happen: how many items, which ones, what changes. An approval without visible scope is a rubber stamp. |
| **Reject with guidance** | Rejecting without saying why just rolls the dice again. Capture the correction and feed it into the next attempt. |

---

## Variants

### AI Output Block

| State | What it does |
|---|---|
| **Generated** | Resting state. Marked `AI-generated`. Actions: Edit, Regenerate, Dismiss |
| **Editing** | The same text as a focused field. Actions: Save, Cancel |
| **Edited** | Marked `Edited by you`. Actions: Undo edit, Edit again |
| **Regenerating** | Guidance field, required. Actions: Regenerate with this, Cancel |

**Edit leads, regenerate follows.** The resting state puts Edit first because editing in place is the cheaper correction. Regeneration discards work the person may have wanted to keep, and it is the action least likely to improve things, since nothing has changed except the sample.

**The edit marker is a provenance claim.** `Edited by you` has to be true. Marking human text as generated, or generated text as human, is the same category of error as a false citation.

**Cancel restores exactly.** Not a near-miss regeneration of the original. The pre-edit text is held and put back verbatim.

**Guidance is required before regenerating.** The field gates the primary action. The reason is given in text, not implied by a disabled button.

### Approval Step

| State | Use for | Says |
|---|---|---|
| **Preview** | Consequential but recoverable | "You can undo this for 30 days." |
| **Confirm** | Irreversible, outward facing | "There is no undo for this one." |

Both count the scope, itemise it, and repeat the count in the primary label: `Apply 12 changes`, not `Apply`. A button that stands alone has to carry its own scope, because that is often all a screen reader user hears before committing.

Confirm states the absence of undo in words. It does not imply safety through tone and it does not rely on a red tint to carry the warning.

Reversibility is a chip, not a sentence. `Undoable for 30 days` or `No undo` sits as a small bordered pill under the scope list, so the single most important fact about the action is scannable rather than buried in a line of prose. The chip carries a dot and a text label, so it survives colour removal.

---

## When an approval step is required

| | |
|---|---|
| **Always** | Anything outward facing (send, publish, post, share), anything that moves money, anything that cannot be undone |
| **Usually** | Bulk operations over more than a handful of items, and any write to a system of record |
| **Not needed** | Reversible, local, single-item changes. A toast with Undo is the right weight |

Apple's line is to avoid automating destructive actions and actions that are hard to undo, and to ask for confirmation before acting significantly on someone's behalf.

## When not to use

- **As a safety blanket over a weak feature.** If a model is wrong often enough to need a gate on every action, the gate is not the fix.
- **Undo you cannot honour.** "Undo" on something already sent is a lie. Say it cannot be unsent.
- **An approval on every action.** Gate everything and people click through everything, which costs you the gate exactly when it mattered.
- **A marker you cannot stand behind.** If you cannot reliably tell human text from generated text in your own data model, do not display either marker.

---

## Platforms

Both sets carry a `Platform` property.

| | Web | Mobile |
|---|---|---|
| Block width | 520 | 358 |
| Actions | Row, wrapping | Full width stack, priority order |
| Touch target | 37px | 48px |

On mobile the actions stack rather than wrap, for the same reason as in [Graceful Failure](graceful-failure.md): a wrapped row puts the secondary action above the primary as often as not.

---

## Accessibility notes

**Edit mode.** Entering it moves focus into the field and announces the mode change once, as a status. Leaving it returns focus to the control that opened it. Focus never lands somewhere the person did not ask to be.

**Undo is a real control, not a toast.** A timed-only undo excludes anyone who reads slowly, uses a screen reader, or looks away, and it is a direct WCAG 2.2 timing failure. If a toast carries undo, the same action must remain reachable in the row afterwards.

**The approval dialog is a modal.** Focus trapped inside, the heading as its accessible name, the scope list readable in order before the buttons. Escape cancels and never commits.

**Labels carry their scope.** `Apply 12 changes`, `Send to 240 people`. A screen reader user hearing only the button needs the count.

**Markers are text.** `Edited by you` survives colour removal, greyscale and forced colours. The tint is reinforcement.

**The guidance field is labelled**, and its requirement is stated in text rather than conveyed by a disabled button alone.

---

## Good and bad examples

### Avoid

```
[ Accept all ]
```
Forty generated rows, one gesture, no way to see which.

```
Changes applied.  [Undo]     (disappears after 5s)
```
Excludes anyone who reads slowly and fails WCAG timing.

```
Are you sure?   [Cancel] [OK]
```
Asks for consent without showing scope. The answer is always yes.

```
[ Regenerate ]    (as the only way to correct anything)
```
Discards work and teaches that the fix for AI is more AI.

### Prefer

```
Apply 12 changes?
12 of 40 rows will change. The other 28 stay as they are.
  · 8 rows get a new owner
  · 3 rows change status to Blocked
  · 1 row has its due date cleared
You can undo this for 30 days.
[ Review all 12 ]  [ Apply 12 changes ]
```

```
Send to 240 people?
  · 240 recipients
  · Posts immediately
  · Cannot be unsent
There is no undo for this one.
[ Cancel ]  [ Send to 240 people ]
```

```
[Edited by you]
Contractors accrue leave at the same rate as salaried staff.
[ Undo edit ]  Edit again
```

---

## Tokens

| Role | Tokens |
|---|---|
| Edited marker | `color/control/edited/{fg, bg, border}` |
| Irreversible | `color/danger/{fg, bg, border}` with `border/width/emphasis` |
| Field | `color/field/{bg, border}`; focus ring uses `color/action/primary/bg` at `border/width/emphasis` |
| Actions | the shared `Action` set, `Kind` = Primary, Secondary, Link |

A human edit is **marked, not warned about**. `color/control/edited` is blue, deliberately not the amber or red of the confidence and danger ramps, because an edit is a correct and expected event.

---

## Acceptance criteria

- [ ] Generated output can be edited in place without leaving the interface.
- [ ] Cancel restores the pre-edit text verbatim.
- [ ] Every action has an undo at the same scope it was taken, bulk included.
- [ ] Undo remains reachable after any toast has gone.
- [ ] Outward-facing, financial and irreversible actions pass through an approval step.
- [ ] The approval shows a counted, itemised scope before commit.
- [ ] The primary button label carries the scope.
- [ ] Irreversibility is stated in words, not implied.
- [ ] Human edits are marked, the marker is text, and it is accurate.
- [ ] Regeneration requires guidance, with the requirement stated in text.

---

## Sources

[Apple Human Interface Guidelines: Generative AI](https://developer.apple.com/design/human-interface-guidelines/generative-ai) and [Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning), for keeping people in control, dismiss and revert and retry, surfacing Edit, Undo, Retry and Adjust near generated content, acknowledging corrections, avoiding automated destructive actions, and the corrections pattern including guided over freeform correction.

[Google People + AI Guidebook](https://pair.withgoogle.com/guidebook/), chapters [User Needs + Defining Success](https://pair.withgoogle.com/chapter/user-needs/) and [Feedback + Control](https://pair.withgoogle.com/chapter/feedback-controls/), for automation versus augmentation, human-in-the-loop, preview, test, edit or undo, editability and reset, and opting out. Licensed CC BY-NC-SA 4.0; paraphrased here with attribution.

[WCAG 2.2](https://www.w3.org/TR/WCAG22/) for the timing constraints that rule out toast-only undo.
