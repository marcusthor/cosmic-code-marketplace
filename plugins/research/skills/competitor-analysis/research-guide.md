# Competitor Analysis - Research Guide

## Effective Search Strategies

### Finding Competitors

**General Discovery:**
```
"[product category] alternatives 2026"
"best [product category] for [audience]"
"[product category] comparison"
"[product category] vs [known competitor]"
"top [product category] tools"
```

**Example for a Task Management App:**
```
"task management app alternatives 2026"
"best task management tools for teams"
"Asana vs Trello vs"
"project management software comparison"
```

### Finding User Pain Points

**Review Sites:**
```
"[competitor] reviews complaints"
"[competitor] app store reviews 1 star"
"[competitor] g2 reviews negative"
"[competitor] capterra reviews problems"
```

**Community Feedback:**
```
"[competitor] reddit complaints"
"[competitor] site:reddit.com problems"
"why I switched from [competitor]"
"[competitor] frustrating site:reddit.com"
```

**Social Media:**
```
"[competitor] problems site:twitter.com"
"[competitor] issues site:x.com"
"[competitor] worst feature"
```

**Technical Communities (for dev tools):**
```
"[competitor] issues site:github.com"
"[competitor] bugs site:stackoverflow.com"
"[competitor] limitations site:news.ycombinator.com"
```

## Pain Point Categories

### 1. Feature Gaps
**What to look for:**
- "I wish [competitor] had..."
- "Missing feature: ..."
- "Would be better if it supported..."

**Example:**
> "Notion is great but lacks offline mode" - Source: Reddit r/Notion

### 2. UX/UI Issues
**What to look for:**
- "Confusing interface"
- "Too many clicks to..."
- "Hard to find..."

**Example:**
> "Figma's component system is too complex for beginners" - Source: Designer News

### 3. Performance Problems
**What to look for:**
- "Slow loading"
- "Crashes frequently"
- "Poor performance on..."

**Example:**
> "VS Code becomes sluggish with large files" - Source: GitHub Issues

### 4. Pricing Concerns
**What to look for:**
- "Too expensive for..."
- "Pricing doesn't match value"
- "Free tier too limited"

**Example:**
> "Slack's pricing jump from free to paid is too steep" - Source: App Store Reviews

### 5. Integration Issues
**What to look for:**
- "Doesn't work with..."
- "Poor API documentation"
- "Limited integrations"

**Example:**
> "Linear has great features but terrible Jira import" - Source: Reddit r/ProductManagement

### 6. Support & Documentation
**What to look for:**
- "Poor customer support"
- "Outdated documentation"
- "No response from team"

**Example:**
> "Submitted bug 6 months ago, still no fix" - Source: ProductHunt comments

## Severity Assessment

### High Severity
- Mentioned across multiple sources
- Blocks core functionality
- Forces users to switch products
- Active discussions/complaints

**Example:**
> Multiple Reddit threads about Notion's lack of offline mode (50+ comments each)

### Medium Severity
- Mentioned in 2-3 sources
- Workarounds exist but inconvenient
- Users complain but stay with product

**Example:**
> Occasional mentions of Figma's slow auto-save on forums

### Low Severity
- Single mention or rare occurrence
- Minor inconvenience
- Affects edge cases only

**Example:**
> One user mentions preferring different keyboard shortcut

## Market Gap Identification

### Pattern 1: Universal Pain Point
**When:** Same complaint across ALL competitors
**Opportunity:** Solve it better than anyone else

**Example:**
> All code editors struggle with large file performance → Opportunity to build with performance-first architecture

### Pattern 2: Underserved Segment
**When:** Tool works for large teams but not small teams (or vice versa)
**Opportunity:** Build for the underserved segment

**Example:**
> Enterprise project management tools too complex for freelancers → Build simple PM tool for solopreneurs

### Pattern 3: Missing Integration
**When:** Users frequently ask for specific integration
**Opportunity:** First-class support for that integration

**Example:**
> Designers want Figma-to-code tools but all existing ones are poor → Build best-in-class Figma integration

### Pattern 4: Price-Feature Mismatch
**When:** Free tier too limited, paid tier too expensive
**Opportunity:** Better pricing model

**Example:**
> Slack free tier limited to 90 days of history → Offer unlimited history on free tier

## Research Documentation

### Source Citation Format

**For Reddit:**
```
Source: Reddit r/[subreddit] - "[comment/post title]" ([date])
```

**For App Store:**
```
Source: [App] [Platform] Store Reviews - [star rating] stars ([date])
```

**For GitHub:**
```
Source: [Repo] GitHub Issues #[number] - "[issue title]"
```

**For Articles:**
```
Source: [Publication] - "[Article Title]" by [Author] ([date])
```

### Frequency Assessment

**Very High (10+ mentions):**
- "Consistently mentioned across sources"
- "Top complaint in multiple reviews"

**High (5-10 mentions):**
- "Frequently appears in discussions"
- "Common theme in feedback"

**Medium (2-4 mentions):**
- "Mentioned in several places"
- "Recurring but not dominant"

**Low (1 mention):**
- "Single occurrence"
- "Isolated feedback"

## Quality Checklist

Before finalizing your analysis, verify:

- [ ] At least 3-5 competitors identified
- [ ] Each competitor has 3+ documented pain points
- [ ] Every pain point has a source citation
- [ ] Severity ratings are based on frequency
- [ ] Market gaps are derived from multiple competitors
- [ ] Top 3-5 opportunities are clearly defined
- [ ] Search queries used are documented
- [ ] Research limitations are noted

## Example Analysis Snippet

### Competitor: Notion

**Pain Point 1: Offline Functionality**
- **Severity:** High
- **Frequency:** Very High (50+ mentions across sources)
- **Source:** Reddit r/Notion, App Store reviews, ProductHunt
- **Description:** Notion requires internet connection for most features. Users cannot access or edit pages offline, making it unreliable for travel or areas with poor connectivity.
- **Opportunity:** Build offline-first note-taking with full editing capabilities and automatic sync when online.

**Pain Point 2: Performance with Large Databases**
- **Severity:** Medium
- **Frequency:** Medium (10+ mentions)
- **Source:** Reddit r/Notion, Twitter/X complaints
- **Description:** Pages with large databases (1000+ entries) become slow to load and interact with. Users report 5-10 second load times.
- **Opportunity:** Optimize database performance with pagination, lazy loading, and local caching.

## Common Pitfalls to Avoid

1. **Don't make up pain points** - All feedback must come from real sources
2. **Don't cherry-pick** - Include both positive and negative feedback
3. **Don't assume** - Verify complaints are current (not from 3 years ago)
4. **Don't ignore context** - Some complaints may be niche use cases
5. **Don't skip documentation** - Always cite sources

## When to Stop Researching

You have enough when:
- 3-5 main competitors identified
- Each has 5+ documented pain points
- Clear patterns emerge across competitors
- At least 3 market gaps identified
- Research is repeating the same findings
