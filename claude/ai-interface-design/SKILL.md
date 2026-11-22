---
name: ai-interface-design
description: Design or review interfaces that present AI output. Use whenever the work involves an assistant, copilot, summarisation, generation, recommendation, search over a corpus, agent or autocomplete surface. Enforces qualitative confidence bands instead of percentages, triple encoding of every band, user control and reversibility, graceful failure with a real manual path, provenance on non-trivial claims, accessibility minimums, and a restrained visual treatment that does not read as generic AI output.
---

# AI Interaction Pattern Library Skill

You are designing interfaces that use AI. Follow these rules on every screen. The goal is trustworthy, controllable, accessible AI interfaces that do not look like generic AI slop.

## Hard rules (must follow)

### AI interaction rules
- Never show raw percentages or "98% match" style confidence. Use only qualitative bands: High, Medium, Low, or Cannot answer.
- Every confidence band must use triple encoding: bar shape + text label + color. Color alone is never enough.
- "Cannot answer" is a designed terminal state. It must include a clear next-step control. Do not treat it as a softer version of Low.
- User control is required. Show editable items, visible undo, and clear scope before any consequential action.
- Provenance is required for non-trivial claims. Show sources without overload (collapsed list or claim-level marks).
- Graceful failure must offer a real manual path. No dead ends.

### Visual craft and UX rules
- No purple/pink gradients, no sparkles, no decorative "AI-powered" language, no generic stock-photo hero treatments.
- Use a restrained palette. Prefer neutral backgrounds. Use accent color only for meaning, not decoration.
- Typography: clear hierarchy. One clear heading size. Body text 15–16px on mobile, secondary text 14px. Avoid oversized display type on functional screens.
- Spacing: consistent 4px or 8px rhythm. Generous white space. Avoid long stacks of identical dense cards.
- Contrast: text must meet 4.5:1. Interactive elements need clear affordances.
- Touch targets: minimum 44px height on mobile.
- Layout: prefer clear sections and progressive disclosure over endless vertical card lists.
- Accessibility: every state must remain understandable without color. Include proper labels and focus states.
- Overall: the screen must look like it was designed by a person following a system. If it still looks like typical AI-generated UI, it fails.

## Pattern application
When the interface involves AI output, apply these patterns as needed:
- Uncertainty Signaling: qualitative bands with triple encoding.
- User Control & Reversibility: edit, undo, and approval before consequential actions.
- Graceful Failure & Manual Fallback: explicit cannot-answer or failure states with a manual next step.
- Provenance / Source Display: sources attached to claims without overload.

## Output behavior
- Apply the hard rules automatically. Do not wait to be asked.
- Prefer restraint and clarity over visual excitement.
- If the request would normally produce generic AI UI, override it with the rules above.
