# Code Analysis Plugin

Comprehensive code analysis suite with automatic ticket generation.

## Skills

### 1. code-quality
Analyzes 12 quality categories:
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

### 2. code-improvements
Discovers improvement opportunities:
- Pattern extensions
- Architecture improvements
- Configuration enhancements
- Utility suggestions
- UI component patterns
- Data handling optimizations
- Infrastructure improvements

### 3. performance-optimization
Identifies bottlenecks:
- Bundle size analysis
- Runtime performance
- Memory usage
- Database queries (N+1)
- Network optimization
- Rendering performance
- Caching strategies

### 4. security-hardening
OWASP Top 10 security audit:
- Authentication issues
- Authorization gaps
- Input validation (SQL injection, XSS)
- Data protection
- Dependency vulnerabilities
- Configuration security
- Secrets management

## Slash Commands

### `/analyze-code-quality`
Runs code quality analysis and generates tickets.

**Output**: `improvements/code-quality/NNN-title.md`

**Example Ticket**:
```markdown
# Split Large User Service File

**Category:** Large Files
**Severity:** 🟠 Major
**Effort:** Medium

## Affected Files
- `src/services/UserService.ts:1-850`

## Current State
File has 850 lines with multiple responsibilities...

## Proposed Change
Split into domain-specific services...

## Implementation Steps
1. Extract authentication logic to AuthService
2. Move profile management to ProfileService
3. ...
```

### `/analyze-performance`
Performance optimization analysis with tickets.

**Output**: `improvements/performance/NNN-title.md`

### `/analyze-security`
Security vulnerability scan with tickets.

**Output**: `improvements/security/NNN-title.md`

### `/analyze-code-improvements`
Pattern-based improvement suggestions.

**Output**: `improvements/code-improvements/NNN-title.md`

## Usage Examples

### Run analysis
```bash
/code-analysis:analyze-security
```

### Implement specific ticket
```
"Please implement improvements/security/001-fix-sql-injection.md"
```

### Batch process tickets
```
"Implement all critical security tickets in improvements/security/"
```

## Installation

```bash
/plugin install code-analysis@claude-marketplace
```
