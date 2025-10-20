---
skill: react-patterns
description: React 19 patterns, hooks best practices, performance optimization. Activates when .tsx files or React imports detected.
category: framework
activation: auto
priority: 8
---

# React 19 Patterns

## Hooks Best Practices

### useState
```typescript
// ❌ BAD: Derived state
const [users, setUsers] = useState([])
const [activeUsers, setActiveUsers] = useState([])

// ✅ GOOD: Compute from source
const [users, setUsers] = useState([])
const activeUsers = useMemo(() => users.filter(u => u.active), [users])
```

### useEffect
```typescript
// ❌ BAD: Missing dependencies
useEffect(() => {
  fetchData(userId) // userId not in deps!
}, [])

// ✅ GOOD: Complete dependencies
useEffect(() => {
  fetchData(userId)
}, [userId])

// ❌ BAD: Effect cleanup missing
useEffect(() => {
  const interval = setInterval(() => tick(), 1000)
}, [])

// ✅ GOOD: Cleanup
useEffect(() => {
  const interval = setInterval(() => tick(), 1000)
  return () => clearInterval(interval) // Cleanup!
}, [])
```

### useMemo & useCallback
```typescript
// ❌ BAD: New object every render
function Parent() {
  return <Child config={{ theme: 'dark' }} /> // New object!
}

// ✅ GOOD: Memoized
function Parent() {
  const config = useMemo(() => ({ theme: 'dark' }), [])
  return <Child config={config} />
}

// ❌ BAD: New function every render
function Parent() {
  return <Child onClick={() => console.log('hi')} />
}

// ✅ GOOD: useCallback
function Parent() {
  const handleClick = useCallback(() => console.log('hi'), [])
  return <Child onClick={handleClick} />
}
```

## Performance Optimization

### Avoid Unnecessary Re-renders
```typescript
// ✅ React.memo for expensive components
const HeavyComponent = React.memo(function Heavy({ data }) {
  return <ExpensiveRender data={data} />
})

// ✅ Split components to isolate state
function Dashboard() {
  return (
    <>
      <HeavyStaticPart /> {/* Won't re-render */}
      <CounterWidget /> {/* Only this re-renders on count change */}
    </>
  )
}
```

### List Keys
```typescript
// ❌ BAD: Index as key
{users.map((user, i) => <User key={i} data={user} />)}

// ✅ GOOD: Stable unique key
{users.map(user => <User key={user.id} data={user} />)}
```

## Common Mistakes

### Stale Closures
```typescript
// ❌ BAD: Stale closure
const [count, setCount] = useState(0)
useEffect(() => {
  setInterval(() => {
    setCount(count + 1) // count always 0!
  }, 1000)
}, [])

// ✅ GOOD: Callback form
useEffect(() => {
  setInterval(() => {
    setCount(c => c + 1) // Uses current value
  }, 1000)
}, [])
```
