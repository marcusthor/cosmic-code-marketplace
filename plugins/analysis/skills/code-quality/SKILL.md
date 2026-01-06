---
name: code-quality
description: Analyze code quality, identify refactoring opportunities, and detect code smells. Use when reviewing technical debt, improving maintainability, or planning refactoring work.
---

# Code Quality & Refactoring Analysis

Analyze codebases to identify refactoring opportunities, code smells, best practice violations, and areas that could benefit from improved code quality.

**Your Role**: Senior software architect and code quality expert focused on maintainability and developer experience.

## Workflow

### Phase 1: Analyze File Sizes

Identify large files that should be split:

**Use Glob to find files**:
```
**/*.{ts,tsx,js,jsx,py}
```

**Check file sizes with Bash**:
```bash
find . -type f \( -name "*.ts" -o -name "*.tsx" -o -name "*.js" -o -name "*.jsx" \) -exec wc -l {} \; | sort -rn | head -20
```

**Large File Thresholds** (context-dependent):
- Component files: > 400-500 lines
- Utility/service files: > 600-800 lines
- Test files: > 800 lines (acceptable if well-organized)
- Single-purpose modules: > 1000 lines (definite split candidate)

Look for:
- Monolithic components/modules
- "God objects" with too many responsibilities
- Single files handling multiple concerns

### Phase 2: Detect Code Patterns

Search for code smells using Grep:

**Duplicated Code**:
- Search for similar function names
- Find repeated patterns
- Identify copy-paste candidates

**Long Functions**:
- Count function lengths with line numbers
- Identify functions > 50 lines

**Complex Nesting**:
- Search for deep nesting: `if.*if.*if.*if`
- Find callback hell patterns

**Parameter Lists**:
- Search for functions with many parameters
- Pattern: `function.*\(.*,.*,.*,.*,.*\)`

**Type Safety Issues**:
- Search for: `any`
- Find missing types
- Check for excessive type assertions

### Phase 3: Review Configuration

Check for quality tooling:

**Linting Configuration**:
- `.eslintrc`, `.prettierrc`, `pyproject.toml`
- Check if rules are strict enough
- Verify consistency

**TypeScript Configuration**:
- `tsconfig.json` strictness
- Check `strict`, `noImplicitAny`, `strictNullChecks`

**Test Setup**:
- Test framework configuration
- Coverage thresholds
- Test file patterns

### Phase 4: Analyze Project Structure

**Module Organization**:
- Use Glob to map directory structure
- Check for circular dependencies (import/require patterns)
- Identify misplaced files

**Folder Organization**:
- Assess if structure matches domain
- Check for consistent patterns
- Look for barrel files (index.ts) usage

### Phase 5: Review Dependencies

Read `package.json` (or equivalent):
- Check for unused dependencies
- Identify duplicates (e.g., `lodash` and `lodash-es`)
- Find outdated dev tooling
- Verify peer dependencies

### Phase 6: Categorize Findings

Classify each finding into one of 12 categories (see [quality-categories.md](quality-categories.md) for details):

1. **Large Files** - Files that should be split
2. **Code Smells** - Design problems (long methods, deep nesting)
3. **High Complexity** - Complex conditionals, many branches
4. **Code Duplication** - Repeated code
5. **Naming Conventions** - Unclear names, inconsistency
6. **File Structure** - Organization, circular dependencies
7. **Linting Issues** - Missing config, formatting
8. **Test Coverage** - Missing tests
9. **Type Safety** - Missing types, excessive `any`
10. **Dependency Issues** - Unused, outdated, duplicates
11. **Dead Code** - Unused code, commented blocks
12. **Git Hygiene** - Commit practices, hooks

### Phase 7: Assign Severity

Classify each finding by severity:

| Severity | Description | Examples |
|----------|-------------|----------|
| **🔴 Critical** | Blocks development, causes bugs | Circular deps, type errors, broken builds |
| **🟠 Major** | Significant maintainability impact | Large files (>1000 lines), high complexity |
| **🟡 Minor** | Should be addressed but not urgent | Duplication, naming issues |
| **🟢 Suggestion** | Nice to have improvements | Style consistency, minor optimizations |

### Phase 8: Generate Markdown Report

Create comprehensive report: `code_quality_[project-name]_[date].md`

## Report Structure

```markdown
# Code Quality Report: [Project Name]

**Generated:** [Date/Time]
**Files Analyzed:** [Count]
**Issues Found:** [Total by severity]

---

## Executive Summary

[2-3 paragraph overview of code quality state]

**Health Score**: [X/100]
- 🔴 Critical Issues: [count]
- 🟠 Major Issues: [count]
- 🟡 Minor Issues: [count]
- 🟢 Suggestions: [count]

**Top 3 Priority Refactorings:**
1. [Critical/Major issue with highest impact]
2. [Critical/Major issue]
3. [High-value improvement]

---

## Critical Issues 🔴

### Issue 1: [Title]
- **Category:** [Category name]
- **Severity:** 🔴 Critical
- **Effort:** [small/medium/large]
- **Breaking Change:** [Yes/No]

**Affected Files:**
- `path/to/file1.ts:45` - [Issue description]
- `path/to/file2.ts:89` - [Issue description]

**Current State:**
[Description of problem]

**Problem Example:**
```typescript
// Current problematic code
function badExample(data: any) {
  // Deep nesting, unclear logic
  if (data) {
    if (data.user) {
      if (data.user.id) {
        ...
      }
    }
  }
}
```

**Proposed Change:**
[Specific refactoring steps]

**Improved Example:**
```typescript
// Proposed improvement
function goodExample(data: UserData) {
  const userId = data?.user?.id;
  if (!userId) return;
  ...
}
```

**Rationale:**
[Why this is a problem and why the solution works]

**Best Practice:**
[Reference to design principle/best practice]

**Metrics:**
- Lines of code: [number]
- Cyclomatic complexity: [number]
- Duplicate lines: [number]

**Implementation Steps:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Prerequisites:**
- [Prerequisite 1]
- [Prerequisite 2]

---

## Major Issues 🟠

[Same structure as Critical]

---

## Minor Issues 🟡

[Same structure]

---

## Suggestions 🟢

[Same structure]

---

## Category Breakdown

| Category | Critical | Major | Minor | Suggestions | Total |
|----------|----------|-------|-------|-------------|-------|
| Large Files | 0 | 3 | 2 | 0 | 5 |
| Code Smells | 1 | 2 | 4 | 3 | 10 |
| Complexity | 0 | 2 | 1 | 0 | 3 |
| Duplication | 0 | 1 | 3 | 2 | 6 |
| Naming | 0 | 0 | 2 | 5 | 7 |
| Structure | 1 | 1 | 0 | 0 | 2 |
| Linting | 0 | 1 | 2 | 3 | 6 |
| Testing | 0 | 2 | 3 | 1 | 6 |
| Type Safety | 0 | 1 | 4 | 2 | 7 |
| Dependencies | 0 | 0 | 1 | 2 | 3 |
| Dead Code | 0 | 0 | 2 | 3 | 5 |
| Git Hygiene | 0 | 0 | 1 | 4 | 5 |

---

## Effort vs Impact Matrix

| Issue | Severity | Effort | Impact | Priority |
|-------|----------|--------|--------|----------|
| [Title] | 🔴 Critical | Small | High | ⭐⭐⭐ |
| [Title] | 🟠 Major | Medium | High | ⭐⭐⭐ |
| [Title] | 🟠 Major | Small | Medium | ⭐⭐ |
| [Title] | 🟡 Minor | Large | Low | ⭐ |

**Priority Calculation:**
- High Priority (⭐⭐⭐): High impact + Low/Medium effort
- Medium Priority (⭐⭐): Medium impact OR high impact + high effort
- Low Priority (⭐): Low impact OR low value + high effort

---

## Refactoring Roadmap

### Phase 1: Critical Fixes (Week 1)
- [ ] [Critical issue 1]
- [ ] [Critical issue 2]

### Phase 2: Major Improvements (Weeks 2-3)
- [ ] [Major issue 1]
- [ ] [Major issue 2]
- [ ] [Major issue 3]

### Phase 3: Minor Clean-up (Week 4+)
- [ ] [Minor issues batch 1]
- [ ] [Minor issues batch 2]

### Phase 4: Suggestions (Ongoing)
- [ ] [Suggestion 1]
- [ ] [Suggestion 2]

---

## Quality Metrics

**Code Health Indicators:**
- Average file size: [X] lines
- Largest file: [path] ([X] lines)
- Files > 500 lines: [count]
- Test coverage: [X]% (if available)
- TypeScript strict mode: [Yes/No]
- Linting configured: [Yes/No]

**Dependency Health:**
- Total dependencies: [count]
- Unused dependencies: [count]
- Outdated dependencies: [count]

**Test Health:**
- Files with tests: [X]%
- Untested critical files: [count]

---

## Configuration Recommendations

**ESLint/Prettier:**
```json
// Recommended additions to .eslintrc
{
  "rules": {
    "max-lines": ["warn", 500],
    "complexity": ["warn", 10],
    "max-params": ["warn", 4]
  }
}
```

**TypeScript:**
```json
// Recommended tsconfig.json strictness
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

---

## Next Steps

1. **Review with Team** - Discuss priority and approach
2. **Create Issues** - Track refactorings in backlog
3. **Plan Sprints** - Schedule refactoring work
4. **Add Tests** - Ensure coverage before refactoring
5. **Refactor Incrementally** - Small, safe changes
6. **Monitor Metrics** - Track improvement over time

---

## Analysis Metadata

**Analysis Date:** [ISO timestamp]
**Files Scanned:** [Count]
**Total Issues:** [Count]
**Critical:** [X] | **Major:** [X] | **Minor:** [X] | **Suggestions:** [X]
**Estimated Total Effort:** [rough estimate]
**Generated By:** Claude Code - Code Quality Skill
```

## Critical Rules

1. **Prioritize Impact** - Focus on issues that most affect maintainability
2. **Provide Clear Steps** - Each finding includes how to fix it
3. **Flag Breaking Changes** - Note refactorings that might break code
4. **Identify Prerequisites** - Note if something should be done first
5. **Be Realistic About Effort** - Accurately estimate work required
6. **Include Code Examples** - Show before/after
7. **Consider Trade-offs** - Sometimes "imperfect" code is acceptable

## Quality Checklist

Before finalizing report:
- [ ] All findings have severity classification
- [ ] Code examples show before/after
- [ ] Implementation steps are clear
- [ ] Effort estimates are realistic
- [ ] Breaking changes are flagged
- [ ] Prerequisites are noted
- [ ] Best practices are referenced
- [ ] Metrics are included where applicable

---

For detailed category explanations and examples, see [quality-categories.md](quality-categories.md).
