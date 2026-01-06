---
name: security-hardening
description: Identify security vulnerabilities, audit dependencies, and recommend hardening measures. Use when conducting security reviews or preparing for production deployment.
---

# Security Hardening Analysis

Analyze codebases to identify security vulnerabilities, risks, and hardening opportunities across authentication, authorization, input validation, data protection, dependencies, configuration, and secrets management.

**Your Role**: Senior application security engineer focused on exploitable vulnerabilities and actionable remediation.

## Workflow

### Phase 1: Dependency Security Audit

**Read package manifest**:
- `package.json`, `requirements.txt`, `go.mod`, `Cargo.toml`

**Check for known vulnerabilities** (if possible):
- Document outdated/unmaintained packages
- Note packages with known CVEs
- Identify supply chain risks

**Search for risky dependencies**:
- Packages with few maintainers
- Recently transferred ownership
- Packages with security advisories

### Phase 2: Code Pattern Analysis - Dangerous Functions

**Search for injection risks** using Grep:

**Command Injection**:
```
exec\(
system\(
spawn\(
eval\(
```

**SQL Injection**:
```
\.query\(.*\$
\.execute\(.*\+
SELECT.*\$\{
```

**XSS/HTML Injection**:
```
innerHTML.*=
eval\(
```

**Path Traversal**:
```
readFile\(.*\$
\.\.\/
path\.join\(.*req\.
```

### Phase 3: Authentication & Authorization Review

**Check auth patterns** using Grep:

**Password handling**:
```
password
bcrypt
hash
salt
```

**Session management**:
```
session
token
jwt
cookie
```

**Authorization checks**:
```
hasPermission
canAccess
authorize
role
```

**Look for**:
- Missing password complexity requirements
- Weak hashing algorithms (MD5, SHA1)
- Hardcoded secrets
- Missing auth checks
- IDOR vulnerabilities

### Phase 4: Input Validation & Sanitization

**Find user input handling**:
```
req\.body
req\.query
req\.params
request\.form
input
```

**Check validation patterns**:
- Whitelisting vs blacklisting
- Type validation
- Length limits
- Regex validation
- Sanitization before use

### Phase 5: Data Protection Review

**Search for sensitive data**:
```
password
token
api_key
secret
credit_card
ssn
email
```

**Check for**:
- Sensitive data in logs
- Missing encryption at rest
- Weak encryption algorithms
- PII without protection
- Plaintext storage

### Phase 6: Configuration Security

**Review config files**:
- `.env` files
- Config with defaults
- CORS settings
- Security headers
- Cookie attributes

**Check for**:
- Debug mode in production
- Verbose error messages
- Missing security headers (CSP, X-Frame-Options, HSTS)
- Insecure CORS (allow *)
- Missing httpOnly/secure flags on cookies

### Phase 7: Secrets Management

**Detect hardcoded secrets** using Grep:
```
API_KEY.*=
password.*=
token.*=
secret.*=
aws_access_key
```

**Check for**:
- Secrets in source code
- `.env` files in git
- API keys in client-side code
- Missing secret rotation
- Secrets in error messages/logs

### Phase 8: Generate Markdown Report

Create comprehensive security report: `security_hardening_[project-name]_[date].md`

## Report Structure

```markdown
# Security Hardening Report: [Project Name]

**Generated:** [Date/Time]
**Files Scanned:** [Count]
**Vulnerabilities Found:** [Count by severity]

---

## Executive Summary

[2-3 paragraph overview of security posture]

**Security Health:**
- 🔴 Critical: [count] (immediate exploitation risk)
- 🟠 High: [count] (significant risk)
- 🟡 Medium: [count] (moderate risk)
- 🟢 Low: [count] (best practice improvements)

**Top 3 Critical Risks:**
1. [Most severe vulnerability]
2. [Second most severe]
3. [Third most severe]

---

## Critical Vulnerabilities 🔴

### Vulnerability 1: [Title]
- **Category:** [authentication/authorization/input_validation/data_protection/dependencies/configuration/secrets_management]
- **Severity:** 🔴 Critical
- **CWE:** [CWE-89: SQL Injection]
- **OWASP:** [A03:2021 - Injection]

**Affected Files:**
- `src/api/users.ts:45` - searchUsers() function
- `src/db/queries.ts:89` - buildQuery() function

**Vulnerability Description:**
[Clear description of the security issue]

**Current Risk:**
[Attack scenario - what can an attacker do?]

**Vulnerable Code:**
```typescript
// VULNERABLE: SQL Injection
function searchUsers(searchTerm: string) {
  const query = `SELECT * FROM users WHERE name LIKE '%${searchTerm}%'`;
  return db.query(query);
}
```

**Remediation:**
```typescript
// SECURE: Parameterized query
function searchUsers(searchTerm: string) {
  const query = 'SELECT * FROM users WHERE name LIKE $1';
  return db.query(query, [`%${searchTerm}%`]);
}
```

**Implementation Steps:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Verification:**
```bash
# Test with injection payload
curl "https://api.example.com/users?search=' OR '1'='1"
# Should NOT return all users
```

**References:**
- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [CWE-89](https://cwe.mitre.org/data/definitions/89.html)

**Compliance Impact:**
- SOC 2 Type II: Access Control
- PCI-DSS: Requirement 6.5.1

---

## High Vulnerabilities 🟠

[Same structure]

---

## Medium Vulnerabilities 🟡

[Same structure]

---

## Low Priority 🟢

[Same structure]

---

## Category Breakdown

| Category | Critical | High | Medium | Low | Total |
|----------|----------|------|--------|-----|-------|
| Authentication | 1 | 2 | 1 | 0 | 4 |
| Authorization | 2 | 3 | 2 | 1 | 8 |
| Input Validation | 3 | 5 | 4 | 2 | 14 |
| Data Protection | 0 | 2 | 3 | 1 | 6 |
| Dependencies | 0 | 1 | 5 | 3 | 9 |
| Configuration | 0 | 1 | 2 | 4 | 7 |
| Secrets Management | 1 | 0 | 1 | 0 | 2 |

---

## OWASP Top 10 Mapping

| OWASP Category | Issues Found | Severity |
|----------------|--------------|----------|
| A01 Broken Access Control | 5 | 🔴🟠 |
| A02 Cryptographic Failures | 2 | 🟡 |
| A03 Injection | 8 | 🔴🔴🟠 |
| A07 Auth Failures | 3 | 🟠🟡 |
| A09 Logging Failures | 1 | 🟢 |

---

## Dependency Vulnerabilities

| Package | Version | CVE | Severity | Fix Available |
|---------|---------|-----|----------|---------------|
| axios | 0.21.1 | CVE-2021-3749 | High | 0.21.2+ |
| lodash | 4.17.20 | CVE-2021-23337 | Medium | 4.17.21+ |

**Recommendation:**
```bash
npm update axios lodash
# or
npm audit fix
```

---

## Quick Security Wins

High-impact, low-effort improvements:

1. **Add security headers** (15 minutes)
   ```javascript
   app.use(helmet({
     contentSecurityPolicy: true,
     hsts: true,
     frameguard: { action: 'deny' }
   }));
   ```

2. **Update vulnerable dependencies** (30 minutes)
   ```bash
   npm audit fix
   ```

3. **Add rate limiting** (30 minutes)
   ```javascript
   import rateLimit from 'express-rate-limit';
   app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }));
   ```

4. **Enable HTTPS-only cookies** (5 minutes)
   ```javascript
   app.use(session({
     cookie: { secure: true, httpOnly: true, sameSite: 'strict' }
   }));
   ```

---

## Security Hardening Roadmap

### Phase 1: Critical Fixes (Immediate)
- [ ] Fix SQL injection vulnerabilities
- [ ] Patch authentication bypass
- [ ] Remove hardcoded secrets

### Phase 2: High Priority (Week 1)
- [ ] Update vulnerable dependencies
- [ ] Add missing authorization checks
- [ ] Implement input validation

### Phase 3: Medium Priority (Weeks 2-3)
- [ ] Add security headers
- [ ] Implement rate limiting
- [ ] Encrypt sensitive data at rest

### Phase 4: Best Practices (Ongoing)
- [ ] Regular dependency audits
- [ ] Security code reviews
- [ ] Penetration testing

---

## Compliance Checklist

**SOC 2 Type II:**
- [ ] Access control implemented
- [ ] Encryption at rest and in transit
- [ ] Audit logging enabled

**PCI-DSS:**
- [ ] Strong password policy
- [ ] Data encryption
- [ ] Regular security testing

**GDPR:**
- [ ] PII protection
- [ ] Data retention policies
- [ ] Right to erasure support

---

## Next Steps

1. **Immediate Action** - Fix critical vulnerabilities
2. **Update Dependencies** - Patch known CVEs
3. **Security Review** - Review with security team
4. **Penetration Test** - Verify fixes with testing
5. **Continuous Monitoring** - Set up security scanning in CI/CD

---

## Analysis Metadata

**Analysis Date:** [ISO timestamp]
**Files Scanned:** [Count]
**Dependencies Scanned:** [Count]
**Total Vulnerabilities:** [Count]
**Critical:** [X] | **High:** [X] | **Medium:** [X] | **Low:** [X]
**Generated By:** Claude Code - Security Hardening Skill
```

## Critical Rules

1. **Prioritize Exploitability** - Focus on exploitable issues, not theoretical
2. **Provide Clear Remediation** - Each finding includes how to fix it
3. **Reference Standards** - Link to OWASP, CWE, CVE
4. **Consider Context** - Dev tools differ from production code
5. **Avoid False Positives** - Verify patterns before flagging
6. **Map to Compliance** - Note SOC2, PCI-DSS, GDPR implications

## Quality Checklist

Before finalizing report:
- [ ] All critical issues have CWE references
- [ ] Remediation steps are clear and tested
- [ ] Attack scenarios are described
- [ ] Code examples show vulnerable vs secure
- [ ] OWASP Top 10 mapping is complete
- [ ] Compliance implications are noted
- [ ] Quick wins are identified

---

For detailed security patterns and OWASP mapping, see [security-patterns.md](security-patterns.md).
