**COMPREHENSIVE SECURITY AUDIT**

Perform deep security analysis using the security-auditor agent.

---

## Scope

Scan entire codebase for:
- OWASP Top 10 vulnerabilities
- Authentication/Authorization issues
- Input validation gaps
- SQL injection vectors
- XSS vulnerabilities
- CSRF vulnerabilities
- Cryptographic failures
- Security misconfigurations
- Framework-specific security issues (Next.js, Web3, etc.)

---

## Process

### 1. Use Security Audit Subagent

```
subagent_type: "general-purpose"
description: "Comprehensive security audit"
prompt: "Perform comprehensive OWASP Top 10 security audit of the entire codebase.

Scan all:
- API routes (app/api/, pages/api/, routes/, endpoints/)
- Database queries (SQL, ORM usage)
- Authentication/authorization code
- User input handling and validation
- Web3/crypto operations (if present)
- Configuration files
- Environment variables usage

For each vulnerability found, provide:
- Severity: 🔥 CRITICAL / ⚠️ HIGH / 📝 MEDIUM / 💡 LOW
- OWASP category (A01-A10)
- File location (file:line)
- Vulnerable code snippet
- Exploit scenario (how it could be exploited)
- Fix/remediation code
- Impact assessment

Focus on:
1. SQL Injection (A03): Raw queries, string concatenation
2. XSS (A03): Unsanitized user input in HTML
3. Broken Access Control (A01): Missing auth checks
4. Authentication Failures (A07): Weak tokens, poor session management
5. Cryptographic Failures (A02): Weak crypto, exposed secrets
6. Security Misconfigurations (A05): Missing headers, default configs
7. CSRF: Missing tokens in forms
8. Insecure Dependencies (A06): Outdated packages with CVEs
9. Logging Failures (A09): Insufficient security logging
10. SSRF (A10): Unvalidated external requests

Return a comprehensive security report with all findings categorized by severity."
```

### 2. Run Automated Scans

```bash
# Dependency vulnerabilities
npm audit

# Find potential SQL injection
rg "execute.*\\$|query.*\\+" --type ts

# Find hardcoded secrets
rg "password.*=|api_key.*=|secret.*=" --type ts

# Find XSS risks
rg "dangerouslySetInnerHTML|innerHTML" --type tsx
```

---

## Output Format

```markdown
# Security Audit Report

**Date:** [Date]
**Files Scanned:** X
**Vulnerabilities Found:** Y

---

## 🔥 CRITICAL (Fix Immediately)

### SQL Injection in User Endpoint
**Severity:** CRITICAL
**OWASP:** A03:2021 – Injection
**File:** `app/api/users/route.ts:23`

**Vulnerable Code:**
```typescript
const users = await db.execute(`SELECT * FROM users WHERE id = ${userId}`)
```

**Exploit Scenario:**
```
userId = "1 OR 1=1; DROP TABLE users;--"
```

**Fix:**
```typescript
const users = await db.select().from(usersTable).where(eq(usersTable.id, userId))
```

**Impact:** Complete database compromise possible

---

## ⚠️ HIGH

[List high severity issues]

---

## 📝 MEDIUM

[List medium severity issues]

---

## 💡 LOW

[List low severity issues]

---

## Compliance Checklist

- [x] A01: Broken Access Control
- [x] A02: Cryptographic Failures
- [ ] A03: Injection ❌ 2 SQL injection vulnerabilities found
- [x] A04: Insecure Design
- [ ] A05: Security Misconfiguration ⚠️ Missing security headers
- [x] A06: Vulnerable Components
- [x] A07: Identification and Authentication Failures
- [x] A08: Software and Data Integrity Failures
- [ ] A09: Logging Failures ⚠️ Insufficient logging
- [x] A10: Server-Side Request Forgery

**OWASP Top 10 Score:** 7/10 ✅

---

## Recommendations

1. **Immediate:** Fix SQL injection vulnerabilities
2. **High Priority:** Add security headers middleware
3. **Medium Priority:** Improve security logging
4. **Nice to Have:** Implement rate limiting on all endpoints

**Overall Security Posture:** Good, with 2 critical issues to address
```

---

**Context:** $ARGUMENTS
