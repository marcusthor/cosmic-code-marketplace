# Research Plugin

Market research commands and skills for competitive analysis and user feedback mining.

## Command

### `/research:competitors`
Competitive analysis and market research report generation.

**Argument**: `[project-name or project-type]`

**Output**: Markdown report with:
- Competitor overview
- User pain points by severity
- Market gaps and opportunities
- Feature comparison matrix
- Recommendations

**Example**:
```bash
/research:competitors "project management SaaS"
```

Generates:
```markdown
# Competitive Analysis: Project Management SaaS

## Competitors Identified
1. Asana
2. Monday.com
3. Linear

## Pain Points Discovered

### High Severity
**Too complex for small teams**
- Source: Reddit r/productivity
- Frequency: 23 mentions
- Opportunity: Simplified interface for 1-10 person teams

## Market Gaps
...
```

## Skill

### competitor-analysis
Conduct comprehensive competitor analysis by researching user feedback, pain points, and market gaps.

**Key Features**:
- Project context gathering
- Web search for competitors
- User feedback analysis (Reddit, Product Hunt, G2, App Store reviews)
- Pain point extraction
- Market gap identification
- Opportunity scoring
- Competitive positioning

**Interactive Questions**:
1. What type of product/service?
2. Target market/industry?
3. Geographic focus?
4. Specific aspects to analyze?

Then conducts automated research and generates comprehensive report.

## Usage

Skill is automatically invoked by Claude based on context:

```
"Research competitors for my SaaS"     → competitor-analysis skill
"Analyze market opportunities"         → competitor-analysis skill
"Find user pain points in the market"  → competitor-analysis skill
```

## Installation

```bash
/plugin install research@cosmic-code-marketplace
```
