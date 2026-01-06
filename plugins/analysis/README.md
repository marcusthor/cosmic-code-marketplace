# Analysis Plugin

Code analysis commands and skills for quality audits, improvements, performance optimization, and security hardening.

## Commands

### `/analysis:code-quality`
Analyzes 12 quality categories and generates improvement tickets.

**Output**: `improvements/code-quality/NNN-title.md`

**Categories**:
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

### `/analysis:code-improvements`
Discovers improvement opportunities and generates tickets.

**Output**: `improvements/code-improvements/NNN-title.md`

**Analyzes**:
- Pattern extensions
- Architecture improvements
- Configuration enhancements
- Utility suggestions
- UI component patterns
- Data handling optimizations

### `/analysis:performance`
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

### `/analysis:security`
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

## Skills

Skills are automatically invoked by Claude based on context:

```
"Analyze code quality issues"          → code-quality skill
"How can I improve performance?"       → performance-optimization skill
"Audit security vulnerabilities"       → security-hardening skill
"Find improvement opportunities"       → code-improvements skill
```

## Installation

```bash
/plugin install analysis@cosmic-code-marketplace
```
