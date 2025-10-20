**PULL REQUEST REVIEW**

Perform comprehensive code review of a pull request using multi-agent analysis.

---

## Process

### 1. Fetch PR Changes

```bash
# Get PR diff (replace NUMBER with PR number from arguments)
gh pr diff $ARGUMENTS

# Or get current branch diff
git diff main...HEAD
```

### 2. Launch Parallel Review Agents

Use Task tool to launch 3 **general-purpose** subagents **IN PARALLEL**:

**Code Review Agent:**
```
subagent_type: "general-purpose"
description: "PR code quality review"
prompt: "Review the PR diff for code quality issues. Focus ONLY on changed lines.

Analyze for:
- Type safety (any usage, proper generics)
- Error handling (missing try-catch, null checks)
- Code organization (SOLID, DRY violations)
- Naming and clarity
- Framework best practices

Return findings as:
🔥 Critical | ⚠️ Warning | 💡 Suggestion
[File:line] Description + Recommendation"
```

**Security Agent:**
```
subagent_type: "general-purpose"
description: "PR security review"
prompt: "Review the PR diff for security vulnerabilities. Focus ONLY on changed lines.

Check for:
- SQL injection (raw queries, string concat)
- XSS (unsanitized input)
- Auth/authz issues
- Exposed secrets
- CSRF risks

Return findings as:
🔥 Critical | ⚠️ High
[File:line] Vulnerability type + Fix"
```

**Performance Agent:**
```
subagent_type: "general-purpose"
description: "PR performance review"
prompt: "Review the PR diff for performance issues. Focus ONLY on changed lines.

Check for:
- N+1 queries
- Missing React optimizations (useMemo, useCallback)
- Large bundle additions
- Inefficient algorithms

Return findings as:
High | Medium | Low Impact
[File:line] Issue + Optimization"
```

**Launch all 3 in parallel, wait for results.**

### 3. Synthesize Findings

Combine results from all 3 agents into unified review.

---

## Output Format

```markdown
# Pull Request Review

**PR:** #$ARGUMENTS
**Files Changed:** X
**Lines Added:** +Y
**Lines Removed:** -Z

---

## 🔥 Critical Issues (Must Fix)

### SQL Injection in User Endpoint
**File:** `app/api/users/route.ts:23`
**Agent:** security-auditor
**Severity:** CRITICAL

**Issue:**
Unsafe string interpolation in database query

**Code:**
```typescript
const users = await db.execute(`SELECT * FROM users WHERE id = ${userId}`)
```

**Recommendation:**
```typescript
const users = await db.select().from(usersTable).where(eq(usersTable.id, userId))
```

---

## ⚠️ Warnings (Should Fix)

[List warnings from all agents]

---

## 💡 Suggestions (Nice to Have)

[List suggestions from all agents]

---

## Summary

**Approve:** ❌ (has critical issues)
**Request Changes:** ✅

**Action Items:**
1. Fix SQL injection vulnerability
2. Address performance concerns
3. Add missing tests

**Overall Quality:** Good foundation, needs security fixes before merge.
```

---

**Context:** PR number or branch name from $ARGUMENTS
