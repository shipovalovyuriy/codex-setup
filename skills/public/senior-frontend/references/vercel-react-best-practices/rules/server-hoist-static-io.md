---
title: Hoist Static I/O to Module Scope
impact: MEDIUM-HIGH
impactDescription: avoids repeated static reads during server rendering
tags: server, io, static-assets, module-scope, nextjs
---

## Hoist Static I/O to Module Scope

Static I/O that does not depend on request data should be initialized at module scope, not inside Server Components, Server Actions, or route handlers. This avoids repeating file reads, font setup, logo loading, or static config parsing on every request.

**Incorrect (reads the same static file per request):**

```tsx
import { readFile } from 'node:fs/promises'

export default async function Layout({ children }: { children: React.ReactNode }) {
  const logoSvg = await readFile('public/logo.svg', 'utf8')

  return (
    <html>
      <body>
        <Header logoSvg={logoSvg} />
        {children}
      </body>
    </html>
  )
}
```

**Correct (starts static I/O once per module instance):**

```tsx
import { readFile } from 'node:fs/promises'

const logoSvgPromise = readFile('public/logo.svg', 'utf8')

export default async function Layout({ children }: { children: React.ReactNode }) {
  const logoSvg = await logoSvgPromise

  return (
    <html>
      <body>
        <Header logoSvg={logoSvg} />
        {children}
      </body>
    </html>
  )
}
```

**Correct for Next.js fonts:**

```tsx
import { Inter } from 'next/font/google'

const inter = Inter({ subsets: ['latin'] })

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

Only hoist request-independent work. Do not hoist values derived from `headers()`, `cookies()`, session state, authorization, locale, AB test buckets, or user-specific configuration.
