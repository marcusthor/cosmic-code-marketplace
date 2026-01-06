# Accessibility Guide - WCAG 2.1 Compliance Checklist

Complete reference for Web Content Accessibility Guidelines (WCAG) 2.1 compliance.

## WCAG Conformance Levels

| Level | Description | Legal Requirement |
|-------|-------------|-------------------|
| **Level A** | Minimum accessibility | Required in many jurisdictions |
| **Level AA** | Recommended standard | Target for most websites |
| **Level AAA** | Enhanced accessibility | Aspirational, not always achievable |

**Target:** WCAG 2.1 Level AA compliance

---

## Principle 1: Perceivable

Information must be presentable to users in ways they can perceive.

### 1.1 Text Alternatives (Level A)

**1.1.1 Non-text Content**

**Requirement:** All non-text content has text alternative.

**Images:**
```tsx
// ❌ Missing alt text
<img src="/product.jpg" />

// ✅ Descriptive alt text
<img src="/product.jpg" alt="Blue running shoes with white laces" />

// ✅ Decorative image (empty alt)
<img src="/decorative-line.svg" alt="" role="presentation" />

// ✅ Complex image (long description)
<img
  src="/chart.png"
  alt="Sales chart showing 40% growth"
  aria-describedby="chart-description"
/>
<p id="chart-description">
  Detailed sales data shows revenue increased from $100K to $140K...
</p>
```

**Icon buttons:**
```tsx
// ❌ No accessible name
<button><TrashIcon /></button>

// ✅ Accessible name
<button aria-label="Delete item">
  <TrashIcon />
</button>

// ✅ Alternative with visible text
<button>
  <TrashIcon aria-hidden="true" />
  <span>Delete</span>
</button>
```

**Input images:**
```tsx
// ✅ Image input with alt
<input type="image" src="search.png" alt="Search" />
```

---

### 1.2 Time-based Media (Level A)

**1.2.1 Audio-only and Video-only**
- Provide transcript for audio-only content
- Provide audio track or transcript for video-only

**1.2.2 Captions (Prerecorded)**
- All prerecorded video with audio has captions

**1.2.3 Audio Description or Media Alternative**
- Provide audio description or transcript for prerecorded video

---

### 1.3 Adaptable (Level A)

**1.3.1 Info and Relationships**

**Semantic HTML:**
```tsx
// ❌ Non-semantic
<div className="heading">Title</div>
<div className="list">
  <div className="item">Item 1</div>
  <div className="item">Item 2</div>
</div>

// ✅ Semantic
<h1>Title</h1>
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```

**Form labels:**
```tsx
// ❌ No label association
<span>Email</span>
<input type="email" />

// ✅ Proper label association
<label htmlFor="email">Email</label>
<input id="email" type="email" />

// ✅ Alternative with aria-label
<input type="email" aria-label="Email address" />
```

**1.3.2 Meaningful Sequence**

Content order in DOM must be logical:
```tsx
// ✅ Logical reading order
<article>
  <h2>Article Title</h2>
  <p className="byline">By Author Name</p>
  <p>Article content...</p>
</article>
```

**1.3.3 Sensory Characteristics**

Don't rely solely on sensory characteristics:
```tsx
// ❌ Relies only on shape
<p>Click the round button to continue</p>

// ✅ Includes text label
<p>Click the "Continue" button (round, green) to proceed</p>
```

---

### 1.4 Distinguishable (Level AA)

**1.4.1 Use of Color (Level A)**

Don't use color as only means of conveying information:
```tsx
// ❌ Color only
<p style={{ color: 'red' }}>Error</p>

// ✅ Color + icon + text
<p className="text-red-600">
  <ErrorIcon aria-hidden="true" />
  Error: Invalid email address
</p>
```

**1.4.3 Contrast (Minimum) (Level AA)**

**Requirement:** 4.5:1 for normal text, 3:1 for large text

**Color contrast ratios:**
```css
/* ❌ Insufficient contrast (2.5:1) */
.text-gray {
  color: #999;  /* On white background */
}

/* ✅ Sufficient contrast (4.6:1) */
.text-gray {
  color: #767676;  /* On white background */
}

/* ✅ Large text can be lighter (3:1) */
.heading {
  font-size: 24px;
  color: #949494;  /* On white */
}
```

**Testing tools:**
- WebAIM Contrast Checker
- Chrome DevTools (Color picker shows ratio)
- axe DevTools

**1.4.4 Resize Text (Level AA)**

Text can be resized up to 200% without loss of functionality:
```css
/* ✅ Use relative units */
.text {
  font-size: 1rem;  /* Not 16px */
}

/* ✅ Responsive text */
.heading {
  font-size: clamp(1.5rem, 4vw, 3rem);
}
```

**1.4.5 Images of Text (Level AA)**

Avoid text in images unless necessary:
```tsx
// ❌ Text in image
<img src="/logo-text.png" alt="Company Name" />

// ✅ Real text with web font
<h1 className="font-brand">Company Name</h1>
```

**1.4.10 Reflow (Level AA)**

Content reflows at 320px width without horizontal scrolling:
```css
/* ✅ Responsive layout */
.container {
  width: 100%;
  max-width: 1200px;
  padding: 0 1rem;
}
```

**1.4.11 Non-text Contrast (Level AA)**

UI components have 3:1 contrast against adjacent colors:
```css
/* ❌ Low contrast button border */
.button {
  background: white;
  border: 1px solid #ddd;  /* 1.2:1 on white */
}

/* ✅ Sufficient contrast */
.button {
  background: white;
  border: 2px solid #767676;  /* 4.5:1 */
}
```

**1.4.12 Text Spacing (Level AA)**

Allow user to adjust spacing without breaking layout:
```css
/* ✅ Support custom spacing */
* {
  line-height: 1.5 !important;
  letter-spacing: 0.12em !important;
  word-spacing: 0.16em !important;
}

/* Don't use fixed heights that break with spacing */
.button {
  min-height: 44px;  /* Not height: 44px */
  padding: 0.5em 1em;
}
```

**1.4.13 Content on Hover or Focus (Level AA)**

Hoverable/focusable content must be:
- **Dismissible:** Can be closed without moving pointer
- **Hoverable:** Pointer can move over content
- **Persistent:** Stays visible until dismissed

```tsx
// ✅ Accessible tooltip
<button
  aria-describedby="tooltip-1"
  onMouseEnter={() => setShowTooltip(true)}
  onMouseLeave={() => setShowTooltip(false)}
  onFocus={() => setShowTooltip(true)}
  onBlur={() => setShowTooltip(false)}
>
  Help
</button>
{showTooltip && (
  <div
    id="tooltip-1"
    role="tooltip"
    onMouseEnter={() => setShowTooltip(true)}
    onMouseLeave={() => setShowTooltip(false)}
  >
    Additional help text
  </div>
)}
```

---

## Principle 2: Operable

User interface must be operable.

### 2.1 Keyboard Accessible (Level A)

**2.1.1 Keyboard**

All functionality available via keyboard:
```tsx
// ❌ Only works with mouse
<div onClick={handleClick}>Click me</div>

// ✅ Keyboard accessible
<button onClick={handleClick}>Click me</button>

// ✅ Custom interactive element
<div
  role="button"
  tabIndex={0}
  onClick={handleClick}
  onKeyDown={(e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      handleClick();
    }
  }}
>
  Click me
</div>
```

**2.1.2 No Keyboard Trap**

Users can navigate away from any component:
```tsx
// ✅ Proper focus trap in modal
<Modal>
  <button ref={firstFocusable}>Close</button>
  {/* Content */}
  <button ref={lastFocusable}>Submit</button>
</Modal>

// Trap focus within modal, but allow ESC to close
useEffect(() => {
  const handleKeyDown = (e: KeyboardEvent) => {
    if (e.key === 'Escape') closeModal();

    if (e.key === 'Tab') {
      // Cycle focus between first and last
      if (e.shiftKey && document.activeElement === firstFocusable.current) {
        e.preventDefault();
        lastFocusable.current?.focus();
      } else if (!e.shiftKey && document.activeElement === lastFocusable.current) {
        e.preventDefault();
        firstFocusable.current?.focus();
      }
    }
  };
  document.addEventListener('keydown', handleKeyDown);
  return () => document.removeEventListener('keydown', handleKeyDown);
}, []);
```

**2.1.4 Character Key Shortcuts (Level A)**

If single-key shortcuts exist, provide way to turn off or remap.

---

### 2.2 Enough Time (Level A)

**2.2.1 Timing Adjustable**

If time limit exists, allow users to:
- Turn off timing
- Adjust before time expires
- Extend time (at least 10x)

**2.2.2 Pause, Stop, Hide**

For moving/blinking content, provide controls:
```tsx
// ✅ Autoplay carousel with controls
<Carousel autoplay={!isPaused}>
  <button onClick={() => setIsPaused(!isPaused)}>
    {isPaused ? 'Play' : 'Pause'}
  </button>
  {/* Slides */}
</Carousel>
```

---

### 2.3 Seizures (Level A)

**2.3.1 Three Flashes or Below Threshold**

No content flashes more than 3 times per second.

---

### 2.4 Navigable (Level AA)

**2.4.1 Bypass Blocks (Level A)**

Provide skip links for repeated content:
```tsx
// ✅ Skip to main content
<a href="#main-content" className="skip-link">
  Skip to main content
</a>

<nav>{/* Navigation */}</nav>

<main id="main-content" tabIndex={-1}>
  {/* Main content */}
</main>
```

**2.4.2 Page Titled (Level A)**

Every page has descriptive title:
```tsx
// ✅ Descriptive page title
<title>Contact Us - Company Name</title>

// In React
<Head>
  <title>Profile Settings - {userName} - App Name</title>
</Head>
```

**2.4.3 Focus Order (Level A)**

Focus order must be logical and intuitive.

**2.4.4 Link Purpose (Level A)**

Link text describes destination:
```tsx
// ❌ Vague link text
<a href="/article">Click here</a>

// ✅ Descriptive link text
<a href="/article">Read our accessibility guide</a>

// ✅ Alternative with context
<p>
  Learn about web accessibility in our
  <a href="/article">comprehensive guide</a>.
</p>
```

**2.4.5 Multiple Ways (Level AA)**

Provide multiple ways to find pages:
- Site map
- Search
- Navigation menu
- Related links

**2.4.6 Headings and Labels (Level AA)**

Headings and labels are descriptive:
```tsx
// ❌ Generic heading
<h2>Information</h2>

// ✅ Descriptive heading
<h2>Shipping Information</h2>

// ❌ Generic label
<label>Input</label>

// ✅ Descriptive label
<label htmlFor="email">Email address</label>
```

**2.4.7 Focus Visible (Level AA)**

Keyboard focus indicator is visible:
```css
/* ✅ Clear focus indicator */
button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* ❌ NEVER do this */
/* *:focus { outline: none; } */
```

---

### 2.5 Input Modalities (Level A)

**2.5.1 Pointer Gestures**

Don't require multipoint or path-based gestures:
```tsx
// ❌ Requires pinch gesture only
<Image pinchToZoom />

// ✅ Provide alternative (+ / - buttons)
<Image>
  <button onClick={zoomIn}>+</button>
  <button onClick={zoomOut}>-</button>
</Image>
```

**2.5.2 Pointer Cancellation**

Allow users to cancel pointer events:
- Use click (up event), not mousedown (down event)
- Allow dragging to be cancelled

**2.5.3 Label in Name**

Visible label matches accessible name:
```tsx
// ✅ Label matches accessible name
<button aria-label="Search">Search</button>

// ❌ Mismatch confuses voice control
<button aria-label="Find">Search</button>
```

**2.5.4 Motion Actuation**

Don't require device motion unless essential:
```tsx
// ✅ Provide alternative to shake-to-undo
<button onClick={undo}>Undo</button>
// Also allow shake gesture
```

---

## Principle 3: Understandable

Information must be understandable.

### 3.1 Readable (Level A)

**3.1.1 Language of Page**

Specify page language:
```html
<!-- ✅ Language specified -->
<html lang="en">

<!-- ✅ Regional variant -->
<html lang="en-US">
```

**3.1.2 Language of Parts (Level AA)**

Specify language changes:
```tsx
// ✅ Language change
<p>
  The French word for hello is <span lang="fr">bonjour</span>.
</p>
```

---

### 3.2 Predictable (Level A)

**3.2.1 On Focus**

Focus doesn't trigger unexpected context change:
```tsx
// ❌ Auto-submit on focus
<input onFocus={submitForm} />

// ✅ Require explicit action
<form onSubmit={handleSubmit}>
  <input />
  <button type="submit">Submit</button>
</form>
```

**3.2.2 On Input**

Changing input doesn't cause unexpected context change:
```tsx
// ❌ Auto-submit on change
<select onChange={submitForm}>

// ✅ Require explicit submit
<select onChange={handleChange}>
  <option>Select</option>
</select>
<button onClick={submitForm}>Submit</button>
```

**3.2.3 Consistent Navigation (Level AA)**

Navigation is consistent across pages:
```tsx
// ✅ Same nav on every page
<Layout>
  <Header>
    <Navigation /> {/* Always same order */}
  </Header>
  <main>{children}</main>
</Layout>
```

**3.2.4 Consistent Identification (Level AA)**

Same functionality has same identification:
```tsx
// ✅ Consistent icon + label
<button aria-label="Search">
  <SearchIcon />
</button>

// On all pages, search always uses SearchIcon and "Search" label
```

---

### 3.3 Input Assistance (Level AA)

**3.3.1 Error Identification (Level A)**

Errors are clearly identified:
```tsx
// ✅ Clear error identification
{errors.email && (
  <div className="text-red-600" role="alert">
    <ErrorIcon aria-hidden="true" />
    Error: Please enter a valid email address
  </div>
)}
```

**3.3.2 Labels or Instructions (Level A)**

Provide labels or instructions:
```tsx
// ✅ Clear label + instructions
<label htmlFor="password">
  Password
  <span className="text-sm text-gray-600">
    Must be at least 8 characters
  </span>
</label>
<input id="password" type="password" />
```

**3.3.3 Error Suggestion (Level AA)**

Suggest how to fix errors:
```tsx
// ❌ Vague error
{errors.password && <p>Invalid password</p>}

// ✅ Specific suggestion
{errors.password && (
  <p>
    Password must contain at least 8 characters, including one uppercase
    letter, one number, and one special character.
  </p>
)}
```

**3.3.4 Error Prevention (Legal, Financial, Data) (Level AA)**

For important submissions, allow review/confirm/undo:
```tsx
// ✅ Confirmation step
<form onSubmit={handleSubmit}>
  {step === 'review' ? (
    <div>
      <h2>Review Your Order</h2>
      <OrderSummary />
      <button type="submit">Confirm Purchase</button>
      <button type="button" onClick={goBack}>Go Back</button>
    </div>
  ) : (
    <div>{/* Order form */}</div>
  )}
</form>
```

---

## Principle 4: Robust

Content must work with current and future technologies.

### 4.1 Compatible (Level A)

**4.1.1 Parsing**

HTML is well-formed (no duplicate IDs, proper nesting).

**4.1.2 Name, Role, Value**

All components have accessible name, role, and state:
```tsx
// ✅ Accessible custom checkbox
<div
  role="checkbox"
  aria-checked={isChecked}
  aria-label="Accept terms and conditions"
  tabIndex={0}
  onClick={toggle}
  onKeyDown={(e) => {
    if (e.key === ' ' || e.key === 'Enter') toggle();
  }}
/>
```

**4.1.3 Status Messages (Level AA)**

Status messages can be determined programmatically:
```tsx
// ✅ Live region for status
<div role="status" aria-live="polite">
  {message}
</div>

// ✅ Alert for errors
<div role="alert" aria-live="assertive">
  {error}
</div>
```

---

## ARIA Patterns

### Common ARIA Attributes

**aria-label:** Accessible name for element
```tsx
<button aria-label="Close dialog">×</button>
```

**aria-labelledby:** References element providing label
```tsx
<h2 id="dialog-title">Confirm Action</h2>
<div role="dialog" aria-labelledby="dialog-title">
```

**aria-describedby:** References element providing description
```tsx
<input
  id="password"
  aria-describedby="password-help"
/>
<p id="password-help">At least 8 characters</p>
```

**aria-hidden:** Hides from assistive tech
```tsx
<span aria-hidden="true">→</span> {/* Decorative icon */}
```

**aria-live:** Announces dynamic content
```tsx
<div aria-live="polite">3 new messages</div>
```

**aria-expanded:** Collapsible content state
```tsx
<button aria-expanded={isOpen}>Menu</button>
```

**aria-controls:** References controlled element
```tsx
<button aria-controls="menu-panel" aria-expanded={isOpen}>
  Menu
</button>
<div id="menu-panel">{/* Menu content */}</div>
```

---

## Testing Checklist

### Automated Testing

**Tools:**
- axe DevTools (browser extension)
- Lighthouse accessibility audit
- WAVE browser extension
- pa11y (CI integration)

**Automated catches:**
- Missing alt text
- Low color contrast
- Missing form labels
- Invalid HTML
- Missing ARIA attributes

### Manual Testing

**Keyboard navigation:**
- [ ] Tab through entire page
- [ ] All interactive elements reachable
- [ ] Focus order is logical
- [ ] Focus indicators visible
- [ ] Can activate with Enter/Space
- [ ] Can close modals with ESC

**Screen reader testing:**
- [ ] Test with NVDA (Windows, free)
- [ ] Test with JAWS (Windows)
- [ ] Test with VoiceOver (macOS/iOS)
- [ ] All images have alt text read
- [ ] Headings create logical outline
- [ ] Form labels are announced
- [ ] Error messages are announced

**Zoom testing:**
- [ ] Test at 200% zoom
- [ ] No horizontal scrolling
- [ ] All content visible
- [ ] No overlapping text

**Color testing:**
- [ ] Color contrast meets ratios
- [ ] Information not conveyed by color alone
- [ ] Test with color blindness simulator

---

## Quick Reference: Common Fixes

| Issue | Fix |
|-------|-----|
| Image without alt | Add `alt=""` attribute |
| Icon button | Add `aria-label="Description"` |
| Low contrast | Darken text or lighten background |
| Missing label | Add `<label htmlFor="id">` or `aria-label` |
| Keyboard trap | Allow ESC or Tab to escape |
| No focus indicator | Add `:focus-visible { outline }` |
| Div button | Use `<button>` or add `role="button"` + keyboard handlers |
| Auto-play video | Add pause/play controls |
| Time limit | Allow extend/disable |
| Non-semantic HTML | Use `<button>`, `<nav>`, `<main>`, etc. |

---

## Accessibility Statement Template

```markdown
# Accessibility Statement for [Site Name]

We are committed to ensuring digital accessibility for people with disabilities. We are continually improving the user experience for everyone and applying the relevant accessibility standards.

## Conformance Status

The Web Content Accessibility Guidelines (WCAG) defines requirements for designers and developers to improve accessibility for people with disabilities. It defines three levels of conformance: Level A, Level AA, and Level AAA. [Site Name] is partially conformant with WCAG 2.1 level AA. Partially conformant means that some parts of the content do not fully conform to the accessibility standard.

## Feedback

We welcome your feedback on the accessibility of [Site Name]. Please let us know if you encounter accessibility barriers:

- Email: [accessibility@example.com]
- Phone: [phone number]

We try to respond to feedback within 2 business days.

## Technical Specifications

[Site Name] accessibility relies on the following technologies to work with the web browsers and assistive technologies on your computer:
- HTML
- WAI-ARIA
- CSS
- JavaScript

## Limitations and Alternatives

Despite our best efforts to ensure accessibility of [Site Name], there may be some limitations. Below is a description of known limitations and potential solutions.

[List known issues and workarounds]

## Assessment Approach

[Site Name] assessed the accessibility of this website by the following approaches:
- Self-evaluation
- External evaluation by [company/person]

This statement was created on [date] using the W3C Accessibility Statement Generator Tool.
```

---

**Remember:** Accessibility is not a feature to add—it's a fundamental aspect of building for the web. Build accessible from the start, not as an afterthought.
