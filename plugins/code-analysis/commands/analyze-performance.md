---
description: Find performance bottlenecks and create optimization tickets
argument-hint: [optional: specific area to optimize]
---

# Performance Optimization Analysis

I'll analyze performance across 7 categories and identify optimization opportunities.

## Step 1: Performance Analysis

Using the **performance-optimization** skill to check:
- **Bundle Size** - Dependencies, code splitting
- **Runtime Performance** - Algorithm complexity, hot paths
- **Memory Usage** - Leaks, unbounded caches
- **Database Performance** - N+1 queries, missing indexes
- **Network Optimization** - Request patterns, caching
- **Rendering Performance** - React re-renders, virtualization
- **Caching Strategies** - Opportunities for memoization

$ARGUMENTS

## Step 2: Create Optimization Tickets

After analysis, I'll:
1. Create `improvements/performance/` directory
2. Generate individual ticket files for each optimization
3. Format: `NNN-short-title.md` (e.g., `001-replace-moment-with-date-fns.md`)
4. Impact levels: 🔴 High, 🟡 Medium, 🟢 Low
5. Each ticket contains:
   - Current performance issue
   - Expected improvement (KB saved, ms faster, etc.)
   - Proposed optimization
   - Implementation steps
   - Before/after metrics

**Starting performance analysis...**
