**AUTONOMOUS TEST FIXING**

Fix all failing tests autonomously until test suite passes.

---

## TERMINATION CONDITION

ONLY stop when:
- ✅ All tests passing
- OR max iterations reached (20)

**DO NOT** ask user to continue. **KEEP WORKING** until done.

---

## EXECUTION LOOP

```
MAX_ITERATIONS = 20
MAX_PER_TEST = 3
iteration = 0

WHILE (tests_failing AND iteration < MAX_ITERATIONS):
    1. Run test suite
    2. Identify failing tests
    3. For each failing test:
       - Analyze failure (up to 3 attempts per test)
       - Apply fix
       - Re-run test
    4. If all pass: BREAK
    iteration++
```

---

## Process

### 1. Run Tests

```bash
npm run test
```

Parse output to identify:
- Failing test names
- Error messages
- Stack traces
- File paths

### 2. Use Test-Runner Agent

```
Use test-runner subagent autonomously.

For each failing test:
1. Read test file
2. Read implementation
3. Analyze error
4. Determine root cause
5. Apply fix (test OR implementation)
6. Re-run to verify

Continue until all tests pass or max iterations reached.
```

### 3. Report Results

```markdown
# Test Fixing Complete

**Total Tests:** 150
**Initially Failing:** 8
**Fixed:** 7
**Remaining:** 1

## Fixed Tests ✅
- auth.spec.ts > login flow (async/await issue)
- api.spec.ts > fetch data (mock setup)
- db.spec.ts > save record (transaction missing)
- validation.spec.ts > validate email (regex fix)
- cache.spec.ts > cache result (cleanup added)
- user.spec.ts > create user (missing await)
- payment.spec.ts > process payment (timeout extended)

## Remaining Issues ⚠️
- webhook.spec.ts > handle webhook
  - Needs external service mock (manual setup required)

## Test Suite Status
✅ 99.3% passing (149/150)
```

**STOP making tool calls when complete.**

---

**Context:** $ARGUMENTS
**BEGIN AUTONOMOUS FIXING NOW.**
