---
skill: nextjs-patterns
description: Next.js 15 patterns and best practices. Activates when app/ directory, page.tsx, layout.tsx, route.ts detected, or package.json contains "next".
category: framework
activation: auto
priority: 9
---

# Next.js 15 Patterns

## Server vs Client Components

### Server Components (Default)
```typescript
// app/page.tsx - Server Component by default
export default async function Page() {
  const data = await db.query.users.findMany() // Direct DB access ✅
  return <UserList users={data} />
}
```

**Rules:**
- ✅ Can be async
- ✅ Direct database access
- ✅ Access to secrets
- ❌ NO hooks (useState, useEffect, etc.)
- ❌ NO event handlers (onClick, etc.)
- ❌ NO browser APIs

### Client Components
```typescript
'use client' // Required at top!

import { useState } from 'react'

export function Counter() {
  const [count, setCount] = useState(0) // Hooks allowed ✅
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>
}
```

**Rules:**
- ✅ Can use hooks
- ✅ Can use event handlers
- ✅ Can use browser APIs
- ❌ CANNOT be async
- ❌ CANNOT import Server Components

## Server Actions

```typescript
'use server'

import { auth } from '@/lib/auth'
import { z } from 'zod'

const schema = z.object({ name: z.string().min(1) })

export async function updateUser(formData: FormData) {
  // 1. Check auth
  const session = await auth()
  if (!session) throw new Error('Unauthorized')

  // 2. Validate input
  const { name } = schema.parse({
    name: formData.get('name')
  })

  // 3. Database operation
  await db.update(users).set({ name })

  // 4. Revalidate
  revalidatePath('/dashboard')

  return { success: true }
}
```

**Security Checklist:**
- ✅ Always validate input (use Zod)
- ✅ Always check authentication
- ✅ Always revalidate after mutations
- ❌ Never trust client data

## Common Mistakes

### ❌ Wrong: Client Component importing Server Component
```typescript
'use client'
import ServerComponent from './server-component' // Error!
```

### ✅ Right: Composition
```typescript
// app/page.tsx (Server Component)
import ClientComponent from './client-component'

export default async function Page() {
  const data = await fetchData()
  return <ClientComponent data={data} /> // Pass data as props
}
```

### ❌ Wrong: Server Action without validation
```typescript
'use server'
export async function deleteUser(id: string) {
  await db.delete(users).where(eq(users.id, id)) // No auth! No validation!
}
```

### ✅ Right: Secure Server Action
```typescript
'use server'
export async function deleteUser(id: string) {
  const session = await auth()
  if (!session) throw new Error('Unauthorized')

  const validId = z.string().uuid().parse(id)

  await db.delete(users).where(eq(users.id, validId))
  revalidatePath('/users')
}
```
