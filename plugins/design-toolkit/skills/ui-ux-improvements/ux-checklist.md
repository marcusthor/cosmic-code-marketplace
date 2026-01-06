# UX Checklist - Usability & Interaction Patterns

Complete reference for evaluating user experience quality across common UI patterns.

## Navigation Patterns

### Primary Navigation

**Essential elements:**
- [ ] Active/current page clearly indicated
- [ ] Consistent placement across pages
- [ ] Clear visual hierarchy
- [ ] Hover states on nav items
- [ ] Logo links to homepage
- [ ] Mobile-responsive (hamburger menu)

**Anti-patterns:**
```tsx
// ❌ No active state
<nav>
  <a href="/home">Home</a>
  <a href="/about">About</a>
</nav>

// ✅ Clear active state
<nav>
  <a href="/home" className={pathname === '/home' ? 'active' : ''}>Home</a>
  <a href="/about" className={pathname === '/about' ? 'active' : ''}>About</a>
</nav>
```

### Breadcrumbs

**When to use:** Deep hierarchies (3+ levels)

**Best practices:**
- [ ] Show full path from root
- [ ] Current page is last item (not linked)
- [ ] Use separator (/, >, →)
- [ ] Make previous levels clickable
- [ ] Truncate if too long (show … in middle)

---

## Form Patterns

### Input Fields

**Best practices:**
- [ ] Labels above inputs (easier to scan)
- [ ] Placeholder text is hint, not label
- [ ] Required fields marked upfront (*)
- [ ] Inputs sized to expected content
- [ ] Clear focus indicators
- [ ] Error messages near field

**Label positioning:**
```tsx
// ❌ Label inside placeholder (disappears on focus)
<input placeholder="Email address" />

// ✅ Label above input
<label htmlFor="email">Email address *</label>
<input id="email" type="email" required />
```

### Validation

**Timing:**
- **On submit:** Show all errors at once
- **On blur:** Validate completed field
- **NOT on change:** Too aggressive, distracting

**Error messages:**
- [ ] Specific (not "Invalid input")
- [ ] Actionable (tell user how to fix)
- [ ] Near the field (not just top of form)
- [ ] Red color + icon for visibility

```tsx
// ❌ Vague error
{errors.email && <p>Invalid</p>}

// ✅ Specific, actionable error
{errors.email && (
  <p className="text-red-600 text-sm mt-1">
    Please enter a valid email address (e.g., user@example.com)
  </p>
)}
```

### Submit Buttons

**States to show:**
- [ ] Default (enabled)
- [ ] Hover
- [ ] Focus
- [ ] Loading (with spinner)
- [ ] Disabled
- [ ] Success (after submit)

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

---

## Feedback Patterns

### Loading States

**Short waits (<1 second):**
- Disable button
- Show subtle spinner

**Medium waits (1-5 seconds):**
- Show loading spinner
- Disable interactions
- Optional: Show progress text

**Long waits (>5 seconds):**
- Show progress bar or percentage
- Explain what's happening
- Provide cancel option

**Skeleton screens:**
```tsx
// ✅ Show structure while loading
{isLoading ? (
  <div className="animate-pulse">
    <div className="h-4 bg-gray-200 rounded w-3/4 mb-2" />
    <div className="h-4 bg-gray-200 rounded w-1/2" />
  </div>
) : (
  <UserProfile data={data} />
)}
```

### Success Feedback

**Inline success:**
```tsx
// ✅ Show success state after action
{isSaved && (
  <span className="text-green-600">
    <CheckIcon /> Saved
  </span>
)}
```

**Toast notifications:**
- Auto-dismiss after 3-5 seconds
- Allow manual dismiss
- Show in consistent location (top-right)
- Stack multiple toasts

### Error States

**Inline errors:**
- Show next to failed field
- Use error color (red)
- Include icon for visibility

**Global errors:**
- Show at top of form/page
- Summarize all errors
- Link to specific fields
- Allow dismiss

**Empty states:**
```tsx
// ✅ Helpful empty state
{items.length === 0 ? (
  <div className="text-center py-12">
    <EmptyIcon className="mx-auto h-12 w-12 text-gray-400" />
    <h3 className="mt-2 text-sm font-medium">No items yet</h3>
    <p className="mt-1 text-sm text-gray-500">
      Get started by creating your first item.
    </p>
    <Button onClick={onCreate} className="mt-4">
      Create Item
    </Button>
  </div>
) : (
  <ItemList items={items} />
)}
```

---

## Button Patterns

### Button Hierarchy

**Primary action:** One per screen
- Solid color background
- High contrast
- Most prominent

**Secondary actions:** Supporting actions
- Outline or ghost style
- Lower contrast

**Tertiary actions:** Least important
- Text-only (link style)

```tsx
// Clear hierarchy
<div className="flex gap-3">
  <Button variant="primary">Save Changes</Button>
  <Button variant="secondary">Cancel</Button>
  <Button variant="ghost">Delete</Button>
</div>
```

### Button States

**Required states:**
- [ ] Default
- [ ] Hover (subtle background change)
- [ ] Focus (visible ring)
- [ ] Active (pressed state)
- [ ] Disabled (reduced opacity, no hover)
- [ ] Loading (spinner, disabled)

### Destructive Actions

**Confirmation pattern:**
- Show confirmation modal for destructive actions
- Clearly state what will be deleted
- Use red/warning color
- Require explicit confirmation

```tsx
// ✅ Confirm before delete
function deleteUser() {
  confirm(
    'Are you sure you want to delete this user? This action cannot be undone.'
  ) && performDelete();
}
```

---

## Modal/Dialog Patterns

### Modal Best Practices

- [ ] Close on ESC key
- [ ] Close on background click (optional)
- [ ] Trap focus inside modal
- [ ] Restore focus on close
- [ ] Prevent body scroll when open
- [ ] Show close button (X in corner)
- [ ] Animate entrance/exit

**Overlay:**
```tsx
// ✅ Proper modal overlay
<div
  className="fixed inset-0 bg-black/50 z-40"
  onClick={onClose}
  aria-hidden="true"
/>
<div
  className="fixed inset-0 z-50 flex items-center justify-center"
  role="dialog"
  aria-modal="true"
>
  <div className="bg-white rounded-lg p-6">
    {/* Modal content */}
  </div>
</div>
```

### Drawer Patterns

**When to use:** Related content, don't need full attention
- Slide from right/left
- Partial overlay (can see page behind)
- Can remain open while interacting with page

---

## List & Table Patterns

### Data Tables

**Essential features:**
- [ ] Column headers
- [ ] Sortable columns (show indicator)
- [ ] Row hover state
- [ ] Pagination or infinite scroll
- [ ] Loading state
- [ ] Empty state

**Row actions:**
```tsx
// ✅ Clear row actions
<tr className="hover:bg-gray-50">
  <td>{user.name}</td>
  <td>{user.email}</td>
  <td className="text-right">
    <button>Edit</button>
    <button>Delete</button>
  </td>
</tr>
```

### Infinite Scroll vs Pagination

**Infinite scroll:**
- ✅ Good for: Feeds, continuous browsing
- ❌ Bad for: Finding specific items, footer access

**Pagination:**
- ✅ Good for: Controlled browsing, SEO
- ❌ Bad for: Slow if many pages

**Hybrid (recommended):**
- Load next page automatically
- Show "Show more" button as fallback

---

## Search Patterns

### Search Input

**Best practices:**
- [ ] Show search icon
- [ ] Clear button when text entered
- [ ] Placeholder with example
- [ ] Focus on keyboard shortcut (/)
- [ ] Show recent searches
- [ ] Autocomplete suggestions

**Debounced search:**
```typescript
// ✅ Debounce to avoid excessive requests
const debouncedSearch = debounce((query: string) => {
  performSearch(query);
}, 300);
```

### Search Results

- [ ] Show result count
- [ ] Highlight matching text
- [ ] Group by category
- [ ] Show "No results" state
- [ ] Suggest alternative spelling
- [ ] Filter/sort options

---

## Mobile-Specific Patterns

### Touch Targets

**Minimum size:** 44x44px (iOS guideline)

```tsx
// ❌ Too small for mobile
<button className="px-2 py-1">Tap me</button>

// ✅ Adequate touch target
<button className="px-4 py-3 min-h-[44px]">Tap me</button>
```

### Mobile Navigation

**Hamburger menu:**
- Icon in top-left or top-right
- Animated icon (burger → X)
- Slide-in drawer
- Overlay background
- Close on item tap or background tap

**Bottom navigation:**
- 3-5 primary items
- Icons + labels
- Highlight active tab
- Fixed position

### Swipe Gestures

**When to use:**
- Swipe to delete (lists)
- Swipe between tabs/pages
- Pull to refresh

**Implementation:**
- Show visual feedback
- Require sufficient swipe distance
- Animate smoothly

---

## Micro-interactions

### Hover States

**Interactive elements:**
- [ ] Slight background change
- [ ] Cursor changes to pointer
- [ ] Subtle scale or lift
- [ ] Underline for links

```css
/* ✅ Smooth hover transitions */
.button {
  transition: background-color 150ms ease;
}

.button:hover {
  background-color: var(--primary-hover);
}
```

### Active/Pressed States

```css
/* ✅ Feedback on click */
.button:active {
  transform: scale(0.98);
}
```

### Focus States

**Always visible for keyboard navigation:**
```css
/* ✅ Clear focus ring */
.button:focus-visible {
  outline: 2px solid var(--primary);
  outline-offset: 2px;
}
```

---

## Animation Guidelines

### Timing

| Duration | Use Case |
|----------|----------|
| 100-150ms | Hover states, instant feedback |
| 200-300ms | Most UI transitions |
| 400-500ms | Modal entrance, page transitions |
| 1000ms+ | Loading animations, complex animations |

### Easing

```css
/* ✅ Natural motion */
transition-timing-function: cubic-bezier(0.4, 0.0, 0.2, 1); /* ease-in-out */

/* Entrance */
transition-timing-function: cubic-bezier(0.0, 0.0, 0.2, 1); /* ease-out */

/* Exit */
transition-timing-function: cubic-bezier(0.4, 0.0, 1, 1); /* ease-in */
```

### Reduce Motion

**Respect user preferences:**
```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Progressive Disclosure

**Show only essential information first:**
- Use "Show more" / "Show less" toggles
- Accordion patterns for FAQs
- Tabs for related content
- Tooltips for supplementary info

```tsx
// ✅ Progressive disclosure
{showDetails ? (
  <>
    <Summary />
    <Details />
    <button onClick={() => setShowDetails(false)}>Show less</button>
  </>
) : (
  <>
    <Summary />
    <button onClick={() => setShowDetails(true)}>Show more</button>
  </>
)}
```

---

## Copy & Microcopy

### Button Text

**Be specific:**
- ❌ "Submit" → ✅ "Create Account"
- ❌ "Click here" → ✅ "View details"
- ❌ "OK" → ✅ "Save changes"

### Error Messages

**Be helpful:**
- ❌ "Error 422" → ✅ "Email already exists"
- ❌ "Invalid input" → ✅ "Password must be at least 8 characters"
- ❌ "Failed" → ✅ "Couldn't connect to server. Try again."

### Empty States

**Be encouraging:**
- ❌ "No data" → ✅ "No items yet. Create your first one!"
- ❌ "Empty" → ✅ "Your inbox is empty. Nice work!"

---

## Performance Perception

### Optimistic UI

**Update UI immediately, rollback if fails:**
```tsx
// ✅ Optimistic update
function likePost(postId: string) {
  // Update UI immediately
  setLiked(true);
  setLikeCount(prev => prev + 1);

  // Send request
  api.likePost(postId).catch(() => {
    // Rollback on error
    setLiked(false);
    setLikeCount(prev => prev - 1);
    toast.error('Failed to like post');
  });
}
```

### Skeleton Screens

**Show structure while loading:**
- Better than spinners for content
- Matches actual layout
- Feels faster to users

### Perceived Performance

**Tricks:**
- Show partial content while loading rest
- Prefetch on hover
- Use transitions to smooth changes
- Show progress indicators

---

## Consistency Checklist

**Across the application:**
- [ ] Button styles consistent
- [ ] Form input styles consistent
- [ ] Spacing follows system (4, 8, 12, 16, 24, 32)
- [ ] Colors from design tokens only
- [ ] Typography scale consistent
- [ ] Border radius values consistent
- [ ] Shadow depths consistent
- [ ] Animation timing consistent

---

## Common UX Mistakes to Avoid

1. **Hiding important actions** - Keep primary actions visible
2. **Too many clicks** - Reduce steps in user flows
3. **No feedback** - Always acknowledge user actions
4. **Inconsistent patterns** - Use same pattern for same action
5. **Ignoring mobile** - Design mobile-first
6. **Poor contrast** - Ensure readability
7. **Tiny touch targets** - Make buttons finger-friendly
8. **No loading states** - Show progress on waits
9. **Unclear errors** - Make error messages helpful
10. **No empty states** - Guide users when content is missing

---

## Testing Your UX

**Manual testing:**
- Use the app as a new user would
- Try on different screen sizes
- Test with keyboard only
- Test with screen reader
- Intentionally trigger errors

**User testing:**
- Watch real users complete tasks
- Note where they get confused
- Ask for feedback after tasks
- Identify friction points

**Analytics:**
- Track drop-off points
- Measure time to complete tasks
- Monitor error rates
- Watch session recordings
