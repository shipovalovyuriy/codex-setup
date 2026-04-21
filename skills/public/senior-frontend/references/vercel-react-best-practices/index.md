# Vercel React Best Practices Index

Comprehensive performance optimization guide for React and Next.js applications, maintained by Vercel. Contains 64 rules across 8 categories, prioritized by impact to guide automated refactoring and code generation.

Use this index to apply the imported `react-best-practices` material with minimal context load.

## When to Apply

Reference these guidelines when:

- Writing new React components or Next.js pages
- Implementing data fetching, either client-side or server-side
- Reviewing code for performance issues
- Refactoring existing React/Next.js code
- Optimizing bundle size or load times

## Source

- Upstream repository: `vercel-labs/agent-skills`
- Imported skill: `skills/react-best-practices`
- Local snapshot files:
  - `full-guide.md` (compiled full document)
  - `upstream-skill.md` (upstream SKILL.md snapshot)
  - `rules/*.md` (rule-by-rule files)

## Apply Rules With Minimal Context

1. Open only this file first.
2. Pick the category from symptoms or task type.
3. Load only the exact rule files needed from `rules/`.
4. Apply highest-priority fixes first.
5. Validate with build, tests, and runtime checks.

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
| --- | --- | --- | --- |
| 1 | Eliminating Waterfalls | CRITICAL | `async-` |
| 2 | Bundle Size Optimization | CRITICAL | `bundle-` |
| 3 | Server-Side Performance | HIGH | `server-` |
| 4 | Client-Side Data Fetching | MEDIUM-HIGH | `client-` |
| 5 | Re-render Optimization | MEDIUM | `rerender-` |
| 6 | Rendering Performance | MEDIUM | `rendering-` |
| 7 | JavaScript Performance | LOW-MEDIUM | `js-` |
| 8 | Advanced Patterns | LOW | `advanced-` |

## Category Selection

- Waterfalls, slow TTFB, sequential fetches:
  - `async-*`
- Large bundles, slow hydration, heavy JS payload:
  - `bundle-*`
- Server action/API throughput, cache inefficiency, large RSC payloads:
  - `server-*`
- Duplicate client fetches/listeners, noisy localStorage patterns:
  - `client-*`
- Frequent rerenders, unstable effects/dependencies:
  - `rerender-*`
- Paint/hydration bottlenecks and visual instability:
  - `rendering-*`
- Tight loops and hot-path JS inefficiencies:
  - `js-*`
- Specialized lifecycle or event-handler patterns:
  - `advanced-*`

## Quick Reference

### 1. Eliminating Waterfalls (CRITICAL)

- `async-defer-await` - Move await into branches where actually used
- `async-parallel` - Use `Promise.all()` for independent operations
- `async-dependencies` - Use `better-all` or early promise creation for partial dependencies
- `async-api-routes` - Start promises early, await late in API routes
- `async-suspense-boundaries` - Use Suspense to stream content

### 2. Bundle Size Optimization (CRITICAL)

- `bundle-barrel-imports` - Import directly, avoid barrel files
- `bundle-dynamic-imports` - Use `next/dynamic` for heavy components
- `bundle-defer-third-party` - Load analytics/logging after hydration
- `bundle-conditional` - Load modules only when a feature is activated
- `bundle-preload` - Preload on hover/focus for perceived speed

### 3. Server-Side Performance (HIGH)

- `server-auth-actions` - Authenticate server actions like API routes
- `server-cache-react` - Use `React.cache()` for per-request deduplication
- `server-cache-lru` - Use LRU cache for cross-request caching
- `server-dedup-props` - Avoid duplicate serialization in RSC props
- `server-hoist-static-io` - Hoist static I/O such as fonts and logos to module level
- `server-serialization` - Minimize data passed to client components
- `server-parallel-fetching` - Restructure components to parallelize fetches
- `server-after-nonblocking` - Use `after()` for non-blocking operations

### 4. Client-Side Data Fetching (MEDIUM-HIGH)

- `client-swr-dedup` - Use SWR for automatic request deduplication
- `client-event-listeners` - Deduplicate global event listeners
- `client-passive-event-listeners` - Use passive listeners for scroll
- `client-localstorage-schema` - Version and minimize localStorage data

### 5. Re-render Optimization (MEDIUM)

- `rerender-defer-reads` - Do not subscribe to state only used in callbacks
- `rerender-memo` - Extract expensive work into memoized components
- `rerender-memo-with-default-value` - Hoist default non-primitive props
- `rerender-dependencies` - Use primitive dependencies in effects
- `rerender-derived-state` - Subscribe to derived booleans, not raw values
- `rerender-derived-state-no-effect` - Derive state during render, not effects
- `rerender-functional-setstate` - Use functional `setState` for stable callbacks
- `rerender-lazy-state-init` - Pass a function to `useState` for expensive values
- `rerender-simple-expression-in-memo` - Avoid memo for simple primitives
- `rerender-split-combined-hooks` - Split hooks with independent dependencies
- `rerender-move-effect-to-event` - Put interaction logic in event handlers
- `rerender-transitions` - Use `startTransition` for non-urgent updates
- `rerender-use-deferred-value` - Defer expensive renders to keep input responsive
- `rerender-use-ref-transient-values` - Use refs for transient frequent values
- `rerender-no-inline-components` - Do not define components inside components

### 6. Rendering Performance (MEDIUM)

- `rendering-animate-svg-wrapper` - Animate a div wrapper, not the SVG element
- `rendering-content-visibility` - Use `content-visibility` for long lists
- `rendering-hoist-jsx` - Extract static JSX outside components
- `rendering-svg-precision` - Reduce SVG coordinate precision
- `rendering-hydration-no-flicker` - Use inline script for client-only data
- `rendering-hydration-suppress-warning` - Suppress expected mismatches
- `rendering-activity` - Use `Activity` component for show/hide
- `rendering-conditional-render` - Use ternary, not `&&`, for conditionals
- `rendering-usetransition-loading` - Prefer `useTransition` for loading state
- `rendering-resource-hints` - Use React DOM resource hints for preloading
- `rendering-script-defer-async` - Use `defer` or `async` on script tags

### 7. JavaScript Performance (LOW-MEDIUM)

- `js-batch-dom-css` - Group CSS changes via classes or `cssText`
- `js-index-maps` - Build `Map` for repeated lookups
- `js-cache-property-access` - Cache object properties in loops
- `js-cache-function-results` - Cache function results in module-level `Map`
- `js-cache-storage` - Cache localStorage/sessionStorage reads
- `js-combine-iterations` - Combine multiple `filter`/`map` passes into one loop
- `js-length-check-first` - Check array length before expensive comparison
- `js-early-exit` - Return early from functions
- `js-hoist-regexp` - Hoist RegExp creation outside loops
- `js-min-max-loop` - Use loop for min/max instead of sort
- `js-set-map-lookups` - Use `Set`/`Map` for O(1) lookups
- `js-tosorted-immutable` - Use `toSorted()` for immutability
- `js-flatmap-filter` - Use `flatMap()` to map and filter in one pass when clear

### 8. Advanced Patterns (LOW)

- `advanced-event-handler-refs` - Store event handlers in refs
- `advanced-init-once` - Initialize app once per app load
- `advanced-use-latest` - Use latest-value refs for stable callbacks

## High-Impact Rule Starter Set

Start with these before broader cleanup:

- `rules/async-parallel.md`
- `rules/async-defer-await.md`
- `rules/async-suspense-boundaries.md`
- `rules/bundle-barrel-imports.md`
- `rules/bundle-dynamic-imports.md`
- `rules/bundle-defer-third-party.md`
- `rules/server-hoist-static-io.md`
- `rules/server-cache-react.md`
- `rules/server-parallel-fetching.md`
- `rules/server-serialization.md`
- `rules/rerender-dependencies.md`
- `rules/rerender-derived-state-no-effect.md`
- `rules/rendering-content-visibility.md`

## PR Integration Pattern

When preparing a change:

1. Document selected rule IDs in the PR description.
2. Attach before/after evidence (bundle size, web-vitals, or trace).
3. Keep unrelated style refactors out of the patch.
4. Skip low-priority micro-optimizations unless measurable.

## Full Compiled Document

For the complete guide with all rules expanded, read `full-guide.md`. For focused work, prefer individual files under `rules/`.

## License Note

The imported upstream material is distributed under MIT in the source repository. Keep attribution when reusing these rule files elsewhere.
