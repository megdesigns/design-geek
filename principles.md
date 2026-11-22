# Principles

**AI Interaction Pattern Library.** A shared vocabulary for designing AI interfaces that people can trust, steer, and recover from.

Six principles. Each one states the rule, says why it holds, gives concrete do/don't guidance, names the patterns it implies, and ends with acceptance criteria a reviewer can check.

Grounded in [Apple Human Interface Guidelines: Generative AI](https://developer.apple.com/design/human-interface-guidelines/generative-ai) and [Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning), and Google's [People + AI Guidebook](https://pair.withgoogle.com/guidebook/). See [Sources](#sources).

---

## Background & Continuity

At CSL Behring I independently explored an AI design system and pattern library, covering components and states for AI interaction patterns, and presented the work to the team. It remained internal.

That exploration made the gap visible. The hard problems in AI interface design are not the components themselves but uncertainty, control, graceful failure and accessibility, and those were the parts it only began to reach. This library is the deliberate next step: scoped to go deeper on exactly those four, held to acceptance criteria rather than opinion, and public so the work is usable outside one organisation.

---

## How to use this document

- **Contributing a pattern?** Every pattern in this library must name the principles it serves and must not violate any of them. If it violates one, that tension belongs in the pattern's write-up, not hidden.
- **Reviewing a design?** Run the acceptance criteria. They are written as falsifiable checks, not as sentiments.
- **Principles conflict.** Provenance versus restraint, automation versus control. When they do, say which one you chose and why. Resolution is a design decision worth recording.

**Layer.** These principles cover the *interface*: what the person sees, decides, and undoes. Model quality, evaluation, dataset sourcing, and fairness auditing are upstream and out of scope here, though they constrain what the interface can honestly claim.

---

## 1. Make uncertainty visible and actionable

**Show how much to trust an output, in a form that changes what the person does next.**

The goal is calibrated trust, meaning the person trusts the system where it is reliable and checks it where it isn't. Both failure directions are real: no trust makes the feature worthless, too much trust makes its errors invisible.

Raw numbers rarely achieve this. Apple's ML guidance is direct that displaying a bare "97% match" does not tell someone enough to make a choice, and advises translating confidence into concepts people already understand: semantic categories like "high chance" / "low chance", ranking that implies confidence, or an actionable recommendation such as "This is a good time to buy." The People + AI Guidebook makes the same point from the other side: it calls numeric indicators risky because they presume the reader understands probability, and asks pointedly what a person is supposed to do differently at 85.8% certain versus 87% certain.

### Do

- **Prefer qualitative bands.** Two to four named levels with documented thresholds. State what action each band invites. A band whose name implies no change in behaviour is decoration.
- **Attach uncertainty to the claim, not the response.** A summary of ten sources is not uniformly reliable. Mark the sentence or field that is shaky. Claim-level signalling is what makes verification cheap; a single header-level badge pushes the reader to re-check everything or nothing.
- **Offer alternatives instead of a score when confidence is low.** The Guidebook's *n-best alternatives* pattern, presenting several candidates rather than one hedged answer, is specifically useful in low-confidence situations, because it hands judgement back to the person.
- **Change the presentation at different thresholds**, not just the label. Apple's example: Photos shows face-recognition results directly when confident, and asks the person to confirm when it is not.
- **Suppress rather than hedge** below a floor. Where confidence genuinely tracks quality, withholding a weak proactive suggestion beats shipping it with a warning.
- **Show numbers where numbers are the subject.** Forecasts, polling, prices, lab values, sports statistics: here intervals and percentages are the literacy people already have, and withholding them is the error.

### Don't

- Don't display a confidence value you have not verified correlates with output quality. Apple is explicit: if you don't know how your confidence values relate to result quality, don't convey confidence at all.
- Don't use false precision. One decimal place implies a calibration you almost certainly do not have.
- Don't present a model's self-reported certainty as the system's certainty without saying so.
- Don't let a confidence band substitute for a citation. They answer different questions.

### Patterns this implies

Confidence band · Claim-level uncertainty marker · N-best alternatives · Threshold-dependent presentation · Low-confidence suppression · Verification prompt

### Acceptance criteria

- [ ] Every uncertainty signal has a documented mapping from model output to displayed state.
- [ ] Each band states the behaviour it recommends, in product language.
- [ ] At least one uncertainty signal is attached below the response level (a claim, field, row, or span), or the write-up says why that is impossible here.
- [ ] The signal survives a screen reader (see Principle 5) and does not rely on colour alone.
- [ ] There is a documented low-confidence floor and a defined behaviour at it.

---

## 2. Preserve control and reversibility

**The person decides. The system proposes, and every proposal can be edited, undone, or declined.**

Apple's generative AI guidance puts agency first: people remain in charge of decision-making, can dismiss content they don't want, and can revert or retry transformations they disagree with. It recommends surfacing Edit, Undo, Retry, and Adjust near generated content, and, critically, acknowledging when a correction takes effect, because that feedback is how an accurate mental model forms. The Guidebook frames the same ground as automation versus augmentation: keep a human in the loop, and let people preview, test, edit, or undo anything the system automates.

Reversibility is not only a safety net. It is what makes experimentation safe, and experimentation is how people learn what a feature can do.

### Do

- **Make output editable in place.** Generated text, structure, and parameters should be ordinary editable content, not a read-only block the person must copy out to change.
- **Keep Undo at the same scope as the action.** A single edit undoes one step; a bulk operation needs one gesture that reverses the whole batch.
- **Confirm before consequential actions.** Apple's line is to avoid automating destructive actions like deleting photos, and actions that are hard to undo like making a purchase on someone's behalf, and generally to ask for confirmation before acting significantly on a person's behalf. Scale the friction to the consequence: a toast with Undo for the cheap and recoverable, an explicit review step for the irreversible, the outward-facing, and the financial.
- **Show what will happen before it happens.** A diff, a preview, a dry run. Approval is meaningless without visible scope: how many items, which ones, and what changes.
- **Confirm the correction landed**, and persist it. Apple's ML guidance is firm that corrections must take effect immediately and stick, so the person doesn't correct the same thing twice.
- **Let people correct their corrections.** The second attempt deserves the same responsiveness as the first.
- **Prefer guided corrections to freeform ones** where the alternatives are knowable. Picking from candidates costs less effort than composing a fix.
- **Allow opt-out, and respect it.** Once the benefit has been explained, a decision not to use the feature is a valid end state, not an objection to overcome.

### Don't

- Don't batch an irreversible action behind a single undifferentiated "Accept all".
- Don't rely on corrections to compensate for weak output. Apple warns this erodes trust and hollows out the feature's value; the Guidebook makes the related point that people abandon a feature when correcting it costs more than doing the task themselves.
- Don't auto-apply AI changes to content the person has already edited without flagging the collision.
- Don't make opting out harder to find than opting in, and don't re-ask after a decline.
- Don't treat a confirmation dialog as a substitute for reversibility. Both, where the stakes warrant.

### Patterns this implies

Editable output · Undo with matching scope · Diff preview · Approval step for consequential actions · Correction acknowledgement · Guided correction · Retry with adjustment · Opt-out control · Reset to non-personalised

### Acceptance criteria

- [ ] Every AI-authored artefact has a path to edit, regenerate, or discard.
- [ ] Every action with an outward effect (send, publish, pay, delete, share) has an explicit approval step showing scope before commit.
- [ ] Undo exists at every scope the UI offers for acting, bulk included.
- [ ] Corrections are acknowledged visibly and persist across sessions.
- [ ] There is a documented way to turn the feature off and to reset what it has learned.

---

## 3. Design for graceful failure and a manual path

**Every AI path has a non-AI path beside it, and failure states are designed, not defaulted.**

This is where most AI interfaces are thinnest, because failure is designed last if at all. Both sources treat it as foundational. The Guidebook's Mental Models chapter says to make sure there is a default experience that does not rely on AI, with the model answer being an offer (*we couldn't map your run, would you like to map it manually?*) instead of a dead end. Its Feedback + Control chapter calls the manual method a safe and useful fallback. Apple, from the product side: ensure a great experience even when generative features aren't available or people opt not to use them, and offer a non-AI fallback where possible.

### Failure is more than one thing

The Guidebook's taxonomy is worth adopting wholesale, because each type needs a different response:

| Type | What happened | What the interface owes |
|---|---|---|
| **Failstate** | The system cannot produce an answer, often correctly, because none exists | Say what the limit is, specifically; offer an alternative route |
| **Context error** | The output is technically right but wrong for this person or moment | Explain the assumption that was made; let them correct it |
| **Background error** | The system is wrong and neither it nor the person notices | Not solvable in the UI; needs monitoring and feedback channels |
| **Happy accident** | Flagged as an error, actually useful | Don't suppress it; learn from the acceptance |

Context errors are the AI-specific addition to the classic user-error / system-error split, and the one teams most often misdiagnose as a model problem when it is an explanation problem.

### Do

- **Write the specific limit, not the generic apology.** "This document is longer than I can read at once, so try a section" beats "Something went wrong." Apple's guidance on limitations says to explain *why* inferior results occur, so the person can adjust.
- **Offer the next move, always.** The Guidebook's restaurant-waiter framing: no Coke, but is Pepsi all right? Never an error without a route out.
- **Make the hand-back to manual easy and legible.** The person needs to know the situation, what to do next, and how. Dropping someone into manual control without orientation is its own failure.
- **Make failure boring.** Quiet, safe, and unremarkable: a normal part of the product rather than a rupture in it.
- **Design the empty, loading, partial, and offline states as first-class.** Generative latency is real; Apple suggests specific progress language, "Finding substitutions for ingredients" rather than "Processing…", because it reduces uncertainty and makes waiting purposeful.
- **Open a feedback channel at the point of failure**, and ask what the person expected to happen.

### Don't

- Don't ship a feature whose only failure state is a spinner that stops.
- Don't expose the stack trace, the raw model error, or the words "an error occurred" as the whole message.
- Don't blame the person for a context error; the assumption was the system's.
- Don't over-explain the mechanism of failure where that helps someone game it. That is the Guidebook's spam-filter caution.
- Don't let the manual path rot. If it is untested, it is not a fallback.

### Patterns this implies

Failstate message with specific limit · Alternative offer · Manual fallback entry point · AI-to-manual handover · Specific progress feedback · Partial result with gap marked · Offline / unavailable state · In-failure feedback prompt

### Acceptance criteria

- [ ] Every AI feature has a documented non-AI path, and that path is exercised in testing.
- [ ] Failstate, context error, empty, loading, partial, and offline states are all specified, not inherited from a default.
- [ ] No terminal error message: every one offers an action.
- [ ] Error copy names the limit or the assumption in plain language.
- [ ] Handover to manual states where the person is and what to do next.

---

## 4. Provide provenance without overload

**Show where this came from, at the grain that supports checking, and no finer.**

Provenance is how someone verifies a claim instead of trusting a vibe. But it competes for the same attention as the content, and an interface dense with citation chrome gets read as noise, then ignored, which is worse than absent.

The Guidebook's framing is *articulate data sources*, structured as scope (what data, for what purpose), reach (this person's data, or everyone's), and removal (can they take it out). Its sharpest instruction is that the best explanation is likely a partial one: leave out what is unknown, too complex, or not useful, and use progressive disclosure for the rest. Apple's complementary pattern is *attribution*: the basis for a result without an account of how the model works. "Because you've read mysteries", not a model card.

### Do

- **Default to one line, expandable.** A compact attribution inline; full sources, timestamps, and retrieval details one interaction away.
- **Attach sources to the claims they support.** A footnote list under a synthesised paragraph makes the reader do the matching. Span-level or sentence-level links make checking a single click.
- **Distinguish grounded from generated.** Retrieved-and-quoted, retrieved-and-paraphrased, and model-generated are three different epistemic states, and the interface should let someone tell them apart.
- **Say when the data is from.** Staleness is the most common and least visible provenance failure. The Guidebook's example of flight-catching travellers needing to know traffic refreshes hourly is about situational stakes: the same staleness matters differently depending on what the person is about to do.
- **Keep attributions factual.** Apple: "Because you've read nonfiction", not "Because you love nonfiction". No implied judgement of someone's emotions, preferences, or beliefs.
- **Pitch the grain in the middle.** Over-general attribution tells the person nothing; over-specific attribution reads as surveillance.
- **Give complete explanations where stakes or law require it**, in high-consequence decisions and regulated domains, even at the cost of density.
- **Cover scope, reach, and removal** anywhere personal data feeds the output.

### Don't

- Don't cite a source the system did not actually read.
- Don't use volume as a proxy for rigour. Eleven citations on one sentence is a smell, not a strength.
- Don't put technical or statistical jargon in an attribution unless the result is itself statistical. Apple's exception covers weather, sports, polling, and scientific data.
- Don't make provenance a modal the person must dismiss to continue.
- Don't let a source link imply the source agrees with the claim. Quote or mark the supporting span.

### Patterns this implies

Inline attribution · Expandable source list · Span-level citation · Grounded vs. generated marker · Freshness / as-of indicator · Scope / reach / removal disclosure · Counterfactual explanation · Example-based explanation

### Acceptance criteria

- [ ] The default view carries at most one line of provenance per output; depth is behind progressive disclosure.
- [ ] Sources resolve to the specific claim, not only the whole response.
- [ ] Every source link reaches a real, reachable location that contains the cited content.
- [ ] Time-sensitive output shows its as-of time.
- [ ] Where personal data informs the output, scope, reach, and removal are all answerable from the UI.

---

## 5. Build for accessibility

**AI output is content. It inherits every accessibility obligation content has, and adds disclosure to the list.**

AI interfaces fail accessibility in a particular way: their most important information is non-textual. Confidence shown by colour, "generated" shown by a shimmer, uncertainty shown by opacity, streaming text that reorders mid-render. None of this survives a screen reader, and some of it does not survive a person's own eyes.

Neither source provides a full accessibility specification. The Guidebook mentions it only in passing, and Apple's generative AI page defers to its [Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility) and [Inclusion](https://developer.apple.com/design/human-interface-guidelines/inclusion) guidance. But the Guidebook's Mental Models chapter is unambiguous on one point: the disclosure that a product is not human must be accessible to everyone regardless of age, technical literacy, education level, or physical ability. Disclosure that only some people can perceive is not disclosure. WCAG 2.2 AA is the floor; the items below are the AI-specific additions on top of it.

### Do

- **Label AI-generated content in text**, available to assistive technology, not by icon, colour, or texture alone.
- **Announce state changes once, deliberately.** Streaming output needs a considered live-region strategy: announce start and completion, not every token. Unmanaged `aria-live` on a token stream is unusable.
- **Make uncertainty non-visually perceivable.** The band's name in the accessible name or an adjacent text node, not encoded in a hue.
- **Meet contrast on the signals too.** Uncertainty markers, citation chips, and "AI" badges are information, and are routinely the lowest-contrast elements on the screen. 4.5:1 for text, 3:1 for the graphical parts that carry meaning.
- **Make every control keyboard-reachable**, including inline accept/reject/regenerate affordances and citation popovers, with visible focus and a trap-free path out of streaming regions.
- **Keep focus stable while content arrives.** Output that appends must not move the person's focus or reflow what they are reading.
- **Give loading states accessible text** that matches the specific progress language from Principle 3.
- **Write disclosure in plain language.** Literacy and technical familiarity vary; "written by AI" travels further than "LLM-generated".
- **Respect reduced-motion** for shimmer, typing, and progress animation.
- **Test with the actual tools**: VoiceOver, NVDA, keyboard only, 200% zoom, forced colours. Not with a contrast plugin alone.

### Don't

- Don't signal "AI" with a sparkle icon and nothing else.
- Don't encode confidence in colour alone, and don't use red/green as the only distinction.
- Don't let a citation be reachable only on hover.
- Don't use low-opacity grey for generated text as the marker of its status.
- Don't put the disclosure where it can be missed: beneath the fold, in a tooltip, or in a dismissed onboarding screen.

### Patterns this implies

Text AI label · Accessible uncertainty name · Managed live region for streaming · Keyboard-reachable inline actions · Focus-stable append · Accessible progress text · Reduced-motion variant · High-contrast signal set

### Acceptance criteria

- [ ] The AI label is in the accessible name or adjacent text, and is not conveyed by colour, icon, or opacity alone.
- [ ] Uncertainty state is announced by a screen reader.
- [ ] Streaming output has a documented live-region strategy; token-by-token announcement is not it.
- [ ] All signal elements meet 4.5:1 (text) or 3:1 (meaningful graphics).
- [ ] Full keyboard operation, including citations and inline actions, with visible focus.
- [ ] Verified on at least one screen reader, keyboard-only, at 200% zoom, and with reduced motion.

---

## 6. Avoid generic AI slop

**Design the interface. Don't assemble the defaults.**

"Slop" is the house style AI features drift into when nobody decides anything: a shimmer border, a sparkle icon, a purple gradient, every output rendered at the same weight regardless of reliability, and a confident paragraph where a caveat belonged. It is not a taste problem. It is a trust problem wearing a visual costume, because an interface that presents everything as equally certain has given the person no way to allocate attention.

Both sources attack it from the same angle. Apple says not to frame the feature as magic and to set clear expectations about what it can and can't do. The Guidebook says to avoid "AI magic" messaging, explain the benefit rather than the technology, and be up front about limits, and warns that anthropomorphic framing creates unrealistic expectations and unintended deception. First-person voice, it notes, implies near-perfect understanding the system does not have.

### The tells

| Tell | What it actually breaks |
|---|---|
| Every output at identical visual weight | No hierarchy, so no basis for allocating scrutiny |
| Sparkle icon as the sole AI signal | Fails disclosure and accessibility at once (Principle 5) |
| Only the happy path designed | No failure, empty, partial, or offline state (Principle 3) |
| Confident prose regardless of certainty | Overclaiming; the hedge is missing where it was needed (Principle 1) |
| "I think…", "I've gone ahead and…" | Implies understanding and agency the system lacks |
| Shimmer as the whole loading story | No specific progress, no sense of what is happening (Principle 3) |
| Explaining the model instead of the benefit | Technology framing, not user framing |
| Decorative percentage | Precision theatre, uncalibrated (Principle 1) |
| Flattery and apology as filler | Occupies the space a concrete next step should hold |

### Do

- **Give reliability a visual register.** Grounded, uncertain, and unverified output should not look identical. Hierarchy is the mechanism by which uncertainty becomes actionable.
- **Design the full state set** before the happy path is called done.
- **Write the limits into the first encounter.** The Guidebook offers a usable template: this is *{product}*, it helps you by *{benefit}*, right now it can't *{limit}*, and you can improve it by *{action}*.
- **Match humanisation to actual capability.** The Guidebook's test: humanise only as far as the system's capabilities match what a person could do.
- **Say the plain thing.** "I couldn't find that" is better than an apology with no content.
- **Make the AI signal yours.** A disclosure that is part of the product's own visual language reads as deliberate; a borrowed sparkle reads as a sticker.

### Don't

- Don't call it magic, in copy or in motion.
- Don't use first-person voice to imply comprehension, intent, or feeling.
- Don't pad with "Great question!" or "I'd be happy to help."
- Don't let gradient, glow, and shimmer stand in for a decision about hierarchy.
- Don't assert a fact the system cannot support, in the register of one that it can.

### Patterns this implies

Reliability-differentiated hierarchy · Capability-and-limits onboarding · Own-brand AI signal · Plain-language system voice · Complete state set · Calibrated register

### Acceptance criteria

- [ ] Output of different reliability is visually distinguishable without reading it.
- [ ] Empty, loading, partial, failure, and offline states all exist and are designed.
- [ ] First-run copy states at least one concrete limitation.
- [ ] No "magic" framing; no first-person claim of understanding or feeling.
- [ ] The AI signal belongs to this product's design language.
- [ ] Every surface passes the tells table above.

---

## Reviewer's short list

Thirteen questions. A pattern or screen that cannot answer them is not ready.

1. Can the person tell how much to trust this?
2. Is that signal attached to the claim, or only to the whole response?
3. Is the confidence mapping documented and verified?
4. Can the output be edited in place?
5. Can every action be undone, at the scope it was taken?
6. Does anything outward-facing or irreversible pass through an approval step showing its scope?
7. Does a correction visibly land, and persist?
8. Is there a non-AI path, and has it been tested?
9. Does every failure state name the limit and offer a next move?
10. Can the person check where this came from in one step, without being buried?
11. Is time-sensitive output stamped with its as-of time?
12. Does a screen reader convey the AI label and the uncertainty state?
13. Would someone who has never seen this product know what it can't do?

---

## Scope and applicability

**Primary deliverables are screen-based.** The patterns here are visual patterns and components for screens: web, mobile, tablet, and embedded displays. That is where they are specified closely enough to build from, and where the acceptance criteria can be checked directly against a running interface.

**The principles transfer; the patterns need translation.** Uncertainty signaling, control and reversibility, and graceful failure with a manual fallback are not properties of screens. They apply wherever a system acts on someone's behalf under uncertainty: robotics, voice interfaces, AR and VR, automotive, industrial controls. What changes is the channel, not the obligation. A confidence band with no visual surface becomes a matter of wording and prosody. A manual fallback in a vehicle or on a factory floor is a physical handover, and the Guidebook's warning that returning control can itself be risky carries far more weight there than it does in a text field.

**This is not an HMI or robotics toolkit.** It does not cover safety certification, control theory, latency budgets for physical actuation, or the regulatory work those domains require. What it offers is the shared vocabulary and the screen patterns, specified closely enough to be adapted rather than guessed at.

---

## Sources

**Apple, Human Interface Guidelines**
[Generative AI](https://developer.apple.com/design/human-interface-guidelines/generative-ai) · [Machine learning](https://developer.apple.com/design/human-interface-guidelines/machine-learning) · [Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility) · [Inclusion](https://developer.apple.com/design/human-interface-guidelines/inclusion)

Drawn on for: responsible design and keeping people in control; transparency and disclosure of AI use; hallucination awareness; confirmation before irreversible or problematic actions; refine and revert affordances; the *confidence*, *attribution*, *limitations*, *corrections*, and *multiple options* patterns; specific progress feedback; non-AI fallback. Both pages were last revised 8 June 2026.

**Google, People + AI Guidebook** (PAIR)
[Guidebook](https://pair.withgoogle.com/guidebook/) · [User Needs + Defining Success](https://pair.withgoogle.com/chapter/user-needs/) · [Mental Models](https://pair.withgoogle.com/chapter/mental-models/) · [Explainability + Trust](https://pair.withgoogle.com/chapter/explainability-trust/) · [Feedback + Control](https://pair.withgoogle.com/chapter/feedback-controls/) · [Errors + Graceful Failure](https://pair.withgoogle.com/chapter/errors-failing/) · [Data Collection + Evaluation](https://pair.withgoogle.com/chapter/data-collection/)

Drawn on for: trust calibration; the four confidence display types (categorical, n-best alternatives, numeric, data visualisation) and the caution against numeric and over-granular confidence; articulating data sources as scope / reach / removal; partial explanations and progressive disclosure; implicit versus explicit feedback and opt-out; control versus automation and human-in-the-loop; preview / test / edit / undo; the error taxonomy (failstate, context error, background error, happy accident); returning control to the person; the capability-and-limits onboarding template; cautions on anthropomorphism and "AI magic".

The People + AI Guidebook is published under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). This document paraphrases its guidance and quotes briefly with attribution; anyone redistributing substantial portions of it should check those terms.

**Also relevant, not yet incorporated:** [WCAG 2.2](https://www.w3.org/TR/WCAG22/) (the accessibility floor referenced in Principle 5), [Microsoft HAX Toolkit](https://www.microsoft.com/en-us/haxtoolkit/), [Google Material Design: AI patterns](https://m3.material.io/).

---

## Changing this document

Principles change slowly; patterns change often. Amending a principle takes a pull request that says what designs the change makes acceptable or unacceptable that weren't before. Adding or revising a pattern does not require touching this file, only declaring which principles the pattern serves.

Acceptance criteria are the part that earns its keep. If a criterion can't be checked, rewrite it until it can, or drop it.
