---
title: Use useDeferredValue for Expensive Renders
impact: MEDIUM
impactDescription: keeps urgent input updates responsive while expensive UI catches up
tags: rerender, useDeferredValue, input, responsiveness, concurrent-rendering
---

## Use useDeferredValue for Expensive Renders

Use `useDeferredValue` when an urgent value, such as text input, drives an expensive render. Render the input from the immediate value and render the heavy subtree from the deferred value so typing stays responsive.

**Incorrect (expensive list blocks typing):**

```tsx
function SearchPage({ items }: { items: Item[] }) {
  const [query, setQuery] = useState('')
  const results = filterItems(items, query)

  return (
    <>
      <input value={query} onChange={event => setQuery(event.target.value)} />
      <ExpensiveResults results={results} />
    </>
  )
}
```

**Correct (heavy render uses deferred value):**

```tsx
import { useDeferredValue, useMemo, useState } from 'react'

function SearchPage({ items }: { items: Item[] }) {
  const [query, setQuery] = useState('')
  const deferredQuery = useDeferredValue(query)
  const isStale = query !== deferredQuery

  const results = useMemo(() => {
    return filterItems(items, deferredQuery)
  }, [items, deferredQuery])

  return (
    <>
      <input value={query} onChange={event => setQuery(event.target.value)} />
      <div style={{ opacity: isStale ? 0.6 : 1 }}>
        <ExpensiveResults results={results} />
      </div>
    </>
  )
}
```

Use this for rendering responsiveness, not as a replacement for network debouncing. For server queries or API calls, still debounce, cache, or cancel requests as appropriate.

Reference: [useDeferredValue](https://react.dev/reference/react/useDeferredValue)
