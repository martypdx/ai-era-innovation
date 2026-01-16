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

---

## Meta-Observation

These strategies aren't just about documentation—they're about creating *retrievable knowledge* that AI can find and apply. The question isn't "is this documented?" but "can AI find and correctly apply this when needed?"
