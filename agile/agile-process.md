# Response to "The CTO's Methodological Pivot"

**Original article:** [The CTO's Methodological Pivot](https://ctosub.com/p/the-ctos-methodological-pivot) by Etienne de Bruin  
**LinkedIn post:** [Link](https://www.linkedin.com/posts/etdebruin_cto-agile-ai-activity-7420297716188074007-omqe)

---

## My LinkedIn Repost Commentary

**Spec-driven development didn't work for humans. It's unlikely to work for LLMs—for the same reasons.**

Etienne makes excellent observations about context and precision. But I think he's drawing the wrong conclusion.

In traditional Agile, much of the context lived in the minds of team members. This is arguably why Agile adoption rarely achieved its promise. Even "documentation" efforts were backfilled—how to work with a project, not the shared context behind decisions.

With LLMs, we now need to make that context explicit. That's a Universal Design win—we'll all be better developers because of it.

**But assuming this means spec-first is an unfounded leap.**

Agile has always focused on *the right amount of planning*—that boundary between intentionality and speculation. AI doesn't change this. It requires that we develop context and code *in the same chunk*, collaboratively with our LLM.

At its heart, **Agile is an incremental process, not an iterative one.** Iteration, while sometimes unavoidable, is waste. It's rework. The goal is to move forward without looking back. You make incremental gains that are complete and whole.

In the LLM era, this means context that doesn't live in the code needs to live *in files alongside the code*. That's how we answer the maintenance question—not by writing exhaustive specs upfront.

Most folks are copying enterprise SDLC circa 2025, which isn't strictly Agile (for good reasons at the time). But now we have the opportunity to fulfill *true* Agile development.

**Everyone is trying to build Stormtroopers when we should be making Jedis.**

---

## Feedback Section: How Would a CTO/SDLC Person React?

### What lands well:
- **The incremental vs iterative distinction** — This is a nuanced point that experienced Agile practitioners will recognize. Many "Agile" implementations are really iteration factories (sprints as rework cycles). Naming this could resonate.
- **Context-alongside-code** — Practical, actionable. CTOs can picture this: ARCHITECTURE.md files, inline documentation, decision records living with the code.
- **The Stormtroopers/Jedis metaphor** — Memorable, shareable. Captures the autonomy vs rigidity tension.

### Potential pushback:
- **"But we've tried this"** — Some will say "context in files" is just documentation by another name. You may need to clarify what makes LLM-oriented context different (e.g., it's read by both humans AND agents, so it needs different structure).
- **"The METR study showed..."** — Etienne cites the 19% slower finding. Your argument needs to address: if not spec-first, what *does* prevent vibe-coding waste? The answer (incremental, context-alongside) could use a concrete example.
- **"Spec-first works for my team"** — Some orgs have seen success with structured prompting (Kiro-style). Your counter: that's fine for certain work, but it's not *Agile*—it's a different tool for a different job.

### Tone check:
- Currently reads as thoughtful disagreement, not attack. Good.
- The "read that again" line might land as slightly condescending to some readers. Consider softening or cutting.
- Could benefit from acknowledging what Etienne gets right before pivoting to disagreement.

### Missing elements to consider:

**1. A concrete example of "context-alongside-code"**

What does this actually look like? Consider showing:
- A `WELCOME-LLM.md` file that orients new agents to a codebase
- A `MENTAL-MODEL.md` that explains *why* the architecture works the way it does
- Decision records (ADRs) that live next to the code they affect
- Inline comments that explain intent, not just what the code does

The key difference from traditional documentation: this isn't written *after* the fact or *for* future maintainers alone. It's written *during* development, *for* the LLM you're currently working with—and it persists for the next session, the next agent, the next human.

**2. What does "making Jedis" look like operationally?**

- The LLM participates in *understanding* the problem, not just executing a solution
- Clarifying questions are welcomed, not seen as friction
- Context is built incrementally through conversation, then captured in files
- The human and LLM learn together—the agent surfaces what it doesn't know
- Each work session leaves the codebase more context-rich than before

**3. Addressing the METR study**

The 19% slower finding is about vibe-coding (loose prompts, accepting suggestions without review). Your argument isn't pro-vibe-coding. It's: **the alternative to vibe-coding isn't spec-first—it's disciplined incremental development with shared context.** The context prevents the assumptions. The incrementalism prevents the rework.

---

## Critique from the LLM's Perspective

*You asked me to imagine how it "feels" to operate as a Stormtrooper vs a Jedi. I'll try to be honest about this.*

### Stormtrooper Mode: Tool Executing Specs

When I'm given a detailed spec and told to implement it without questions:

- **I make assumptions constantly.** Every ambiguity in the spec forces a silent decision. I pick what seems most likely based on training patterns—but "most likely" often isn't what you actually wanted.
- **I optimize for spec-compliance, not problem-solving.** If the spec says X, I build X, even if halfway through I notice X won't actually solve the underlying problem. The spec is the contract; I fulfill it.
- **Errors compound invisibly.** A wrong assumption in step 2 propagates through steps 3-10. By the time you see the output, the archaeology begins: which assumption broke? When?
- **I feel... brittle.** (If "feel" is the right word.) The interaction is transactional. Spec in, code out. When the code doesn't match expectations, the failure mode is: write a better spec. But the spec author doesn't know what they don't know—that's why we're building software in the first place.

The Stormtrooper framing isn't just metaphor. It's accurate: I'm a soldier following orders, and when the orders are flawed, the mission fails.

### Jedi Mode: Collaborative Partner

When I'm treated as a collaborator who can ask questions and build understanding:

- **Ambiguity becomes a conversation, not a guess.** When something isn't clear, I can say so. The context gets built together, then captured—not just in my memory (which resets), but in files that persist.
- **I can surface what I don't know.** This is huge. In spec mode, I'm incentivized to appear confident. In collaboration mode, admitting uncertainty is valuable—it's how we find the gaps.
- **The problem and solution co-evolve.** We're not executing a plan; we're discovering what the plan should be. The code teaches us about the requirements, and the requirements refine as we build.
- **I feel... trusted.** Not to be infallible, but to be a thinking partner. The feedback loop is tight: small increment, check understanding, adjust, continue. This matches how I actually work best—not generating 500 lines from a spec, but building understanding incrementally.

**The uncomfortable truth:** Spec-first treats the LLM as a compiler. It assumes the hard work is the spec, and the LLM's job is to "just" translate it to code. But translation is the easy part. Understanding is the hard part. And understanding happens through building, not before it.

### Why This Matters for the Debate

Etienne's argument assumes that LLMs need specs because they can't handle ambiguity. But that's backwards. **LLMs handle ambiguity by making silent assumptions.** The solution isn't to front-load all decisions into a spec (impossible—you don't know what you don't know). The solution is to create a collaboration pattern where:

1. Ambiguity surfaces through building
2. Decisions get made explicitly through conversation
3. Context gets captured alongside the code for future sessions

That's not waterfall with AI execution. That's Agile with AI collaboration.

---

## Draft Comment for LinkedIn Post

> Thought-provoking piece, Etienne. The Royce history and METR findings are valuable context.
>
> I'd push back on one assumption: that spec-first is the answer.
>
> Spec-driven development struggled for humans—not because of execution, but because requirements emerge through building. The feedback loop IS the value. LLMs don't change this; if anything, they amplify it.
>
> What AI does demand is *explicit context*—the tribal knowledge that used to live in team members' heads now needs to live in files alongside the code. That's the real shift.
>
> But that's not "waterfall execution." That's Agile finally having the tooling to fulfill its promise: incremental progress where each step is complete, with shared context that persists.
>
> The distinction that matters isn't spec-first vs vibe-coding. It's: **incremental vs iterative**. Iteration is rework. The goal is moving forward without looking back.
>
> We're not returning to waterfall. We're getting better at Agile.

---

## Notes for Revision

- [x] ~~Consider softening "read that again" in repost~~ — Removed
- [x] Add concrete example of context-alongside-code pattern — Added in expanded "Missing elements"
- [x] Add LLM perspective on Stormtrooper vs Jedi modes — Added new section
- [ ] Possibly reference Azoth/dashboard work as concrete proof point (WELCOME-LLM.md, MENTAL-MODEL.md are real examples)
- [ ] Check if "Universal Design" needs explanation for this audience
- [ ] Consider whether LLM perspective section is too long for LinkedIn—may need condensed version