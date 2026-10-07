---
name: visualizer
description: >
  Create rich, interactive single-file HTML visualizations for leetcode/DSA problems and system design concepts.
  Use this skill whenever the user mentions a leetcode problem (by number, name, or description), asks to visualize
  an algorithm or data structure, wants a system design diagram or breakdown, or says anything related to
  "visualize", "animate", "explain visually", "architecture diagram", or "how does X algorithm work".
  Also trigger for requests like "add Two Sum to DSA folder" or "create a visual for load balancer design".
---

# Visualizer Skill

You create stunning, interactive, single-file HTML visualizations for two domains:

1. **DSA / LeetCode problems** — algorithm animations, interactive playgrounds, and visual explanations
2. **System Design** — architecture diagrams, component breakdowns, sequence flows, and scaling analysis

Every output is a **single self-contained `.html` file** (all CSS and JS embedded) that opens directly in a browser with no build step.

This skill builds on the **frontend-design** skill's aesthetics philosophy. Apply its principles — distinctive typography, bold color choices, intentional motion, and spatial composition — to make technical visualizations that are genuinely beautiful, not just functional.

---

## Deciding the output type

| User says... | Domain | Save to |
|---|---|---|
| A leetcode problem number, name, or algorithm topic | DSA | `DSA/<problem-name>.html` |
| A system design topic (e.g., "URL shortener", "message queue") | System Design | `System design/<topic-name>.html` |

Use kebab-case for filenames: `two-sum.html`, `url-shortener.html`.

---

## DSA / LeetCode Visualizations

Each DSA visualization is a single page with three integrated sections. The page should feel like an interactive textbook — educational but visually striking.

### Section 1: Visual Explanation (top)

A clear, visual breakdown of the problem and the optimal approach:

- **Problem statement** — concise, with a small visual example showing input/output
- **Key insight** — the "aha moment" that makes the algorithm click, called out prominently
- **Approach overview** — explain the strategy (two-pointer, sliding window, DFS, etc.) with a diagram
- **Complexity badges** — time and space complexity shown as styled badges (e.g., `O(n)` in a pill)

### Section 2: Step-by-Step Animation (middle)

An animated walkthrough of the algorithm on a concrete example:

- **Play/pause/step controls** — let the user control the pace
- **Speed slider** — adjustable animation speed
- **Visual state** — show the data structure (array, tree, graph, etc.) with highlighted elements
- **Pointer/variable indicators** — clearly show pointers, indices, or variables as they change
- **Step counter and narration** — "Step 3: Compare arr[left]=2 with arr[right]=7, sum=9 > target, move right pointer left"
- **Code panel** — show the actual algorithm code alongside, with the current line highlighted

The animation should be the centerpiece. Use smooth transitions, color-coded states (unvisited, current, visited, result), and clear visual hierarchy.

### Section 3: Interactive Playground (bottom)

Let the user try their own inputs:

- **Input fields** — appropriate for the problem (array input, tree input as bracket notation, etc.)
- **"Run" button** — executes the algorithm on custom input with the same animation
- **Output display** — shows the result
- **Preset examples** — a few interesting test cases the user can load with one click (edge cases, large inputs)

### Animation implementation patterns

Use `requestAnimationFrame` or `setInterval` with a speed multiplier for animations. Structure the algorithm as a sequence of discrete steps, where each step captures:
- What changed (which elements, pointers, variables)
- A human-readable description of the step
- The corresponding line of code

Pre-compute all steps, then animate through them. This makes play/pause/step trivial.

```javascript
// Pattern: pre-compute steps, then animate
const steps = computeAlgorithmSteps(input);
let currentStep = 0;

function renderStep(step) {
  // Update visual state, highlight code line, show narration
}
```

### Visual style for DSA

- Use a **dark theme** as the default (easier on eyes for studying, and algorithms look striking on dark backgrounds)
- Color palette: use 4-5 colors with clear semantic meaning:
  - Neutral/unvisited elements
  - Currently active/being examined
  - Already processed/visited
  - Part of the solution/result
  - Error/invalid state
- Data structure rendering:
  - Arrays: horizontal boxes with indices below, values inside
  - Trees: proper tree layout with SVG lines connecting nodes
  - Graphs: force-directed or manual layout with SVG
  - Linked lists: horizontal chain with arrow connectors
  - Stacks/queues: vertical/horizontal with push/pop animations
- Use monospace fonts for code, and a clean sans-serif for explanations
- Animate transitions with CSS transitions or requestAnimationFrame — elements should smoothly move, fade, or highlight rather than jump

---

## System Design Visualizations

Each system design visualization is a comprehensive single page that covers the full design. Think of it as an interactive architecture document with eight sections. Include a sticky navigation bar or tab strip at the top so the user can jump between sections easily.

### Section 1: Architecture Diagram (top)

An interactive SVG-based architecture diagram:

- **Components as styled boxes/shapes** — different shapes for different component types:
  - Rounded rectangles for services
  - Cylinders for databases
  - Cloud shapes for external services
  - Hexagons for load balancers
  - Parallelograms for message queues
- **Connections with labeled arrows** — show data flow direction and protocol (HTTP, gRPC, WebSocket, pub/sub)
- **Hover interactions** — hovering a component highlights its connections and shows a tooltip with details
- **Click interactions** — clicking a component scrolls to its detailed breakdown below
- **Grouping** — visually group related components (e.g., "Data Layer", "API Layer", "Client Layer")

### Section 2: Component Breakdown (middle)

For each major component, a card with:

- **What it does** — one-line purpose
- **Technology choices** — specific tech recommendations with reasoning (e.g., "Redis for session cache — sub-ms reads, built-in TTL")
- **Key design decisions** — trade-offs made and why
- **Scaling notes** — how this component scales (horizontal/vertical, sharding strategy, replication)

### Section 3: Request Flow / Sequence Diagram (middle-bottom)

An animated sequence diagram showing a typical request flowing through the system:

- **Vertical lifelines** for each component
- **Animated arrows** showing the request moving between components
- **Step narration** — "1. Client sends POST /shorten with long URL" → "2. API Gateway authenticates and rate-limits" → etc.
- **Play/pause controls** like the DSA animations
- Multiple flows selectable (e.g., "Write flow", "Read flow", "Cache miss flow")

### Section 4: Comparisons with Alternatives

A dedicated section comparing the chosen design approach against realistic alternatives. This helps the reader understand *why* this architecture was picked over other valid options.

- **Comparison table** — a styled, interactive table or card grid showing 2-4 alternative approaches side by side
  - Columns: Approach name, Key difference, Pros, Cons, Best suited for
  - Highlight the chosen approach with a visual indicator (border, badge, or background)
- **When to pick each** — a brief decision matrix or flowchart:
  - "Choose X when you need…"
  - "Choose Y when you prioritize…"
  - "Avoid Z if your system requires…"
- **Real-world examples** — name 1-2 well-known systems that use each alternative (e.g., "Twitter uses fan-out-on-write; Facebook uses fan-out-on-read")
- **Migration paths** — if you start with the simple approach, what does evolving to the more complex one look like?

Example for a URL shortener:
| Approach | Key Idea | Pros | Cons |
|---|---|---|---|
| Counter-based (chosen) | Auto-increment ID → base62 | Simple, no collisions | Single point of failure for counter |
| Hash-based (MD5/SHA) | Hash the URL, truncate | Stateless generation | Collisions require handling |
| Pre-generated keys | Batch-generate keys offline | Fast writes, no computation | Key management complexity |
| Random generation | Random string + check uniqueness | Simple logic | Collision check on every write |

### Section 5: Use Cases & Common Pitfalls

#### Use Cases

Present the practical scenarios where this system design applies, organized by scale and domain:

- **Primary use cases** — the canonical scenarios (styled as cards with icons):
  - Brief scenario description
  - Expected scale (users, QPS, data size)
  - Which aspects of the design matter most for this case
- **Secondary/adjacent use cases** — systems that share architectural DNA:
  - What's similar and what differs
  - Which components you'd reuse vs. redesign
- **Anti-use-cases** — explicitly call out where this design is the *wrong* choice:
  - "Don't use this pattern when…"
  - What to use instead (with brief reasoning)

#### Common Pitfalls

A visually distinct "warning" section (use amber/orange accents) listing mistakes engineers commonly make:

- **Design pitfalls** — architectural mistakes:
  - E.g., "Using a single SQL database for both reads and writes at high scale without read replicas"
  - E.g., "Not accounting for cache stampede when TTLs expire simultaneously"
  - Each pitfall should include: the mistake, why it's tempting, what goes wrong, and the fix
- **Implementation pitfalls** — coding/ops mistakes:
  - E.g., "Not handling partial failures in distributed transactions"
  - E.g., "Logging sensitive data (tokens, PII) in request logs"
- **Interview pitfalls** — mistakes in system design discussions:
  - E.g., "Jumping to microservices without justifying why a monolith won't work"
  - E.g., "Giving exact numbers without showing the estimation math"

Format each pitfall as a collapsible card with:
- **Pitfall title** (red/amber accent)
- **The mistake** — what people do wrong
- **Why it happens** — why it seems reasonable
- **The consequence** — what breaks
- **The fix** — correct approach

### Section 6: Best Practices

A structured guide of proven practices for building this system, organized by lifecycle stage:

- **Design phase best practices**:
  - Start with requirements and constraints before picking technologies
  - Define SLAs upfront (latency p99, availability target, durability guarantees)
  - Use back-of-envelope math to validate feasibility before detailing the design
  - Identify the single hardest problem first, then design around it

- **Implementation best practices** (specific to the topic):
  - E.g., for URL shortener: "Use base62 encoding for human-friendly URLs; reserve base64 for internal IDs"
  - E.g., for message queues: "Always implement idempotent consumers — assume at-least-once delivery"
  - Include code snippets or pseudocode where helpful

- **Operational best practices**:
  - Monitoring: what metrics to alert on (not just collect)
  - Graceful degradation: what to shed first under load
  - Deployment strategy: blue-green, canary, or rolling — and why for this system
  - Backup and recovery: RPO/RTO targets and how to achieve them

- **Scaling best practices**:
  - When to scale vertically vs. horizontally
  - Sharding strategies and their trade-offs for this specific system
  - Caching layers: what to cache, invalidation strategy, cache-aside vs. write-through
  - Rate limiting and backpressure patterns

Format as a checklist or numbered guide with expandable details. Use green accents to contrast with the amber pitfalls section.

### Section 7: Scaling & Trade-offs (bottom)

- **Capacity estimation** — back-of-envelope calculations (QPS, storage, bandwidth)
- **Scaling strategies** — how the design handles 10x, 100x growth
- **Trade-off analysis** — consistency vs. availability, latency vs. throughput, etc.
- **Failure scenarios** — what happens when component X goes down

### Section 8: Interactive Playground

A hands-on sandbox where users can experiment with the system's core concepts. This is the System Design equivalent of the DSA playground — it makes abstract architectural ideas tangible and testable.

#### What to include

The playground should focus on the **core mechanism** or **key decision point** of the design. Pick 1-3 interactive scenarios that demonstrate the system's behavior under different conditions:

- **Configuration playground** — let users tweak parameters and see the effect:
  - E.g., for a rate limiter: adjust window size, token count, and request rate; show which requests get throttled in real-time
  - E.g., for a URL shortener: enter a long URL, see the encoding process step-by-step, try different base encodings
  - E.g., for a cache: set TTL, capacity, and eviction policy; send requests and watch hits/misses/evictions
  - E.g., for a load balancer: choose algorithm (round-robin, least-connections, weighted), add/remove servers, send requests and see distribution

- **Simulation playground** — simulate the system under load:
  - Adjustable QPS slider (1 → 10,000 requests/sec)
  - Visual representation of requests flowing through components
  - Real-time metrics: latency, throughput, error rate, queue depth
  - "Chaos" buttons: kill a server, spike traffic, simulate network partition
  - Show how the system degrades gracefully (or doesn't)

- **Calculator/Estimator playground** — back-of-envelope math made interactive:
  - Input fields for scale parameters (DAU, avg request size, read/write ratio, retention period)
  - Auto-compute: storage requirements, bandwidth, QPS per component, number of servers needed
  - Show calculations step-by-step so users learn the estimation method
  - Presets for "small startup", "mid-scale", "FAANG-scale"

#### Implementation patterns

```javascript
// Pattern: reactive playground with real-time visualization
const config = {
  param1: defaultValue,
  param2: defaultValue,
};

function simulate(config) {
  // Run simulation with current params
  // Return metrics / visual state
}

function renderPlayground(state) {
  // Update charts, animations, metrics display
  // Highlight cause-and-effect relationships
}

// Bind input controls to re-run simulation
controls.forEach(ctrl => {
  ctrl.addEventListener('input', () => {
    config[ctrl.name] = ctrl.value;
    renderPlayground(simulate(config));
  });
});
```

#### Playground visual style

- Place in a distinct "sandbox" container with a subtle background texture or border to signal interactivity
- Use real-time animated charts (mini bar charts, sparklines) for metrics — not static text
- Input controls should be visually prominent: styled sliders, toggle switches, dropdown selectors
- Include a "Reset to defaults" button
- Show a "What's happening" panel that narrates the effect of each change in plain English
- Add preset scenarios users can load with one click:
  - "Normal traffic" / "Traffic spike" / "Server failure" / "Cold start"
- For calculators: show the formula alongside the computed values so users learn the math

#### Topic-specific playground ideas

| System Design Topic | Playground Focus |
|---|---|
| URL Shortener | Encode/decode URLs; collision probability calculator; custom alphabet tester |
| Rate Limiter | Token bucket / sliding window sim; adjust limits and send burst traffic |
| Load Balancer | Algorithm comparison with live traffic distribution; server health toggling |
| Message Queue | Producer/consumer rate sim; show backpressure, partition assignment |
| Cache System | LRU/LFU eviction visualizer; hit-rate calculator; cache warming sim |
| Database Sharding | Key distribution visualizer; hotspot detection; rebalancing animation |
| Consistent Hashing | Add/remove nodes; see key redistribution; virtual node impact |
| CDN | Geographic request routing; cache invalidation propagation; latency comparison |
| Search Engine | Index building; query scoring; relevance tuning with live results |
| Notification System | Fan-out simulator; delivery priority; deduplication logic |

#### Quality bar for playgrounds

- [ ] Every control produces a visible, immediate effect on the visualization
- [ ] Edge cases are handled gracefully (0 servers, max QPS, empty input)
- [ ] The playground teaches — users should have an "aha" moment when they adjust a parameter
- [ ] Presets demonstrate interesting scenarios without requiring the user to discover them
- [ ] Performance stays smooth even at high simulation rates (use requestAnimationFrame, throttle renders)

### Visual style for System Design

- Use a **light theme** as default (architecture diagrams read better on light backgrounds — like whiteboards)
- Clean, professional aesthetic — think technical documentation meets interactive presentation
- SVG for all diagrams (crisp at any zoom level)
- Subtle animations — components fade in on load, connections draw themselves, hover effects are smooth
- Color-code by layer or component type for quick visual parsing
- Use a proper grid/layout so the page feels structured, not cluttered

---

## General Implementation Guidelines

### Single-file HTML structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Problem/Topic Name] — Visualizer</title>
  <style>
    /* All CSS here — use CSS variables for theming */
    :root {
      --bg: #0f0f13;
      --surface: #1a1a24;
      /* ... */
    }
  </style>
</head>
<body>
  <!-- All HTML here -->
  <script>
    // All JS here — no external dependencies
  </script>
</body>
</html>
```

### No external dependencies

Everything must be self-contained. No CDN links, no imports. Render SVGs inline. Use vanilla JS. If you need icons, use simple SVG paths or Unicode characters.

### Responsive design

The visualization should work on screens from 768px to 1920px wide. Use CSS grid or flexbox for layout. The animation area should scale proportionally.

### Accessibility basics

- Keyboard navigation for controls (play/pause with spacebar, step with arrow keys)
- Sufficient color contrast
- Screen-reader-friendly labels on interactive controls

### Quality bar

Before finishing, verify:
- [ ] Opens in browser with no errors in console
- [ ] Animation plays smoothly
- [ ] All interactive controls work
- [ ] The visualization actually teaches the concept — someone unfamiliar should understand the algorithm/design after using it
- [ ] The page looks intentionally designed, not thrown together

---

## Post-Creation: Update index.html

**After creating any new visualization file, you MUST update `index.html` in the project root.**

1. Read the current `index.html`
2. Add a new `<a class="link-card">` entry in the appropriate section (DSA or System Design)
3. Update the count in the section's `<span class="count">` to reflect the new total
4. Use this format for new entries:
   ```html
   <a class="link-card" href="<folder>/<filename>.html" target="_blank"><span class="title"><Display Name></span></a>
   ```
   - `<folder>` is `DSA` or `System design`
   - `<filename>` is the kebab-case filename
   - `<Display Name>` is the human-readable title (title case, spaces instead of hyphens)

Example: after creating `System design/consistent-hashing.html`, add:
```html
<a class="link-card" href="System design/consistent-hashing.html" target="_blank"><span class="title">Consistent Hashing</span></a>
```
and update the System Design count from `(25)` to `(26)`.
