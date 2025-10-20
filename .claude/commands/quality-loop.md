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

Launch 3 **general-purpose** subagents **IN PARALLEL** using Task tool:

### Code Review Subagent
```
subagent_type: "general-purpose"
description: "Code quality review"
prompt: "Review all recently modified files for code quality issues.

Focus on:
- Type safety (avoid any, proper generics)
- Error handling (try-catch, null checks)
- Code organization (SOLID principles, DRY)
- Naming conventions (meaningful names)
- Security patterns (input validation, SQL injection prevention)
- Framework best practices (detect Next.js/React/etc and apply appropriate patterns)

For each issue found:
- Severity: 🔥 Critical / ⚠️ Warning / 💡 Suggestion
- File location (file:line)
- Description and recommendation

Return a structured list of issues."
```

### Security Audit Subagent
```
subagent_type: "general-purpose"
description: "Security audit"
prompt: "Audit the codebase for security vulnerabilities using OWASP Top 10 checklist.

Check for:
1. SQL Injection (raw queries, string concatenation)
2. XSS (unsanitized user input in HTML)
3. Authentication issues (missing checks, weak tokens)
4. Authorization bypasses (inadequate permission checks)
5. Security misconfigurations (exposed secrets, weak crypto)
6. CSRF vulnerabilities (missing tokens in forms)
7. Insecure dependencies (outdated packages with CVEs)

For each vulnerability:
- Severity: 🔥 Critical / ⚠️ Warning
- File location (file:line)
- Vulnerability type
- Recommended fix

Return a structured list of vulnerabilities."
```

### Performance Analysis Subagent
```
subagent_type: "general-purpose"
description: "Performance analysis"
prompt: "Analyze the codebase for performance issues and optimization opportunities.

Check for:
1. Database: N+1 queries, missing indexes, slow queries
2. React: Unnecessary re-renders, missing useMemo/useCallback, large component trees
3. API: Missing caching, synchronous operations
4. Bundle: Large dependencies, unoptimized imports
5. Images/Assets: Unoptimized files, missing lazy loading

For each issue:
- Impact: High / Medium / Low
- File location (file:line)
- Issue description
- Optimization recommendation

Return a structured list of performance issues."
```

**Launch all 3 in parallel.** Wait for all to complete, then synthesize findings.

---

## PHASE 2: Test Execution

Run tests directly (no subagent needed for simple execution):

```bash
# Auto-detect test framework and run
if [ -f "vitest.config.ts" ] || grep -q "vitest" package.json; then
  npm run test
elif [ -f "jest.config.js" ] || grep -q "jest" package.json; then
  npm test
elif [ -f "pytest.ini" ] || [ -f "pyproject.toml" ]; then
  pytest
else
  echo "No test framework detected"
fi
```

**Capture and analyze results:**
- Total tests run
- Passing tests
- Failing tests (with error messages and stack traces)
- Test file locations for failures

---

## PHASE 3: Fix Loop (Autonomous)

Prioritize and fix issues in this order:
1. 🔥 **CRITICAL** - Security vulnerabilities (fix immediately)
2. ⚠️ **HIGH** - Test failures and breaking bugs
3. 📝 **MEDIUM** - Performance issues
4. 💡 **LOW** - Code style and suggestions

**For each issue (highest priority first):**

### Security Vulnerabilities (🔥 Critical)
- Apply fix using Edit tool
- Re-run security checks (grep for patterns)
- Validate fix doesn't break tests

### Test Failures (⚠️ High)
- Read failing test file
- Analyze error message and stack trace
- Determine root cause:
  * Implementation bug → Fix implementation
  * Test outdated → Update test
  * Missing mock/setup → Add proper test setup
- Apply fix using Edit tool
- Re-run specific test file to verify
- If still failing after 3 attempts, document and skip

### Performance Issues (📝 Medium)
- Low-risk fixes (adding useMemo, fixing N+1 queries) → Apply immediately
- Complex optimizations → Document for manual review

### Code Quality (💡 Low)
- Straightforward fixes (type annotations, naming) → Apply
- Architectural changes → Document for manual review

**Loop control:**
- Max 20 fixes per iteration
- Track fixed vs remaining issues
- Break if no progress (same issues repeat 2x)

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
