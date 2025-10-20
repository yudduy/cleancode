---
skill: typescript-patterns
description: TypeScript best practices, strict mode, type safety. Activates when .ts or .tsx files detected.
category: language
activation: auto
priority: 8
---

# TypeScript Patterns

## Type Safety

### Never use `any`
```typescript
// ❌ BAD
function process(data: any) { return data.value }

// ✅ GOOD
function process(data: { value: string }) { return data.value }
// or
function process<T extends { value: string }>(data: T) { return data.value }
```

### Proper Generics
```typescript
// ❌ BAD: Not type-safe
function first(arr: any[]) { return arr[0] }

// ✅ GOOD: Preserves type
function first<T>(arr: T[]): T | undefined { return arr[0] }
```

### Union Types
```typescript
// ✅ Use union types for state
type Status = 'idle' | 'loading' | 'success' | 'error' // Not string!

// ✅ Discriminated unions
type Result =
  | { status: 'success'; data: User }
  | { status: 'error'; error: string }
```

## Strict Mode

Enable in `tsconfig.json`:
```json
{
  "strict": true,
  "noUncheckedIndexedAccess": true,
  "noImplicitReturns": true
}
```

## Common Mistakes

### Non-null Assertions
```typescript
// ❌ AVOID: Non-null assertion
const user = users.find(u => u.id === id)!

// ✅ BETTER: Handle null case
const user = users.find(u => u.id === id)
if (!user) throw new Error('User not found')
```

### Type vs Interface
```typescript
// ✅ Use interface for objects (extendable)
interface User {
  id: string
  name: string
}

// ✅ Use type for unions/complex types
type Status = 'active' | 'inactive'
type Result<T> = { data: T } | { error: string }
```
