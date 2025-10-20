---
skill: drizzle-best-practices
description: Drizzle ORM patterns, query optimization, relationships. Activates when drizzle.config.ts detected or drizzle-orm imports found.
category: database
activation: auto
priority: 8
---

# Drizzle ORM Best Practices

## Query Optimization

### Avoid N+1 Queries
```typescript
// ❌ BAD: N+1 queries
const users = await db.select().from(usersTable)
for (const user of users) {
  user.posts = await db.select().from(postsTable).where(eq(postsTable.userId, user.id))
}

// ✅ GOOD: Single join
const usersWithPosts = await db.select()
  .from(usersTable)
  .leftJoin(postsTable, eq(usersTable.id, postsTable.userId))
```

### Select Only Needed Columns
```typescript
// ❌ BAD: Select all
const users = await db.select().from(usersTable)

// ✅ GOOD: Select specific columns
const users = await db.select({
  id: usersTable.id,
  name: usersTable.name
}).from(usersTable)
```

### Use Prepared Statements
```typescript
// ✅ Prepared statement (faster for repeated queries)
const getUserById = db.select().from(usersTable).where(eq(usersTable.id, sql.placeholder('id'))).prepare()
const user = await getUserById.execute({ id: '123' })
```

## Relationships

```typescript
// Define relations
export const usersRelations = relations(users, ({ many }) => ({
  posts: many(posts)
}))

// Query with relations
const usersWithPosts = await db.query.users.findMany({
  with: { posts: true }
})
```

## Transactions

```typescript
await db.transaction(async (tx) => {
  await tx.insert(users).values({ name: 'John' })
  await tx.insert(posts).values({ title: 'Post' })
  // Both or neither
})
```

## Migrations

- Always use migrations (not push in production)
- Generate: `drizzle-kit generate`
- Apply: `drizzle-kit migrate`
- Never modify generated migrations
