# Coco - Cosmic Code Commands

Unified command suite for code analysis, design improvements, and competitive research.

## Commands

### Analysis Commands

#### `/coco:analysis:code-quality`
Runs code quality analysis and generates improvement tickets.

**Output**: `improvements/code-quality/NNN-title.md`

**Categories Analyzed**:
- Large files (>500 lines)
- Code smells
- Complexity
- Duplication
- Naming conventions
- File structure
- Linting issues
- Test coverage
- Type safety
- Dependencies
- Dead code
- Git hygiene

#### `/coco:analysis:code-improvements`
Discovers code improvement opportunities and creates tickets.

**Output**: `improvements/code-improvements/NNN-title.md`

**Analyzes**:
- Pattern extensions
- Architecture improvements
- Configuration enhancements
- Utility suggestions
- UI component patterns
- Data handling optimizations

#### `/coco:analysis:performance`
Identifies performance bottlenecks and generates optimization tickets.

**Output**: `improvements/performance/NNN-title.md`

**Analyzes**:
- Bundle size
- Runtime performance
- Memory usage
- Database queries (N+1)
- Network optimization
- Rendering performance
- Caching strategies

#### `/coco:analysis:security`
Security vulnerability scan with OWASP Top 10 mapping.

**Output**: `improvements/security/NNN-title.md`

**Analyzes**:
- Authentication issues
- Authorization gaps
- Input validation (SQL injection, XSS)
- Data protection
- Dependency vulnerabilities
- Configuration security
- Secrets management

### Design Commands

#### `/coco:design:ux`
UI/UX analysis with WCAG 2.1 accessibility compliance checking.

**Output**: `improvements/ux/NNN-title.md`

**Analyzes**:
- Component structure
- Accessibility (WCAG 2.1)
- Interaction patterns
- Visual consistency
- Form usability
- Responsive design

#### `/coco:design:documentation`
Documentation gap analysis across README, API docs, and inline comments.

**Output**: `improvements/documentation/NNN-title.md`

**Gap Categories**:
- README completeness
- API documentation
- Inline comments
- Code examples
- Architecture docs
- Troubleshooting guides

### Research Commands

#### `/coco:research:competitors`
Competitive analysis and market research report generation.

**Output**: Markdown report with:
- Competitor overview
- User pain points by severity
- Market gaps and opportunities
- Feature comparison matrix
- Recommendations

## Usage

```bash
# Run code quality analysis
/coco:analysis:code-quality

# Analyze performance
/coco:analysis:performance

# Check accessibility
/coco:design:ux

# Research competitors
/coco:research:competitors "project management SaaS"
```

## Ticket Workflow

All analysis commands generate individual ticket files:

```
improvements/
├── code-quality/
│   ├── 001-split-large-user-service.md
│   └── 002-reduce-complexity-auth.md
├── performance/
│   ├── 001-optimize-bundle-size.md
│   └── 002-add-react-memo.md
├── security/
│   ├── 001-fix-sql-injection.md
│   └── 002-add-rate-limiting.md
├── ux/
│   ├── 001-add-alt-text-images.md
│   └── 002-improve-keyboard-navigation.md
└── documentation/
    ├── 001-add-api-examples.md
    └── 002-document-error-codes.md
```

Each ticket contains:
- Problem description
- Affected files with line numbers
- Current state
- Proposed solution
- Implementation steps

## Installation

```bash
/plugin install coco@cosmic-code-marketplace
```
