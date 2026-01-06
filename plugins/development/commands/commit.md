---
description: Create storytelling-focused commits following Conventional Commits with optional Jira context
argument-hint: [--no-verify]
---

# Git Commit

I'll create a well-formatted commit that tells a complete story for future code archeology.

## Step 1: Gather Context

Using the **commit** skill to:
- Check staged and unstaged changes
- Review the diff to understand what changed
- Check recent commits for style consistency
- Extract Jira ticket from branch name (if applicable)

$ARGUMENTS

## Step 2: Ask Why

I'll ask you why you made these changes, presenting options based on the diff analysis.

## Step 3: Determine Commit Strategy

Based on the analysis:
- **Small changes**: Simple one-line commit
- **Significant changes**: Storytelling format with Why/What/Problem
- **Mixed changes**: Suggest splitting into multiple commits

## Step 4: Create Commit

Format: `<type>(<scope>): <description>`

For significant changes, includes:
- Why this change was needed
- What changed (technical summary)
- Problem solved
- Jira ticket reference (if found)

**Starting commit analysis...**
