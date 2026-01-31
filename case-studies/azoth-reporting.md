# Case Study: Azoth for Business Reporting

Using a non-React JSX library (Azoth) for AI-generated business reports.

## Context

**Project:** Web-native reporting system replacing Looker Studio
**Challenge:** Get AI to generate Azoth JSX without drifting to React patterns
**Library:** [Azoth](https://github.com/azothjs/azoth) — hypermedia-focused, event-driven JSX

## Why This Is Hard

Azoth and React both use JSX, but think fundamentally differently:

| Concept | React | Azoth |
|---------|-------|-------|
| Source of truth | State | DOM (hypermedia) |
| Data flow | State → render → DOM | Data → channel → DOM |
| Updates | Re-render component tree | Direct DOM mutation |
| Async handling | useEffect + useState | Channels |

The JSX syntax looks similar. The mental model is opposite.

## Approach

1. **Simple use case first** — Reports are initial render + data. No complex state, no user interactions modifying state. Pattern: `fetch → channel → template → done`.

2. **Document patterns during development** — As AI slips into React patterns, document corrections as contrast docs.

3. **Explicit prompting** — "Generate Azoth JSX" not just "generate JSX"

4. **Keep Azoth source in context** — AI can reference actual implementation, not just training data

## Observations

*(To be filled in as project progresses)*

- Triggers that caused React drift
- Corrections that worked
- Patterns that stuck after documentation

## Outcome

*(TBD)*

---

*This case study is being developed alongside the [WRE Reporting Project](../related/wre-dashboard-api/docs/pivot-feasibility.md)*
