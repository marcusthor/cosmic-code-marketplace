# Design Plugin

Design commands and skills for frontend development, UI/UX analysis, and documentation review.

## Commands

### `/design:ux`
UI/UX analysis with WCAG 2.1 accessibility compliance checking.

**Output**: `improvements/ux/NNN-title.md`

**Analyzes**:
- Component structure
- Accessibility (WCAG 2.1)
- Interaction patterns
- Visual consistency
- Form usability
- Responsive design

**Example Ticket**:
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

### `/design:documentation`
Documentation gap analysis across README, API docs, and inline comments.

**Output**: `improvements/documentation/NNN-title.md`

**Gap Categories**:
- README completeness
- API documentation
- Inline comments
- Code examples
- Architecture docs
- Troubleshooting guides

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

### 3. documentation-gaps
Identify documentation gaps across README, API docs, inline comments, and examples.

**Key Features**:
- 6 gap categories (README, API docs, inline comments, examples, architecture, troubleshooting)
- Audience targeting (developers, users, contributors, maintainers)
- Documentation coverage metrics
- Quick wins identification

## Usage

Skills are automatically invoked by Claude based on context:

```
"Create a dashboard UI"                → frontend-design skill
"Review accessibility compliance"      → ui-ux-improvements skill
"Find documentation gaps"              → documentation-gaps skill
```

## Installation

```bash
/plugin install design@cosmic-code-marketplace
```
