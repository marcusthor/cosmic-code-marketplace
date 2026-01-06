# Claude Code Marketplace

Collection of production-ready Claude Code skills and slash commands for development workflows.

## 📦 Available Plugins

### 🗄️ Supabase Toolkit
Complete Supabase development suite with 6 skills:
- Database function best practices
- Migration file generation
- Edge Function templates
- RLS policy optimization
- Realtime implementation patterns
- PostgreSQL style guide

### 🔍 Code Analysis
Comprehensive code audit toolkit with 4 skills + 4 commands:
- Code quality & refactoring opportunities
- Pattern-based improvements
- Performance optimization
- Security vulnerability detection
- **Auto-generates improvement tickets** in `improvements/` directory

### 🎨 Design Toolkit
Frontend design and UX optimization with 3 skills + 2 commands:
- Production-grade frontend design
- UI/UX improvements (WCAG 2.1)
- Documentation gap analysis

### 📊 Competitor Insights
Market research and competitive analysis:
- User feedback mining
- Pain point identification
- Market gap discovery

## 🚀 Installation

### 1. Add Marketplace
```bash
/plugin marketplace add marcusthor/claude-marketplace
```

### 2. Install Plugins
```bash
# Install specific plugins
/plugin install supabase-toolkit@claude-marketplace
/plugin install code-analysis@claude-marketplace
/plugin install design-toolkit@claude-marketplace
/plugin install competitor-insights@claude-marketplace

# Or install all
/plugin install supabase-toolkit@claude-marketplace code-analysis@claude-marketplace design-toolkit@claude-marketplace competitor-insights@claude-marketplace
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
/code-analysis:analyze-code-quality

# Creates: improvements/code-quality/001-split-large-file.md
#          improvements/code-quality/002-reduce-complexity.md
#          etc.

# Other commands
/code-analysis:analyze-performance
/code-analysis:analyze-security
/design-toolkit:analyze-ux
/competitor-insights:analyze-competitors
```

## 🎫 Ticket Workflow

Commands in `code-analysis` plugin generate individual ticket files:

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
/plugin marketplace add marcusthor/claude-marketplace

# Install desired plugins
/plugin install supabase-toolkit@claude-marketplace
```

### Project-specific plugins
Add to `.claude/settings.json`:
```json
{
  "extraKnownMarketplaces": {
    "claude-marketplace": {
      "source": {
        "source": "github",
        "repo": "marcusthor/claude-marketplace"
      }
    }
  },
  "enabledPlugins": {
    "code-analysis@claude-marketplace": true,
    "supabase-toolkit@claude-marketplace": true
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
