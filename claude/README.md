# Using this library with Claude

[`ai-interface-design/SKILL.md`](ai-interface-design/SKILL.md) turns the principles in this repo into working instructions for an AI assistant. It is one file, usable three ways.

It is a rule set rather than a method. Two groups of hard rules, one covering AI interaction (qualitative bands instead of percentages, triple encoding, cannot-answer as a designed terminal state, user control, provenance, a real manual path) and one covering visual craft and UX (restrained palette, type and spacing, 4.5:1 contrast, 44px touch targets, no decorative AI styling). Below those, the four patterns to reach for, and an output rule telling it to apply all of this without being asked.

---

## As a Claude Project instruction

Best when most of your work is AI interface design and you want it applied to every message without asking.

1. Create a Project at [claude.ai/projects](https://claude.ai/projects).
2. Open **Set custom instructions**.
3. Paste everything in `SKILL.md` **below the closing `---` of the frontmatter**. The `name` and `description` block at the top is for skill loading and does nothing here.
4. Optionally add [`principles.md`](../principles.md) and the files in [`patterns/`](../patterns/) to the Project knowledge, so it can cite your own patterns rather than restating the general case.

## As an Agent Skill

Best when you want it to load only on design work and stay out of the way otherwise. Works in Claude Code and on claude.ai where skills are available.

Claude Code, user level:

```bash
cp -r claude/ai-interface-design ~/.claude/skills/
```

Or project level, so it travels with a repo:

```bash
cp -r claude/ai-interface-design /path/to/your-project/.claude/skills/
```

The `description` in the frontmatter is what decides when it loads, so keep it specific if you edit it. Invoke directly with `/ai-interface-design`.

## As a one-off prompt

Paste the file into a conversation ahead of the request. Fine for a single component, and it costs you the consistency the other two give.

---

## What good output looks like

A request like "design the state where our assistant cannot answer a policy question" should come back with a cannot-answer state that carries a next-step control rather than an apology, a confidence treatment readable without colour, a manual path that is not a retry button, and no percentage anywhere on the screen.

If it comes back with "72% confident" set on a violet gradient, the instructions are not loaded.

---

## Tuning it

- **Name your palette.** "Restrained palette" and "accent colour only for meaning" are the two rules most likely to be interpreted loosely. Replace them with your actual neutrals and your one accent, and the guesswork stops.
- **Keep the visual reject list current.** The line banning gradients, sparkles and decorative AI language is the part that ages fastest, because the visual tells of generic AI output move every few months.
- **Point it at your patterns.** Once [`patterns/`](../patterns/) covers more ground, add a line telling it to check there before designing from scratch.
- **Say what a consequential action is in your product.** The control rule turns on that phrase, and sending, paying, deleting and publishing are not the same bar in every product.

A note on scope: these instructions assume screen-based interfaces. The requirements transfer to voice, AR and VR, automotive and industrial contexts, but the touch target, contrast and layout rules all assume a screen. See [Scope and applicability](../principles.md#scope-and-applicability).
