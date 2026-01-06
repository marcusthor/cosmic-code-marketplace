# Security Patterns Reference - OWASP Top 10 Mapping

Detection patterns and remediation strategies for OWASP Top 10 2021 security vulnerabilities.

## OWASP Top 10 Overview

| Rank | Category | CWE References | Severity Range |
|------|----------|----------------|----------------|
| A01 | Broken Access Control | CWE-200, CWE-352 | Critical-High |
| A02 | Cryptographic Failures | CWE-259, CWE-327 | Critical-High |
| A03 | Injection | CWE-89, CWE-77, CWE-79 | Critical |
| A04 | Insecure Design | CWE-209, CWE-256 | High-Medium |
| A05 | Security Misconfiguration | CWE-16, CWE-2 | High-Medium |
| A06 | Vulnerable Components | CWE-1104 | High-Medium |
| A07 | Authentication Failures | CWE-287, CWE-798 | Critical |
| A08 | Data Integrity Failures | CWE-502 | High-Medium |
| A09 | Logging Failures | CWE-778 | Medium-Low |
| A10 | SSRF | CWE-918 | High |

---

## A01: Broken Access Control

### Detection Patterns

Search for authorization middleware:
```
hasPermission
canAccess
authorize
requireAuth
isOwner
checkRole
```

### Common Vulnerabilities

**IDOR (Insecure Direct Object Reference)**:
- User can access resources by changing IDs
- Missing ownership verification
- Client-side only access control

**Missing Authorization**:
- API endpoints without middleware
- DELETE/UPDATE without checks
- Admin features accessible by regular users

### Remediation Strategy

**Always verify authorization**:
```typescript
async function getDocument(userId: number, docId: number) {
  const doc = await db.documents.findOne({ id: docId });
  if (!doc) throw new NotFoundError();

  // Verify ownership
  if (doc.ownerId !== userId) {
    throw new ForbiddenError('Access denied');
  }

  return doc;
}
```

**Implement RBAC**:
```typescript
function requirePermission(permission: string) {
  return async (req, res, next) => {
    if (!req.user.hasPermission(permission)) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    next();
  };
}

app.delete('/api/users/:id', requirePermission('user.delete'), handler);
```

---

## A02: Cryptographic Failures

### Detection Patterns

Search for weak algorithms:
```
md5
sha1
base64  # When used for security
```

Search for sensitive data:
```
password
credit_card
ssn
token
api_key
secret
```

### Common Vulnerabilities

**Weak Password Hashing**:
- Using MD5 or SHA1
- No salt
- Fast algorithms (SHA256 for passwords)

**Missing Encryption**:
- Sensitive data stored in plaintext
- Credentials in configuration
- PII without protection

### Remediation Strategy

**Strong Password Hashing**:
```typescript
import bcrypt from 'bcrypt';

const SALT_ROUNDS = 10;

async function hashPassword(password: string): Promise<string> {
  return bcrypt.hash(password, SALT_ROUNDS);
}

async function verifyPassword(
  password: string,
  hash: string
): Promise<boolean> {
  return bcrypt.compare(password, hash);
}
```

**Encrypt Sensitive Data**:
```typescript
import crypto from 'crypto';

const ALGORITHM = 'aes-256-gcm';
const KEY = Buffer.from(process.env.ENCRYPTION_KEY!, 'hex');

function encrypt(plaintext: string): string {
  const iv = crypto.randomBytes(16);
  const cipher = crypto.createCipheriv(ALGORITHM, KEY, iv);

  const encrypted = Buffer.concat([
    cipher.update(plaintext, 'utf8'),
    cipher.final()
  ]);

  const tag = cipher.getAuthTag();

  return JSON.stringify({
    iv: iv.toString('hex'),
    data: encrypted.toString('hex'),
    tag: tag.toString('hex')
  });
}
```

---

## A03: Injection

### Detection Patterns

**SQL Injection**:
```
\.query\(.*\$\{
SELECT.*\+
WHERE.*concat
\.raw\(
```

**Command Injection**:
```
child_process
shell.*true
```

**XSS**:
```
\.html\(
\.innerHTML
```

### Remediation Strategy

**SQL Injection Prevention**:
```typescript
// Use parameterized queries
async function searchUsers(searchTerm: string) {
  const query = 'SELECT * FROM users WHERE name LIKE $1';
  const params = [`%${searchTerm}%`];
  return db.query(query, params);
}

// Use ORM with safe methods
async function getUserById(userId: number) {
  return db.users.findOne({
    where: { id: userId }
  });
}
```

**Command Injection Prevention**:
```typescript
import { spawn } from 'child_process';

async function runSafeCommand(userArg: string) {
  // Validate input
  if (!/^[a-zA-Z0-9_-]+$/.test(userArg)) {
    throw new Error('Invalid input');
  }

  // Use argument array without shell
  const process = spawn('command', [userArg], {
    shell: false
  });
}
```

**XSS Prevention**:
```typescript
// Use textContent for plain text
element.textContent = userInput;

// Use DOM methods
const div = document.createElement('div');
div.textContent = userInput;
parent.appendChild(div);

// Sanitize if HTML is needed
import DOMPurify from 'dompurify';
const clean = DOMPurify.sanitize(userHTML, {
  ALLOWED_TAGS: ['b', 'i', 'em', 'strong'],
  ALLOWED_ATTR: []
});
```

---

## A04: Insecure Design

### Detection Patterns

Search for missing protections:
```
rate.*limit
throttle
account.*lock
```

### Common Vulnerabilities

- Missing rate limiting on auth endpoints
- No account lockout after failed attempts
- Predictable session IDs
- Insufficient entropy in tokens

### Remediation Strategy

**Rate Limiting**:
```typescript
import rateLimit from 'express-rate-limit';

const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15 minutes
  max: 5,                     // 5 attempts
  skipSuccessfulRequests: true,
  message: 'Too many attempts'
});

app.post('/api/login', authLimiter, loginHandler);
```

**Secure Random Generation**:
```typescript
import crypto from 'crypto';

function generateSessionId(): string {
  return crypto.randomBytes(32).toString('hex');
}

function generateToken(): string {
  return crypto.randomBytes(48).toString('base64url');
}
```

---

## A05: Security Misconfiguration

### Detection Patterns

Search for missing headers:
```
Content-Security-Policy
Strict-Transport-Security
X-Frame-Options
X-Content-Type-Options
```

Search for debug mode:
```
DEBUG=
NODE_ENV=development
printStackTrace
```

### Remediation Strategy

**Security Headers**:
```typescript
import helmet from 'helmet';

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      styleSrc: ["'self'"],
      objectSrc: ["'none'"],
      frameSrc: ["'none'"],
    },
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true,
  },
  frameguard: { action: 'deny' },
}));
```

**Secure Cookies**:
```typescript
res.cookie('session', sessionId, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict',
  maxAge: 3600000,
  domain: '.example.com',
});
```

**CORS Configuration**:
```typescript
import cors from 'cors';

app.use(cors({
  origin: ['https://app.example.com'],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
}));
```

---

## A06: Vulnerable Components

### Detection Strategy

**Check dependencies**:
```bash
npm audit
npm outdated
yarn audit
```

**Search package.json for**:
- Known CVE packages
- Outdated versions
- Unmaintained libraries

### Remediation Strategy

```bash
# Regular auditing
npm audit

# Automatic fixes
npm audit fix

# Update outdated packages
npm update
```

**Package replacement recommendations**:
- moment → date-fns (smaller, maintained)
- request → axios or native fetch
- lodash → lodash-es (tree-shakeable)

---

## A07: Authentication Failures

### Detection Patterns

Search for weak patterns:
```
password.length
session.*expir
token.*valid
```

### Remediation Strategy

**Strong Password Policy**:
```typescript
function validatePasswordStrength(password: string): boolean {
  if (password.length < 12) return false;
  if (!/[A-Z]/.test(password)) return false;
  if (!/[a-z]/.test(password)) return false;
  if (!/\d/.test(password)) return false;
  if (!/[!@#$%^&*]/.test(password)) return false;
  return true;
}
```

**Session Management**:
```typescript
interface Session {
  userId: number;
  createdAt: number;
  expiresAt: number;
  lastActivity: number;
}

const SESSION_TIMEOUT = 60 * 60 * 1000;  // 1 hour
const IDLE_TIMEOUT = 15 * 60 * 1000;      // 15 min

function createSession(userId: number): string {
  const sessionId = crypto.randomBytes(32).toString('hex');
  const now = Date.now();

  sessions.set(sessionId, {
    userId,
    createdAt: now,
    expiresAt: now + SESSION_TIMEOUT,
    lastActivity: now,
  });

  return sessionId;
}
```

---

## A08: Data Integrity Failures

### Detection Patterns

Search for deserialization:
```
JSON.parse
yaml.load
unserialize
```

### Remediation Strategy

**Validate Deserialized Data**:
```typescript
import { z } from 'zod';

const UserSchema = z.object({
  id: z.number().int().positive(),
  email: z.string().email(),
  role: z.enum(['user', 'admin']),
});

function loadUserData(jsonString: string) {
  const raw = JSON.parse(jsonString);
  return UserSchema.parse(raw);  // Validates structure
}
```

---

## A09: Security Logging Failures

### Detection Patterns

**Missing logs for**:
- Authentication attempts
- Authorization failures
- Input validation errors
- Admin actions

**Logged sensitive data**:
- Passwords, tokens, API keys
- Credit card numbers
- Session IDs

### Remediation Strategy

**Security Event Logging**:
```typescript
import winston from 'winston';

const securityLogger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'security.log' }),
  ],
});

async function login(email: string, password: string, req: Request) {
  const user = await findUserByEmail(email);

  if (!user || !await verifyPassword(password, user.hash)) {
    securityLogger.warn('Failed login', {
      email,
      ip: req.ip,
      timestamp: new Date().toISOString(),
    });
    throw new UnauthorizedError();
  }

  securityLogger.info('Successful login', {
    userId: user.id,
    ip: req.ip,
  });

  return createSession(user);
}
```

**Sanitize Logs**:
```typescript
function sanitizeLogData(data: any) {
  const sensitive = ['password', 'token', 'apiKey'];
  const sanitized = { ...data };

  for (const key of sensitive) {
    if (key in sanitized) {
      sanitized[key] = '[REDACTED]';
    }
  }

  return sanitized;
}
```

---

## A10: Server-Side Request Forgery

### Detection Patterns

Search for user-controlled URLs:
```
fetch\(.*req\.
axios.*params
request\(.*query
```

### Remediation Strategy

**URL Validation**:
```typescript
const ALLOWED_DOMAINS = [
  'api.example.com',
  'cdn.example.com',
];

const BLOCKED_IPS = [
  /^127\./,           // localhost
  /^10\./,            // Private
  /^172\.(1[6-9]|2[0-9]|3[0-1])\./,
  /^192\.168\./,      // Private
  /^169\.254\./,      // Link-local
];

async function fetchExternal(url: string) {
  const parsed = new URL(url);

  // Protocol check
  if (!['http:', 'https:'].includes(parsed.protocol)) {
    throw new Error('Invalid protocol');
  }

  // Domain allow-list
  if (!ALLOWED_DOMAINS.includes(parsed.hostname)) {
    throw new Error('Domain not allowed');
  }

  // Block internal IPs
  for (const pattern of BLOCKED_IPS) {
    if (pattern.test(parsed.hostname)) {
      throw new Error('Internal IPs blocked');
    }
  }

  // Timeout protection
  const controller = new AbortController();
  const timeout = setTimeout(() => controller.abort(), 5000);

  try {
    return await fetch(url, {
      signal: controller.signal,
      redirect: 'manual'
    });
  } finally {
    clearTimeout(timeout);
  }
}
```

---

## Severity Classification

### Critical 🔴
**Immediate exploitation, high impact**
- Authentication bypass
- SQL injection in production
- Remote code execution
- Hardcoded production credentials

**Response**: Fix immediately

### High 🟠
**Exploitable, significant impact**
- Stored XSS
- Authorization bypass (IDOR)
- Sensitive data exposure
- Known CVEs with exploits

**Response**: Fix within 1-2 weeks

### Medium 🟡
**Specific scenario exploitation**
- Reflected XSS
- Missing rate limiting
- Weak password policy
- Information disclosure

**Response**: Fix in next sprint

### Low 🟢
**Low risk, defense-in-depth**
- Missing security headers
- Verbose errors
- Outdated deps (no CVE)
- Minor config issues

**Response**: Regular maintenance

---

## CWE Reference Table

| CWE | Name | OWASP |
|-----|------|-------|
| CWE-79 | Cross-Site Scripting | A03 |
| CWE-89 | SQL Injection | A03 |
| CWE-200 | Information Exposure | A01 |
| CWE-287 | Improper Authentication | A07 |
| CWE-352 | CSRF | A01 |
| CWE-502 | Deserialization | A08 |
| CWE-798 | Hardcoded Credentials | A07 |
| CWE-918 | SSRF | A10 |

---

## Compliance Mapping

### SOC 2 Type II
- CC6.1: Access controls (A01, A07)
- CC6.6: Encryption (A02)
- CC7.2: Monitoring (A09)

### PCI-DSS
- 6.5.1: Injection (A03)
- 6.5.3: Crypto (A02)
- 6.5.10: Auth (A07)
- 10: Logging (A09)

### GDPR
- Art 32: Security (A02, A05)
- Art 25: Privacy by design (A04)

---

## Recommended Tools

**Static Analysis**:
- ESLint security plugins
- Semgrep
- SonarQube

**Dependency Scanning**:
- npm audit
- Snyk
- Dependabot

**Dynamic Testing**:
- OWASP ZAP
- Burp Suite

**Secret Detection**:
- truffleHog
- git-secrets
- GitGuardian
