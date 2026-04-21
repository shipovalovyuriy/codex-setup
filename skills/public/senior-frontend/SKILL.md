---
name: senior-frontend
description: Senior frontend implementation and review for React, Next.js, TypeScript, and Tailwind with production patterns, performance optimization, and Vercel React best-practice rules. Use when building or refactoring frontend features, reviewing React or Next.js code, fixing UX or performance regressions, generating component scaffolds, or hardening frontend quality gates.
---

# Senior Frontend

Build and review production frontend code with practical architecture rules and performance-first defaults.

## Execute This Workflow

1. Classify the request.
- Identify whether the task is implementation, refactor, debugging, performance, or review.
- Detect runtime and stack details: React version, Next.js router mode, rendering model, state/data libraries, styling system.
- Read the current feature flow and dependency usage before changing files.

2. Load only the required references.
- Start with `/references/frontend_best_practices.md` for cross-cutting quality constraints.
- Use `/references/react_patterns.md` for component and state architecture.
- Use `/references/nextjs_optimization_guide.md` for App Router and SSR performance.
- For performance-sensitive React/Next.js tasks, load `/references/vercel-react-best-practices/index.md`, then open only relevant files from `/references/vercel-react-best-practices/rules/`.
- Treat the Vercel guide as the default React/Next.js performance reference when writing or reviewing components, pages, data fetching, bundle size, load time, hydration, or render behavior.

3. Implement or review in priority order.
- Resolve blocking correctness and accessibility issues first.
- Resolve critical performance patterns next: `async-*` and `bundle-*` rule families.
- Resolve server-side performance patterns (`server-*`) before client micro-optimizations.
- Apply medium and low-priority patterns only when they improve measurable outcomes or simplify maintenance.

4. Use scripts when they accelerate delivery.
- `python scripts/component_generator.py <target>` for scaffolding helpers.
- `python scripts/bundle_analyzer.py <target> --verbose` for quick structure and report output.
- `python scripts/frontend_scaffolder.py <target>` for repository-level setup helpers.

5. Validate and report outcomes.
- Run project linters, tests, type checks, and build commands.
- Report what changed, which rule IDs were applied, and what metrics/checks improved.
- If no measurable impact exists, prefer simpler code.

## Mandatory Engineering Discipline

- Do not change code until full context is understood: current component flow, shared utilities, and dependency capabilities.
- Inspect libraries and existing wrappers first; do not duplicate behavior that is already provided under the hood.
- Reuse existing hooks, components, utilities, and patterns before creating new ones.
- Leave no code trash: remove temporary debug code, unused imports, dead components/helpers, commented-out blocks, and stale TODOs.
- Optimize for readability and runtime cost: avoid unnecessary re-renders, extra bundles, and redundant abstractions.

## Use Vercel Rules By Impact

Prioritize categories in this order when optimizing React/Next.js performance:

1. `async-*` (eliminate waterfalls)
2. `bundle-*` (reduce shipped JavaScript)
3. `server-*` (improve server throughput and payloads)
4. `client-*` (control client-side data and listeners)
5. `rerender-*` (reduce render churn)
6. `rendering-*` (optimize paint/hydration behavior)
7. `js-*` (micro-optimizations)
8. `advanced-*` (specialized patterns)

## Reference Map

- Main index for imported Vercel material:
  - `/references/vercel-react-best-practices/index.md`
- Full upstream snapshot:
  - `/references/vercel-react-best-practices/full-guide.md`
- Rule files:
  - `/references/vercel-react-best-practices/rules/*.md` (64 rules across 8 categories)
- Local implementation guides:
  - `/references/react_patterns.md`
  - `/references/nextjs_optimization_guide.md`
  - `/references/frontend_best_practices.md`
