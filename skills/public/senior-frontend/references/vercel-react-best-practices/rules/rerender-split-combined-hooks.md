---
title: Split Hooks with Independent Dependencies
impact: MEDIUM
impactDescription: prevents unrelated dependency changes from recomputing or resubscribing
tags: rerender, hooks, dependencies, useMemo, useEffect
---

## Split Hooks with Independent Dependencies

Do not combine unrelated work into one hook when each piece depends on different inputs. Split `useMemo`, `useEffect`, and custom hooks by dependency group so unrelated changes do not trigger expensive recomputation or resubscription.

**Incorrect (one query change recomputes totals):**

```tsx
function Dashboard({ orders, query, currency }: Props) {
  const viewModel = useMemo(() => {
    const filtered = orders.filter(order => order.name.includes(query))
    const totals = calculateTotals(orders, currency)

    return { filtered, totals }
  }, [orders, query, currency])

  return <DashboardView data={viewModel} />
}
```

**Correct (independent dependencies):**

```tsx
function Dashboard({ orders, query, currency }: Props) {
  const filtered = useMemo(() => {
    return orders.filter(order => order.name.includes(query))
  }, [orders, query])

  const totals = useMemo(() => {
    return calculateTotals(orders, currency)
  }, [orders, currency])

  return <DashboardView filtered={filtered} totals={totals} />
}
```

This also applies to effects: one effect for the websocket subscription, another effect for document title updates, another for analytics. Each effect should represent one synchronization process.

Reference: [React effects lifecycle](https://react.dev/learn/lifecycle-of-reactive-effects)
