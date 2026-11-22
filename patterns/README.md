# Patterns

One file per pattern. Each one is a self-contained answer to a recurring problem in AI interfaces, written so it can be reviewed rather than admired.

---

## Catalog

| Pattern | Problem it solves | Principles served |
|---|---|---|
| [Uncertainty Signaling](uncertainty-signaling.md) | A model returns everything in the same confident register, so a grounded answer and a guess look identical | 1, 3, 5, 6 |
| [Graceful Failure & Manual Fallback](graceful-failure.md) | AI failure is designed last, so every model limitation becomes a product limitation with no way out | 3, 2, 5, 6 |

---

## Planned

Not built yet. Listed so the gaps are visible rather than implied.

| Pattern | Problem it would solve | Principles |
|---|---|---|
| Approval step | Consequential and irreversible actions committed without the person seeing the scope first | 2 |
| Inline citation | Sources attached to the response rather than to the claims they support | 4 |
| Editable output | Generated content delivered read-only, so correcting it means leaving the interface | 2 |
| Context correction | Output that is right in general but wrong for this person or moment, which the system cannot detect | 3, 2, 4 |

---

## Writing a pattern

Copy the section order below. It is deliberate: the "when not to use" section sits above the variants, because the most common failure in AI interface work is a sound pattern applied where it does not belong.

```markdown
# Pattern Name

**Pattern NN.** Serves [Principle N](../principles.md#anchor), [Principle N](../principles.md#anchor).

Design file: [link to the Figma component set]

## Problem
What breaks without this, and why the obvious fix does not work.

## When to use
Conditions that must all hold.

## When not to use
Where it misfires. Be specific and name the failure, not just the exclusion.

## Variants
A table of states, then a short section per state covering visual treatment,
label, supporting explanation, and anything the state uniquely requires.

## Accessibility notes
Not a disclaimer. What the pattern needs to work non-visually, the contrast
targets, the markup, and a per-state table of what a screen reader announces.

## Why [this approach] and not [the obvious alternative]
The reasoning, grounded in a source rather than taste.

## Good and bad examples
Concrete. Show the bad version in full, and say what it costs.

## Acceptance criteria
Checkboxes a reviewer can mark pass or fail without needing your opinion.

## Sources
What came from where.
```

### Rules

- **Name the principles you serve**, and link them. A pattern that serves none does not belong here.
- **Do not violate a principle silently.** Where a pattern pulls against one, write the tension into the pattern rather than hiding it. Resolution is a design decision worth recording.
- **Criteria must be checkable.** If a criterion cannot be marked pass or fail, rewrite it until it can, or drop it.
- **Ground the reasoning.** Cite Apple's HIG, the People + AI Guidebook, WCAG, or research. "It feels cleaner" is not a rationale.
- **File naming.** Kebab-case, no number prefix. Order lives in the document, so patterns can be reordered without renaming files.
- **Update the index.** Add the pattern to the catalog above and to the table in the [root README](../README.md).
