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

### 1. Use Security-Auditor Agent

```
Use security-auditor subagent for comprehensive audit.

Scan:
- All API routes
- All database queries
- All authentication code
- All user input handling
- All Web3/crypto code
- Configuration files
- Environment variables usage

Return detailed security report with:
- Vulnerabilities categorized by severity
- OWASP mapping
- Code examples
- Remediation steps
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
