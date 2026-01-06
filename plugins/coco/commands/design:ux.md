---
description: Analyze UI/UX quality and create improvement tickets
argument-hint: [optional: specific component or flow]
---

# UI/UX Improvements Analysis

I'll analyze user experience quality across 5 categories and identify improvements.

## Step 1: UX Analysis

Using the **ui-ux-improvements** skill to check:
- **Accessibility** - WCAG 2.1 compliance, screen reader support
- **Usability** - Navigation clarity, form UX, feedback mechanisms
- **Visual Consistency** - Design tokens, spacing, typography
- **Interaction** - Loading states, animations, micro-interactions
- **Responsive Design** - Mobile-first, touch targets, breakpoints

Checking:
- Component structure and patterns
- Color contrast ratios
- Keyboard navigation
- Form validation
- Error states
- Empty states

$ARGUMENTS

## Step 2: Create UX Tickets

After analysis, I'll:
1. Create `improvements/ux/` directory
2. Generate individual ticket files for each issue
3. Format: `NNN-short-title.md` (e.g., `001-add-alt-text-to-images.md`)
4. Categories: Accessibility, Usability, Visual, Interaction, Responsive
5. Each ticket contains:
   - Current UX issue
   - User impact
   - WCAG reference (for a11y issues)
   - Proposed improvement
   - Implementation steps
   - Before/after code examples

**Starting UX analysis...**
