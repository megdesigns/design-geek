# Using this library with Claude

[`ai-interface-design/SKILL.md`](ai-interface-design/SKILL.md) turns the principles in this repo into working instructions for an AI assistant. It is one file, usable three ways.

It encodes the four requirements (uncertainty signaling, control and reversibility, graceful failure with a manual path, provenance), a required state inventory, an explicit reject list, the grounding in Apple HIG and the People + AI Guidebook, and a fixed output format covering component, states, accessibility, rationale and Figma steps.

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

A request like "design the state where our assistant cannot answer a policy question" should come back with a named component, the full state inventory with non-applicable states justified, real copy rather than placeholders, an announcement and focus strategy, two or three cited decisions, and Figma steps naming variant properties and tokens.

If it comes back with a happy path, a spinner, and "Error: something went wrong", the instructions are not loaded.

---

## Tuning it

- **Add your own tokens.** Replace the generic token roles in the Figma section with your real variable names and it will stop inventing them.
- **Keep the reject list current.** It is the part that ages fastest, because the visual tells of generic AI output move every few months.
- **Point it at your patterns.** Once [`patterns/`](../patterns/) covers more ground, add a line telling it to check there before designing from scratch.

A note on scope: these instructions assume screen-based interfaces. The requirements transfer to voice, AR and VR, automotive and industrial contexts, but the state inventory and the Figma section both assume a screen. See [Scope and applicability](../principles.md#scope-and-applicability).
