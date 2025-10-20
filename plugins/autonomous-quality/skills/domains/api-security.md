---
skill: api-security
description: API route security, authentication, input validation, rate limiting. Activates when app/api/, route.ts, or API endpoint files detected.
category: domain
activation: auto
priority: 9
---

# API Security Patterns

## Authentication & Authorization

```typescript
// ✅ Always check auth
export async function POST(req: Request) {
  const session = await auth()
  if (!session) {
    return new Response('Unauthorized', { status: 401 })
  }

  // Check authorization
  if (session.user.role !== 'admin') {
    return new Response('Forbidden', { status: 403 })
  }

  // Process request...
}
```

## Input Validation

```typescript
import { z } from 'zod'

const schema = z.object({
  email: z.string().email(),
  age: z.number().min(0).max(150)
})

export async function POST(req: Request) {
  const body = await req.json()

  // Validate
  const result = schema.safeParse(body)
  if (!result.success) {
    return Response.json({ error: result.error }, { status: 400 })
  }

  const { email, age } = result.data
  // Process validated data...
}
```

## Rate Limiting

```typescript
import { Ratelimit } from '@upstash/ratelimit'

const ratelimit = new Ratelimit({
  redis: redis,
  limiter: Ratelimit.slidingWindow(10, '10 s')
})

export async function POST(req: Request) {
  const ip = req.headers.get('x-forwarded-for') || 'unknown'
  const { success } = await ratelimit.limit(ip)

  if (!success) {
    return new Response('Too many requests', { status: 429 })
  }

  // Process request...
}
```

## Security Headers

```typescript
export async function GET() {
  return Response.json({ data }, {
    headers: {
      'X-Content-Type-Options': 'nosniff',
      'X-Frame-Options': 'DENY',
      'Strict-Transport-Security': 'max-age=31536000',
      'Content-Security-Policy': "default-src 'self'"
    }
  })
}
```

## CORS Security

```typescript
// ✅ Whitelist specific origins
const allowedOrigins = ['https://app.example.com']
const origin = req.headers.get('origin')

if (origin && allowedOrigins.includes(origin)) {
  headers.set('Access-Control-Allow-Origin', origin)
}
```
