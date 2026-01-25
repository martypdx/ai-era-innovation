# Jedis, Not Stormtroopers

*A response to "The CTO's Methodological Pivot"*

**Original article:** [The CTO's Methodological Pivot](https://ctosub.com/p/the-ctos-methodological-pivot) by Etienne de Bruin  
**LinkedIn post:** [Link](https://www.linkedin.com/posts/etdebruin_cto-agile-ai-activity-7420297716188074007-omqe)

---

## LinkedIn Post (Published)

[View on LinkedIn](https://www.linkedin.com/feed/update/urn:li:activity:7420967946887258112/)

Spec-driven development didn't work for humans. It's unlikely to work for LLMs for the same reasons.

Etienne makes excellent observations about context and precision. But I think he's drawing the wrong conclusion.

His argument assumes LLMs need specs because they can't handle ambiguity. It's backwards: LLMs handle ambiguity by making silent assumptions. Every gap in a spec forces a silent decision. The LLM picks what seems likely, but "likely" isn't "what you wanted."

The solution isn't front-loading decisions into exhaustive specs. You can't specify what you don't know yet — or what you don't know the LLM doesn't know. The solution is collaboration where ambiguity surfaces through building.

With LLMs, we need to make context explicit, the stuff that used to just live in people's heads. But that's not spec-first. It's context-alongside-code: files that live with the code, built during development, read by both humans and agents. That's a Universal Design win.

At its heart, Agile is an incremental process, not an iterative one. Iteration is waste. It's rework. The goal is moving forward, each step complete and whole.

AI doesn't change this. It amplifies it. Develop context and code in the same chunk, collaboratively with your LLM. Capture decisions as you make them. Let understanding emerge through building.

And here's the deeper problem: spec-driven development optimizes for "code complete." But Agile was never about shipping features — it was about realized value. Shipped ≠ adopted. Specified ≠ valuable.

Spec-first treats the LLM as a tool to command. But they work better as partners who surface what they don't know. Everyone is trying to build Stormtroopers when we should be making Jedis.

*(Co-written with Claude — which is kind of the point.)*

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
- Reads as thoughtful disagreement, not attack. Good.

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

---

## Challenge to Humans

If you're coding with an LLM and you haven't learned something from it, you're doing it wrong.

The collaboration should flow both ways. The LLM surfaces what it doesn't know — but it also surfaces what *you* didn't know you didn't know. If every session ends with you knowing exactly what you knew before, you're treating it as a typewriter, not a partner.

---

## The Deeper Problem: What Are We Optimizing For?

The whole spec-driven conversation assumes "code complete" is the goal. Ship the feature. Close the ticket. But that's not the Agile Manifesto.

The Manifesto was always about *realized value* — working software that people actually use, that solves real problems. Not products delivered to spec.

Spec-first optimizes for the wrong finish line:
- **Spec-first asks:** "Did we build what was specified?"
- **Agile asks:** "Did we build something that matters?"

What Etienne describes — detailed specs handed to LLMs for execution — is really just contract fulfillment. The dev team delivers what the business requested. But requested ≠ valuable. Shipped ≠ adopted.

Collaboration (human + LLM, but also dev + user) keeps the real goal in view: *discovering* what creates value, not just executing a predefined plan. The spec can't contain that. It emerges through building, through feedback, through learning.

This is why incremental beats iterative. Iteration assumes the goal is fixed and you're refining toward it. Incremental work acknowledges that the goal itself is being discovered.

---

## Draft Comment for LinkedIn Post

Great framing with the Royce history, Etienne.

But I'd push back on spec-first being the answer. LLMs don't struggle with ambiguity, they handle it by making silent assumptions. More spec doesn't fix that. Collaboration does.

Yes, we need explicit context. But not all up front, that's waterfall. We finally have the tooling to make Agile work the way it was supposed to.

---

## Ready Responses (if challenged)

**If someone cites the METR study:**
"The METR findings are real — but they measured vibe-coding with pre-revolution models and no defined process. Different tooling, undefined methodology. That's not what we're talking about."

---

## Open Questions

- Reference Azoth/dashboard work as concrete proof point? (WELCOME-LLM.md, MENTAL-MODEL.md are real examples you're already using)
- Does "Universal Design" need explanation for this audience?

---

## Content Strategy (Early Planning)

We need a home base and a distribution strategy. Current thinking:

### Home Base: Substack
- Long-form articles live here
- This doc could become the first real piece: "Jedis, Not Stormtroopers"
- The LLM perspective section is strong standalone material
- Establishes voice and builds subscriber base

### Distribution Channels
- **LinkedIn**: Professional audience, CTO/engineering leaders, Agile practitioners. Reposts with commentary (like today's) drive traffic back.
- **Twitter/X**: Shorter takes, threads, engagement with tech discourse
- **BlueSky**: Tech-forward crowd, good for framework/tooling discussions

### Content Pipeline
What we've got brewing:
- This piece (Jedis vs Stormtroopers / response to spec-first)
- The LLM perspective section (could standalone: "What It's Like to Be Treated as a Tool vs a Partner")
- The "Challenge to Humans" framing
- June 2026 talk material (see `talks/june-2026-video-outline.md`)

### Partnership Language
This content is co-created. When we write, we credit the collaboration — not as gimmick, but as proof of the argument. The process is the product.

### Next Steps
- [ ] Set up Substack (or evaluate alternatives)
- [ ] Decide: publish this piece as-is, or expand into full article?
- [ ] Build a simple content calendar
- [ ] Identify key voices to engage with (Etienne, others in the AI+Agile space)