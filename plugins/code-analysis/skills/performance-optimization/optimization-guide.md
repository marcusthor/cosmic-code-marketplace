# Performance Optimization Guide

Complete reference for the 7 performance optimization categories with anti-patterns, best practices, and metrics.

## Category Overview

| Category | Focus | Common Tools | Impact Area |
|----------|-------|--------------|-------------|
| Bundle Size | JavaScript/CSS payload | webpack-bundle-analyzer | Initial load time |
| Runtime Performance | Execution speed | Chrome DevTools Profiler | Interaction responsiveness |
| Memory Usage | RAM consumption | Memory profiler, heap snapshots | Long-session stability |
| Database Performance | Query efficiency | EXPLAIN, query analyzers | Data loading speed |
| Network Optimization | HTTP performance | Network tab, Lighthouse | Content delivery |
| Rendering Performance | Paint/layout | React DevTools, Performance tab | Visual responsiveness |
| Caching | Data reuse | Cache-Control, service workers | Repeat visit speed |

---

## 1. Bundle Size Optimizations

### Goal
Reduce JavaScript/CSS payload to improve initial load time.

### Anti-Patterns

**❌ Importing entire libraries:**
```javascript
// BAD: Imports entire lodash (70KB)
import _ from 'lodash';
const result = _.map(array, fn);

// GOOD: Import only what's needed (2KB)
import map from 'lodash/map';
const result = map(array, fn);

// BETTER: Use lodash-es for tree-shaking
import { map } from 'lodash-es';
const result = map(array, fn);
```

**❌ Heavy dependencies for simple tasks:**
```javascript
// BAD: moment.js (300KB) for basic formatting
import moment from 'moment';
const formatted = moment(date).format('MMM DD, YYYY');

// GOOD: date-fns (30KB, tree-shakeable)
import { format } from 'date-fns';
const formatted = format(date, 'MMM dd, yyyy');

// BEST: Native Intl API (0KB)
const formatted = new Intl.DateTimeFormat('en-US', {
  month: 'short', day: '2-digit', year: 'numeric'
}).format(date);
```

**❌ Full icon libraries:**
```javascript
// BAD: Import entire Font Awesome (900KB+)
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUser, faHome, faSearch } from '@fortawesome/free-solid-svg-icons';

// GOOD: Individual icon packages
import { FaUser, FaHome, FaSearch } from 'react-icons/fa';
```

### Best Practices

1. **Use bundle analyzer:**
   ```bash
   npx webpack-bundle-analyzer dist/stats.json
   ```

2. **Code splitting:**
   ```javascript
   // Route-based splitting
   const Dashboard = React.lazy(() => import('./Dashboard'));
   const Settings = React.lazy(() => import('./Settings'));

   <Suspense fallback={<Loading />}>
     <Routes>
       <Route path="/dashboard" element={<Dashboard />} />
       <Route path="/settings" element={<Settings />} />
     </Routes>
   </Suspense>
   ```

3. **Tree-shaking:**
   - Use ES modules (`import/export`)
   - Avoid side effects in modules
   - Mark side-effect-free in package.json: `"sideEffects": false`

### Metrics to Track
- **Bundle size (gzipped)**: < 200KB initial
- **Largest chunks**: < 50KB each
- **Time to Interactive**: < 3.8s

---

## 2. Runtime Performance

### Goal
Optimize execution speed for faster interactions.

### Anti-Patterns

**❌ O(n²) when O(n) is possible:**
```javascript
// BAD: O(n²) - nested loops
const matched = users.map(user => {
  const posts = allPosts.filter(p => p.userId === user.id);
  return { user, posts };
});

// GOOD: O(n) - single pass with Map
const postsByUser = new Map();
allPosts.forEach(post => {
  if (!postsByUser.has(post.userId)) {
    postsByUser.set(post.userId, []);
  }
  postsByUser.get(post.userId).push(post);
});
const matched = users.map(user => ({
  user,
  posts: postsByUser.get(user.id) || []
}));

// BETTER: Use groupBy (when available)
const postsByUser = Object.groupBy(allPosts, p => p.userId);
```

**❌ Repeated expensive operations:**
```javascript
// BAD: Parsing in loop
items.forEach(item => {
  const config = JSON.parse(item.configString);
  process(config);
});

// GOOD: Memoize or pre-parse
const configs = items.map(item => JSON.parse(item.configString));
configs.forEach(config => process(config));
```

**❌ Expensive regex in hot paths:**
```javascript
// BAD: Complex regex called frequently
function validateEmail(email) {
  return /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/.test(email);
}
emails.forEach(email => validateEmail(email)); // Regex compiled each time

// GOOD: Compile regex once
const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
function validateEmail(email) {
  return emailRegex.test(email);
}
```

### Best Practices

1. **Profile before optimizing:**
   - Chrome DevTools > Performance
   - Identify actual bottlenecks
   - Measure impact of changes

2. **Use appropriate data structures:**
   - Set for uniqueness checks (O(1) vs O(n))
   - Map for key-value lookups (O(1) vs O(n))
   - Typed arrays for numeric data

3. **Debounce/throttle expensive operations:**
   ```javascript
   import { debounce } from 'lodash-es';

   const handleSearch = debounce((query) => {
     // Expensive operation
     search(query);
   }, 300);
   ```

### Metrics to Track
- **Task duration**: < 50ms per task
- **Total Blocking Time**: < 200ms
- **Long tasks**: 0 tasks > 50ms

---

## 3. Memory Usage

### Goal
Prevent memory leaks and reduce memory footprint.

### Anti-Patterns

**❌ Event listeners without cleanup:**
```javascript
// BAD: Memory leak
useEffect(() => {
  window.addEventListener('resize', handleResize);
  // Missing cleanup!
}, []);

// GOOD: Proper cleanup
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

**❌ Unbounded caches:**
```javascript
// BAD: Grows indefinitely
const cache = new Map();
function getData(id) {
  if (!cache.has(id)) {
    cache.set(id, fetchData(id));
  }
  return cache.get(id);
}

// GOOD: LRU cache with size limit
import LRUCache from 'lru-cache';
const cache = new LRUCache({ max: 100 });
```

**❌ Retaining large objects:**
```javascript
// BAD: Holds entire response in memory
const allData = await fetchLargeDataset();
const ids = allData.map(item => item.id); // Only need IDs

// GOOD: Extract only what's needed, discard rest
const ids = (await fetchLargeDataset()).map(item => item.id);
```

### Best Practices

1. **Clean up timers:**
   ```javascript
   useEffect(() => {
     const interval = setInterval(poll, 1000);
     return () => clearInterval(interval);
   }, []);
   ```

2. **Weak references for caches:**
   ```javascript
   const cache = new WeakMap();
   cache.set(obj, expensiveComputation(obj));
   // Auto-garbage collected when obj is no longer referenced
   ```

3. **Memory profiling:**
   - Chrome DevTools > Memory > Heap snapshot
   - Look for detached DOM nodes
   - Check for retained objects

---

## 4. Database Performance

### Goal
Optimize query efficiency and reduce latency.

### Anti-Patterns

**❌ N+1 query problem:**
```sql
-- BAD: N+1 queries
SELECT * FROM users;  -- 1 query
-- Then for each user (N queries):
SELECT * FROM posts WHERE user_id = ?;

-- GOOD: Single query with JOIN
SELECT u.*, p.*
FROM users u
LEFT JOIN posts p ON p.user_id = u.id;
```

```javascript
// BAD: N+1 in ORM
const users = await User.findAll();
for (const user of users) {
  user.posts = await Post.findAll({ where: { userId: user.id } });
}

// GOOD: Eager loading
const users = await User.findAll({
  include: [{ model: Post }]
});
```

**❌ Missing indexes:**
```sql
-- BAD: Full table scan
SELECT * FROM users WHERE email = 'user@example.com';
-- No index on email column = slow

-- GOOD: Add index
CREATE INDEX idx_users_email ON users(email);
```

**❌ SELECT * over-fetching:**
```sql
-- BAD: Fetches all columns (including large BLOB)
SELECT * FROM articles WHERE id = 123;

-- GOOD: Only needed columns
SELECT id, title, author_id, created_at
FROM articles
WHERE id = 123;
```

### Best Practices

1. **Use EXPLAIN to analyze queries:**
   ```sql
   EXPLAIN ANALYZE
   SELECT * FROM users
   WHERE created_at > '2024-01-01'
   ORDER BY email;
   ```

2. **Add indexes strategically:**
   - WHERE clauses
   - ORDER BY columns
   - JOIN columns
   - Foreign keys

3. **Pagination for large result sets:**
   ```sql
   -- BAD: No limit
   SELECT * FROM posts ORDER BY created_at DESC;

   -- GOOD: Paginated
   SELECT * FROM posts
   ORDER BY created_at DESC
   LIMIT 20 OFFSET 0;

   -- BETTER: Cursor-based
   SELECT * FROM posts
   WHERE id > last_seen_id
   ORDER BY id
   LIMIT 20;
   ```

---

## 5. Network Optimization

### Goal
Reduce network latency and bandwidth usage.

### Anti-Patterns

**❌ Sequential requests:**
```javascript
// BAD: Sequential (total: 3s)
const user = await fetch('/api/user');
const posts = await fetch('/api/posts');
const comments = await fetch('/api/comments');

// GOOD: Parallel (total: 1s)
const [user, posts, comments] = await Promise.all([
  fetch('/api/user'),
  fetch('/api/posts'),
  fetch('/api/comments')
]);
```

**❌ Large payloads:**
```javascript
// BAD: Fetch entire user object for just the name
const user = await fetch('/api/users/123');  // Returns 50 fields
console.log(user.name);

// GOOD: Field selection
const user = await fetch('/api/users/123?fields=name');
```

**❌ No caching:**
```javascript
// BAD: Re-fetch static data on every request
const config = await fetch('/api/config');

// GOOD: Cache with appropriate headers
fetch('/api/config', {
  headers: { 'Cache-Control': 'max-age=3600' }
});
```

### Best Practices

1. **Compression:**
   ```javascript
   // Server: Enable gzip/brotli
   response.setHeader('Content-Encoding', 'gzip');
   ```

2. **Request caching:**
   ```javascript
   // SWR pattern
   import useSWR from 'swr';
   const { data } = useSWR('/api/user', fetcher, {
     revalidateOnFocus: false,
     revalidateOnReconnect: false
   });
   ```

3. **Prefetching:**
   ```javascript
   // Prefetch on hover
   <Link
     to="/dashboard"
     onMouseEnter={() => prefetch('/api/dashboard-data')}
   >
     Dashboard
   </Link>
   ```

---

## 6. Rendering Performance

### Goal
Optimize React rendering and DOM updates.

### Anti-Patterns

**❌ Unnecessary re-renders:**
```jsx
// BAD: Component re-renders on every parent render
function ExpensiveComponent({ data }) {
  // Expensive computation
  const processed = expensiveProcess(data);
  return <div>{processed}</div>;
}

// GOOD: Memoized component
const ExpensiveComponent = React.memo(({ data }) => {
  const processed = expensiveProcess(data);
  return <div>{processed}</div>;
});
```

**❌ Inline object/array creation:**
```jsx
// BAD: New object on every render causes child re-render
<Child config={{ theme: 'dark', size: 'large' }} />

// GOOD: Stable reference
const config = useMemo(() => ({ theme: 'dark', size: 'large' }), []);
<Child config={config} />
```

**❌ Large lists without virtualization:**
```jsx
// BAD: Renders 10,000 items (slow)
<div>
  {items.map(item => <Item key={item.id} data={item} />)}
</div>

// GOOD: Virtualized (renders only visible ~20 items)
import { FixedSizeList } from 'react-window';
<FixedSizeList
  height={600}
  itemCount={items.length}
  itemSize={50}
>
  {({ index, style }) => <Item style={style} data={items[index]} />}
</FixedSizeList>
```

### Best Practices

1. **useCallback for stable function references:**
   ```jsx
   const handleClick = useCallback((id) => {
     deleteItem(id);
   }, []);
   ```

2. **useMemo for expensive computations:**
   ```jsx
   const sortedData = useMemo(() => {
     return data.sort((a, b) => a.value - b.value);
   }, [data]);
   ```

3. **Code splitting for large components:**
   ```jsx
   const HeavyChart = React.lazy(() => import('./HeavyChart'));
   ```

---

## 7. Caching Strategies

### Goal
Reuse computed/fetched data to avoid redundant work.

### Cache Types

**1. In-Memory Caching:**
```javascript
const cache = new Map();
function expensiveOperation(input) {
  if (cache.has(input)) {
    return cache.get(input);
  }
  const result = compute(input);
  cache.set(input, result);
  return result;
}
```

**2. HTTP Caching:**
```javascript
// Client
fetch('/api/data', {
  cache: 'force-cache'  // Use cached if available
});

// Server
response.setHeader('Cache-Control', 'public, max-age=31536000, immutable');
```

**3. Service Worker Caching:**
```javascript
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request).then(cached => {
      return cached || fetch(event.request);
    })
  );
});
```

**4. CDN Caching:**
- Static assets on CDN
- Edge caching for API responses
- Cache invalidation strategies

---

## Performance Budgets

Set and monitor these targets:

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| **Time to Interactive** | < 3.8s | 3.8-7.3s | > 7.3s |
| **First Contentful Paint** | < 1.8s | 1.8-3.0s | > 3.0s |
| **Largest Contentful Paint** | < 2.5s | 2.5-4.0s | > 4.0s |
| **Total Blocking Time** | < 200ms | 200-600ms | > 600ms |
| **Cumulative Layout Shift** | < 0.1 | 0.1-0.25 | > 0.25 |
| **Bundle Size (gzip)** | < 200KB | 200-400KB | > 400KB |

---

## Summary: Impact vs Effort

**High Impact, Low Effort (Do First!):**
- Replace heavy dependencies
- Add React.memo to expensive components
- Implement code splitting on routes
- Add database indexes
- Enable HTTP caching

**High Impact, High Effort (Plan Carefully):**
- Refactor algorithmic complexity
- Implement comprehensive caching strategy
- Database query optimization
- Large-scale architecture changes

**Low Impact, Any Effort (Do Last):**
- Micro-optimizations
- Premature abstractions
- Over-engineering

**Remember:** Measure, optimize, measure again. Performance optimization is data-driven, not guesswork.
