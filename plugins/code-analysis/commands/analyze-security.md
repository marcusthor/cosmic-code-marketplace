---
description: Audit security vulnerabilities and create hardening tickets
argument-hint: [optional: specific security area]
---

# Security Hardening Analysis

I'll audit security across OWASP Top 10 categories and identify vulnerabilities.

## Step 1: Security Audit

Using the **security-hardening** skill to check:
- **Authentication** - Password policies, MFA, session management
- **Authorization** - Access controls, IDOR, privilege escalation
- **Input Validation** - SQL injection, XSS, command injection
- **Data Protection** - Encryption, PII exposure, sensitive data in logs
- **Dependencies** - Known CVEs, outdated packages
- **Configuration** - Security headers, debug mode, CORS
- **Secrets Management** - Hardcoded credentials, secrets in git

Mapping to:
- OWASP Top 10 2021
- CWE references
- WCAG compliance (where applicable)

$ARGUMENTS

## Step 2: Create Security Tickets

After analysis, I'll:
1. Create `improvements/security/` directory
2. Generate individual ticket files for each vulnerability
3. Format: `NNN-short-title.md` (e.g., `001-fix-sql-injection-users.md`)
4. Severity levels: 🔴 Critical, 🟠 High, 🟡 Medium, 🟢 Low
5. Each ticket contains:
   - Vulnerability description
   - CWE/OWASP reference
   - Current risk
   - Attack scenario
   - Remediation steps
   - Secure code example

**Starting security audit...**
