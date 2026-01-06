# Development Plugin

Development tools for creating and managing Claude Code skills.

## Skills

### skill-creator

Guide for creating effective skills that extend Claude's capabilities with specialized knowledge, workflows, or tool integrations.

**Triggered by:**
- "Help me create a new skill"
- "I want to build a skill for..."
- "Create a skill that..."
- "Update this existing skill"

**Features:**
- Step-by-step skill creation process
- Progressive disclosure design principles
- Best practices for SKILL.md structure
- Guidelines for bundled resources (scripts, references, assets)

**Bundled Scripts:**

| Script | Purpose |
|--------|---------|
| `init_skill.py` | Initialize new skill with template structure |
| `package_skill.py` | Package skill into distributable zip file |
| `quick_validate.py` | Validate skill structure and frontmatter |

## Usage

### Creating a New Skill

```bash
# Initialize a new skill
python scripts/init_skill.py my-new-skill --path ./skills

# This creates:
# ./skills/my-new-skill/
# ├── SKILL.md          (template with TODOs)
# ├── scripts/          (example script)
# ├── references/       (example reference doc)
# └── assets/           (example asset placeholder)
```

### Validating a Skill

```bash
# Quick validation
python scripts/quick_validate.py ./path/to/skill
```

### Packaging a Skill

```bash
# Package for distribution (validates first)
python scripts/package_skill.py ./path/to/skill

# With custom output directory
python scripts/package_skill.py ./path/to/skill ./dist
```

## Skill Structure

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter (name, description)
│   └── Markdown instructions
└── Bundled Resources (optional)
    ├── scripts/       - Executable code
    ├── references/    - Documentation
    └── assets/        - Templates, images, etc.
```

## Installation

```bash
/plugin install development@cosmic-code-marketplace
```
