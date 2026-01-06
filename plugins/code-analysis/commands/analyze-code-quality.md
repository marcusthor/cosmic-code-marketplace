---
description: Analyze code quality issues and create refactoring tickets
argument-hint: [optional: specific module to analyze]
---

# Code Quality Analysis

I'll analyze code quality across 12 categories and identify refactoring opportunities.

## Step 1: Run Quality Analysis

Using the **code-quality** skill to check:
- Large files (>500 lines)
- Code smells (deep nesting, long methods)
- Complexity issues
- Duplication patterns
- Naming conventions
- File structure
- Linting issues
- Test coverage
- Type safety
- Dependencies
- Dead code
- Git hygiene

$ARGUMENTS

## Step 2: Create Refactoring Tickets

After analysis, I'll:
1. Create `improvements/code-quality/` directory
2. Generate individual ticket files for each issue
3. Format: `NNN-short-title.md` (e.g., `001-split-large-user-service.md`)
4. Severity indicators: 🔴 Critical, 🟠 Major, 🟡 Minor, 🟢 Suggestion
5. Each ticket contains:
   - Issue description
   - Current state (code examples)
   - Proposed refactoring
   - Benefits
   - Files to modify

**Starting quality analysis...**
