# Design Toolkit Plugin

Frontend design, UI/UX optimization, and documentation gap analysis.

## Skills

### 1. frontend-design
Create distinctive, production-grade frontend interfaces with high design quality.

**Key Features**:
- Design thinking workflow
- Typography guidance
- Color/theme strategies
- Motion animations
- Spatial composition
- Visual details
- Avoiding generic AI aesthetics

### 2. ui-ux-improvements
Analyze UI/UX quality, accessibility, and user experience.

**Key Features**:
- Component structure analysis
- WCAG 2.1 accessibility audit
- Interaction patterns
- Visual consistency
- Form usability
- Responsive design
- Accessibility scoring

**References**:
- [ux-checklist.md](skills/ui-ux-improvements/ux-checklist.md) - Usability patterns
- [accessibility-guide.md](skills/ui-ux-improvements/accessibility-guide.md) - WCAG compliance guide

### 3. documentation-gaps
Identify documentation gaps across README, API docs, inline comments, and examples.

**Key Features**:
- 6 gap categories (README, API docs, inline comments, examples, architecture, troubleshooting)
- Audience targeting (developers, users, contributors, maintainers)
- Documentation coverage metrics
- Quick wins identification

## Slash Commands

### `/analyze-ux`
Runs UI/UX analysis and generates improvement tickets.

**Output**: `improvements/ux/NNN-title.md`

**Example**:
```markdown
# Add Alt Text to Product Images

**Category:** Accessibility
**Severity:** 🔴 Critical (WCAG Level A)
**Effort:** Small

## Affected Components
- `src/components/ProductCard.tsx:45`

## Current State
Images missing alt attributes...

## Proposed Change
Add descriptive alt text...

## WCAG Reference
Success Criterion 1.1.1 Non-text Content
```

### `/analyze-documentation`
Documentation gap analysis with tickets.

**Output**: `improvements/documentation/NNN-title.md`

## Usage

```bash
# Run UX analysis
/design-toolkit:analyze-ux

# Check documentation gaps
/design-toolkit:analyze-documentation
```

## Installation

```bash
/plugin install design-toolkit@cosmic-code-marketplace
```
