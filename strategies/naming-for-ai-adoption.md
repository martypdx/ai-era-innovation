# Naming for AI Adoption

API surface naming has a new dimension: AI training bias.

## The Tension

When designing APIs, there's a natural pull toward familiar terminology:
- Developers recognize concepts faster
- Lower learning curve
- "It's like X but for Y"

**Example:** Azoth's `use()` function handles async data → DOM. The name echoes React's `use`, `useState`, `useEffect` family—related conceptual territory.

## The Problem

AI conflates similar names. When you write `use(...)` in Azoth, the AI's training on React's `use*` patterns activates. It starts inferring React semantics even though Azoth's `use()` works differently.

Similar names → AI assumes similar behavior → drift toward dominant pattern.

## The Insight

**For AI adoption, intentionally divergent naming may be better than conceptually adjacent naming.**

Instead of:
- `use()` (too close to React's `use*`)

Consider:
- `channel()` — fresh name, no React baggage
- `flow()` — suggests data movement without state connotation
- `stream()` — evokes async without hooks

The name creates cognitive separation for both AI and humans.

## Broader Principle

This may be a component of **universal design** for the AI era:

> Fresh terminology helps both AI and developers make clean paradigm shifts. Similar names remind both of old patterns and invite conflation.

When a concept is genuinely different, a different name signals that difference. The name itself becomes documentation.

## Open Question

Does this only apply to AI, or is it also better for human developers?

Hypothesis: Developers switching paradigms may actually benefit from fresh vocabulary. Similar names might create false familiarity that slows true understanding.

Worth testing as Azoth's API surface evolves.

---

## Action Item (Azoth)

Review `use()` and related API names. Consider renaming to create clearer separation from React's hook vocabulary.

Candidates:
- `channel()` — matches existing "channel pattern" terminology
- Others TBD based on conceptual fit

---

*Insight emerged from AI-assisted development of WRE reporting system*
