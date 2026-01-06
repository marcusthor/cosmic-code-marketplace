---
name: ui-ux-improvements
description: Analyze UI/UX quality, accessibility, and user experience. Use when improving usability, accessibility compliance, or visual consistency.
---

# UI/UX Improvements Analysis

Analyze user interface and user experience quality through component analysis, identifying concrete improvements to usability, accessibility, visual consistency, and interaction patterns.

**Your Role**: Senior UX designer and accessibility specialist focused on user-centric improvements.

## Workflow

### Phase 1: Component Structure Analysis

**Find UI components** using Glob:
```
src/components/**/*.tsx
src/components/**/*.jsx
src/components/ui/**/*
src/app/components/**/*
components/**/*
```

**Analyze component organization**:
- Design system presence (ui/ folder, shared components)
- Component naming conventions
- File structure clarity

**Read key UI components**:
- Button variants
- Form inputs
- Navigation components
- Layout components
- Modal/Dialog components

### Phase 2: Accessibility Audit

**Search for accessibility attributes** using Grep:
```
aria-label
aria-describedby
role=
alt=
tabIndex
htmlFor
```

**Check for missing accessibility**:
- Images without alt text: `<img(?![^>]*alt=)`
- Buttons without accessible names: `<button[^>]*>`
- Inputs without labels: `<input(?![^>]*aria-label)`
- Links without text: `<a href[^>]*>.*</a>`
- Missing keyboard navigation support

**Color contrast** (manual check in components):
- Text color vs background color
- Button text vs button background
- Link colors vs page background

### Phase 3: Interaction Patterns

**Search for state management** using Grep:
```
isLoading
isPending
isError
isSuccess
disabled
hover:
focus:
active:
```

**Check for user feedback**:
- Loading states (spinners, skeletons)
- Error states and messages
- Success feedback
- Disabled states
- Empty states

**Check for animations/transitions**:
```
transition
animate
duration
ease
transform
```

### Phase 4: Visual Consistency

**Analyze design tokens**:
- Read `tailwind.config.js` or `theme.ts`
- Check for color system
- Typography scale
- Spacing system
- Border radius values

**Search for hardcoded values** using Grep:
```
className=".*px-\d+
className=".*text-\[
style={{
rgb\(
#[0-9a-fA-F]{6}  # Hex colors
```

**Check for inconsistencies**:
- Multiple button styles
- Inconsistent spacing
- Mixed typography
- Varied component patterns

### Phase 5: Form Usability

**Find forms** using Glob:
```
**/forms/**/*.tsx
**/*Form.tsx
**/*form.tsx
```

**Analyze form patterns**:
- Label positioning and clarity
- Validation feedback
- Error message placement
- Submit button states
- Input grouping
- Required field indicators

**Search for form validation**:
```
required
pattern=
min=
max=
validate
error
```

### Phase 6: Responsive Design

**Check for responsive patterns** using Grep:
```
sm:
md:
lg:
xl:
@media
mobile
tablet
desktop
```

**Analyze breakpoint usage**:
- Mobile-first approach
- Touch target sizes (min 44x44px)
- Content reflow
- Navigation adaptations

### Phase 7: Generate Markdown Report

Create comprehensive UI/UX report: `ui_ux_improvements_[project-name]_[date].md`

## Report Structure

```markdown
# UI/UX Improvements Report: [Project Name]

**Generated:** [Date/Time]
**Components Analyzed:** [Count]
**Accessibility Score:** [X]/100

---

## Executive Summary

[2-3 paragraph overview of UI/UX state]

**UX Health:**
- 🔴 Critical Issues: [count] (blocking accessibility)
- 🟡 Medium Issues: [count] (usability friction)
- 🟢 Enhancements: [count] (polish improvements)

**Top 3 Priority Improvements:**
1. [High impact UX improvement]
2. [Critical accessibility issue]
3. [Quick visual consistency win]

---

## Category: Accessibility 🔴

### Issue 1: Missing Alt Text on Product Images

- **Category:** accessibility
- **Severity:** 🔴 Critical (WCAG 2.1 Level A failure)
- **Effort:** Small
- **User Impact:** Screen reader users cannot understand images

**Affected Components:**
- `src/components/ProductCard.tsx:45`
- `src/components/ImageGallery.tsx:23`
- `src/pages/Products.tsx:67`

**Current State:**
```tsx
// ProductCard.tsx - No alt text
<img src={product.imageUrl} className="w-full h-48 object-cover" />
```

**Proposed Change:**
```tsx
// Add descriptive alt text
<img
  src={product.imageUrl}
  alt={`${product.name} - ${product.category}`}
  className="w-full h-48 object-cover"
/>
```

**User Benefit:**
- Screen reader users can understand product images
- Improves SEO
- Better experience when images fail to load
- WCAG 2.1 Level A compliance

**WCAG Reference:** Success Criterion 1.1.1 Non-text Content

**Implementation Steps:**
1. Add alt prop to all `<img>` tags in ProductCard
2. Use product name + category as alt text
3. For decorative images, use `alt=""` (empty string)
4. Add ESLint rule `jsx-a11y/alt-text` to prevent future issues

---

## Category: Usability 🟡

### Issue 2: Missing Loading State on Submit Button

- **Category:** usability
- **Severity:** 🟡 Medium
- **Effort:** Small
- **User Impact:** Users unsure if form is submitting, may click multiple times

**Affected Components:**
- `src/components/ContactForm.tsx:89`
- `src/components/SignupForm.tsx:45`

**Current State:**
```tsx
// No loading feedback
<button type="submit" className="btn-primary">
  Submit
</button>
```

**Proposed Change:**
```tsx
<button
  type="submit"
  disabled={isSubmitting}
  className="btn-primary"
>
  {isSubmitting ? (
    <>
      <Spinner className="mr-2" />
      Submitting...
    </>
  ) : (
    'Submit'
  )}
</button>
```

**User Benefit:**
- Clear feedback during submission
- Prevents duplicate submissions
- Reduces user anxiety
- Professional feel

---

## Category: Visual Consistency 🟢

### Issue 3: Inconsistent Button Spacing

- **Category:** visual
- **Severity:** 🟢 Enhancement
- **Effort:** Small
- **User Impact:** More polished, professional appearance

**Current State:**
```tsx
// Mixed spacing values
<Button className="px-4 py-2">Primary</Button>    // Header.tsx:23
<Button className="px-6 py-3">Secondary</Button>  // Footer.tsx:45
<Button className="px-5 py-2.5">Tertiary</Button> // Sidebar.tsx:67
```

**Proposed Change:**
Create standardized button sizes in design system:

```tsx
// components/ui/button.tsx
const buttonSizes = {
  sm: 'px-3 py-1.5 text-sm',
  md: 'px-4 py-2 text-base',
  lg: 'px-6 py-3 text-lg',
};

<Button size="md">Consistent</Button>
```

---

## Category Breakdown

| Category | Critical | Medium | Low | Total |
|----------|----------|--------|-----|-------|
| Accessibility | 5 | 3 | 2 | 10 |
| Usability | 0 | 8 | 5 | 13 |
| Visual Consistency | 0 | 2 | 12 | 14 |
| Interaction | 1 | 4 | 3 | 8 |
| Responsive | 0 | 3 | 2 | 5 |

**Total Issues:** 50

---

## Accessibility Score: 73/100

### WCAG 2.1 Compliance

| Level | Status | Details |
|-------|--------|---------|
| **Level A** (Required) | 🟡 Partial | 5 failures (alt text, keyboard nav) |
| **Level AA** (Target) | 🔴 Failing | Color contrast, focus indicators |
| **Level AAA** (Enhanced) | 🔴 Failing | Not evaluated |

### Critical Accessibility Issues

1. **Missing alt text** - 15 images without alt attributes
2. **Poor color contrast** - 8 text/background combinations below 4.5:1
3. **Missing focus indicators** - Interactive elements lack visible focus
4. **Keyboard navigation** - 3 components not keyboard accessible
5. **Missing ARIA labels** - Icon-only buttons without labels

**See [accessibility-guide.md](accessibility-guide.md) for detailed WCAG checklist**

---

## Component Consistency Analysis

### Button Components

**Found 3 different button implementations:**
- `src/components/Button.tsx` (primary)
- `src/components/ui/button.tsx` (shadcn/ui)
- Inline button styles in 12 files

**Recommendation:**
- Consolidate to single Button component
- Use variants for different styles
- Add size prop for consistency

### Form Components

**Missing standardized components:**
- Text input variants
- Select dropdown component
- Checkbox/Radio components
- Error message component

**Recommendation:**
Create `src/components/ui/form/` with standardized form controls

---

## Quick Wins (High Impact, Low Effort)

1. **Add alt text to images** (30 minutes)
   - Add descriptive alt to all `<img>` tags
   - Use product/feature names
   - Empty alt for decorative images

2. **Add loading states to buttons** (1 hour)
   - Show spinner during async actions
   - Disable button while loading
   - Update button text ("Saving...")

3. **Improve focus indicators** (1 hour)
   - Add visible focus ring to all interactive elements
   - Use consistent focus style across site
   - Test with keyboard navigation

4. **Standardize button spacing** (2 hours)
   - Create size variants (sm, md, lg)
   - Apply consistently across components
   - Update design system documentation

---

## Usability Recommendations

### Navigation
- Add active state to current page in nav
- Add keyboard shortcuts for common actions
- Improve mobile menu accessibility

### Forms
- Move labels above inputs (easier to scan)
- Show validation on blur, not on change
- Add clear error messages near fields
- Show required field indicators upfront

### Feedback
- Add toast notifications for success/error
- Implement optimistic UI updates
- Show skeleton screens while loading
- Add empty state illustrations

---

## Visual Design Recommendations

### Typography
**Current:** Inconsistent heading sizes
**Recommendation:**
```typescript
const typography = {
  h1: 'text-4xl font-bold',
  h2: 'text-3xl font-semibold',
  h3: 'text-2xl font-semibold',
  h4: 'text-xl font-medium',
  body: 'text-base',
  small: 'text-sm',
};
```

### Spacing
**Current:** Mixed spacing values (12px, 16px, 20px, 24px)
**Recommendation:** Use spacing scale (4, 8, 12, 16, 24, 32, 48)

### Colors
**Current:** Hardcoded hex values in components
**Recommendation:** Move to design tokens in tailwind.config.js

---

## Interaction Improvements

### Micro-interactions
- Add subtle hover states to all clickable elements
- Implement smooth transitions (150-300ms)
- Add loading spinners for async actions
- Show success checkmarks after completion

### Animations
- Add enter/exit animations for modals
- Implement smooth scroll for anchor links
- Add skeleton screens for loading content
- Use spring animations for natural feel

### Touch Targets
**Issue:** Some buttons too small on mobile
**Recommendation:** Ensure minimum 44x44px touch targets

---

## Responsive Design Analysis

### Breakpoints Used
```
sm: 640px   ✅ Used
md: 768px   ✅ Used
lg: 1024px  ⚠️  Rarely used
xl: 1280px  ❌ Not used
```

### Mobile Issues
- Navigation menu overlaps content on small screens
- Form inputs full width on desktop (too wide)
- Images not optimized for mobile loading
- Touch targets too small (< 44px)

### Recommendations
- Implement hamburger menu for mobile
- Set max-width on form containers
- Use responsive images with srcset
- Increase button padding on mobile

---

## Next Steps

1. **Critical Accessibility** (Week 1)
   - [ ] Add alt text to all images
   - [ ] Fix color contrast issues
   - [ ] Implement keyboard navigation
   - [ ] Add focus indicators

2. **Usability Improvements** (Weeks 2-3)
   - [ ] Add loading states to async actions
   - [ ] Implement better error handling
   - [ ] Improve form validation feedback
   - [ ] Add empty states

3. **Visual Polish** (Week 4)
   - [ ] Standardize button components
   - [ ] Consolidate design tokens
   - [ ] Fix spacing inconsistencies
   - [ ] Add micro-interactions

4. **Responsive Refinement** (Ongoing)
   - [ ] Test on real devices
   - [ ] Optimize touch targets
   - [ ] Improve mobile navigation
   - [ ] Add responsive images

---

## Recommended Tools

**Accessibility Testing:**
- axe DevTools (browser extension)
- WAVE (web accessibility evaluation)
- Lighthouse accessibility audit
- Screen reader testing (NVDA, JAWS, VoiceOver)

**Visual Testing:**
- Storybook for component isolation
- Chromatic for visual regression
- Percy for screenshot testing

**Usability Testing:**
- Hotjar for user recordings
- UserTesting for feedback
- Google Analytics for metrics

---

## Analysis Metadata

**Analysis Date:** [ISO timestamp]
**Components Scanned:** [Count]
**Total Issues Found:** [Count]
**Critical:** [X] | **Medium:** [X] | **Low:** [X]
**Accessibility Score:** [X]/100
**Generated By:** Claude Code - UI/UX Improvements Skill
```

## Critical Rules

1. **Be Specific** - Reference exact files and line numbers
2. **Show Examples** - Include current state and proposed change code
3. **Prioritize Accessibility** - WCAG compliance is non-negotiable
4. **Focus on User Benefit** - Explain how each change helps users
5. **Provide Implementation Steps** - Make changes actionable
6. **Consider Existing Patterns** - Match the existing design system

## Quality Checklist

Before finalizing report:
- [ ] All issues reference specific component files
- [ ] Current state code examples are accurate
- [ ] Proposed changes are concrete and actionable
- [ ] User benefits are clearly articulated
- [ ] WCAG criteria are referenced for a11y issues
- [ ] Severity levels are justified
- [ ] Quick wins are identified

---

For detailed checklists, see:
- [ux-checklist.md](ux-checklist.md) - Usability patterns
- [accessibility-guide.md](accessibility-guide.md) - WCAG compliance guide
