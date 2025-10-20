---
skill: code-quality-fundamentals
description: Universal code quality principles - clean code, SOLID, DRY, readable code. Always active.
category: core
activation: auto
priority: 5
---

# Code Quality Fundamentals

## Clean Code Principles

### 1. Meaningful Names
```typescript
// ❌ BAD
const d = 86400000 // What is d?
function calc(a, b) { return a + b * 0.15 } // What does this calculate?

// ✅ GOOD
const MILLISECONDS_PER_DAY = 86400000
function calculateTotalWithTax(subtotal, taxRate) {
  return subtotal + (subtotal * taxRate)
}
```

### 2. Functions Do One Thing
```typescript
// ❌ BAD: Does multiple things
function processUser(user) {
  validateUser(user)
  saveToDatabase(user)
  sendEmail(user)
  logActivity(user)
  return user
}

// ✅ GOOD: Single responsibility
function processNewUser(user) {
  validateUser(user)
  const savedUser = saveUser(user)
  notifyUser(savedUser)
  return savedUser
}
```

### 3. DRY (Don't Repeat Yourself)
```typescript
// ❌ BAD: Repeated logic
const activeUsers = users.filter(u => u.status === 'active' && u.verified === true)
const activePremium = users.filter(u => u.status === 'active' && u.verified === true && u.plan === 'premium')

// ✅ GOOD: Extract common logic
const isActive = (user) => user.status === 'active' && user.verified
const activeUsers = users.filter(isActive)
const activePremium = users.filter(u => isActive(u) && u.plan === 'premium')
```

### 4. Proper Error Handling
```typescript
// ❌ BAD: Silent failures
function getUser(id) {
  try {
    return db.findUser(id)
  } catch (e) {
    return null // Error ignored!
  }
}

// ✅ GOOD: Explicit error handling
function getUser(id) {
  try {
    return db.findUser(id)
  } catch (error) {
    logger.error('Failed to fetch user', { id, error })
    throw new Error(`User ${id} not found`)
  }
}
```

## SOLID Principles

### S - Single Responsibility
Each class/function has ONE reason to change.

### O - Open/Closed
Open for extension, closed for modification.

### L - Liskov Substitution
Subtypes must be substitutable for base types.

### I - Interface Segregation
Many specific interfaces better than one general.

### D - Dependency Inversion
Depend on abstractions, not concretions.

## Code Smells to Avoid

- Long functions (>20 lines)
- Deep nesting (>3 levels)
- Magic numbers
- God classes
- Duplicated code
- Dead code
- Comments explaining bad code (fix the code instead)
