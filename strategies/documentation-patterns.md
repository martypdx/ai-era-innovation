# Documentation Patterns for AI Adoption

Strategies for documenting libraries/frameworks so AI assistants can use them correctly despite training bias toward incumbents.

## The Core Problem

AI training data reflects market share. React has 10-100x more examples than alternatives. Without explicit guidance, AI defaults to dominant patterns even when using other tools.

## Strategies

### 1. Explicit Contrast Docs

Show "do this, not that" comparisons against the dominant pattern.

```markdown
## Handling Async Data

### React (don't do this)
```jsx
const [data, setData] = useState([]);
useEffect(() => {
  fetchData().then(setData);
}, []);
```

### Azoth (do this)
```jsx
const DataChannel = use(fetchData(), items => items.map(Item));
```
```

**Why it works:** AI can retrieve the contrast and apply the correct pattern. The explicit "don't do this" creates a decision boundary.

### 2. Named Patterns

Give your patterns memorable names that can be invoked.

- ❌ "Do it the Azoth way"
- ✅ "Use the channel pattern"

Named patterns are more retrievable. "Use the channel pattern for async data" is specific; "do it like Azoth does" requires the AI to know what that means.

### 3. Philosophy Documentation

Explain the *why*, not just the *how*. AI can use conceptual understanding to reason about edge cases.

**Example from Azoth:**
> "State and the UI cannot both be the source of truth. In hypermedia, the UI is truth—data flows into it, not the other way around."

This helps AI understand *when* to apply patterns, not just *what* the syntax is.

### 4. Complete Examples Over API Reference

API docs tell you what functions exist. Examples show how to think.

For AI adoption, prioritize:
- Full working examples
- "Here's how you approach this problem"
- Mental model walkthroughs

### 5. Counter-Examples

Show what NOT to do and explain why it's wrong in this context.

```markdown
## Common Mistakes

### ❌ Managing state for derived values
```jsx
// WRONG: This is React thinking
const [count, setCount] = useState(items.length);
```

In Azoth, you don't store derived values as state. The channel output *is* the value.
```

### 6. Type Definitions as Guardrails

Strong types prevent incorrect usage. If `useState` doesn't exist in your type definitions, AI can't use it.

TypeScript definitions are both documentation and enforcement.

### 7. Terminology Discipline

Avoid overloaded terms from dominant frameworks.

- Don't call things "hooks" if they're not React hooks
- Don't say "component lifecycle" if you don't have one
- Create fresh terminology for fresh concepts

### 8. LLM Mental Model Documents

Instead of writing documentation FOR the AI, have the AI write its understanding and you correct it.

**The pattern:**
1. Create a document where the LLM makes its mental model explicit
2. Include: what it thinks it understands, uncertainties, questions, and where its biases might show
3. Human reviews and provides corrections
4. LLM updates the document iteratively

**Why it works:**
- Surfaces hidden assumptions and biases the LLM has absorbed from training
- Creates documentation that directly addresses what the LLM gets wrong
- The corrections become highly targeted training signal
- Produces docs that anticipate AI misunderstandings (because they came from AI misunderstandings)

**Example structure:**
```markdown
# My Mental Model of [Framework]

## What I Believe I Understand
[LLM writes its understanding with contrasts to dominant frameworks]

## Where My [React/etc] Bias Might Be Showing
[LLM identifies its own potential misconceptions]

## Questions I Have
[Gaps that need clarification]

## Inconsistencies I Notice
[Self-identified contradictions in understanding]
```

*See: `/azoth/docs/MENTAL-MODEL.md` for a live example of this pattern.*

### 9. Implementation Tests as Developer Documentation

**The conventional wisdom:** Test at the interface level. Implementation details shouldn't matter. API contracts are stable; internals change.

**The developer mindset behind this:** As developers, we're trained to think in terms of *information hiding*. Encapsulation. Separation of concerns. We deliberately create barriers between subsystems so that:
- Changes don't ripple unexpectedly
- Modules only know what they need to know
- Contracts are well-defined at boundaries
- "Leaky abstractions" are avoided

This is good system design. When building software, you *want* components that don't need to understand each other's internals. The less a module knows about another module's implementation, the more loosely coupled the system.

**Why this instinct fails with LLMs:** The LLM isn't a module in your system. It's not a consumer of your API that should be shielded from implementation changes. It's a *collaborator* trying to understand and work on the system alongside you.

Applying the "hide implementation details" instinct to LLM collaboration is like:
- Asking a new team member to fix bugs without reading the source code
- Onboarding a developer by only showing them the API docs, never the codebase
- Expecting someone to debug a transpiler by only knowing what it outputs, not how it transforms

The encapsulation instinct is so ingrained that it feels *wrong* to expose internals. But the LLM isn't a subsystem you're protecting from coupling—it's a pair programmer who needs to understand the whole picture.

**What detailed implementation tests provide:**

1. **Possibility space mapping** — When debugging Thoth's boolean prop bug, the compiler tests showed that `attr="static"` *did* produce correct output (`attr: "static"`). This immediately narrowed the bug to null-value attributes, not "all static props." Without these tests, I'd have been guessing.

2. **Intermediate representation visibility** — Tests that show internal structures (AST nodes, template metadata, binding maps) let the LLM understand *how* the system transforms input to output. This enables reasoning about edge cases, not just pattern matching on examples.

3. **Design intent revelation** — Seeing *why* code paths differ (e.g., `jsxOnly=true` for DOM elements vs `false` for components) lets the LLM make informed decisions about fixes. It's the difference between "this line crashed" and "this design assumption doesn't cover this case."

4. **Hypothesis verification** — Before writing a fix, the LLM can check existing tests to verify that the proposed solution produces output consistent with existing patterns.

**Example from practice:**

Debugging a transpiler crash on `<Component flag />`:
- Compiler tests showed `<Component attr="static"/>` → `{ attr: "static" }` ✓
- Tests showed intermediate `expr` values and binding types
- Code inspection revealed `jsxOnly` parameter controlling two code paths
- Fix was informed: create synthetic `{ type: 'Literal', value: true }` to match existing static prop handling

Without implementation tests, this would have been trial-and-error. With them, it was systematic.

**The mindset shift:**

Don't ask "what does the LLM need to *use* this?" — ask "what does the LLM need to *understand* this?"

Traditional API docs answer: "What can I call and what will it return?"
Implementation tests answer: "How does it work, and what did the designer intend?"

For LLM-assisted development, the second question is often more valuable.

**Practical takeaway:** Keep detailed implementation tests even when you have comprehensive API tests. They serve different purposes. API tests ensure contracts; implementation tests enable collaboration.

---

## Meta-Observation

These strategies aren't just about documentation—they're about creating *retrievable knowledge* that AI can find and apply. The question isn't "is this documented?" but "can AI find and correctly apply this when needed?"
