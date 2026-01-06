# Code Quality Categories - Complete Reference

This guide provides detailed explanations and examples for all 12 code quality categories.

## Category Overview

| Category | Focus | Common Issues | Typical Severity |
|----------|-------|---------------|------------------|
| Large Files | File size & scope | >500 line files, monoliths | Major |
| Code Smells | Design problems | Long methods, deep nesting | Minor-Major |
| Complexity | Cognitive load | Complex conditionals, many branches | Major |
| Duplication | Repeated code | Copy-paste, similar patterns | Minor-Major |
| Naming | Readability | Unclear names, inconsistency | Minor |
| Structure | Organization | Folder structure, circular deps | Major-Critical |
| Linting | Code style | Missing config, inconsistent format | Suggestion-Minor |
| Testing | Test coverage | Missing tests, uncovered paths | Major |
| Type Safety | Type correctness | Missing types, excessive `any` | Minor-Critical |
| Dependencies | Package management | Unused, outdated, duplicates | Minor-Major |
| Dead Code | Unused code | Commented code, unreachable paths | Minor |
| Git Hygiene | Version control | Commit practices, hooks | Suggestion |

---

## 1. Large Files

### What to Look For

Files that exceed reasonable size thresholds (context-dependent):

- **Component files**: > 400-500 lines
- **Utility/service files**: > 600-800 lines
- **Test files**: > 800 lines (often acceptable if well-organized)
- **Single-purpose modules**: > 1000 lines (definite split candidate)

### Common Problems

- Monolithic components handling multiple concerns
- "God objects" with too many responsibilities
- Single files that should be multiple modules
- Poor cohesion and high coupling

### Example Issue

**❌ Bad - Large monolithic file**:
```typescript
// src/api/handlers.ts (1200 lines)
export function handleUserCreate() { /* 100 lines */ }
export function handleUserUpdate() { /* 80 lines */ }
export function handleUserDelete() { /* 60 lines */ }
export function handleProductList() { /* 120 lines */ }
export function handleProductCreate() { /* 90 lines */ }
export function handleOrderSubmit() { /* 150 lines */ }
export function handleOrderCancel() { /* 80 lines */ }
// ... 500 more lines
```

**✅ Good - Split by domain**:
```typescript
// src/api/users/handlers.ts (300 lines)
export function handleCreate() { /* 100 lines */ }
export function handleUpdate() { /* 80 lines */ }
export function handleDelete() { /* 60 lines */ }

// src/api/products/handlers.ts (250 lines)
export function handleList() { /* 120 lines */ }
export function handleCreate() { /* 90 lines */ }

// src/api/orders/handlers.ts (280 lines)
export function handleSubmit() { /* 150 lines */ }
export function handleCancel() { /* 80 lines */ }
```

---

## 2. Code Smells

### What to Look For

Design problems that indicate poor code structure:

- Long methods/functions (>50 lines)
- Deep nesting (>3 levels)
- Too many parameters (>4)
- Feature envy (method uses more from another class)
- Primitive obsession
- Inappropriate intimacy between modules

### Common Patterns

**Long Parameter List**:
```typescript
// ❌ Bad - 8 parameters
function createUser(
  name: string,
  email: string,
  phone: string,
  address: string,
  city: string,
  state: string,
  zip: string,
  country: string
) { }

// ✅ Good - Object parameter
interface UserDetails {
  name: string;
  email: string;
  phone: string;
  address: Address;
}

function createUser(details: UserDetails) { }
```

**Deep Nesting**:
```typescript
// ❌ Bad - 4 levels of nesting
if (user) {
  if (user.isActive) {
    if (user.hasPermission('edit')) {
      if (document.isEditable) {
        // finally do something
      }
    }
  }
}

// ✅ Good - Early returns
if (!user) return;
if (!user.isActive) return;
if (!user.hasPermission('edit')) return;
if (!document.isEditable) return;

// do something
```

**Feature Envy**:
```typescript
// ❌ Bad - Order class is envious of Customer data
class Order {
  customer: Customer;

  getCustomerDiscount() {
    return this.customer.level * this.customer.years * this.customer.purchases;
  }
}

// ✅ Good - Move logic to Customer
class Customer {
  level: number;
  years: number;
  purchases: number;

  calculateDiscount() {
    return this.level * this.years * this.purchases;
  }
}

class Order {
  customer: Customer;

  getCustomerDiscount() {
    return this.customer.calculateDiscount();
  }
}
```

---

## 3. High Complexity

### What to Look For

- High cyclomatic complexity (>10)
- Complex conditionals with many branches
- Overly clever code that's hard to understand
- Functions doing too many things

### Example Issue

**❌ Bad - Complex conditional logic**:
```typescript
function calculatePrice(product, user, date, coupon) {
  let price = product.basePrice;

  if (user.isPremium && product.category === 'electronics' && date.getDay() === 1) {
    price *= 0.9;
  } else if (user.isEmployee && product.category !== 'clearance') {
    price *= 0.8;
  } else if (coupon && coupon.isValid(date) && product.eligibleForCoupon) {
    if (coupon.type === 'percentage') {
      price *= (1 - coupon.value / 100);
    } else if (coupon.type === 'fixed') {
      price -= coupon.value;
    }
  }

  if (product.onSale && (!coupon || coupon.stacksWithSales)) {
    price *= product.saleMultiplier;
  }

  return price;
}
```

**✅ Good - Simplified with strategy pattern**:
```typescript
interface PricingStrategy {
  calculate(base: number): number;
}

class PremiumMondayPricing implements PricingStrategy {
  calculate(base: number) { return base * 0.9; }
}

class EmployeePricing implements PricingStrategy {
  calculate(base: number) { return base * 0.8; }
}

class CouponPricing implements PricingStrategy {
  constructor(private coupon: Coupon) {}
  calculate(base: number) { return this.coupon.apply(base); }
}

function calculatePrice(product: Product, strategy: PricingStrategy): number {
  let price = product.basePrice;
  price = strategy.calculate(price);

  if (product.onSale) {
    price = product.applySale(price);
  }

  return price;
}
```

---

## 4. Code Duplication

### What to Look For

- Copy-pasted code blocks
- Similar logic that could be abstracted
- Repeated patterns that should be utilities
- Near-duplicate components

### Example Issue

**❌ Bad - Duplicated validation**:
```typescript
// UserForm.tsx
const validateEmail = (v) => /^[^@]+@[^@]+\.[^@]+$/.test(v);
const validatePhone = (v) => /^\d{10}$/.test(v);

// ContactForm.tsx
const validateEmail = (v) => /^[^@]+@[^@]+\.[^@]+$/.test(v);
const validatePhone = (v) => /^\d{10}$/.test(v);

// SignupForm.tsx
const validateEmail = (v) => /^[^@]+@[^@]+\.[^@]+$/.test(v);
const validatePhone = (v) => /^\d{10}$/.test(v);
```

**✅ Good - Extracted validators**:
```typescript
// lib/validation.ts
export const validators = {
  email: (v: string) => /^[^@]+@[^@]+\.[^@]+$/.test(v),
  phone: (v: string) => /^\d{10}$/.test(v),
  required: (v: any) => v != null && v !== '',
};

// UserForm.tsx, ContactForm.tsx, SignupForm.tsx
import { validators } from '@/lib/validation';

const isEmailValid = validators.email(email);
const isPhoneValid = validators.phone(phone);
```

---

## 5. Naming Conventions

### What to Look For

- Inconsistent naming styles (camelCase, snake_case, PascalCase mixed)
- Unclear or cryptic variable names
- Abbreviations that hurt readability
- Names that don't reflect purpose

### Example Issues

**❌ Bad Naming**:
```typescript
const d = new Date();  // What is 'd'?
const tmp = processData(data);  // Temporary what?
const flg = true;  // What flag?
const usrMgr = new UserManager();  // Abbreviations

function calc(a, b) { return a + b; }  // What is being calculated?
```

**✅ Good Naming**:
```typescript
const currentDate = new Date();
const processedUserData = processData(data);
const isEmailValid = true;
const userManager = new UserManager();

function calculateTotalPrice(basePrice: number, taxRate: number) {
  return basePrice + (basePrice * taxRate);
}
```

---

## 6. File Structure

### What to Look For

- Poor folder organization (no clear domain boundaries)
- Inconsistent module boundaries
- Circular dependencies
- Misplaced files (utils in components folder, etc.)
- Missing index/barrel files

### Example Issue

**❌ Bad - Flat structure**:
```
src/
├── UserList.tsx
├── UserDetails.tsx
├── UserForm.tsx
├── ProductList.tsx
├── ProductCard.tsx
├── OrderHistory.tsx
├── useAuth.ts
├── useProducts.ts
├── api.ts
├── utils.ts
└── ... 50 more files
```

**✅ Good - Domain-organized**:
```
src/
├── features/
│   ├── users/
│   │   ├── components/
│   │   │   ├── UserList.tsx
│   │   │   ├── UserDetails.tsx
│   │   │   └── UserForm.tsx
│   │   ├── hooks/
│   │   │   └── useUsers.ts
│   │   └── api.ts
│   ├── products/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── api.ts
│   └── orders/
│       ├── components/
│       ├── hooks/
│       └── api.ts
├── shared/
│   ├── hooks/
│   │   └── useAuth.ts
│   └── utils/
│       └── validation.ts
└── lib/
    └── api-client.ts
```

---

## 7. Linting Issues

### What to Look For

- Missing ESLint/Prettier configuration
- Inconsistent code formatting
- Unused variables/imports
- Missing or inconsistent rules

### Example Configuration

**✅ Recommended ESLint config**:
```json
{
  "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended"],
  "rules": {
    "max-lines": ["warn", 500],
    "max-lines-per-function": ["warn", 100],
    "complexity": ["warn", 10],
    "max-params": ["warn", 4],
    "max-depth": ["warn", 3],
    "no-console": ["warn", { "allow": ["warn", "error"] }],
    "no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "error"
  }
}
```

---

## 8. Test Coverage

### What to Look For

- Missing unit tests for critical logic
- Components without test files
- Untested edge cases
- Missing integration tests
- Low coverage percentages

### Example Issue

**❌ Missing tests**:
```
src/features/users/
├── components/
│   ├── UserList.tsx         (no test file)
│   ├── UserDetails.tsx      (no test file)
│   └── UserForm.tsx         (no test file)
└── utils/
    └── validation.ts        (no test file)
```

**✅ With tests**:
```
src/features/users/
├── components/
│   ├── UserList.tsx
│   ├── UserList.test.tsx
│   ├── UserDetails.tsx
│   ├── UserDetails.test.tsx
│   ├── UserForm.tsx
│   └── UserForm.test.tsx
└── utils/
    ├── validation.ts
    └── validation.test.ts
```

---

## 9. Type Safety

### What to Look For

- Missing TypeScript types
- Excessive `any` usage
- Incomplete type definitions
- Runtime type mismatches
- Type assertions without validation

### Example Issues

**❌ Bad - Excessive any**:
```typescript
function processData(data: any) {
  const result: any = transform(data as any);
  return result;
}

const userData: any = await fetchUser();
console.log(userData.name);  // No type safety
```

**✅ Good - Proper types**:
```typescript
interface UserData {
  id: number;
  name: string;
  email: string;
}

function processData(data: UserData): ProcessedUser {
  const result = transform(data);
  return result;
}

const userData: UserData = await fetchUser();
console.log(userData.name);  // Type-safe
```

---

## 10. Dependency Issues

### What to Look For

- Unused dependencies in package.json
- Duplicate dependencies (e.g., `lodash` and `lodash-es`)
- Outdated dev tooling
- Missing peer dependencies

### Example

**❌ Problems in package.json**:
```json
{
  "dependencies": {
    "lodash": "^4.17.21",
    "lodash-es": "^4.17.21",  // Duplicate
    "moment": "^2.29.1",       // Unused
    "axios": "^0.21.1"         // Outdated (has vulnerabilities)
  }
}
```

**✅ Cleaned up**:
```json
{
  "dependencies": {
    "lodash-es": "^4.17.21",   // Keep only ES modules version
    "axios": "^1.6.0"          // Updated to latest
  }
}
```

---

## 11. Dead Code

### What to Look For

- Unused functions/components
- Commented-out code blocks
- Unreachable code paths
- Deprecated features not removed

### Example

**❌ Dead code**:
```typescript
function oldImplementation() {
  // This was replaced 6 months ago
  // TODO: Remove this
}

export function currentFunction() {
  // Old approach - commented out
  // const result = oldImplementation();
  // return result;

  // New approach
  return newImplementation();
}

function neverUsed() {
  // This function is never imported or called
  return 'unused';
}
```

**✅ Cleaned**:
```typescript
export function currentFunction() {
  return newImplementation();
}
```

---

## 12. Git Hygiene

### What to Look For

- Large commits that should be split
- Missing commit message standards
- Lack of branch naming conventions
- Missing pre-commit hooks

### Recommendations

**Commit Message Convention**:
```
type(scope): brief description

Examples:
feat(auth): add password reset flow
fix(api): handle null response in user endpoint
refactor(components): extract shared button logic
docs(readme): update installation instructions
```

**Pre-commit Hook** (`.husky/pre-commit`):
```bash
#!/bin/sh
npm run lint
npm run type-check
npm test
```

---

## Summary: Severity Guidelines

**Critical (🔴)**:
- Circular dependencies
- Type errors in production code
- Security vulnerabilities
- Broken builds

**Major (🟠)**:
- Files >1000 lines
- High cyclomatic complexity (>15)
- Missing tests for critical features
- Significant code duplication

**Minor (🟡)**:
- Files 500-800 lines
- Moderate duplication
- Inconsistent naming
- Missing types in non-critical code

**Suggestion (🟢)**:
- Style inconsistencies
- Missing documentation
- Outdated dependencies (non-security)
- Minor optimizations
