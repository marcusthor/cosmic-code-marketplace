# Pattern Examples - Code Improvements

This guide provides examples of good (code-revealed) vs bad (not code-revealed) improvement suggestions.

## Good Code Improvements

These improvements are **code-revealed** - they extend existing patterns and leverage existing infrastructure.

### Trivial Effort (1-2 hours)

#### ✅ Add search to user list
**Why it's good:**
- Search pattern already exists in product list (`src/products/ProductList.tsx:145`)
- Search input component is reusable (`src/components/SearchInput.tsx`)
- API supports search parameter (GET `/api/users?search=term`)

**Implementation:**
1. Import `SearchInput` component
2. Add `searchTerm` state
3. Pass search param to existing API call
4. Copy filtering logic from product list

---

#### ✅ Add keyboard shortcut for save
**Why it's good:**
- Keyboard shortcut system exists (`src/utils/keyboardShortcuts.ts`)
- Save handler already implemented
- Other shortcuts follow same pattern (Ctrl+S for save in editor)

**Implementation:**
1. Import `useKeyboardShortcut` hook
2. Register "Ctrl+S" with existing `handleSave` function
3. Add keyboard hint to UI (pattern in `Button.tsx:89`)

---

### Small Effort (Half day)

#### ✅ Add CSV export
**Why it's good:**
- JSON export pattern exists (`src/export/exportToJSON.ts`)
- CSV library already in dependencies (`package.json:42`)
- Export button pattern exists in toolbar

**Implementation:**
1. Create `exportToCSV.ts` following `exportToJSON.ts` structure
2. Add CSV option to existing export dropdown
3. Reuse data transformation logic
4. Add CSV formatting using existing library

---

#### ✅ Add dark mode to settings modal
**Why it's good:**
- Dark mode CSS variables exist (`src/styles/themes.css`)
- Theme toggle component exists (`src/components/ThemeToggle.tsx`)
- Other modals support dark mode

**Implementation:**
1. Import `useTheme` hook used elsewhere
2. Apply theme classes to modal container
3. Test all form elements with dark theme (pattern exists)

---

### Medium Effort (1-3 days)

#### ✅ Add pagination to comments
**Why it's good:**
- Pagination pattern exists for posts (`src/posts/PostList.tsx:200`)
- Pagination component is reusable (`src/components/Pagination.tsx`)
- API supports limit/offset parameters

**Implementation:**
1. Copy pagination state management from posts
2. Reuse `Pagination` component
3. Adapt API call to use limit/offset
4. Copy loading/error states pattern

---

#### ✅ Add new filter type to dashboard
**Why it's good:**
- Filter system is established (`src/filters/FilterBar.tsx`)
- Filter components follow consistent pattern
- Backend supports additional filter parameters

**Implementation:**
1. Create new filter component following existing pattern
2. Add filter to filter bar
3. Update API query params
4. Copy filter state management pattern

---

### Large Effort (3-7 days)

#### ✅ Add webhook support
**Why it's good:**
- Event system exists (`src/events/EventEmitter.ts`)
- HTTP handlers exist for external requests (`src/api/external/`)
- Webhook retry pattern exists in notification system

**Implementation:**
1. Create webhook event types in existing event system
2. Add webhook registry table (pattern from notification subscriptions)
3. Implement HTTP POST handler using existing patterns
4. Add retry logic from notification system
5. Create webhook management UI using CRUD patterns

---

#### ✅ Add bulk operations to admin panel
**Why it's good:**
- Single operations exist for all entities
- Batch API pattern exists (`src/api/batch.ts`)
- Selection UI pattern exists in table component

**Implementation:**
1. Add checkbox selection (pattern from file manager)
2. Create bulk action buttons (pattern from email client)
3. Implement batch API calls using existing pattern
4. Add progress indicator (pattern from upload)
5. Handle partial failures (pattern from import)

---

### Complex Effort (1-2 weeks)

#### ✅ Add multi-tenant support
**Why it's good:**
- Data layer uses Prisma with middleware support
- Auth system includes organization context
- Row-level filtering exists in query layer
- Tenant-scoped caching pattern exists

**Implementation:**
1. Add `tenantId` to all relevant tables (migration pattern exists)
2. Implement tenant middleware in Prisma (pattern from audit logging)
3. Add tenant context to request (pattern from user context)
4. Update all queries to include tenant filter
5. Add tenant switching UI (pattern from user switching)
6. Test isolation between tenants

---

#### ✅ Add plugin system
**Why it's good:**
- Extension points exist throughout app (`src/plugins/hooks/`)
- Dynamic module loading infrastructure exists
- Plugin lifecycle hooks pattern exists
- Plugin config system exists (`plugins.config.ts`)

**Implementation:**
1. Define plugin interface (extend existing hook types)
2. Implement plugin loader (pattern from theme loader)
3. Add plugin lifecycle management
4. Create plugin SDK using existing APIs
5. Build example plugin following patterns
6. Add plugin marketplace UI using existing module list pattern

---

## Bad Code Improvements

These improvements are **NOT code-revealed** - they don't extend existing patterns or leverage existing infrastructure.

### ❌ Add real-time collaboration
**Why it's bad:**
- No WebSocket infrastructure exists
- No operational transform library
- No conflict resolution patterns
- Requires entirely new architectural patterns

**This is not code-revealed** - it's a strategic feature that requires new infrastructure.

---

### ❌ Add AI-powered suggestions
**Why it's bad:**
- No ML integration exists
- No AI API clients configured
- No prompt engineering infrastructure
- No model selection patterns

**This is not code-revealed** - requires completely new technical stack.

---

### ❌ Add multi-language support (i18n)
**Why it's bad:**
- No i18n architecture exists (no `useTranslation` hook)
- No translation file structure
- No locale detection
- No right-to-left (RTL) support patterns

**This is not code-revealed** - requires new architectural foundation.

---

### ❌ Add feature X because users want it
**Why it's bad:**
- User research is not "code-revealed"
- This is strategic product planning
- No existing patterns referenced
- Doesn't leverage existing infrastructure

**This is product management, not code analysis.**

---

### ❌ Improve user onboarding
**Why it's bad:**
- Too vague and strategic
- No specific patterns referenced
- Product decision, not technical opportunity
- Doesn't identify what code enables it

**This is a product goal, not a code-revealed improvement.**

---

## Decision Framework

Use this flowchart to determine if an improvement is code-revealed:

```
Does the pattern already exist in the codebase?
    ├─ No → NOT code-revealed ❌
    └─ Yes → Continue

Is the infrastructure/dependency already in place?
    ├─ No → NOT code-revealed ❌
    └─ Yes → Continue

Can you point to specific files that contain the pattern?
    ├─ No → NOT code-revealed ❌
    └─ Yes → Continue

Can you describe implementation using existing code?
    ├─ No → NOT code-revealed ❌
    └─ Yes → Code-revealed! ✅
```

## Pattern Maturity Assessment

When evaluating patterns, consider maturity:

**Mature Pattern** (ready to extend):
- Used in 3+ places
- Has tests
- Well-documented
- Stable API
- **Example:** CRUD operations used across 5 entities

**Emerging Pattern** (use with caution):
- Used in 1-2 places
- May lack tests
- Limited documentation
- API might change
- **Example:** New authentication flow used in one module

**Experimental Pattern** (not ready):
- Prototype only
- No tests
- Poor documentation
- Unstable
- **Example:** Feature flag system with TODOs

**Guideline:** Only suggest extensions for mature patterns. Emerging patterns need more validation first.

## Effort Estimation Guide

### Trivial (1-2 hours)
- Direct copy-paste with name changes
- No new logic needed
- No new dependencies
- Minimal testing needed
- **Example:** Add "Last Name" field (pattern: "First Name" field)

### Small (Half day)
- Some adaptation of existing pattern
- Minor new logic
- Uses existing dependencies
- Some test coverage needed
- **Example:** Add date range filter (pattern: single date filter exists)

### Medium (1-3 days)
- Significant adaptation of pattern
- New logic with existing patterns as guide
- May add small dependency
- Comprehensive tests needed
- **Example:** Add user roles (pattern: basic auth exists)

### Large (3-7 days)
- Complex adaptation of patterns
- New subsystem using existing architecture
- May integrate new service
- Full test coverage
- **Example:** Add audit logging (pattern: event system exists)

### Complex (1-2 weeks)
- Major feature using foundation
- Multiple patterns combined
- Significant integration work
- Extensive testing
- **Example:** Add multi-tenancy (pattern: scoped data access exists)

## Red Flags

Watch for these signs that an "improvement" is NOT code-revealed:

🚩 **No file paths referenced** - If you can't point to existing code, it's not code-revealed

🚩 **"We should add..."** - Strategic language instead of pattern extension language

🚩 **Requires new architecture** - If it needs new foundational patterns, it's not code-revealed

🚩 **"Users want..."** - User research is product management, not code analysis

🚩 **Vague implementation** - If you can't describe specific steps using existing code, not code-revealed

🚩 **"Similar to X in industry"** - Industry benchmarks are not code patterns

🚩 **Requires significant research** - Code-revealed improvements have clear paths

## Summary

**Code-revealed improvements**:
- ✅ Extend existing patterns
- ✅ Leverage existing infrastructure
- ✅ Reference specific files and code
- ✅ Have clear implementation paths
- ✅ Span all effort levels (trivial → complex)

**Not code-revealed**:
- ❌ Require new architectural patterns
- ❌ Based on user research or strategy
- ❌ Need significant new dependencies
- ❌ Can't point to existing code
- ❌ Vague or aspirational

**Remember**: The code tells you what's possible. Your job is to discover and document those possibilities.
