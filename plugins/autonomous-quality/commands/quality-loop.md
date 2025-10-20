**AUTONOMOUS CODE QUALITY LOOP**

You will run a comprehensive autonomous quality improvement cycle until all gates pass.

---

## TERMINATION CONDITION

ONLY stop when:
- ✅ All tests passing
- ✅ No critical/warning code review issues
- ✅ No security vulnerabilities
- ✅ Performance checks passed

**DO NOT** ask user to continue. **KEEP WORKING** until complete.

---

## EXECUTION LOOP

```
MAX_ITERATIONS = 10
iteration = 0

WHILE (quality_gates_failing OR tests_failing) AND iteration < MAX_ITERATIONS:
    PHASE 1: Multi-Agent Review (Parallel)
    PHASE 2: Test Execution
    PHASE 3: Fix Loop
    PHASE 4: Validation
    iteration++

IF all_gates_passed:
    OUTPUT completion message (plain text)
ELSE:
    OUTPUT status + remaining issues
```

---

## PHASE 1: Multi-Agent Code Review

Launch 3 subagents **IN PARALLEL** using Task tool:

```
Use code-reviewer, security-auditor, and performance-analyst subagents in parallel.

For code-reviewer:
  Context: Review all changed files
  Focus: Code quality, patterns, maintainability

For security-auditor:
  Context: Audit for security vulnerabilities
  Focus: OWASP Top 10, authentication, input validation

For performance-analyst:
  Context: Check for performance issues
  Focus: N+1 queries, unnecessary re-renders, bundle size
```

**Wait for all 3 to complete.** Synthesize findings.

---

## PHASE 2: Test Execution

Use test-runner subagent:

```
Use test-runner subagent to execute all tests.

Run the test suite and report:
- Total tests
- Passing tests
- Failing tests
- Error messages and stack traces
```

---

## PHASE 3: Fix Loop (Autonomous)

Prioritize issues:
1. 🔥 **CRITICAL** (security vulnerabilities)
2. ⚠️ **HIGH** (test failures, breaking bugs)
3. 📝 **MEDIUM** (performance issues)
4. 💡 **LOW** (code style, suggestions)

For each issue (highest priority first):

```
IF issue.type == "security":
    Apply fix immediately
    Verify with security-auditor

ELSE IF issue.type == "test_failure":
    Use test-runner to fix
    Re-run specific test

ELSE IF issue.type == "performance":
    Apply fix if low-risk
    Document if complex

ELSE IF issue.type == "code_quality":
    Apply fix if straightforward
```

**Loop control:**
- Max 20 fixes per iteration
- Break if no progress (same issues 2x in a row)

---

## PHASE 4: Final Validation

Run all quality gates:

```bash
# 1. Tests
npm run test

# 2. Linting
npm run lint

# 3. Type check
npm run type-check
```

**IF all pass:** GOTO Completion
**IF any fail:** GOTO Fix Loop

---

## COMPLETION

Output plain text (triggers loop termination):

```
🎉 QUALITY LOOP COMPLETE

✅ All Quality Gates Passed:
  - Code Review: No critical issues
  - Security Audit: No vulnerabilities
  - Performance Check: No major issues
  - Tests: XXX/XXX passing (100%)
  - Linting: Clean
  - Type Check: Clean

📊 Summary:
  - Iterations: X
  - Issues Fixed: Y
  - Tests Fixed: Z
  - Duration: ~MM minutes

📝 Remaining Minor Items:
  - [List any minor suggestions if applicable]

Your code is production-ready!
```

**STOP making tool calls** (loop terminates).

---

## PROGRESS REPORTING

After each iteration:

```
🔄 Quality Loop - Iteration X/10

✅ Phase 1 - Review Complete
  Code: 3 issues | Security: 1 vuln | Performance: 2 optimizations

⚠️ Phase 2 - Testing: 5 tests failing

🔧 Phase 3 - Fixes: 4/6 applied
  ✅ Fixed SQL injection
  ✅ Fixed async test
  ✅ Added useMemo
  ✅ Fixed type error
  ⏳ Working on remaining test failures...

➡️ Continuing to next iteration...
```

---

**Context:** $ARGUMENTS

**BEGIN AUTONOMOUS EXECUTION NOW.**
