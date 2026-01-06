---
name: competitor-analysis
description: Conduct comprehensive competitor analysis for projects by researching user feedback, pain points, and market gaps. Use when analyzing competitors, researching market positioning, or identifying product opportunities.
---

# Competitor Analysis

Conduct comprehensive competitor analysis for projects by researching actual user feedback, pain points, and market opportunities.

## How This Works

This skill will:
1. **Ask you about your project** - Gather context about what you're building
2. **Research competitors** - Use web search to find alternatives and competitors
3. **Analyze user feedback** - Search reviews, forums, and communities for real pain points
4. **Identify market gaps** - Find opportunities for differentiation
5. **Generate report** - Create a comprehensive Markdown analysis document

## Process

### Phase 1: Gather Project Context

First, I'll ask you questions about your project using the AskUserQuestion tool:

**Required Information:**
- **Project name** - What's the name of your project?
- **Project type** - What kind of product/tool is this? (e.g., "task management app", "code editor", "developer library")
- **Target audience** - Who are the primary users? (e.g., "developers", "small business owners", "students")
- **Product vision** - What problem does your project solve?
- **Known competitors** (optional) - Any competitors you're already aware of?

### Phase 2: Identify Competitors

Using WebSearch, I'll find competitors with these search strategies:

**Search Queries:**
- `"[project type] alternatives 2026"` - Find current alternatives
- `"best [project type] tools"` - Discover popular options
- `"[project type] vs"` - Find comparison articles
- `"[specific feature] software"` - Search by key features

**Competitor Selection:**
- Direct competitors (same product type, same audience)
- Indirect competitors (different approach, same problem)
- Market leaders (most compared against)

For each competitor, I'll document:
- Name and website URL
- Brief description
- Relevance to your project (high/medium/low)
- Market position

### Phase 3: Research User Feedback

For each identified competitor, I'll search for real user feedback:

**Sources to Search:**

1. **App Store & Reviews**
   - `"[competitor name] reviews complaints"`
   - `"[competitor name] app store reviews problems"`

2. **Community Discussions**
   - `"[competitor name] reddit complaints"`
   - `"[competitor name] issues site:reddit.com"`
   - `"[competitor name] problems site:twitter.com OR site:x.com"`

3. **Technical Forums** (for developer tools)
   - `"[competitor name] issues site:stackoverflow.com"`
   - `"[competitor name] problems site:github.com"`

**Pain Points to Extract:**
- Common complaints (mentioned repeatedly)
- Missing features (user wishes)
- UX problems (usability issues)
- Performance issues (speed, reliability)
- Pricing concerns (cost complaints)
- Support issues (customer service problems)

For each pain point, I'll document:
- Clear description
- Source (where found)
- Severity (high/medium/low)
- Frequency (how often mentioned)
- Opportunity (how your project could address it)

### Phase 4: Identify Market Gaps

Analyze pain points across all competitors to find:

**Common Patterns:**
- Problems no one solves well
- Universally requested features
- Shared frustrations across the market

**Differentiation Opportunities:**
- Where your project can excel
- Unique approaches to common problems
- Underserved market segments

### Phase 5: Generate Analysis Report

Create a comprehensive Markdown report with:

```markdown
# Competitor Analysis: [Project Name]

**Analysis Date:** [Date]
**Project Type:** [Type]
**Target Audience:** [Audience]

## Executive Summary

[Key findings and top opportunities]

## Competitors Overview

### [Competitor 1 Name]
- **Website:** [URL]
- **Relevance:** High/Medium/Low
- **Description:** [Brief description]
- **Market Position:** [Position description]

**User Pain Points:**
1. **[Pain Point Title]**
   - **Severity:** High/Medium/Low
   - **Frequency:** [How often mentioned]
   - **Source:** [Where found]
   - **Description:** [Detailed description]
   - **Opportunity:** [How to address]

**Strengths:**
- [What users like]
- [Positive aspects]

---

## Market Gaps

### Gap 1: [Gap Title]
- **Opportunity Size:** High/Medium/Low
- **Affected Competitors:** [List]
- **Description:** [Gap description]
- **Suggested Feature:** [Feature idea]

---

## Insights Summary

### Top Pain Points Across Market
1. [Pain point 1]
2. [Pain point 2]
3. [Pain point 3]

### Differentiation Opportunities
1. [Opportunity 1]
2. [Opportunity 2]

### Market Trends
1. [Trend 1]
2. [Trend 2]

---

## Research Metadata

**Search Queries Used:**
- [Query 1]
- [Query 2]

**Sources Consulted:**
- [Source 1]
- [Source 2]

**Limitations:**
- [Any research limitations]
```

## Critical Rules

1. **Use Real Research** - All pain points must come from actual user feedback via WebSearch
2. **Document Sources** - Every pain point needs a documented source
3. **Focus on User Feedback** - Look for complaints and wishes, not just feature lists
4. **Be Specific** - Clear descriptions of problems and opportunities
5. **Include Evidence** - Quote or reference actual user comments when possible

## Handling Edge Cases

### No Competitors Found
If the project is truly unique:
- Note the first-mover advantage
- Research adjacent markets
- Look for indirect competitors solving related problems

### Internal Tools / Libraries
For developer tools where traditional competitors don't apply:
- Search for alternative libraries/packages
- Check GitHub issues on similar projects
- Search Stack Overflow for common domain problems

### Limited Search Results
If WebSearch returns limited results:
- Document the limitation
- Include whatever competitors were found
- Note that additional research may be needed
- Search for broader category alternatives

## Example Usage

**User:** "I need a competitor analysis for my project"

**Skill Response:**
1. Uses AskUserQuestion to gather project details
2. Performs web research using WebSearch
3. Analyzes user feedback from multiple sources
4. Creates comprehensive Markdown report
5. Saves report to file (asks for filename)

## Output Location

The report will be saved as:
- `competitor_analysis_[project-name]_[date].md` in the current directory

Or you can specify a custom filename when prompted.

---

## Begin Analysis

When invoked, I will:
1. Use **AskUserQuestion** to collect project information
2. Use **WebSearch** extensively to research competitors and user feedback
3. Use **Write** to create the final Markdown report
4. Present key findings and save the complete analysis

**Ready to start?** I'll begin by asking about your project details.
