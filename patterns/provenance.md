# Provenance / Source Display

**Pattern 04.** Serves [Principle 4](../principles.md#4-provide-provenance-without-overload) (provenance without overload), [Principle 1](../principles.md#1-make-uncertainty-visible-and-actionable) (uncertainty), [Principle 5](../principles.md#5-build-for-accessibility) (accessibility) and [Principle 6](../principles.md#6-avoid-generic-ai-slop) (avoid generic AI slop).

Design file: [Source Display component set](https://www.figma.com/design/YqEqVIuX639b3zh9ljDinP/Research?node-id=52-306)

---

## Problem

Provenance competes for attention with the content it describes. Strip it out and nobody can check anything. Pile it on and the interface fills with citation chrome that people learn to skip, which is worse than absent because it looks like diligence.

The specific failure is **grain**. A footnote list under a synthesised paragraph makes the reader do the matching. They cannot tell which sentence came from which source, so they re-check all of it or none of it, and in practice none of it.

The Guidebook's framing is to articulate data sources, structured as scope, reach and removal, with the sharp instruction that the best explanation is likely a partial one. Apple's complementary pattern is attribution: the basis for a result without an account of how the model works.

---

## When to use

- Output is presented as grounded in sources, retrieved content, or the person's own data.
- The person can act on the claim, and being wrong has a cost.
- There is a real artefact to point at. A document, a record, a page.

## When not to use

- **The system did not actually read the source.** A retrieved title is not evidence the content was used. Citing it is a false claim with a link attached.
- **Nothing grounds the output.** That is the No source state, not a thinner version of Sourced.
- **As a stand-in for a confidence band.** Coverage and reliability are different questions. A fully sourced answer can still be wrong, and [Uncertainty Signaling](uncertainty-signaling.md) answers the other one.
- **Where the volume would drown the content.** If every sentence needs three citations, the format is wrong, not the citation count.

---

## Variants

| State | Chip | Treatment |
|---|---|---|
| **Sourced** | `3 sources` | Inline `[n]` markers on each claim, list collapsed |
| **Expanded** | `3 sources` | The disclosed layer: every source with publisher and as-of date |
| **Partially sourced** | `1 of 2 claims sourced` | Sourced spans marked `[n]`, the unsupported span marked `(unverified)` in text |
| **No source** | `No source for this` | No markers. Neutral treatment and a manual route out |

### Sourced

Markers sit on the claims they support, not in a pile at the bottom. The resting state is one line: a chip stating coverage and a control to open the list.

### Expanded

Each source carries **publisher and as-of date**. Staleness is the most common and least visible provenance failure, and the Guidebook frames it as situational stakes: the same hour-old data matters differently depending on what the person is about to do.

Opening the list never reflows the content above it. The reader does not lose their place as a reward for checking.

### Partially sourced

The common real case, and the one most interfaces get wrong by marking the whole response. Marking the response tells the reader to re-check everything or nothing. Marking the unsupported span makes the check cheap and specific.

`(unverified)` is written in text, on the span that has the gap. Not a tint, not a dotted underline, not a hover.

### No source

Says plainly that nothing backs this, and hands over a manual route. It is not an error and not a confidence band. Never dress an unsourced claim in a sourced treatment.

---

## Disclosure rules

| | |
|---|---|
| **One line by default** | A chip stating coverage, plus a control to open the list. Nothing else in the resting state |
| **Claim-level markers** | Inline, on the span they support |
| **List on demand** | Progressive disclosure. The best explanation is likely a partial one |
| **Never a modal** | Provenance the person must dismiss to keep reading is a tax, not a service |
| **Opening never reflows** | The list expands below; content above does not move |

### Keep the kinds apart

Retrieved and quoted, retrieved and paraphrased, and model generated are three different epistemic states. An interface that renders them identically is making a claim it cannot support. A source link never implies the source agrees with the sentence beside it.

### Scope, reach, removal

Where personal or workspace data feeds the output, the UI must answer all three: what data is used for what purpose, whether it is this person's data or everyone's, and whether they can take it out.

---

## Platforms

| | Web | Mobile |
|---|---|---|
| Block width | 560 | 358 |
| Provenance row | Chip and control inline | Wraps when needed |
| Markers | unchanged | unchanged |

Markers do not shrink on mobile. They are the claim-level carrier of the whole pattern and are already small.

---

## Accessibility notes

**The disclosure control is a button** with `aria-expanded`, and the list it reveals is the element it controls. Not a hover affordance: a citation reachable only on hover is unreachable by keyboard and on touch.

**Markers are not bare brackets.** `[1]` alone announces as a stray character. Each marker carries an accessible name identifying its source, and the visible bracket is decorative.

**Markers are not colour only.** They carry bracket glyphs and a weight change, so a sourced span survives greyscale and forced colours.

**Expanding announces once, politely, and does not move focus.** Focus stays on the control so the reader can collapse it again without hunting for it.

**The unverified span is marked in text.** `(unverified)` is readable by a screen reader. A tint or dotted underline alone is not.

**The source list is a real list** in the markup, so a screen reader announces how many sources there are before reading them.

**Contrast.** Markers and chip text meet 4.5:1 against their backgrounds. Chip borders meet 3:1 so the boundary survives forced colours.

---

## Good and bad examples

### Avoid

```
Sources: [1] [2] [3] [4] [5] [6]
```
Under a synthesised paragraph. The reader does the matching.

```
Contractors accrue leave at the same rate as salaried staff. ⓘ
```
Sources behind a hover tooltip. Unreachable by keyboard, unreachable on touch.

```
Sources: 11
```
On a one-line answer. Volume as a proxy for rigour.

```
[3 sources]   (on a claim with no retrieved source behind it)
```
The treatment makes a claim the content cannot support.

### Prefer

```
Contractors accrue leave at the same rate as salaried staff[1].
Unused leave is paid out at contract end[2].

[3 sources]  Show sources
```

```
[1] Employee Handbook 2026, section 4.3
    People Ops · updated 12 Nov 2026
```
Publisher and as-of date, so staleness is visible without opening anything else.

```
Unused leave is paid out at contract end (unverified).

[1 of 2 claims sourced]  Show the 1 source
```
The gap named in text, on the span that has it.

---

## Tokens

| State | Tokens |
|---|---|
| Sourced | `color/source/full/{fg, bg, border}`, aliased to the blue ramp |
| Partially sourced | `color/source/partial/{fg, bg, border}`, the amber ramp |
| No source | `color/source/none/{fg, bg, border}`, neutral slate |
| Disclosed list | `color/surface/muted` with `color/border/subtle` at `border/width/default` |

**Provenance is blue, never green.** It must not read as a confidence band, because coverage and reliability are different questions and sharing a palette would merge them in the reader's head.

**Partially sourced shares the amber ramp with Medium confidence, by intent.** Both mean the same thing to the reader: check this before you rely on it.

**No source is slate, not red.** It is a limit, not an error.

---

## Acceptance criteria

- [ ] The default view carries at most one line of provenance per output.
- [ ] Markers resolve to the specific claim, not only to the whole response.
- [ ] Every source link reaches a real location containing the cited content.
- [ ] Every disclosed source shows publisher and as-of date.
- [ ] Partially sourced marks the unsupported span in text, on the span.
- [ ] No source states the absence and offers a manual route.
- [ ] The disclosure control is a keyboard-operable button with `aria-expanded`.
- [ ] Expanding does not move focus or reflow content above it.
- [ ] Markers survive colour removal.
- [ ] Where personal data informs the output, scope, reach and removal are answerable from the UI.

---

## Sources

[Apple Human Interface Guidelines: Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning) for the attribution pattern, keeping attributions factual and free of implied judgement, avoiding statistical jargon except where the result is itself statistical, and balancing attribution against UI complexity.

[Google People + AI Guidebook](https://pair.withgoogle.com/guidebook/), chapter [Explainability + Trust](https://pair.withgoogle.com/chapter/explainability-trust/), for articulating data sources as scope, reach and removal, partial explanations with progressive disclosure, and situational stakes including the freshness example. Licensed CC BY-NC-SA 4.0; paraphrased here with attribution.
