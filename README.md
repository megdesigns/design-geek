# AI Interaction Pattern Library

An open library of interaction patterns for AI interfaces, built around a question most AI UI skips: how does someone decide what to trust, and what do they do when it is wrong?

Most AI interface work is a thin layer over a model's output. This library is the opposite starting point. It begins with the harder problems, which are uncertainty, control, failure and accessibility, and works back to the components.

Two things live here. [**principles.md**](principles.md) is the governing document, six principles with acceptance criteria a reviewer can check. **patterns/** holds the patterns, each one declaring which principles it serves and how.

---

## Start here

| | |
|---|---|
| [**Principles**](principles.md) | The six principles, each with do/don't guidance and falsifiable acceptance criteria |
| [**Patterns**](patterns/) | The catalog, one file per pattern |
| [**Reviewer's short list**](principles.md#reviewers-short-list) | Thirteen questions to run against any screen |
| [**Use it with Claude**](claude/) | The principles as working instructions for an AI assistant |

---

## Index: principles to patterns

Every pattern serves at least one principle. Every principle should eventually have patterns that make it concrete. This table is the map between them, and the gaps in it are the roadmap.

| Principle | Patterns |
|---|---|
| [1. Make uncertainty visible and actionable](principles.md#1-make-uncertainty-visible-and-actionable) | [Uncertainty Signaling](patterns/uncertainty-signaling.md) |
| [2. Preserve control and reversibility](principles.md#2-preserve-control-and-reversibility) | [Graceful Failure & Manual Fallback](patterns/graceful-failure.md) (action hierarchy, cancel) |
| [3. Design for graceful failure and a manual path](principles.md#3-design-for-graceful-failure-and-a-manual-path) | [Graceful Failure & Manual Fallback](patterns/graceful-failure.md) · [Uncertainty Signaling](patterns/uncertainty-signaling.md) (cannot-answer state) |
| [4. Provide provenance without overload](principles.md#4-provide-provenance-without-overload) | None yet |
| [5. Build for accessibility](principles.md#5-build-for-accessibility) | [Graceful Failure & Manual Fallback](patterns/graceful-failure.md) · [Uncertainty Signaling](patterns/uncertainty-signaling.md) |
| [6. Avoid generic AI slop](principles.md#6-avoid-generic-ai-slop) | [Graceful Failure & Manual Fallback](patterns/graceful-failure.md) · [Uncertainty Signaling](patterns/uncertainty-signaling.md) |

Accessibility is deliberately not a pattern of its own. It is a constraint every pattern carries, so each one has its own accessibility section rather than deferring to a shared page nobody reads.

---

## Structure

```
design-geek/
├── README.md             You are here. Overview and the principle-to-pattern index.
├── principles.md         The six principles. Changes slowly.
├── patterns/
│   ├── README.md         Pattern catalog and the contribution shape.
│   └── *.md              One file per pattern.
└── claude/
    ├── README.md         How to load the instructions, three ways.
    └── ai-interface-design/
        └── SKILL.md      The principles as an assistant instruction.
```

---

## How to use it

**Designing something.** Find the pattern, read its "when not to use" section before its variants. Most mistakes in AI UI are a pattern applied where it does not belong, not a pattern built badly.

**Reviewing something.** Run the [reviewer's short list](principles.md#reviewers-short-list), then the acceptance criteria on whichever patterns the screen uses. Both are written as checks rather than opinions, so a review produces a pass or a fail rather than a discussion.

**Contributing a pattern.** See [patterns/README.md](patterns/README.md) for the required structure. The rule is that a pattern names the principles it serves and does not violate any of them. Where a pattern genuinely pulls against a principle, that tension belongs in the write-up rather than hidden in it.

---

## Grounding

The principles are grounded in [Apple's Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/generative-ai) for generative AI and machine learning, and [Google's People + AI Guidebook](https://pair.withgoogle.com/guidebook/). Each document cites what came from where.

The People + AI Guidebook is licensed CC BY-NC-SA 4.0. Material from it is paraphrased with attribution rather than reproduced.
