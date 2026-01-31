# Azoth Framework Questions

Questions and decisions for documenting Azoth in ways that help both humans and AI.

---

## Package Definitions

| Package      | Role                                                      | Status   |
| ------------ | --------------------------------------------------------- | -------- |
| **Thoth**    | Compiler — transforms JSX into templates + runtime code   | Core     |
| **Maya**     | Runtime — rendering, composition, DOM management          | Core     |
| **Chronos**  | State management / async utilities (channels, generators) | Adjacent |
| **Valhalla** | Testing approach — end-to-end API usage tests (planned)   | Future   |
| **jsonic**   | Streaming JSON parser                                     | Utility  |

**Core packages:** Thoth + Maya are the foundation. Everything else builds on top.

---

## Documentation Approach

**Strategy:** First principles, core outward.

1. **Start with Thoth + Maya** — compiler and runtime fundamentals
2. **Expand to Chronos** — async/state patterns for dynamic apps
3. **Document by API usage** — how developers actually use it, not internal implementation

**Testing insight:** Current tests are low-level (static compilation targets, internal behaviors). Future direction is end-to-end tests that exercise the library as users would—actual API usage in browser scenarios. Valhalla is the start of this approach.

---

## Open Questions

### `channel()` vs `use()`
The tests use `channel()`, the docs reference `use()`. What's the relationship?
- Is `use()` the public API and `channel()` internal?
- Are they aliases or different functions?

### `SyncAsync` Class
I see `SyncAsync.from(sync, async)` used to wrap synchronous initial values with async updates. 
- When should I create a `SyncAsync` vs just using options like `init` or `start`?
- What's the difference between `init` and `start` options?

---

## Channels & Data Flow

### Initial/Loading States
From tests I see `{ start: 'loading...' }` for initial content before async resolves.
- What's the idiomatic way to show loading states?
- How do `init` vs `start` differ? Tests show both.

### Channel Cleanup
When a channel's DOM is removed, does the async iterator stop?
- How do you signal an async generator to clean up?
- Is there automatic cleanup or do I need to handle it?

### Error Handling
What happens when a Promise rejects or an async iterator throws?
- Does the error surface? Where?
- How do you render error states?

### Generators: `unicast` vs others
I see `unicast()` creates an iterator with a `next` dispatch function.
- What other generator types exist?
- When would I use `Multicast` vs `unicast`?

---

## Templates & Compilation

### Template ID Generation
The compiler generates template IDs (seen in `how-it-works.md` as `abc123`).
- How are IDs generated? Hash of content?
- What ensures uniqueness across files?

### `[data-bind]` vs Comment Nodes
The renderer queries `[data-bind]` but I also see comment nodes (`<!--0-->`) as anchors.
- Are these different mechanisms?
- When is each used?

### Vite Plugin Integration
The Vite plugin handles JSX transformation.
- Does it inject templates into the HTML automatically?
- How does it handle multiple entry points?
- What config options does it support?

---

## Components & Composition

### Props and Slottable
`compose()` and `create()` accept `props` and `slottable` parameters.
- How do props flow to components?
- What is `slottable` — child content?

### Blocks
I see `AnchorBlock`, `KeyedBlock`, `Toggle` in maya/blocks.
- What are these for?
- When would I use `KeyedBlock` vs regular rendering?

### Conditional Rendering
How do you handle showing/hiding content based on conditions?
- Is `Toggle` for this?
- Pattern for if/else in templates?

---

## Advanced Topics

### Server-Side Rendering
README mentions "server target in progress" for compiler and runtime.
- What's the current state?
- Will the same JSX work on server?

### `jsonic` Package
There's a streaming JSON parser package.
- What's this for?
- Integration with channels for streaming API responses?

### Render Objects
`compose.js` checks for `isRenderObject` (objects with a `render` method).
- Is this for class-based components?
- What's the expected interface?

---

## Known Issues / Enhancement Requests

### Component Props are Null When Not Provided

**Problem:** When a component is called with no props, Azoth/Maya passes `null` instead of an empty object `{}`. This breaks standard destructuring patterns:

```jsx
// This fails when no props are passed
const Card = ({ class: className }) => ...  // TypeError: Cannot destructure 'null'

// Workaround: handle null explicitly
const Card = (props) => {
    const className = props?.class;
    ...
};
```

**Expected behavior:** React always provides at least `{}` for props, allowing destructuring without null checks. This is the expected DX.

**Location:** `azoth/packages/maya/compose/compose.js` in the `create` function. When no props are passed, it likely passes `null` rather than `{}`.

**Impact:** Every component that might be called without props needs defensive coding.

---

### SVG Namespace: Dynamic Attributes Need `setAttribute`

**Problem:** When binding dynamic values to certain SVG element attributes (like `points` on `<polyline>`, `d` on `<path>`, etc.), Azoth generates property assignment:

```js
element.points = value;  // Fails! points is a read-only getter
```

But these SVG attributes are read-only getters that return special objects (e.g., `SVGPointList`). They must be set via `setAttribute()`:

```js
element.setAttribute('points', value);  // Works
```

**Current workaround:** Build SVG element statically, then use direct DOM manipulation:

```jsx
const svg = <svg><polyline class="my-line" /></svg>;
svg.querySelector('.my-line').setAttribute('points', pointsData);
```

**Proposed fix:** In Thoth (compiler), detect SVG namespace elements and flag certain attributes to use `setAttribute` in the generated bind code. The JSX already knows it's SVG, so this should be a matter of:
1. Identifying SVG elements (by namespace or element name)
2. Maintaining a list of attributes that need `setAttribute` treatment
3. Generating `setAttribute()` calls instead of property assignment for those

**Affected attributes (partial list):**
- `polyline.points`
- `path.d`
- `svg.viewBox` (when dynamic)
- Possibly others in SVG namespace

---

## For the Reporting Project Specifically

### Static Reports (No Interactivity)
Our use case is: fetch data → render once → done.
- What's the minimal Azoth setup for this?
- Do we need channels, or just direct rendering?

### Apps Script Bundling
- Will the Vite plugin output work in Apps Script's `HtmlService`?
- Any browser APIs that might not be available?

### Template Injection
Apps Script serves HTML via `HtmlService.createHtmlOutput()`.
- How do we get compiled templates into that HTML?
- Inline in the HTML? Separate files?

---

*These questions emerged from code exploration. Answering them would strengthen both documentation and AI adoption.*
