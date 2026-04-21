---
title: Use defer or async on Script Tags
impact: MEDIUM
impactDescription: prevents scripts from blocking HTML parsing and rendering
tags: rendering, scripts, defer, async, nextjs
---

## Use defer or async on Script Tags

Plain `<script src="...">` blocks HTML parsing while the browser downloads and executes the script. Use `defer` for scripts that need DOM order after parsing, `async` for independent scripts, or `next/script` strategies in Next.js.

**Incorrect (parser-blocking script):**

```tsx
export function DocumentHead() {
  return <script src="https://example.com/widget.js" />
}
```

**Correct for ordered scripts:**

```tsx
export function DocumentHead() {
  return <script src="https://example.com/widget.js" defer />
}
```

**Correct for independent scripts:**

```tsx
export function AnalyticsScript() {
  return <script src="https://analytics.example.com/sdk.js" async />
}
```

**Correct in Next.js:**

```tsx
import Script from 'next/script'

export function AnalyticsScript() {
  return (
    <Script
      src="https://analytics.example.com/sdk.js"
      strategy="afterInteractive"
    />
  )
}
```

Do not use `async` for scripts that depend on execution order. For analytics, logging, chat widgets, and other non-critical third-party code, prefer delayed loading patterns from `bundle-defer-third-party`.

Reference: [Next.js Script component](https://nextjs.org/docs/app/api-reference/components/script)
