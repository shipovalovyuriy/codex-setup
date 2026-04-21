---
title: Use flatMap to Map and Filter in One Pass
impact: LOW-MEDIUM
impactDescription: reduces intermediate arrays in hot paths
tags: javascript, arrays, flatMap, filtering, iteration
---

## Use flatMap to Map and Filter in One Pass

When code maps values and then filters out empty results, `flatMap()` can combine both operations into one pass and avoid an intermediate array. Use this only when the `flatMap()` version stays readable; for complex logic, a plain loop is often clearer.

**Incorrect (two passes and intermediate array):**

```typescript
const visibleLinks = items
  .map(item => item.url ? { href: item.url, label: item.title } : null)
  .filter((link): link is Link => link !== null)
```

**Correct (one pass):**

```typescript
const visibleLinks = items.flatMap(item => {
  if (!item.url) return []

  return [{ href: item.url, label: item.title }]
})
```

**Alternative for hot paths with more logic:**

```typescript
const visibleLinks: Link[] = []

for (const item of items) {
  if (!item.url) continue
  visibleLinks.push({ href: item.url, label: item.title })
}
```

Do not replace simple, readable code unless this path is hot or the intermediate arrays are large.
