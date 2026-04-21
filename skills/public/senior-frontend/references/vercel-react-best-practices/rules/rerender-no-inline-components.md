---
title: Do Not Define Components Inside Components
impact: MEDIUM
impactDescription: avoids remounts, state loss, and broken memoization
tags: rerender, component-identity, memoization, state
---

## Do Not Define Components Inside Components

Component functions defined inside another component get a new identity on every render. React treats them as different component types, which can remount children, reset state, and defeat memoization.

**Incorrect (Row is recreated every render):**

```tsx
function UserTable({ users }: { users: User[] }) {
  function Row({ user }: { user: User }) {
    const [expanded, setExpanded] = useState(false)

    return (
      <tr onClick={() => setExpanded(value => !value)}>
        <td>{user.name}</td>
        <td>{expanded ? user.email : null}</td>
      </tr>
    )
  }

  return <table>{users.map(user => <Row key={user.id} user={user} />)}</table>
}
```

**Correct (stable component identity):**

```tsx
function UserRow({ user }: { user: User }) {
  const [expanded, setExpanded] = useState(false)

  return (
    <tr onClick={() => setExpanded(value => !value)}>
      <td>{user.name}</td>
      <td>{expanded ? user.email : null}</td>
    </tr>
  )
}

function UserTable({ users }: { users: User[] }) {
  return <table>{users.map(user => <UserRow key={user.id} user={user} />)}</table>
}
```

If the inner component needs parent values, pass them as props. If it is just a small render fragment with no hooks or component identity needs, use a local helper function that returns JSX instead of a component.

Reference: [Preserving and resetting state](https://react.dev/learn/preserving-and-resetting-state)
