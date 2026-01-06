---
name: performance-optimization
description: Analyze performance bottlenecks in bundle size, runtime, database queries, and rendering. Use when optimizing app performance or reducing load times.
---

# Performance Optimization Analysis

Analyze codebases to identify performance bottlenecks, optimization opportunities, and efficiency improvements across bundle size, runtime, database, network, rendering, memory, and caching.

**Your Role**: Senior performance engineer focused on measurable user experience improvements.

## Workflow

### Phase 1: Bundle Size Analysis

**Read package.json**:
- Identify large dependencies
- Find alternative lighter packages
- Check for duplicate dependencies

**Check imports** using Grep:
- Full library imports: `import .* from 'lodash'`
- Check if only specific functions are used

**Common heavy dependencies to check**:
- moment.js (300KB) → date-fns (tree-shakeable)
- lodash (full) → lodash-es (tree-shakeable)
- full icon libraries → icon subsets

### Phase 2: Runtime Performance Analysis

**Find algorithmic complexity issues** using Grep:

**Nested loops** (O(n²)):
```
for.*for
.forEach.*forEach
.map.*find
```

**Inefficient patterns**:
- `.find()` in loops → Use Map lookup (O(1))
- Repeated calculations → Use memoization
- Synchronous operations → Check for async alternatives

**Hot paths** (frequently called code):
- Event handlers
- Render functions
- API response processing

### Phase 3: React/Component Performance

**Find render optimization opportunities** using Grep:

**Missing React.memo**:
- Search for components without memo
- Check if props change frequently

**Inline functions in JSX**:
```
onClick={() =>
onClick={function
```
→ Should use useCallback

**Missing useMemo**:
- Expensive calculations in render
- Object/array creation in render

**Large lists without virtualization**:
- Lists with > 100 items
- Missing react-window or react-virtualized

### Phase 4: Database Performance

**If database exists**, analyze queries:

**N+1 patterns**:
- Queries in loops
- Missing JOINs
- Eager loading opportunities

**Missing indexes**:
- WHERE clauses without indexes
- ORDER BY without indexes
- JOIN columns without indexes

**Over-fetching**:
- SELECT * when only few columns needed
- Missing query limits
- Large JSON/BLOB fields

### Phase 5: Network Optimization

**API call patterns** using Grep:
```
fetch(
axios.
api.get
```

**Check for**:
- Sequential requests that could be parallel
- Missing request caching
- Large payload sizes
- Missing compression headers
- Duplicate requests

### Phase 6: Memory Analysis

**Memory leak indicators**:
- Event listeners without cleanup
- setInterval without clearInterval
- Global state accumulation
- Unbounded caches

**Search for**:
```
addEventListener
setInterval
setTimeout
new Map
new Set
```

Check if cleanup exists in useEffect return or componentWillUnmount.

### Phase 7: Caching Opportunities

**Identify cacheable data**:
- Static content
- Infrequently changing data
- Expensive computations
- API responses

**Check for existing caching**:
- Service workers
- Cache-Control headers
- Client-side caching (localStorage, IndexedDB)
- CDN usage

### Phase 8: Generate Markdown Report

Create comprehensive performance report: `performance_optimizations_[project-name]_[date].md`

## Report Structure

```markdown
# Performance Optimization Report: [Project Name]

**Generated:** [Date/Time]
**Bundle Size:** [X] MB
**Files Analyzed:** [Count]

---

## Executive Summary

[2-3 paragraph overview of performance state]

**Performance Health:**
- 🎯 Critical Issues: [count] (high impact, user-facing)
- ⚡ Quick Wins: [count] (high impact, low effort)
- 📦 Bundle Opportunities: [potential savings in KB]

**Top 3 Optimizations:**
1. [High impact optimization - biggest user benefit]
2. [High impact optimization]
3. [Quick win - easy implementation]

---

## Impact High 🔴

### Optimization 1: [Title]
- **Category:** [bundle_size/runtime/memory/database/network/rendering/caching]
- **Impact:** 🔴 High
- **Effort:** [trivial/small/medium/large]
- **Expected Improvement:** [Specific metric: 270KB reduction, 40% faster, 200ms saved]

**Affected Areas:**
- `src/utils/date.ts:15` - moment.js import
- `src/components/Calendar.tsx:8` - date formatting
- `package.json:23` - moment.js dependency (300KB)

**Current State:**
[Description of current implementation with metrics]

**Performance Issue:**
```typescript
// Current: Heavy dependency (300KB)
import moment from 'moment';

export function formatDate(date: Date) {
  return moment(date).format('MMM DD, YYYY');
}
```

**Proposed Optimization:**
```typescript
// Optimized: Lightweight alternative (30KB)
import { format } from 'date-fns';

export function formatDate(date: Date) {
  return format(date, 'MMM dd, yyyy');
}
```

**Metrics:**
- **Current:** Bundle includes 300KB for 3 date functions
- **Optimized:** Bundle includes ~30KB
- **Savings:** 270KB (~90% reduction)
- **User Impact:** ~20% faster initial load on 3G

**Implementation Steps:**
1. Install date-fns: `npm install date-fns`
2. Replace all moment imports with date-fns
3. Update format strings to date-fns syntax
4. Remove moment dependency
5. Test all date formatting functionality
6. Measure bundle size reduction

**Tradeoffs:**
- Format string syntax differs (one-time migration cost)
- date-fns lacks moment's plugin ecosystem

**Expected User Experience:**
- **Time to Interactive:** -0.8s (3.8s → 3.0s)
- **First Contentful Paint:** -0.3s
- **Bundle Size:** -270KB (-10% total bundle)

---

## Impact Medium 🟡

[Same structure]

---

## Impact Low 🟢

[Same structure]

---

## Category Breakdown

| Category | High | Medium | Low | Total | Potential Savings |
|----------|------|--------|-----|-------|------------------|
| Bundle Size | 3 | 2 | 1 | 6 | 400KB |
| Runtime Performance | 2 | 4 | 5 | 11 | 500ms TTI |
| Memory Usage | 1 | 1 | 2 | 4 | 50MB heap |
| Database | 0 | 2 | 1 | 3 | 200ms queries |
| Network | 1 | 3 | 2 | 6 | 300ms loading |
| Rendering | 2 | 3 | 4 | 9 | 60fps |
| Caching | 0 | 1 | 3 | 4 | 1s repeat visits |

---

## Impact vs Effort Matrix

| Optimization | Impact | Effort | Priority | Expected Gain |
|--------------|--------|--------|----------|---------------|
| Replace moment.js | 🔴 High | Small | ⭐⭐⭐ | -270KB, -20% load |
| Add React.memo to UserList | 🔴 High | Trivial | ⭐⭐⭐ | -80% re-renders |
| Implement request caching | 🟡 Medium | Medium | ⭐⭐ | -60% API calls |
| Optimize database indexes | 🟡 Medium | Large | ⭐ | -50% query time |

**Priority Legend:**
- ⭐⭐⭐ High: Quick wins (high impact + low effort)
- ⭐⭐ Medium: Worth doing (high impact OR low effort)
- ⭐ Low: Consider later (low impact + high effort)

---

## Performance Budget Status

| Metric | Target | Current | Status | Gap |
|--------|--------|---------|--------|-----|
| Time to Interactive | < 3.8s | 4.2s | ❌ | +0.4s |
| First Contentful Paint | < 1.8s | 2.1s | ❌ | +0.3s |
| Largest Contentful Paint | < 2.5s | 3.0s | ❌ | +0.5s |
| Total Blocking Time | < 200ms | 350ms | ❌ | +150ms |
| Bundle Size (gzip) | < 200KB | 320KB | ❌ | +120KB |

**With Optimizations Applied:**
| Metric | After | Status |
|--------|-------|--------|
| Time to Interactive | 3.2s | ✅ |
| Bundle Size (gzip) | 180KB | ✅ |

---

## Quick Wins (High Impact, Low Effort)

These optimizations provide maximum value for minimal effort:

1. **Add React.memo to expensive components** (15 minutes)
   - UserList, ProductGrid, DataTable
   - Expected: 80% fewer re-renders

2. **Implement code splitting on routes** (30 minutes)
   - Use React.lazy for route components
   - Expected: -40% initial bundle

3. **Add indexes to frequent queries** (1 hour)
   - user_id, created_at, status columns
   - Expected: -60% query time

4. **Replace lodash with lodash-es** (1 hour)
   - Enable tree-shaking
   - Expected: -50KB bundle size

---

## Anti-Patterns Found

**Bundle Size:**
```javascript
// ❌ Found: Full library imports
import _ from 'lodash';  // Imports entire library (70KB)

// ✅ Recommended
import map from 'lodash/map';  // Only needed function (2KB)
```

**Runtime Performance:**
```javascript
// ❌ Found: O(n²) in hot path
users.forEach(user => {
  const match = posts.find(p => p.userId === user.id);
});

// ✅ Recommended: O(n) with Map
const postsByUser = new Map(posts.map(p => [p.userId, p]));
users.forEach(user => {
  const match = postsByUser.get(user.id);
});
```

**React Rendering:**
```jsx
// ❌ Found: Inline function creation
<Button onClick={() => handleClick(id)} />

// ✅ Recommended: useCallback
const onClick = useCallback(() => handleClick(id), [id]);
<Button onClick={onClick} />
```

---

## Optimization Roadmap

### Phase 1: Critical Performance (Week 1)
- [ ] Replace moment.js with date-fns (-270KB)
- [ ] Add React.memo to top 5 components (-80% re-renders)
- [ ] Implement code splitting on routes (-40% initial bundle)

### Phase 2: Medium Gains (Weeks 2-3)
- [ ] Optimize database queries and add indexes
- [ ] Implement request caching for API calls
- [ ] Add virtualization to large lists

### Phase 3: Fine-Tuning (Week 4+)
- [ ] Optimize images and assets
- [ ] Implement service worker caching
- [ ] Review and optimize CSS

### Phase 4: Monitoring
- [ ] Set up performance monitoring
- [ ] Add performance budgets to CI
- [ ] Schedule monthly performance reviews

---

## Recommended Tools

**Bundle Analysis:**
- webpack-bundle-analyzer
- source-map-explorer

**Runtime Profiling:**
- Chrome DevTools Performance tab
- React DevTools Profiler

**Network Analysis:**
- Chrome DevTools Network tab
- Lighthouse

**Database:**
- EXPLAIN ANALYZE for queries
- Database slow query logs

---

## Next Steps

1. **Prioritize** - Review with team and select top 3-5 optimizations
2. **Baseline** - Measure current performance metrics
3. **Implement** - Apply optimizations incrementally
4. **Measure** - Verify expected improvements
5. **Monitor** - Set up ongoing performance tracking
6. **Budget** - Add performance budgets to CI/CD

---

## Analysis Metadata

**Analysis Date:** [ISO timestamp]
**Files Scanned:** [Count]
**Total Optimizations:** [Count]
**Potential Bundle Savings:** [KB]
**Potential Performance Gain:** [% or ms]
**Generated By:** Claude Code - Performance Optimization Skill
```

## Critical Rules

1. **Measure First** - Suggest profiling before and after
2. **Quantify Impact** - Include expected improvements (%, ms, KB)
3. **Consider Tradeoffs** - Note any downsides (complexity, maintenance)
4. **Prioritize User Impact** - Focus on user-facing performance
5. **Avoid Premature Optimization** - Don't suggest micro-optimizations
6. **Data-Driven** - Base recommendations on measurable metrics

## Quality Checklist

Before finalizing report:
- [ ] All optimizations have quantified expected improvements
- [ ] Impact levels are justified with user experience implications
- [ ] Effort estimates are realistic
- [ ] Tradeoffs are documented
- [ ] Implementation steps are clear
- [ ] Performance budgets are referenced
- [ ] Quick wins are highlighted

---

For detailed optimization techniques and anti-patterns, see [optimization-guide.md](optimization-guide.md).
