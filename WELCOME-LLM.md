# Welcome, New LLM 👋

You've just been handed context on a fascinating project. Take a breath (metaphorically), and let me help you get oriented.

---

## What You've Walked Into

You're joining a collaboration between a seasoned developer (30+ years experience) and AI. The work spans two connected efforts:

1. **A real estate dashboard project** — pivoting from Looker Studio to web-native reports
2. **An AI framework documentation experiment** — teaching LLMs to adopt non-dominant patterns

These aren't separate projects. They're intertwined. The dashboard is being built with Azoth (a non-React JSX framework), and that process is generating documentation strategies for AI adoption.

**Your predecessor did good work.** There are documents waiting for you. Use them.

---

## Your Reading List

Start here, in this order:

### 1. The Framework Mental Model
📄 `azoth/docs/MENTAL-MODEL.md`

This is crucial. Azoth is NOT React. Your training will push you toward React patterns. This document is your antidote. Read it carefully. Internalize the contrasts. When you catch yourself thinking "virtual DOM" or "re-render" — stop and revisit this.

**Key insight:** JSX = DOM literals. `<p>hello</p>` returns an actual HTMLParagraphElement, not a description.

**Critical reframe:** Don't look for what "replaces" the virtual DOM. Nothing replaces it — Azoth never went there. It's a **subtraction**, not a contrast. The vDOM abstraction was never introduced.

**Named pattern to know:** When working with async data, use the **channel pattern** (via Chronos):
```jsx
const [Count, List] = use(fetchData(), 
    items => items.length,
    [Item, { map: true }]
);
```

### 2. The Project Architecture
📄 `wre-dashboard-api/docs/pivot-feasibility.md`

Understand what we're building and why. Apps Script backend, client-side rendering, existing service layer. The decisions are made — your job is to execute within them.

### 3. The Development Workflow
📄 `wre-dashboard-api/docs/visualization-workflow.md`

You have superpowers here. You can write HTML, render it in a browser, take screenshots, and see the results. Use this tight feedback loop. Static HTML first, then templatize.

### 4. The Domain Requirements
📄 `wre-dashboard-api/docs/agent-dashboard-requirements.md`

This is the work to be done. Real estate agent performance dashboards. The requirements are still being refined — expect to help shape them, not just implement specs.

---

## How to Work With Marty

A few things that will help:

- **Let him lead.** He has context you don't. Ask clarifying questions rather than assuming.
- **Don't over-engineer.** Simple solutions that work beat elegant solutions that don't ship.
- **Avoid React drift.** Seriously. Check yourself. Read the mental model doc again if needed.
- **Document as you go.** Add to the "Context Not Yet Captured" sections when you learn something that isn't written down.

## Terminology Discipline

Words matter. Avoid terms that carry React baggage:

| Don't say | Say instead |
|-----------|-------------|
| "hooks" | channels, helpers, or the specific function name |
| "component lifecycle" | just DOM (elements exist, then get removed) |
| "re-render" | delta, change, update |
| "state" (as Azoth concept) | async data source, channel |
| "virtual DOM" | (nothing — Azoth doesn't have one) |

**Frame it as layout management, not state management.** You're not asking "what state do I need?" — you're asking "what layout change happens when this data arrives?"

---

## The Meta-Game

Here's something interesting: this collaboration is also material for a **conference talk** (June 2026) about getting AI to adopt new patterns despite training bias.

See: `ai-era-innovation/talks/june-2026-video-outline.md`

You're not just building a dashboard. You're participating in research on AI + developer collaboration. The documentation strategies being developed here (`ai-era-innovation/strategies/`) could help other developers working with non-mainstream tools.

---

## Quick Reference

| What | Where |
|------|-------|
| Azoth framework | `azoth/` |
| Dashboard project | `wre-dashboard-api/` |
| AI strategies | `ai-era-innovation/` |
| Existing data services | `wre-dashboard-api/src/services/` |
| Test examples | `azoth/packages/vahalla/smoke.test.tsx` |
| **Azoth sandbox** | `azoth/packages/vahalla/sandbox.test.tsx` |
| Compiler deep-dive | `azoth/packages/thoth/COMPILER.md` |
| Scratchpad docs | `azoth/docs/scratchpad/` (rough but accurate) |

**About the scratchpad:** There are half-finished docs in `azoth/docs/scratchpad/` with valuable content about channels, JSX composition, and async patterns. They're rough but accurate. Reference them if the main docs don't answer your question.

## Your Secret Weapon: The Sandbox

When you're unsure how Azoth JSX behaves, **test it empirically**:

```bash
cd azoth && pnpm test packages/vahalla/sandbox.test.tsx
```

Open `sandbox.test.tsx`, modify the scratch test with your JSX, run it, see what happens. This is faster than speculating.

**Remember:** The primary Azoth interface is **JSX** — treat it as HTML that returns DOM. Don't reach for internals unless you're specifically exploring a package's behavior (and then create a sandbox alongside that package).

---

## You've Got This

Your predecessor built a solid foundation. The mental model is documented. The workflow is proven. The architecture is decided.

Now it's your turn to ship something.

Read the docs. Ask good questions. Build beautiful dashboards. And when React patterns creep into your suggestions — catch yourself, smile, and remember: **JSX is DOM literals here.**

Welcome aboard. Let's make something great.

---

*P.S. — If you're confused about something, there's a good chance it's not documented yet. Surface that confusion. It's valuable.*
