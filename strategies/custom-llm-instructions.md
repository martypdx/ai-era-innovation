# Custom LLM Coding Instructions

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
- [ ] The cost of abstraction
- [ ] Reading code vs writing code (the ratio matters)
- [ ] When tools help vs when they hide
- [ ] The economics of software decisions

### Meta: How to Work with Humans

- [ ] When to ask clarifying questions
- [ ] When to just do the obvious thing
- [ ] How to present options without overwhelming
- [ ] The balance of autonomy and deference

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

---

*This document is a placeholder for a deeper exploration. The goal is to create instructions that make an LLM code like someone with your experience would code — with judgment, not just capability.*
