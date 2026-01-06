---
description: Identify documentation gaps and create documentation tickets
argument-hint: [optional: specific area to document]
---

# Documentation Gaps Analysis

I'll identify missing or incomplete documentation across your codebase.

## Step 1: Analyze Documentation

Using the **documentation-gaps** skill to find:
- Missing README sections
- Undocumented APIs
- Functions without JSDoc/docstrings
- Missing code examples
- Incomplete architecture docs
- Missing troubleshooting guides

Analyzing for these audiences:
- **Developers** (internal team)
- **Users** (end users)
- **Contributors** (new team members)
- **Maintainers** (ops/deployment)

$ARGUMENTS

## Step 2: Create Documentation Tickets

After analysis, I'll:
1. Create `improvements/documentation/` directory
2. Generate individual ticket files for each gap
3. Format: `NNN-short-title.md` (e.g., `001-document-auth-api.md`)
4. Priority levels: 🔴 High, 🟡 Medium, 🟢 Low
5. Each ticket contains:
   - What's missing
   - Target audience
   - Proposed content outline
   - Example documentation
   - Files to update

**Starting documentation analysis...**
