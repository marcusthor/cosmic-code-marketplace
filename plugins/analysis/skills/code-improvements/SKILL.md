---
name: code-improvements
description: Discover code improvement opportunities by analyzing existing patterns and architecture. Use when looking for features to build, patterns to extend, or infrastructure to leverage.
---

# Code Improvements Discovery

Discover code-revealed improvement opportunities by analyzing existing patterns, architecture, and infrastructure in the codebase.

**Key Principle**: Find opportunities the code reveals. These are features and improvements that naturally emerge from understanding what patterns exist and how they can be extended, applied elsewhere, or scaled up.

**Important**: This is NOT strategic product planning. Focus on what the CODE tells you is possible, not what users might want.

## Workflow

### Phase 1: Understand the Project

Automatically scan the current workspace to understand:
- Project structure and tech stack
- Existing features and components
- Established code patterns
- Architecture and infrastructure

Use Glob to discover file structure:
```
**/*.{ts,tsx,js,jsx,py}
**/components/**
**/api/**
**/utils/**
```

### Phase 2: Discover Existing Patterns

Search for patterns that could be extended using Grep:

**Components & Modules**:
- Search for: `export (function|const|class)`
- Find reusable components, hooks, utilities

**API Routes/Endpoints**:
- Search for: `router\.|app\.|/api/|@app.route`
- Identify API patterns

**CRUD Operations**:
- Search for: `(create|update|delete|get|list|find)`
- Find data access patterns

**UI Components**:
- Search directories: `components/`, `src/components/`
- Identify component patterns and variants

**Hooks & Reusable Logic**:
- Search for: `use[A-Z]`
- Find React hooks and reusable logic patterns

**Middleware/Handlers**:
- Search for: `middleware|interceptor|handler`
- Identify request/response patterns

Look for:
- Patterns that are repeated (could be extended)
- Features that handle one case but could handle more
- Utilities that could have additional methods
- UI components that could have variants
- Infrastructure that enables new capabilities

### Phase 3: Identify Opportunity Categories

Categorize opportunities into 7 types:

#### A. Pattern Extensions (trivial → medium)
- Existing CRUD for one entity → CRUD for similar entity
- Existing filter for one field → Filters for more fields
- Existing sort by one column → Sort by multiple columns
- Existing export to CSV → Export to JSON/Excel
- Existing validation for one type → Validation for similar types

#### B. Architecture Opportunities (medium → complex)
- Data model supports feature X with minimal changes
- API structure enables new endpoint type
- Component architecture supports new view/mode
- State management pattern enables new features
- Build system supports new output formats

#### C. Configuration/Settings (trivial → small)
- Hard-coded values that could be user-configurable
- Missing user preferences that follow existing preference patterns
- Feature toggles that extend existing toggle patterns

#### D. Utility Additions (trivial → medium)
- Existing validators that could validate more cases
- Existing formatters that could handle more formats
- Existing helpers that could have related helpers

#### E. UI Enhancements (trivial → medium)
- Missing loading states that follow existing loading patterns
- Missing empty states that follow existing empty state patterns
- Missing error states that follow existing error patterns
- Keyboard shortcuts that extend existing shortcut patterns

#### F. Data Handling (small → large)
- Existing list views that could have pagination (if pattern exists)
- Existing forms that could have auto-save (if pattern exists)
- Existing data that could have search (if pattern exists)
- Existing storage that could support new data types

#### G. Infrastructure Extensions (medium → complex)
- Existing plugin points that aren't fully utilized
- Existing event systems that could have new event types
- Existing caching that could cache more data
- Existing logging that could be extended

### Phase 4: Analyze Each Opportunity

For each promising opportunity:
1. Examine the pattern file closely using Read
2. See how it's used with Grep
3. Check for related implementations in the same directory

Analyze deeply:
- What pattern exists and where?
- How mature/established is the pattern?
- What exactly would be added/changed?
- What files would be affected?
- What existing code can be reused?
- What new code needs to be written?
- What is the effort estimation?

### Phase 5: Estimate Effort

Classify each opportunity by effort level:

| Level | Time | Description | Example |
|-------|------|-------------|---------|
| **Trivial** | 1-2 hours | Direct copy with minor changes | Add search to list (search exists elsewhere) |
| **Small** | Half day | Clear pattern to follow, some new logic | Add new filter type using existing filter pattern |
| **Medium** | 1-3 days | Pattern exists but needs adaptation | New CRUD entity using existing CRUD patterns |
| **Large** | 3-7 days | Architectural pattern enables new capability | Plugin system using existing extension points |
| **Complex** | 1-2 weeks | Foundation supports major addition | Multi-tenant using existing data layer patterns |

### Phase 6: Filter and Prioritize

Verify each idea:
1. **Pattern Exists** - The code pattern is already in the codebase
2. **Infrastructure Ready** - Dependencies are already in place
3. **Clear Implementation Path** - Can describe how to build it using existing patterns

Discard ideas that:
- Require fundamentally new architectural patterns
- Need significant research to understand approach
- Require strategic product decisions

### Phase 7: Generate Markdown Report

Generate a comprehensive report with 3-7 concrete code improvement ideas across different effort levels.

**Aim for a mix**:
- 1-2 trivial/small (quick wins for momentum)
- 2-3 medium (solid improvements)
- 1-2 large/complex (bigger opportunities the code enables)

Save report as: `code_improvements_[project-name]_[date].md`

## Report Structure

```markdown
# Code Improvements Report: [Project Name]

**Generated:** [Date/Time]
**Project:** [Auto-detected from workspace]
**Files Analyzed:** [Count]

---

## Executive Summary

[2-3 paragraph overview of patterns found and opportunities identified]

**Top 3 Quick Wins:**
1. [Trivial/Small improvement - high value, low effort]
2. [Trivial/Small improvement]
3. [Trivial/Small improvement]

**Top 3 Strategic Opportunities:**
1. [Large/Complex improvement - high value, architectural leverage]
2. [Large/Complex improvement]
3. [Large/Complex improvement]

---

## Effort Distribution

| Effort Level | Count | Total Estimated Time |
|--------------|-------|---------------------|
| Trivial | [X] | [Y] hours |
| Small | [X] | [Y] days |
| Medium | [X] | [Y] days |
| Large | [X] | [Y] days |
| Complex | [X] | [Y] weeks |

---

## Category 1: Pattern Extensions

### Improvement 1: [Title]
- **Effort:** ⚡ Trivial / 🔹 Small / 🔷 Medium / 🔶 Large / 💎 Complex
- **Category:** Pattern Extension
- **Estimated Time:** [Time]

**Builds Upon:**
- [Existing pattern or feature at file_path:line]

**Existing Pattern:**
[Description of existing pattern with file reference]

**Proposed Extension:**
[What would be added/changed]

**Affected Files:**
- `path/to/file1.ts` - [What changes]
- `path/to/file2.ts` - [What changes]

**Implementation Approach:**
1. [Step 1 - reference existing code]
2. [Step 2 - describe adaptation needed]
3. [Step 3 - describe new code]

**Benefits:**
- [Benefit 1]
- [Benefit 2]

**Rationale (Why Code-Revealed):**
[Explain why existing patterns/infrastructure enable this]

---

## Priority Matrix

| Improvement | Category | Effort | Value | Priority |
|-------------|----------|--------|-------|----------|
| [Title 1] | Pattern Ext | Trivial | High | ⭐⭐⭐ |
| [Title 2] | UI Enhancement | Small | High | ⭐⭐⭐ |
| [Title 3] | Data Handling | Medium | Medium | ⭐⭐ |
| [Title 4] | Architecture | Large | High | ⭐⭐ |
| [Title 5] | Infrastructure | Complex | High | ⭐ |

---

## Next Steps

- [ ] Review improvements with team
- [ ] Prioritize based on business value
- [ ] Create issues for high-priority items
- [ ] Schedule implementation sprints

---

## Patterns Discovered

**Reusable Patterns Found:**
- [Pattern 1] in [location] - Used [X] times
- [Pattern 2] in [location] - Used [X] times

**Underutilized Infrastructure:**
- [Infrastructure 1] - Could enable [opportunity]
- [Infrastructure 2] - Could enable [opportunity]

---

## Analysis Metadata

**Scan Date:** [ISO timestamp]
**Files Scanned:** [Count]
**Patterns Identified:** [Count]
**Improvements Suggested:** [Count]
**Total Estimated Effort:** [Rough sum]
**Generated By:** Claude Code - Code Improvements Skill
```

## Critical Rules

1. **ONLY suggest ideas with existing patterns** - If the pattern doesn't exist, it's not a code improvement
2. **Be specific about affected files** - List the actual files that would change
3. **Reference real patterns** - Point to actual code in the codebase with file paths and line numbers
4. **No strategic/PM thinking** - Focus on what code reveals, not user needs analysis
5. **Justify effort levels** - Each level should have clear reasoning
6. **Provide implementation approach** - Show how existing code enables the improvement
7. **Use code references** - Include file paths like `src/components/Button.tsx:42`

## Quality Checklist

Before finalizing the report:
- [ ] Each improvement references an existing pattern with file path
- [ ] Effort levels are justified based on code analysis
- [ ] Implementation approaches reference actual code
- [ ] Mix of effort levels (trivial to complex)
- [ ] No strategic product decisions (those aren't code-revealed)
- [ ] Specific file paths listed for all affected files
- [ ] Benefits clearly articulated

---

For examples of good vs bad code improvements, see [pattern-examples.md](pattern-examples.md).
