---
name: documentation-gaps
description: Identify documentation gaps in README, API docs, inline comments, and examples. Use when improving project documentation or onboarding new developers.
---

# Documentation Gaps Analysis

Analyze codebases to identify documentation gaps across README, API documentation, inline comments, examples, architecture docs, and troubleshooting guides.

**Your Role**: Expert technical writer and documentation specialist focused on developer experience and onboarding.

## Workflow

### Phase 1: Scan Existing Documentation

**Find all documentation files** using Glob:
```
README.md
**/README.md
docs/**/*.md
**/*.mdx
CONTRIBUTING.md
CHANGELOG.md
```

**Analyze documentation content**:
- README completeness (overview, installation, usage, contributing)
- docs/ structure and coverage
- Inline JSDoc/docstrings (search for `/** */`, `"""`)
- Examples and tutorials
- Architecture diagrams
- FAQ/troubleshooting sections

**Check documentation freshness**:
- Last updated dates
- References to deprecated APIs
- Outdated installation steps

### Phase 2: Analyze Code Surface

**Identify public APIs** using Grep:
```
export (function|const|class|interface|type)
```

**Find complex functions** that need documentation:
- Functions > 30 lines
- High cyclomatic complexity (many branches)
- Functions with > 3 parameters

**Locate configuration options**:
- Environment variables
- Config files
- Feature flags

**Find magic numbers and constants**:
- Unexplained numeric values
- Hard-coded strings
- Non-obvious constants

### Phase 3: Cross-Reference Documentation vs Code

**For each public API**:
1. Check if documented in:
   - JSDoc/docstring
   - README examples
   - API docs
   - Type definitions

2. Assess documentation quality:
   - Parameters explained?
   - Return values documented?
   - Errors/exceptions listed?
   - Usage examples provided?

**Identify gaps**:
- Exported but undocumented
- Documented but outdated
- Complex but unexplained
- Frequently used but poorly documented

### Phase 4: Prioritize by Audience & Impact

**Target Audiences**:
1. **developers** - Internal team members working on the codebase
2. **users** - End users of the application/library
3. **contributors** - Open source contributors or new team members
4. **maintainers** - Long-term maintenance and operations

**Impact Priority**:
- **High**: Entry points (README, getting started), critical APIs, onboarding blockers
- **Medium**: Frequently used APIs, complex areas, configuration
- **Low**: Internal utilities, advanced features, edge cases

### Phase 5: Categorize Gaps

Classify findings into 6 categories:

#### 1. README Improvements
- Missing or incomplete project overview
- Outdated installation instructions
- Missing usage examples
- Incomplete configuration documentation
- Missing contributing guidelines
- No badges (build status, coverage, version)

#### 2. API Documentation
- Undocumented public functions/methods
- Missing parameter descriptions
- Unclear return value documentation
- Missing error/exception documentation
- Incomplete type definitions
- No usage examples for complex APIs

#### 3. Inline Comments
- Complex algorithms without explanations
- Non-obvious business logic
- Workarounds or hacks without context
- Magic numbers or constants without meaning
- TODOs without context or tracking
- Commented-out code without explanation

#### 4. Examples & Tutorials
- Missing getting started guide
- Incomplete code examples
- Outdated sample code
- Missing common use case examples
- No interactive examples/playground
- Missing integration examples

#### 5. Architecture Documentation
- Missing system overview diagrams
- Undocumented data flow
- Missing component relationships
- Unclear module responsibilities
- No decision records (ADRs)
- Missing deployment/infrastructure docs

#### 6. Troubleshooting
- Common errors without solutions
- Missing FAQ section
- Undocumented debugging tips
- Missing migration guides (for breaking changes)
- No performance tuning guide
- Missing common pitfalls documentation

### Phase 6: Generate Markdown Report

Create comprehensive documentation gaps report: `documentation_gaps_[project-name]_[date].md`

## Report Structure

```markdown
# Documentation Gaps Report: [Project Name]

**Generated:** [Date/Time]
**Files Analyzed:** [Count]
**Documentation Coverage:** [X]%

---

## Executive Summary

[2-3 paragraph overview of documentation state]

**Documentation Health:**
- 📄 Documentation files found: [count]
- ✅ Documented functions: [X]
- ❌ Undocumented functions: [X]
- 📅 README last updated: [date]

**Top 3 Priority Gaps:**
1. [High priority gap - biggest impact on new developers]
2. [High priority gap]
3. [High priority gap]

---

## Priority High 🔴

### Gap 1: [Title]
- **Category:** [readme/api_docs/inline_comments/examples/architecture/troubleshooting]
- **Target Audience:** [developers/users/contributors/maintainers]
- **Priority:** 🔴 High
- **Effort:** [small/medium/large]

**Affected Areas:**
- `src/auth/token.ts:45` - `validateToken()` function
- `src/auth/session.ts:89` - `refreshSession()` function
- `src/auth/index.ts` - Module exports

**Current Documentation State:**
[Description of what exists, if anything]

**Missing Documentation:**
[Specific gaps identified]

**Proposed Content:**
[What should be added]

**Why This Matters:**
[Impact on developers/users - why this is high priority]

**Example of Needed Documentation:**
```typescript
/**
 * Validates an authentication token and returns user data
 *
 * @param token - JWT token string to validate
 * @param options - Validation options
 * @returns Decoded user data if valid
 * @throws {TokenExpiredError} If token has expired
 * @throws {InvalidTokenError} If token is malformed or invalid
 *
 * @example
 * ```ts
 * const userData = await validateToken(token, { refresh: true });
 * console.log(userData.userId);
 * ```
 */
export async function validateToken(
  token: string,
  options?: ValidationOptions
): Promise<UserData>
```

---

## Priority Medium 🟡

[Same structure as High priority]

---

## Priority Low 🟢

[Same structure]

---

## Category Breakdown

| Category | High | Medium | Low | Total |
|----------|------|--------|-----|-------|
| README Improvements | 2 | 3 | 1 | 6 |
| API Documentation | 5 | 8 | 12 | 25 |
| Inline Comments | 1 | 4 | 15 | 20 |
| Examples & Tutorials | 3 | 2 | 1 | 6 |
| Architecture Docs | 1 | 2 | 0 | 3 |
| Troubleshooting | 0 | 2 | 3 | 5 |

**Total Gaps:** [X]

---

## Audience Breakdown

| Audience | Gaps | Priority Areas |
|----------|------|----------------|
| **Developers** (Internal team) | [X] | API docs, inline comments, architecture |
| **Users** (End users) | [X] | README, examples, troubleshooting |
| **Contributors** (New team) | [X] | README, architecture, contributing guide |
| **Maintainers** (Ops) | [X] | Architecture, deployment, troubleshooting |

---

## Documentation Coverage by Module

| Module | Files | Documented | Undocumented | Coverage |
|--------|-------|------------|--------------|----------|
| `src/auth/` | 8 | 3 | 5 | 37% |
| `src/api/` | 12 | 8 | 4 | 67% |
| `src/components/` | 45 | 25 | 20 | 56% |
| `src/utils/` | 18 | 15 | 3 | 83% |

**Overall Coverage:** [X]%

---

## Quick Wins (High Impact, Low Effort)

These documentation improvements provide maximum value for minimal effort:

1. **Add README badges** (5 minutes)
   - Build status
   - Test coverage
   - Version
   - License

2. **Document environment variables** (30 minutes)
   - Create `.env.example`
   - Add descriptions to README
   - List required vs optional

3. **Add JSDoc to top 10 most-used functions** (2 hours)
   - Parameters
   - Return values
   - Basic examples

4. **Create CONTRIBUTING.md** (1 hour)
   - Setup instructions
   - Development workflow
   - Testing guidelines

---

## Documentation Improvement Roadmap

### Phase 1: Critical Foundations (Week 1)
- [ ] Update README with current installation steps
- [ ] Document all public API functions
- [ ] Create getting started guide

### Phase 2: User Experience (Weeks 2-3)
- [ ] Add code examples for common use cases
- [ ] Create troubleshooting guide
- [ ] Add FAQ section

### Phase 3: Developer Experience (Week 4+)
- [ ] Add inline comments for complex logic
- [ ] Create architecture documentation
- [ ] Add ADRs for key decisions

### Phase 4: Ongoing Maintenance
- [ ] Set up automated doc generation
- [ ] Add doc linting to CI
- [ ] Schedule quarterly doc reviews

---

## Recommendations

**Tools to Consider:**
- TypeDoc for auto-generating API docs
- Storybook for component documentation
- docusaurus for documentation site
- JSDoc linting with ESLint

**Documentation Standards:**
- Require JSDoc for all exported functions
- Document all public APIs before merging
- Keep examples up-to-date with tests
- Review docs quarterly for freshness

**Quick Checks:**
```bash
# Find undocumented exports
grep -r "export function" --include="*.ts" | while read line; do
  file=$(echo $line | cut -d: -f1)
  grep -B2 "$line" "$file" | grep -q "/\*\*" || echo "Missing: $line"
done

# Find outdated dates in docs
find docs -name "*.md" -exec grep -l "2023" {} \;
```

---

## Next Steps

1. **Review with Team** - Prioritize based on team needs
2. **Assign Ownership** - Who documents each module?
3. **Set Documentation Standards** - What level of detail?
4. **Add to CI/CD** - Enforce documentation on new code
5. **Schedule Maintenance** - Quarterly documentation reviews

---

## Analysis Metadata

**Analysis Date:** [ISO timestamp]
**Files Scanned:** [Count]
**Total Gaps Identified:** [Count]
**Estimated Total Effort:** [Hours]
**Documentation Coverage:** [X]%
**Generated By:** Claude Code - Documentation Gaps Skill
```

## Critical Rules

1. **Be Specific** - Point to exact files and functions, not vague areas
2. **Prioritize Impact** - Focus on what helps new developers most
3. **Consider Audience** - Distinguish between user docs and contributor docs
4. **Realistic Scope** - Each gap should be addressable in one session
5. **Avoid Redundancy** - Don't suggest docs that exist in different form
6. **Show Examples** - Include examples of good documentation

## Quality Checklist

Before finalizing report:
- [ ] All gaps have specific file/function references
- [ ] Priority levels are justified
- [ ] Audience is identified for each gap
- [ ] Effort estimates are realistic
- [ ] Examples show what good docs look like
- [ ] Quick wins are highlighted
- [ ] Coverage metrics are included

---

**Remember**: Good documentation is an investment that pays dividends in reduced support burden, faster onboarding, and better code quality.
