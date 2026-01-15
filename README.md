# Innovating in the Era of AI: Teaching LLMs New Tricks

Best practices and strategies for introducing new patterns, libraries, and approaches when AI assistants are biased toward dominant incumbents.

## The Problem

AI coding assistants are trained on the world's codebases—where dominant patterns dominate. When you introduce something new, the AI drifts toward what it knows.

**Example:** JSX libraries. React dominates training data. Ask an AI to write JSX for a non-React library, and it slips into `useState`, `useEffect`, virtual DOM thinking—even when those concepts don't apply.

## This Repository

Strategies, patterns, and case studies for getting AI to adopt non-dominant approaches.

### Contents

```
strategies/
├── documentation-patterns.md   # 7 strategies for AI-friendly docs
└── naming-for-ai-adoption.md   # API naming to avoid conflation

case-studies/
└── azoth-reporting.md          # Live case study (in progress)

talks/
├── june-2026-cfp.md            # Conference talk proposal
└── june-2026-video-outline.md  # 2-min video outline
```

## Origin

This work emerges from building production systems at a real estate brokerage using [Azoth](https://github.com/azothjs/azoth), a hypermedia-focused JSX library. The challenge: how do you get AI assistance when the AI wants to write React?

## Key Insights (So Far)

1. **Documentation must be contrastive** — Show "do this, not that" against dominant patterns
2. **Name things differently** — Similar names invite AI conflation; fresh terminology creates clean breaks
3. **Philosophy docs matter** — AI can reason from concepts, not just syntax
4. **The name is documentation** — When a concept is different, a different name signals that difference

---

*Active development — strategies refined through real project work*
