# Competitor Insights Plugin

Market research and competitive analysis tool for discovering user pain points and market gaps.

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

**References**:
- [research-guide.md](skills/competitor-analysis/research-guide.md) - Research strategies
- [report-template.md](skills/competitor-analysis/report-template.md) - Report format

## Slash Command

### `/analyze-competitors`
Runs competitive analysis and generates market research report.

**Argument**: `[project-name or project-type]`

**Output**: Markdown report with:
- Competitor overview
- User pain points by severity
- Market gaps and opportunities
- Feature comparison matrix
- Recommendations

**Example**:
```bash
/competitor-insights:analyze-competitors "project management SaaS"
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

## Usage

Interactive skill asks clarifying questions:
1. What type of product/service?
2. Target market/industry?
3. Geographic focus?
4. Specific aspects to analyze?

Then conducts automated research and generates comprehensive report.

## Installation

```bash
/plugin install competitor-insights@claude-marketplace
```
