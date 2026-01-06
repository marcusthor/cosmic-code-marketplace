# Cosmic Code Marketplace

Collection of production-ready Claude Code plugins for development workflows.

## 📦 Available Plugins

### ⚡ Coco - Command Suite
Unified command interface with 7 slash commands:
- **Analysis**: `/coco:analysis:code-quality`, `/coco:analysis:code-improvements`, `/coco:analysis:performance`, `/coco:analysis:security`
- **Design**: `/coco:design:ux`, `/coco:design:documentation`
- **Research**: `/coco:research:competitors`
- **Auto-generates improvement tickets** in `improvements/` directory

### 🗄️ Supabase Toolkit
Complete Supabase development suite with 6 skills:
- Database function best practices
- Migration file generation
- Edge Function templates
- RLS policy optimization
- Realtime implementation patterns
- PostgreSQL style guide

### 🔍 Code Analysis
Code audit skills (4 skills):
- Code quality & refactoring opportunities
- Pattern-based improvements
- Performance optimization
- Security vulnerability detection

### 🎨 Design Toolkit
Design skills (3 skills):
- Production-grade frontend design
- UI/UX improvements (WCAG 2.1)
- Documentation gap analysis

### 📊 Competitor Insights
Market research skill:
- User feedback mining
- Pain point identification
- Market gap discovery

## 🚀 Installation

### 1. Add Marketplace
```bash
/plugin marketplace add marcusthor/cosmic-code-marketplace
```

### 2. Install Plugins
```bash
# Install commands (recommended - includes all slash commands)
/plugin install coco@cosmic-code-marketplace

# Install skills
/plugin install supabase-toolkit@cosmic-code-marketplace
/plugin install code-analysis@cosmic-code-marketplace
/plugin install design-toolkit@cosmic-code-marketplace
/plugin install competitor-insights@cosmic-code-marketplace

# Or install all
/plugin install coco@cosmic-code-marketplace supabase-toolkit@cosmic-code-marketplace code-analysis@cosmic-code-marketplace design-toolkit@cosmic-code-marketplace competitor-insights@cosmic-code-marketplace
```

### 3. Verify Installation
```bash
/plugin list
```

## 📖 Usage

### Automatic Skill Invocation
Skills are triggered automatically based on context:
```
"Analyze code quality issues"          → code-quality skill
"How can I improve performance?"       → performance-optimization skill
"Create a Supabase migration"          → supabase-migrations skill
"Review accessibility compliance"      → ui-ux-improvements skill
```

### Slash Commands (with Ticket Generation)
```bash
# Generate code quality tickets
/coco:analysis:code-quality

# Creates: improvements/code-quality/001-split-large-file.md
#          improvements/code-quality/002-reduce-complexity.md
#          etc.

# Other commands
/coco:analysis:performance       # Performance optimization
/coco:analysis:security          # Security audit
/coco:design:ux                  # UI/UX analysis
/coco:research:competitors       # Competitive analysis
```

## 🎫 Ticket Workflow

Commands in `coco` plugin generate individual ticket files:

```
improvements/
├── code-quality/
│   ├── 001-split-large-user-service.md
│   ├── 002-reduce-complexity-auth.md
│   └── 003-remove-duplicate-validation.md
├── performance/
│   ├── 001-replace-moment-with-date-fns.md
│   ├── 002-add-react-memo-to-list.md
│   └── 003-optimize-database-query.md
└── security/
    ├── 001-fix-sql-injection.md
    └── 002-add-rate-limiting.md
```

Each ticket contains:
- Problem description
- Affected files
- Current code
- Proposed fix
- Implementation steps

Use tickets to:
1. Review and prioritize issues
2. Track implementation progress
3. Reference as implementation tasks: `"Implement improvements/security/001-fix-sql-injection.md"`

## 🔧 Multi-Machine Setup

### Clone to another machine
```bash
# Add marketplace once
/plugin marketplace add marcusthor/cosmic-code-marketplace

# Install desired plugins
/plugin install coco@cosmic-code-marketplace
```

### Project-specific plugins
Add to `.claude/settings.json`:
```json
{
  "extraKnownMarketplaces": {
    "cosmic-code-marketplace": {
      "source": {
        "source": "github",
        "repo": "marcusthor/cosmic-code-marketplace"
      }
    }
  },
  "enabledPlugins": {
    "coco@cosmic-code-marketplace": true,
    "code-analysis@cosmic-code-marketplace": true,
    "supabase-toolkit@cosmic-code-marketplace": true
  }
}
```

## 📝 License

MIT - See [LICENSE](LICENSE) for details.

## 🤝 Contributing

Contributions welcome! Open an issue or pull request.

## 🔗 Links

- [Claude Code Documentation](https://code.claude.com/docs)
- [Plugin Development Guide](https://code.claude.com/docs/en/plugins.md)
- [Marketplace Guide](https://code.claude.com/docs/en/plugin-marketplaces.md)
