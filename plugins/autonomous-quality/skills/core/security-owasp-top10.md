---
skill: security-owasp-top10
description: OWASP Top 10 security vulnerabilities and prevention patterns. Always active.
category: core
activation: auto
priority: 10
---

# OWASP Top 10 Security Patterns

## 1. Broken Access Control
- ✅ Always check authentication
- ✅ Always check authorization
- ✅ Never trust client-side access control

## 2. Cryptographic Failures
- ✅ Hash passwords (bcrypt, argon2)
- ✅ Use HTTPS
- ✅ Encrypt sensitive data at rest

## 3. Injection
- ✅ Use parameterized queries (NEVER string concatenation)
- ✅ Validate and sanitize ALL input
- ✅ Use ORM properly

## 4. Insecure Design
- ✅ Implement rate limiting
- ✅ Add CSRF tokens
- ✅ Use secure password reset flows

## 5. Security Misconfiguration
- ✅ Set security headers (CSP, HSTS, X-Frame-Options)
- ✅ Disable debug in production
- ✅ Keep dependencies updated

## 6. Vulnerable Components
- ✅ Run `npm audit` regularly
- ✅ Update dependencies
- ✅ Remove unused dependencies

## 7. Authentication Failures
- ✅ Enforce strong passwords
- ✅ Implement account lockout
- ✅ Use secure session management

## 8. Data Integrity Failures
- ✅ Verify signatures
- ✅ Use integrity checks
- ✅ Validate deserialization

## 9. Logging Failures
- ✅ Log security events
- ✅ Don't log sensitive data
- ✅ Monitor for suspicious activity

## 10. SSRF
- ✅ Validate URLs
- ✅ Whitelist allowed domains
- ✅ Never trust user-provided URLs
