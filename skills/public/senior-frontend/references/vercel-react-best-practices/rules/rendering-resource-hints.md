---
title: Use Resource Hints for Critical Assets
impact: MEDIUM
impactDescription: improves perceived performance by starting critical downloads earlier
tags: rendering, resource-hints, preload, preconnect, react-dom, nextjs
---

## Use Resource Hints for Critical Assets

Use resource hints for assets and origins that are needed soon but cannot be discovered early enough by the browser. In React, prefer React DOM resource hint APIs where supported; in Next.js, prefer framework primitives such as `next/font`, `next/image`, and metadata when they cover the case.

**Incorrect (browser discovers external origin late):**

```tsx
function ProductHero() {
  return <img src="https://cdn.example.com/products/hero.webp" alt="" />
}
```

**Correct (preconnect and preload intentional critical resources):**

```tsx
import { preconnect, preload } from 'react-dom'

function ProductHero() {
  preconnect('https://cdn.example.com')
  preload('https://cdn.example.com/products/hero.webp', {
    as: 'image',
    fetchPriority: 'high',
  })

  return <img src="https://cdn.example.com/products/hero.webp" alt="" />
}
```

Use resource hints sparingly. Preloading too many assets competes with truly critical work and can make performance worse.

Reference: [React DOM resource preloading APIs](https://react.dev/reference/react-dom/preload)
