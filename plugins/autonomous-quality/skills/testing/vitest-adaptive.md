---
skill: vitest-patterns
description: Vitest testing patterns, mocking, async tests. Activates when vitest.config.ts detected or vitest imports found.
category: testing
activation: auto
priority: 8
---

# Vitest Patterns

## Test Structure

```typescript
import { describe, it, expect, beforeEach, afterEach, vi } from 'vitest'

describe('UserService', () => {
  beforeEach(() => {
    // Setup before each test
  })

  afterEach(() => {
    vi.restoreAllMocks() // Always cleanup!
  })

  it('should create user', async () => {
    const user = await createUser({ name: 'John' })
    expect(user).toMatchObject({ name: 'John' })
  })
})
```

## Mocking

```typescript
// Mock module
vi.mock('./api', () => ({
  fetchData: vi.fn(() => Promise.resolve({ id: 1 }))
}))

// Mock function
const mockFn = vi.fn()
mockFn.mockReturnValue('value')
mockFn.mockResolvedValue('async value')

// Spy on function
const spy = vi.spyOn(object, 'method')
expect(spy).toHaveBeenCalledWith('arg')
```

## Async Tests

```typescript
// ✅ Proper async test
it('should fetch data', async () => {
  const data = await fetchData()
  expect(data).toBeDefined()
})

// ✅ Test async errors
it('should handle errors', async () => {
  await expect(failingFunction()).rejects.toThrow('Error message')
})
```

## Best Practices

- Descriptive test names
- One assertion per test (when possible)
- Always cleanup mocks (`vi.restoreAllMocks()`)
- No `.only` or `.skip` in committed code
- Test edge cases and errors
