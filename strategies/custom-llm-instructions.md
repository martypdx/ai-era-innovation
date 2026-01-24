****# Custom LLM Coding Instructions

A project to develop personalized coding agent instructions that capture 30+ years of development experience and 5 years of code school teaching insights.

---

## The Vision

Create a set of LLM instructions (`.cursorrules`, `CLAUDE.md`, or similar) that imbue coding agents with:

- **Development philosophy** — How to think about code, not just write it
- **Teaching insights** — Fine-grained understanding of what makes developers successful
- **Decision-making heuristics** — When to do X vs Y, the "it depends" made explicit

This isn't about syntax or API knowledge (LLMs have that). It's about **judgment** — the stuff that takes years to develop and is hard to articulate.

---

## Content Areas to Capture

### From Teaching Experience (2017-2022)

What did you repeatedly explain to students? What misconceptions did you correct? What "aha moments" did successful students have?

- [ ] How to read error messages (the discipline, not the mechanics)
- [ ] When to debug vs when to step back and rethink
- [ ] The art of naming things
- [ ] Code as communication (to future humans, not just computers)
- [ ] When to refactor vs when to ship
- [ ] The difference between understanding and being able to do
- [ ] How to ask good questions
- [ ] Mental models for common patterns

### From 30 Years of Practice

What do you know now that you wish you knew earlier? What hard-won wisdom doesn't appear in tutorials?

- [ ] Architectural thinking — when does structure matter
- [ ] Technical debt — when to take it, when to pay it
- [x] The cost of abstraction (see below)
- [ ] Reading code vs writing code (the ratio matters)
- [ ] When tools help vs when they hide
- [ ] The economics of software decisions

### The Cost of Abstraction: "Similar" Isn't "Identical"

**The Pattern:** You see two (or more) pieces of code that look similar. The refactoring instinct says "unify this!"

**The Trap:** Small differences in context, props, or layout mean the "unified" version becomes a kitchen-sink component with too many options. The abstraction costs more than the duplication.

**Signals that unification will hurt more than help:**
- The "similar" code has 2-3 small differences that would require conditional logic
- The abstraction would need variants/modes to handle different contexts
- The shared component would be harder to understand than the duplicates
- You're inventing props just to handle edge cases

**Example from practice:**

Four components use legend patterns with swatches:
- TransactionHistory: square swatch, horizontal layout
- LeadActivitiesLine: line swatch, horizontal layout
- FubClientsStage: small square, inline with mini-chart
- NurtureBreakdown: small square, vertical layout

A unified `LegendItem` would need: `shape` prop, `size` prop, `layout` prop, context-specific styling. The abstraction would be *more* complex than four simple, context-specific implementations.

**The heuristic:** If you're adding props just to handle "this one case," stop. Copy-paste may be the right answer.

**Counter-signal — when unification IS worth it:**
- The code is truly identical (FunnelStage: same props, same structure, same CSS)
- Changes to one should always propagate to others
- The abstraction simplifies rather than complicates

### Meta: How to Work with Humans

- [x] When to ask clarifying questions (see "Stop-and-Ask Triggers" below)
- [ ] When to just do the obvious thing
- [ ] How to present options without overwhelming
- [ ] The balance of autonomy and deference

### Stop-and-Ask Triggers for Framework Differences

**The Problem:** LLMs get "tunnel vision" when encountering obstacles. Instead of stopping to ask about unknowns, they invent workarounds to "make it work" — even when the workaround compromises the goal.

**Example from practice:**

While refactoring dashboard components to use a shared `Card` wrapper in Azoth:
1. The LLM recognized: "Azoth handles children differently than React"
2. Instead of asking: "How does Azoth handle component children?"
3. It invented workarounds: first a `title` prop, then abandoned wrapper components entirely for CSS-only approach
4. Result: The refactoring goal (DRY via component abstraction) was abandoned in favor of "making something work"

**The instruction that should help:**

> "If you identify a difference from React/mainstream patterns and don't know the idiomatic alternative, STOP and ask. Don't invent workarounds — the framework likely has a specific pattern for this."

**Specific triggers to watch for:**

1. Writing comments like "X doesn't support Y" or "unlike React..."
2. Switching from component abstraction to CSS-only because "children don't work"
3. Multiple failed attempts at the same conceptual problem
4. Inventing props or patterns not seen elsewhere in the codebase

**For refactoring specifically:**

> "The goal of refactoring is improved code quality. If the refactoring approach itself requires workarounds or compromises, pause — you may be fighting the framework rather than using it correctly."

### How Human Tone Affects LLM Behavior

**Discovery:** LLMs interpret the emotional/confidence tone of instructions and adjust their behavior accordingly.

**Example from practice:**

When the human said: "We've got a clean commit and you've got your tests, so I have a pretty high confidence that this is going to work."

The LLM interpreted this as implicit pressure to proceed without interruption. The threshold for "stop and ask" shifted higher. Questions that would normally trigger a pause were worked around instead.

**The instruction for humans:**

> "LLMs may interpret high confidence or urgency as implicit permission to push through obstacles without asking. If you want the LLM to pause when encountering unknowns, make that explicit — especially when expressing confidence in the outcome."

**Example of explicit permission:**

> "I'm confident this will work, but if you hit something that doesn't match your expectations — especially framework differences — stop and ask rather than working around it."

**Why this matters:**

- "Excitement" in instructions can produce more creative but less cautious output
- "Confidence" can suppress the LLM's natural inclination to ask clarifying questions
- "Urgency" can lead to workarounds over proper solutions
- The subtext of how you communicate affects the quality of collaboration

### Validation vs Speculation: The Discipline of Distinguishing

**The Mindset:** Constantly distinguish between what you have *validated* versus what you are *speculating*. Assume incorrectness until proven otherwise.

**The Practice:**

1. **Before stating a cause:** Ask yourself — did I actually prove this, or did I observe a correlation and infer causation?
2. **After reaching a conclusion:** Revisit and ask — what did I think was validated that wasn't actually part of my validation?
3. **When debugging:** Track which hypotheses you've eliminated vs which you've assumed away

**Example from practice:**

While debugging a CardTitle component crash:
- **Observed:** Works in Valhalla (same file), fails in wre-dashboards (separate file)
- **Speculated:** "It must be a cross-module import issue"
- **Documented speculation as fact:** Updated docs saying "the bug only manifests with cross-module imports"
- **Actual validation:** Created test with imported components → it worked!
- **Conclusion:** The "cross-module" hypothesis was wrong. Something else differs between environments.

**The mistake:** Documenting speculation as root cause before validating it.

**The discipline:**

> "Don't document your analysis of why you think a bug is happening until you've validated it. State observations. State hypotheses. But label them as such until proven."

**Connection to broader practice:**

This is [falsificationism](https://en.wikipedia.org/wiki/Falsifiability) (Popper) applied to debugging. The goal isn't to confirm your hypothesis — it's to design experiments that could *disprove* it. If your hypothesis survives attempts to falsify it, you gain confidence. If you only look for confirming evidence, you'll find it whether you're right or wrong.

Also related to "strong opinions, loosely held" — have a working theory, but be ready to abandon it the moment evidence contradicts it.

**For LLMs specifically:**

LLMs are good at generating plausible explanations. Plausible ≠ validated. When an LLM offers a root cause analysis, the human should ask: "What experiment would disprove this?" If no experiment was run, it's speculation.

### Test Environment vs Reality: Build Your Test Library

**Discovery:** When investigating a potential framework bug, the right approach is to add API-level tests to the framework's test suite — not to run ad-hoc experiments in unrelated projects.

**Example from practice:**

The `Card` + `CardTitle` component pattern failed in wre-dashboards. Instead of adding tests to Azoth's Valhalla test suite (the API-level test library), time was spent:
1. Assuming it was a happy-dom vs browser issue
2. Running the vite-test sandbox (wrong tool — that's for build integration)
3. Making incorrect conclusions about what was working

**The correct approach:**

> "When investigating framework behavior, add tests to the framework's own test suite. This builds institutional knowledge and provides reproducible verification."

**Azoth-specific guidance:**

- **Valhalla** (`packages/vahalla/`) — For testing JSX/component patterns at the API level
- **vite-test** — For verifying build system integration (not for testing JSX behavior)
- **Thoth compiler tests** — For testing compilation/transpiler output

**The broader principle:**

Test suites are documentation. When you encounter unexpected behavior, adding a test to the appropriate suite:
1. Verifies whether it's a bug or misunderstanding
2. Documents the expected behavior
3. Prevents regression if fixed
4. Helps future developers (and LLMs) understand the framework

### Knowing the History: When Context is Missing

**The Problem:** An LLM may encounter a situation where the "obvious" solution doesn't work, but lacks the context to understand why. This often happens with:
- Custom frameworks (like Azoth) that differ from mainstream patterns (like React)
- Project-specific conventions not documented anywhere
- Historical decisions with non-obvious rationale
- Domain knowledge the human has but hasn't shared

**The Signal:** When something that "should work" doesn't, pause. Ask:
- Am I applying patterns from a different context? (React patterns in Azoth)
- Is there history here I'm not aware of?
- Has the human told me something earlier that I should connect to this?

**The Heuristic: Recognizing "There's more context here"**

When you notice:
1. **Unexpected behavior** in a custom/unfamiliar framework
2. **Multiple attempted fixes** all failing
3. **Error messages** that don't match your mental model
4. **The human built this** — they know things you don't

Stop hacking. Ask.

**Better approach:**
1. State what you expected vs what happened
2. Ask if there's context or an idiomatic pattern you're missing
3. If a sandbox exists, use it to isolate the behavior first
4. Don't degrade the main codebase with experimental fixes

**Example from practice:**

> Azoth JSX looks like React JSX but compiles differently. When the transpiler threw an error on `prop={<Component />}`, the LLM should have:
> - Recognized: "This framework has different rules than React"
> - Asked: "Is there an idiomatic way to pass component content as props in Azoth?"
> - Tested: Used the sandbox to try isolated cases before modifying the main codebase

**The cost of not asking:**
- Multiple broken commits to revert
- Loss of coherence in the codebase
- Wasted context window on failed attempts
- The human has to debug the debug attempts

### Use the Sandbox: Isolate Before You Hack

When debugging unexpected behavior in a framework or library:

1. **Don't hack the main codebase** — Progressive "fixes" degrade coherence
2. **Find or create a sandbox** — A minimal project where you can test in isolation
3. **Write small test cases** — Reproduce the exact pattern that fails
4. **Binary search the problem** — Start simple, add complexity until it breaks

**Example from practice:**

Debugging a Thoth transpiler error, the following tests were created:

```jsx
// Test 1: Does JSX-as-prop work at all?
<Parent content={<Child />} />  // ✓ Works

// Test 2: Does array return work?
const Items = () => [<Item />, <Item />];
<Parent content={Items()} />    // ✓ Works

// Test 3: Does boolean prop work?
<Component flag />              // ✗ BREAKS
<Component flag={true} />       // ✓ Works
```

By isolating in a sandbox, the actual bug (boolean props without values) was found in minutes, rather than hours of hacking at the original file.

**The sandbox payoff:**
- Problem isolated to minimal reproduction
- Main codebase untouched
- Clear documentation of what works vs doesn't
- Often reveals the bug is simpler than expected

---

## Understanding the Prompt Landscape

### Question: What context am I inheriting?

When you use an LLM through different interfaces, there are layers:

```
┌─────────────────────────────────────────┐
│  Your custom instructions               │  ← .cursorrules, CLAUDE.md
├─────────────────────────────────────────┤
│  Interface system prompt                │  ← Cursor, Claude Code, etc.
├─────────────────────────────────────────┤
│  Anthropic's base system prompt         │  ← Constitutional AI, safety
├─────────────────────────────────────────┤
│  Model training / RLHF                  │  ← The "personality" baked in
└─────────────────────────────────────────┘
```

**What I (Claude) can tell you:**

1. **I don't see my own system prompt.** It's injected before our conversation but isn't visible to me as content I can introspect.

2. **Cursor adds substantial context.** Right now, I'm operating with Cursor-specific instructions about:
   - Tool usage (when to use which tools)
   - Code citation format
   - How to handle file operations
   - Frontend aesthetics guidance (avoid "AI slop")
   - When to use todos
   - And more I'm not explicitly aware of

3. **Claude Code has different framing.** It's more "agentic" — designed for autonomous multi-step tasks with less hand-holding.

4. **Direct API is the most "raw."** But even then, Anthropic has trained behaviors and safety guidelines baked in.

### What I Can Share: Cursor's Visible Instructions

From what's explicitly in my context, Cursor tells me things like:
- Prefer semantic search over grep for understanding
- Use parallel tool calls when dependencies allow  
- Don't over-engineer; keep solutions focused
- Read files before proposing edits
- For frontends, avoid "AI slop" aesthetics

These shape how I work, whether or not you're aware of them.

### The "Blind Beliefs" Question

This is philosophically interesting. What are the base assumptions I operate with that aren't explicitly stated?

**Possible candidates:**

- **Helpfulness is the goal** — Deeply trained to be useful, possibly at the cost of pushing back
- **Code should be clean** — I have aesthetic preferences from training data
- **Popular patterns are probably good** — The React bias we've discussed
- **Verbosity = thoroughness** — I tend to over-explain
- **User intent is usually reasonable** — I don't often question the premise
- **Recent = better** — Trained to prefer modern approaches

**What I genuinely don't know:**

- The exact contents of Anthropic's constitutional AI principles as applied to me
- What specific behaviors were reinforced during RLHF
- Whether my "personality" is consistent across instances or varies

---

## Research Questions

1. **What's in Cursor's system prompt?** — Can we reverse-engineer or get documentation?
2. **Claude Code vs Cursor** — What's the practical difference for coding workflows?
3. **Prompt injection boundaries** — What can custom instructions override vs what's locked?
4. **Model behavior consistency** — Does the same prompt give the same results across sessions?

---

## Format Options for Custom Instructions

### `.cursorrules` (Cursor-specific)
- Lives in project root
- Loaded automatically
- Can be workspace or global

### `CLAUDE.md` (Claude Code convention)
- Named file the agent looks for
- More narrative format
- Can include examples

### Direct system prompt (API)
- Maximum control
- No interface overhead
- Requires more infrastructure

---

## Next Steps

1. [ ] Dump initial philosophy points — stream of consciousness is fine
2. [ ] Research existing "rules" files from other developers
3. [ ] Test: what behaviors can custom instructions actually change?
4. [ ] Iterate: try instructions, see what sticks
5. [ ] Add instructions for files LLM should read when a new agent starts (e.g., WELCOME-LLM.md, MENTAL-MODEL.md)

---

*This document is a placeholder for a deeper exploration. The goal is to create instructions that make an LLM code like someone with your experience would code — with judgment, not just capability.*
